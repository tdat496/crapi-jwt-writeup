# Phân Tích Chuỗi Lỗ Hổng Xác Thực (JWT) & Logic Nghiệp Vụ (BFLA) Trên OWASP crAPI

**Tác giả:** Nguyễn Trung Đạt  
**Ngày công bố:** 01/11/2026  
**Nền tảng kiểm thử:** OWASP crAPI v1.0 (Local Docker Environment)  
**Mức độ nghiêm trọng tổng thể:** Critical (CVSS 9.8)  

---

## Giới thiệu
Repository này chứa báo cáo kiểm thử bảo mật chuyên sâu về cơ chế xác thực **JSON Web Token (JWT)**, kiểm soát truy cập cấp chức năng (**BFLA**) và logic nghiệp vụ thương mại điện tử trên nền tảng **OWASP crAPI**. Báo cáo chứng minh cách kẻ tấn công có thể mạo danh bất kỳ người dùng nào, chiếm đoạt hoàn toàn tài khoản Admin (Account Takeover) và thao túng giá sản phẩm về $0 để gian lận tài chính trực tiếp.

## Tóm tắt các lỗ hổng đã phát hiện

| # | Lỗ hổng | Endpoint bị ảnh hưởng | Phân loại (OWASP API 2023) | Mức độ | Mô tả ngắn gọn |
|---|---------|-----------------------|----------------------------|--------|----------------|
| 1 | **JWT "None" Algorithm Attack** | Hầu hết các API xác thực (`/workshop/api/shop/orders`, `/community/.../comment`) | API2:2023 - Broken Authentication | **Critical (9.1)** | Bỏ qua xác thực chữ ký JWT bằng `"alg": "none"`, cho phép mạo danh bất kỳ người dùng nào |
| 2 | **Full Admin Account Takeover** | `POST /identity/api/auth/forgot-password` | API2:2023 - Broken Authentication | **Critical (9.8)** | Kết hợp JWT giả mạo để đặt lại mật khẩu và chiếm toàn quyền tài khoản `admin@example.com` |
| 3 | **BFLA & Price Manipulation** | `PUT /workshop/api/shop/products` | API5:2023 - BFLA & Business Logic Flaw | **High (8.8)** | Người dùng thường gọi được API quản trị để sửa giá sản phẩm về `$0.00` và mua hàng miễn phí |

## Đánh giá cơ chế phòng thủ (Negative Findings)
Bên cạnh các lỗ hổng phát hiện được, báo cáo cũng ghi nhận các cơ chế phòng thủ JWT hoạt động hiệu quả trên hệ thống trước các kỹ thuật tấn công nâng cao:
- **JWK Injection (CVE-2018-0114):** Bị chặn (`401 Unauthorized`).
- **Algorithm Confusion (RS256 ➔ HS256):** Bị chặn (`401 Unauthorized`).
- **Kid Header Path Traversal:** Không bị ảnh hưởng.

## Cấu trúc Repository
- `resources/`: Chứa file báo cáo đầy đủ định dạng PDF.
- `screenshots/`: Chứa các hình ảnh minh chứng (PoC) thực nghiệm trên Burp Suite & giao diện ứng dụng.
- `README.md`: File tóm tắt dự án này.

## Xem báo cáo đầy đủ
Bạn có thể tải xuống hoặc xem báo cáo chi tiết tại: [Bao_Cao_JWT_BFLA_crAPI.pdf](./resources/Bao_Cao_JWT_BFLA_crAPI.pdf)

## Responsible Disclosure
Báo cáo này được thực hiện trên môi trường lab cục bộ và công bố với mục đích giáo dục, nâng cao nhận thức bảo mật. Tác giả không chịu trách nhiệm cho bất kỳ hành vi lạm dụng nào từ thông tin trong báo cáo này.

## Liên hệ
- **Email:** trungdatnguyen9@gmail.com
- **GitHub:** [tdat496](https://github.com/tdat496)

---
© 2026 Nguyễn Trung Đạt. All rights reserved.