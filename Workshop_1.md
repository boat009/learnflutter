# Workshop 1: สร้างระบบ Login ด้วย Prompt
###  โจทย์
####    จงเขียน Prompt เพื่อสั่ง AI สร้าง ระบบ Login ครบชุด โดยมีรายละเอียดดังนี้
          Flutter สร้างหน้า LoginScreen
          API ASP.NET Core สร้าง /api/auth/login
          SQL Server ออกแบบตาราง Users
          และต้องมีเงื่อนไข:
          ตรวจสอบว่า Email และ Password ไม่ว่าง
          ถ้า Login สำเร็จ ต้องได้ JWT Token
          ต้องเก็บ Token ลง Flutter Secure Storage

###  ✍️ ตัวอย่าง Prompt ที่ควรเขียน (ก่อนดูเฉลย)
####  🔹 สำหรับ Flutter
#####    plaintext
          Create a Flutter LoginScreen with:
          - Text fields for Email and Password
          - Validate that fields are not empty
          - Button to call '/api/auth/login' API
          - If successful, save JWT token into Flutter Secure Storage
          - Show loading spinner while API is calling
####  🔹 สำหรับ Backend C# API
#####    plaintext
          Create an ASP.NET Core Web API controller named AuthController with:
          - POST /api/auth/login endpoint
          - Accept email and password
          - Validate against Users table
          - If valid, return a JWT token
          - If invalid, return 401 Unauthorized
          Use Entity Framework Core and JWT Authentication.
####   🔹 สำหรับ SQL Server
######    plaintext
          Create a table named Users with fields:
          - Id (Primary Key, int, Identity)
          - Email (nvarchar(255), Unique)
          - PasswordHash (nvarchar(max))
          - CreatedAt (datetime)
          Hash passwords using BCrypt when saving users.
###  ✅ เฉลย Workshop 1 (ตัวอย่างโค้ดจริง)
####  🔸 Flutter LoginScreen ตัวอย่าง
#####  dart
        import 'package:flutter/material.dart';
        import 'package:http/http.dart' as http;
        import 'package:flutter_secure_storage/flutter_secure_storage.dart';
        import 'dart:convert';
        
        class LoginScreen extends StatefulWidget {
          @override
          _LoginScreenState createState() => _LoginScreenState();
        }
        
        class _LoginScreenState extends State<LoginScreen> {
          final emailController = TextEditingController();
          final passwordController = TextEditingController();
          final storage = FlutterSecureStorage();
          bool isLoading = false;
        
          void login() async {
            setState(() => isLoading = true);
            final response = await http.post(
              Uri.parse('https://yourapi.com/api/auth/login'),
              headers: {'Content-Type': 'application/json'},
              body: jsonEncode({
                'email': emailController.text,
                'password': passwordController.text,
              }),
            );
            if (response.statusCode == 200) {
              var token = jsonDecode(response.body)['token'];
              await storage.write(key: 'jwt_token', value: token);
              // Navigate to HomeScreen or next page
            } else {
              // Show error
            }
            setState(() => isLoading = false);
          }
        
          @override
          Widget build(BuildContext context) {
            return Scaffold(
              appBar: AppBar(title: Text('Login')),
              body: isLoading
                  ? Center(child: CircularProgressIndicator())
                  : Padding(
                      padding: EdgeInsets.all(20),
                      child: Column(
                        children: [
                          TextField(controller: emailController, decoration: InputDecoration(labelText: 'Email')),
                          TextField(controller: passwordController, decoration: InputDecoration(labelText: 'Password'), obscureText: true),
                          SizedBox(height: 20),
                          ElevatedButton(onPressed: login, child: Text('Login'))
                        ],
                      ),
                    ),
            );
          }
        }
####  🔸 ASP.NET Core AuthController ตัวอย่าง
#####    csharp
          [ApiController]
          [Route("api/auth")]
          public class AuthController : ControllerBase
          {
              private readonly ApplicationDbContext _dbContext;
              private readonly ITokenService _tokenService;
          
              public AuthController(ApplicationDbContext dbContext, ITokenService tokenService)
              {
                  _dbContext = dbContext;
                  _tokenService = tokenService;
              }
          
              [HttpPost("login")]
              public async Task<IActionResult> Login([FromBody] LoginRequest request)
              {
                  var user = await _dbContext.Users.FirstOrDefaultAsync(u => u.Email == request.Email);
                  if (user == null || !BCrypt.Net.BCrypt.Verify(request.Password, user.PasswordHash))
                      return Unauthorized(new { message = "Invalid email or password" });
          
                  var token = _tokenService.GenerateToken(user);
                  return Ok(new { token });
              }
          }
          
          public class LoginRequest
          {
              public string Email { get; set; }
              public string Password { get; set; }
          }
####  🔸 SQL Server Table ตัวอย่าง
#####    sql
          CREATE TABLE Users (
              Id INT IDENTITY(1,1) PRIMARY KEY,
              Email NVARCHAR(255) UNIQUE NOT NULL,
              PasswordHash NVARCHAR(MAX) NOT NULL,
              CreatedAt DATETIME DEFAULT GETDATE()
          );

