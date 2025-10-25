# Báo cáo: Thiết kế Sơ đồ UML cho Mini App E-Learning

## Thông tin sinh viên

- **Họ và tên:** `Nguyễn Văn A`
- **Mã số sinh viên:** `XXXXXXXX`
- **Lớp:** `IT-XXXX`

---

## Giới thiệu Đề tài

Dự án này tập trung vào việc phân tích và thiết kế hệ thống cho một "Mini App E-Learning", một ứng dụng hỗ trợ học tập trực tuyến dành cho sinh viên và giảng viên. Tài liệu này trình bày các sơ đồ UML cốt lõi để mô tả kiến trúc và hành vi của hệ thống, bao gồm: Sơ đồ Use Case, Sơ đồ Tuần tự, Sơ đồ Lớp và Sơ đồ Trạng thái.

---

### **Câu 1: Sơ đồ Use Case (UC) & Sơ đồ Tuần tự (SQ)**

#### **1.1. Sơ đồ Use Case (Use Case Diagram)**

Sơ đồ Use Case mô tả các chức năng chính của hệ thống từ góc nhìn của người dùng (actors).

- **Tác nhân (Actors):**
  - **Sinh viên:** Người học, tham gia khóa học.
  - **Giảng viên:** Người dạy, quản lý khóa học.

- **Sơ đồ (Đã sửa lỗi dòng trống):**
```mermaid
graph TD
    %% Khai báo Actors (Tác nhân) cho đúng chuẩn hình người
    actor SV as "Sinh viên"
    actor GV as "Giảng viên"

    %% System boundary (Ranh giới hệ thống)
    subgraph "Hệ thống Mini App E-Learning"
        %% Khai báo Use Cases cho đúng chuẩn hình oval
        UC1("Đăng nhập / Đăng ký")
        UC2("Xem danh sách khóa học")
        UC3("Đăng ký khóa học")
        UC4("Học bài")
        UC5("Làm bài tập / Thi")
        UC6("Xem điểm")
        UC7("Quản lý khóa học")
        UC8("Tải lên tài liệu")
        UC9("Tạo bài tập / Thi")
        UC10("Chấm điểm")
        UC11("Tham gia thảo luận")
    end

    %% Associations (Liên kết giữa Actor và Use Case)
    SV --> UC1
    SV --> UC2
    SV --> UC3
    SV --> UC4
    SV --> UC5
    SV --> UC6
    SV --> UC11

    GV --> UC1
    GV --> UC7
    GV --> UC8
    GV --> UC9
    GV --> UC10
    GV --> UC11

    %% <<include>> relationships (Quan hệ bao gồm)
    %% Một Use Case bắt buộc phải gọi một Use Case khác
    UC3 ..> UC1 : <<include>>
    UC4 ..> UC1 : <<include>>
    UC5 ..> UC1 : <<include>>
    UC7 ..> UC1 : <<include>>
```

#### **1.2. Sơ đồ Tuần tự (Sequence Diagram)**

Mô tả luồng tương tác giữa các đối tượng để thực hiện một Use Case cụ thể.

**a. Luồng chức năng "Đăng nhập"**
```mermaid
sequenceDiagram
    participant User as Người dùng
    participant LoginUI as "Giao diện Đăng nhập"
    participant System as "Hệ thống"
    participant Database as "Cơ sở dữ liệu"

    User->>LoginUI: 1. Nhập email và mật khẩu
    LoginUI->>System: 2. Gửi yêu cầu xác thực (email, password)
    System->>Database: 3. Truy vấn thông tin người dùng (email)
    Database-->>System: 4. Trả về thông tin (nếu có)
    System->>System: 5. So sánh mật khẩu
    alt Đăng nhập thành công
        System-->>LoginUI: 6. Gửi token/session thành công
        LoginUI-->>User: 7. Chuyển hướng đến trang chủ
    else Đăng nhập thất bại
        System-->>LoginUI: 6. Báo lỗi (sai thông tin)
        LoginUI-->>User: 7. Hiển thị thông báo lỗi
    end
```

**b. Luồng chức năng "Sinh viên nộp bài tập"**
```mermaid
sequenceDiagram
    participant Student as Sinh viên
    participant CourseUI as "Giao diện Khóa học"
    participant AssignmentUI as "Giao diện Bài tập"
    participant System as "Hệ thống"
    participant Database as "Cơ sở dữ liệu"

    Student->>CourseUI: 1. Chọn bài tập cần nộp
    CourseUI->>AssignmentUI: 2. Hiển thị chi tiết bài tập
    Student->>AssignmentUI: 3. Tải lên file bài làm và nhấn "Nộp bài"
    AssignmentUI->>System: 4. Gửi yêu cầu Nộp bài (studentId, assignmentId, file)
    System->>Database: 5. Lưu file và cập nhật trạng thái bài nộp
    Database-->>System: 6. Xác nhận lưu thành công
    System-->>AssignmentUI: 7. Gửi thông báo thành công
    AssignmentUI-->>Student: 8. Hiển thị "Nộp bài thành công!"
```

---

### **Câu 2: Sơ đồ Lớp (Class Diagram)**

Sơ đồ lớp mô tả cấu trúc tĩnh của hệ thống, bao gồm các lớp, thuộc tính, phương thức và mối quan hệ giữa chúng.

```mermaid
classDiagram
    class NguoiDung {
        - maND: String
        - hoTen: String
        - email: String
        - matKhau: String
        + dangNhap()
        + dangXuat()
    }

    class SinhVien {
        - maSV: String
        + dangKyKhoaHoc(KhoaHoc)
        + nopBai(BaiTap, file)
        + xemDiem()
    }

    class GiangVien {
        - maGV: String
        + taoKhoaHoc(thongTin)
        + themBaiHoc(KhoaHoc, BaiHoc)
        + chamDiem(BaiNop, diem)
    }

    class KhoaHoc {
        - maKH: String
        - tenKH: String
        - moTa: String
    }

    class BaiHoc {
        - maBH: String
        - tieuDe: String
        - noiDung: String
        - loaiTaiLieu: String
    }

    class BaiTap {
        - maBT: String
        - tieuDe: String
        - deBai: String
        - hanNop: Date
    }

    class BaiNop {
        - maNop: String
        - fileNop: String
        - ngayNop: Date
        - diem: float
        - trangThai: String
    }

    NguoiDung <|-- SinhVien
    NguoiDung <|-- GiangVien

    GiangVien "1" -- "0..*" KhoaHoc : Quản lý
    KhoaHoc "1" -- "1..*" BaiHoc : Chứa
    KhoaHoc "1" -- "0..*" BaiTap : Có
    SinhVien "1..*" -- "0..*" KhoaHoc : Tham gia
    SinhVien "1" -- "0..*" BaiNop : Nộp
    BaiTap "1" -- "0..*" BaiNop : Là bài nộp của
```

---

### **Câu 3: Sơ đồ Trạng thái (State Diagram)**

Sơ đồ trạng thái mô tả các trạng thái của một đối tượng trong vòng đời của nó. Dưới đây là sơ đồ trạng thái cho đối tượng **"Bài nộp" (`BaiNop`)**.

```mermaid
stateDiagram-v2
    [*] --> ChuaNop : Bài tập được tạo
    
    state "Chưa nộp" as ChuaNop
    state "Đang làm" as DangLam
    state "Đã nộp" as DaNop
    state "Đã chấm điểm" as DaChamDiem
    state "Quá hạn" as QuaHan

    ChuaNop --> DangLam : Sinh viên bắt đầu làm
    DangLam --> ChuaNop : Lưu nháp và thoát
    DangLam --> DaNop : Nộp bài
    
    ChuaNop --> QuaHan : Hết hạn nộp
    DangLam --> QuaHan : Hết hạn nộp
    
    DaNop --> DaChamDiem : Giảng viên chấm điểm
    DaChamDiem --> [*]
    
    note right of DaNop
        Sau khi nộp,
        sinh viên không
        thể chỉnh sửa.
    end note
```
