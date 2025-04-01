# XXE Vulnerability CTF Exercise

## Mục Lục
1. [Yêu cầu](#yêu-cầu)
2. [Lý thuyết](#lý-thuyết)
3. [Thực hành](#thực-hành)
   - [Cấu trúc yêu cầu](#cấu-trúc-yêu-cầu)
   - [Lỗi XXE](#lỗi-xxe)
4. [Hướng dẫn Fix Lỗi XXE](#hướng-dẫn-fix-lỗi-xxe)

## Yêu cầu

### Lý thuyết
- Tìm hiểu lỗi **XXE (XML External Entity)**:
   - **Định nghĩa**: XXE là một lỗ hổng bảo mật xảy ra khi một ứng dụng xử lý XML có thể bị khai thác để đọc các tệp tin hệ thống hoặc thực thi mã độc từ một nguồn ngoài dự kiến.
   - **Phân loại**: Có thể chia XXE thành các loại sau:
     - **External Entity Injection**: Lỗi này cho phép kẻ tấn công khai thác các tệp XML chứa tham chiếu đến các tài nguyên ngoài (ví dụ: tệp hệ thống, URL).
     - **Blind XXE**: Lỗi XXE không trả về thông tin trực tiếp cho người tấn công, nhưng vẫn có thể gây tổn hại (ví dụ: rò rỉ thông tin qua kết quả thay đổi).
   - **Cách khai thác và phòng chống**:
     - **Khai thác**: Kẻ tấn công có thể gửi một tệp XML chứa tham chiếu đến một tài nguyên ngoài như tệp trên hệ thống hoặc một dịch vụ HTTP. Điều này có thể dẫn đến việc rò rỉ thông tin nhạy cảm.
     - **Phòng chống**:
       - Vô hiệu hóa hoặc hạn chế việc xử lý các External Entities trong XML.
       - Sử dụng các thư viện bảo mật khi phân tích XML (ví dụ: `lxml` với Python).
       - Đảm bảo các thư viện xử lý XML không cho phép đọc các tài nguyên ngoài hệ thống.

### Thực hành
- Sinh viên cân nhắc làm các bài **labs** liên quan đến XXE trước khi bắt đầu viết mã CTF để hiểu rõ hơn về lỗi XXE và cách khai thác nó.

#### Cấu trúc yêu cầu
- Sinh viên cần xây dựng một trang web theo cấu trúc sau (không yêu cầu giao diện đẹp):
  - **Chức năng upload file XML**: Cho phép người dùng tải lên một tệp XML chứa danh sách sinh viên với các thông tin như **Tên**, **Năm sinh**, và **Trường học**.
  - **Parse và hiển thị thông tin**: Sau khi nhận được tệp XML, hệ thống sẽ phân tích tệp và in ra một bảng thông tin sinh viên.
  - **Chức năng chứa lỗi BLIND XXE**: Code phải chứa một lỗi BLIND XXE để mô phỏng một tình huống bảo mật dễ bị khai thác.
  - **Sửa lỗi XXE**: Tạo ra một phiên bản mã sửa lỗi XXE, đặt tên theo định dạng `_fixed` (ví dụ: `xxeparser_fixed.php`).

### Lỗi XXE
- **Blind XXE**: Khi hệ thống không trả về thông tin trực tiếp về các tệp hoặc tài nguyên ngoài, nhưng vẫn có thể thực thi các lệnh hoặc truy cập dữ liệu trên hệ thống đích. Lỗi này có thể bị lợi dụng để thực thi mã độc, dò tìm thông tin hệ thống hoặc tấn công từ chối dịch vụ (DoS).

## Hướng dẫn Fix Lỗi XXE

1. **Sử dụng thư viện bảo mật khi phân tích XML**:
   - Đảm bảo không sử dụng các tính năng External Entities trong các thư viện phân tích XML.
   
2. **Ví dụ code sửa lỗi XXE** (sử dụng Python `lxml`):
   ```python
   from lxml import etree
   
   parser = etree.XMLParser(resolve_entities=False)  # Disable External Entities
   tree = etree.parse("input.xml", parser)
