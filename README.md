# learnflutter
# 📖 Phase 1: พื้นฐานการเขียน Prompt เพื่อสั่ง AI เขียนโปรแกรม
## บทที่ 1: รู้จักกับ AI Prompt Programming
###  1.1 AI Prompt คืออะไร
    Prompt คือ "คำสั่ง" หรือ "ข้อความอธิบาย" ที่เราเขียนเพื่อสื่อสารกับ AI ให้ทำงานตามที่เราต้องการ
    ในงานเขียนโปรแกรม เราใช้ Prompt เพื่อสั่งให้ AI:
    สร้างโค้ด
    สร้างไฟล์โปรเจกต์
    แก้บั๊ก
    Refactor โค้ด
    เพิ่มฟีเจอร์ใหม่

###  1.2 ความสามารถของ AI Coding
    AI ในปัจจุบัน (เช่น ChatGPT, Copilot) สามารถ:
    สร้างโค้ดตั้งแต่ต้น
    ออกแบบสถาปัตยกรรมระบบ
    สร้าง API, Database, UI
    แนะนำโครงสร้างที่ดีที่สุด (Best Practices)
### 1.3 ข้อจำกัด
    อาจพลาดได้ถ้า Context ไม่ชัด
    ความปลอดภัยต้องให้คนตรวจสอบ
    บางครั้งออกแบบไม่ดีที่สุดในงานขนาดใหญ่
# บทที่ 2: แนะนำระบบที่เราจะสร้าง (Installment Collection System)
##    2.1 แนวคิดระบบเก็บเงินค่างวด
        เราจะสร้างระบบที่มีความสามารถ:
        ลูกค้า Login ผ่านมือถือ
        ดูสัญญาของตัวเอง
        จ่ายเงินค่างวดผ่าน QR Code
        ส่งข้อมูลการชำระเงินกลับเข้าระบบ

##    2.2 โครงสร้างข้อมูลพื้นฐาน
        Contract Table: ข้อมูลสัญญา
        Payment Table: ข้อมูลการชำระเงิน
        User Table: ข้อมูลผู้ใช้งาน

###    ตัวอย่าง Model C#:
####    csharp
        public class Contract {
        public int Id { get; set; }
        public string ContractNumber { get; set; }
        public string CustomerName { get; set; }
        public DateTime DueDate { get; set; }
        public decimal OutstandingAmount { get; set; }
}
##    บทที่ 3: แนวทางการเขียน Prompt อย่างไรให้ AI สร้างโค้ดดี
###    3.1 โครงสร้างการเขียน Prompt
        Context: อธิบายสถานการณ์
        Instruction: สั่งงานแบบเฉพาะเจาะจง
        Example: ถ้ามีตัวอย่างจะทำให้แม่นยำยิ่งขึ้น
####    ตัวอย่าง:
        องค์ประกอบ	ตัวอย่าง
        Context	ฉันกำลังสร้าง Mobile App ด้วย Flutter สำหรับเก็บเงินค่างวด
        Instruction	ช่วยสร้างหน้าจอ Login ที่มีการตรวจสอบข้อมูล และเมื่อสำเร็จ ให้เก็บ Token ลง Storage
        Example	ตัวอย่าง API: POST /api/auth/login
####    Prompt ที่ได้:
####    plaintext
        Create a Flutter screen for user login with form validation. When the login is successful, call API endpoint '/api/auth/login' with email and password, then save the received JWT token using Flutter Secure Storage.
###    3.2 เทคนิคการเขียน Prompt ให้แม่น
        ชี้เฉพาะว่าใช้ภาษาอะไร (เช่น Flutter, C#, SQL)
        ระบุขนาด Scope เช่น "เฉพาะหน้าจอ Login" หรือ "เฉพาะ API Post"
        บอก Style หรือ Convention ถ้าต้องการ เช่น "Clean Architecture" หรือ "MVC Pattern"
        ใช้ Bullet หรือ Number list เพื่อเพิ่มความชัดเจน

##    บทที่ 4: ตัวอย่างการเขียน Prompt เบื้องต้น
###    4.1 สร้าง Flutter Project
####    Prompt:
        Create a new Flutter project called "installment_collection_app" with the following packages:
        - provider
        - http
        - flutter_secure_storage
####    คำอธิบาย:
        "Create a new Flutter project" คือ คำสั่งเริ่มต้น
        ชื่อโปรเจกต์ชัดเจน
        ระบุ Dependencies ที่ต้องการใช้งานเลย เพื่อไม่ต้องมาเพิ่มภายหลัง

##    4.2 สร้าง API Backend C#
###    Prompt:
        Create an ASP.NET Core Web API project named "InstallmentCollectionAPI". Add a ContractController with endpoints to:
        - Get all contracts
        - Get contract by ID
        - Create a contract
        - Update a contract
        - Delete a contract
        Use Entity Framework Core with SQL Server.
###    คำอธิบาย:
    ระบุชัดว่าต้องการ Controller อะไรบ้าง
    บอก Framework, ORM ที่ใช้ (Entity Framework Core)
    ชี้เป้าไปที่ Database ที่ใช้ (SQL Server)

#    บทที่ 5: วิธีสั่งให้ AI สร้างระบบ Authentication
##     5.1 องค์ประกอบระบบ Login
        รับ Email/Password
        ส่งไปยัง API
        รับ JWT Token
        บันทึก Token ไว้ใช้งานในอนาคต

###    Prompt:

####    plaintext    คัดลอก แก้ไข
        In Flutter, create a LoginScreen with:
        - Email and Password fields
        - Validate inputs
        - Call '/api/auth/login' API with email and password
        - If successful, save the JWT token securely

####    ตัวอย่างโค้ดที่ได้:
#####    dart คัดลอก แก้ไข
        final response = await http.post(Uri.parse('https://yourapi.com/api/auth/login'),
          body: jsonEncode({'email': email, 'password': password}),
          headers: {'Content-Type': 'application/json'}
        );

        if (response.statusCode == 200) {
              var token = jsonDecode(response.body)['token'];
              await storage.write(key: 'jwt_token', value: token);
        }
#    บทที่ 6: การเขียน Prompt เพื่อสั่งให้ AI วาดหน้าจอ (UI)
###   Prompt:
####    plaintext
        Create a Flutter ListView that displays a list of contracts. Each contract shows Contract Number, Customer Name, and Due Date.
####    อธิบาย:
        บอกว่าใช้ Flutter
        บอกว่าให้ใช้ Widget อะไร (ListView)
        ระบุข้อมูลที่ต้องโชว์บน Card / List Item

####    ตัวอย่างโค้ดที่ได้:
####    dart
        ListView.builder(
          itemCount: contracts.length,
          itemBuilder: (context, index) {
            final contract = contracts[index];
            return ListTile(
              title: Text(contract.contractNumber),
              subtitle: Text(contract.customerName),
              trailing: Text(contract.dueDate.toString()),
            );
          },
        );
#    บทที่ 7: วิธีตรวจสอบและปรับปรุง Prompt
##    7.1 ถ้าโค้ดไม่ตรงกับที่คิด ต้อง
###    เพิ่มรายละเอียดเพิ่มเติมใน Prompt ถามให้ AI ปรับแก้บางส่วน เจาะจงเทคนิค หรือ Library ที่ต้องการใช้งาน

####    ตัวอย่าง:
####    plaintext
        Improve the previous LoginScreen by adding a loading spinner when calling API.
#    บทที่ 8: การเขียน Prompt สำหรับ Database Design
####    Prompt:
####    plaintext
        Design a SQL Server database for installment collection with the following tables:
        - Users (id, email, passwordHash)
        - Contracts (id, contractNumber, customerName, dueDate, outstandingAmount)
        - Payments (id, contractId, paymentDate, amount)
        Add relationships and indexes where appropriate.
####    อธิบาย:
        บอกชัดเจนว่าต้องการ Table อะไร
        ให้ AI ช่วยออกแบบ Relationship
        ได้ ER Diagram และ SQL Script ทันที

#    บทที่ 9: หลักการวัดคุณภาพของ Prompt ที่ดี
##    9.1 Prompt ที่ดีต้อง
        สั้น แต่ครบ
        ชัดเจน
        มี Context ครบถ้วน
        มีตัวอย่างประกอบ (ถ้ามี)
        เปิดโอกาสให้ AI สร้างเพิ่มได้ (Flexible)

#    บทที่ 10: Workshop ท้าย Phase 1
####    โจทย์:
        เขียน Prompt สร้าง Flutter หน้า Login
        เขียน Prompt สร้าง ASP.NET Core API สำหรับ Login
        เขียน Prompt ออกแบบ Database Table สำหรับเก็บผู้ใช้งาน

####    ตัวอย่างคำตอบ:
####    plaintext
#####       Flutter:
                Create a LoginScreen with email/password fields, validate input, and call /api/auth/login API. Save JWT token using flutter_secure_storage.

#####        C# API:
                Create an AuthController with a POST /login endpoint that validates user credentials and returns a JWT token if successful.

#####        SQL Server:
                Create a Users table with columns: id (PK), email (unique), passwordHash, createdAt.
#    📌 สรุป Phase 1
        เข้าใจว่า Prompt คือเครื่องมือสื่อสารกับ AI
        เขียน Prompt อย่างเป็นระบบ
        สร้างทั้ง Frontend, Backend และ Database ด้วยการสั่งงาน AI
        ปูพื้นฐานให้พร้อมเริ่มลงมือสร้างโปรแกรมจริงใน Phase 2

