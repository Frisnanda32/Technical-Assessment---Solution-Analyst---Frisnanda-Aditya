# Technical-Assessment---Solution-Analyst---Frisnanda-Aditya


Test Application Design
PT. XYZ adalah sebuah perusahaan fintech yang ingin mengembangkan mobile apps mereka, dalam
upaya menjangkau pengguna yang lebih luas mereka ingin mengembakan aplikasi pinjaman online.
Potential High Level User Story:
 User melakukan registrasi dengan data dir,emaili, nomor telepon dan upload foto beserta
KTP
 User dapat login dengan password atau biometric (jika ada di perangkat mobilenya) • User
dapat melihat Sisa hutang dan tagihan perbulan yang harus di bayarkan (Jika ada)
 User dapat meminjam uang paling besar Rp. 12.000.000 dengan tenor maksimal 1 taun.
 User dalam proses peminjaman akan di proses dengan hasil diterima atau ditolak
 Jika pinjaman diterima maka akan ada notifikasi lewat email dan nomot telepon yang
terdaftar
 User tidak dapat melakukan peminjaman uang jika sedang ada proses peminjaman dan
belum di lunaskan.
Tugas Anda:
1. Buatlah high level design architecture atas project mobile apps ini.
2. Spesifikasikan design Screen Flow dan ERD atas rancangan yang ingin anda buat.
3. Buatlah detail design untuk API dengan menggunakan tools design seperti UML, ERD,flowchart
etc.
4. Buatlah detail design untuk screen behavior dari mobile apps berdasarkan screen flow diatas.



1. Buatlah high level design architecture atas project mobile apps ini.

+--------------------------+
|     Mobile Application     |
|----------------------------|
| - UI (Flutter/ReactNative) |
| - Local Storage (SQLite)   |
| - Biometric SDK            |
+------------▲---------------+
             |
             ▼
+---------------------------+
|       API Gateway         |
|---------------------------|
| - Authentication Service  |
| - Loan Service            |
| - Notification Service    |
| - User Management         |
+------------▲-------------+
             |
             ▼
+---------------------------+
|     Application Layer     |
|---------------------------|
| - Business Logic          |
| - Validation Layer        |
| - Integration Layer       |
+------------▲-------------+
             |
             ▼
+---------------------------------+
|     Database (PostgreSQL/MySQL) |
|---------------------------------|
| - User Table                    |
| - Loan Table                    |
| - Repayment Table               |
| - Notification Log              |
+---------------------------------+

2. Spesifikasikan design Screen Flow dan ERD atas rancangan yang ingin anda buat.
a. Screen Flow Diagram
[Start]
   |
   ▼
[Welcome Screen]
   └──► [Login/Register]
             ├──► [Register Screen]
             │       └──► Upload KTP & Photo
             └──► [Login Screen]
                     └──► Password / Biometric
                              |
                              ▼
                      [Home Screen]
                              ├──► View Status Pinjaman
                              ├──► Ajukan Pinjaman
                              └──► Logout
							  
b. ERD (Entity Relationship Diagram)
[User]
- user_id (PK)
- name
- email
- phone
- password_hash
- biometric_flag
- photo_url
- ktp_url
- created_at

[Loan]
- loan_id (PK)
- user_id (FK)
- amount
- status (pending/accepted/rejected)
- start_date
- end_date
- tenor_months

[Repayment]
- repayment_id (PK)
- loan_id (FK)
- due_date
- amount_due
- paid_flag
- paid_at

[Notification]
- notif_id (PK)
- user_id (FK)
- message
- channel (email/sms)
- sent_at

3. Buatlah detail design untuk API dengan menggunakan tools design seperti UML, ERD,flowchart
etc.
a. UML - Use Case Diagram
Use Cases:
- Register
- Login
- View Loan Status
- Apply for Loan
- Receive Notification

B. Flowchart: Loan Application
[User applies for loan]
     |
     ▼
[System checks active loan?]
     ├──Yes→ [Reject: cannot apply]
     └──No → [Evaluate eligibility]
                     |
                     ▼
           [Decision: Approve/Reject]
                     |
        ┌────────────┴────────────┐
        ▼                         ▼
[Send email & SMS]        [Send rejection notif]
        |
        ▼
[Update loan status]
C. Sample API Specs
1. POST /api/register

json
Copy
Edit
Request:
{
  "name": "John",
  "email": "john@email.com",
  "phone": "0812xxxx",
  "password": "hashed_pwd",
  "ktp_image": "base64",
  "photo": "base64"
}
Response: 201 Created
2. POST /api/login

json
Copy
Edit
Request:
{
  "email": "john@email.com",
  "password": "xxxxx"
}
Response: 
{
  "token": "jwt_token",
  "biometric_enabled": true
}
3. GET /api/loans

json
Copy
Edit
Response:
{
  "loan_status": "accepted",
  "remaining_amount": 4500000,
  "monthly_due": 750000
}
4. POST /api/loans

json
Copy
Edit
Request:
{
  "amount": 12000000,
  "tenor": 12
}
Response:
{
  "status": "pending",
  "message": "Loan application submitted"
}




4. Buatlah detail design untuk screen behavior dari mobile apps berdasarkan screen flow diatas.
A. Register Screen
	- Input: Name, Email, Phone, Password
	- Upload: KTP & Selfie
	- Validation: Mandatory field check, phone format
B. Login Screen
	- Input: Email, Password / Biometric
	- Biometric check via FaceID/TouchID if enabled
C. Home Screen
	- Show: Summary of loan status (amount, due)
	- Action: Button "Ajukan Pinjaman" jika belum ada pinjaman aktif
D. Loan Application Screen
	- Input: Jumlah pinjaman (max 12jt), Tenor (max 12 bulan)
	- Submit: Button "Ajukan"
	- Result: Informasi status aplikasi (pending/accepted/rejected)
E. Notification Behavior
	- Upon approval: Pop-up + Email + SMS
	- Upon rejection: Pop-up + Email
