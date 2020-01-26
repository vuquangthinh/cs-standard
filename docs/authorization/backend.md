# Sử dụng ABAC - ACAC

- Tất cả các action cơ bản đều được phân quyền với name ~ API path.
- Tuỳ thuộc độ phức tạp của hệ thống, authorization xử lý phân quyền bằng name_code và ngữ cảnh tương ứng với từng name_code.
- Tất cả các mức phân quyền đều được gọi chung là permission có thể kế thừa hoặc không (VD RBAC có kế thừa role).
- Kiểm soát quyền hay không dựa trên việc có hay không có permission + ngữ cảnh (context: user_id, post_id ...)
- Các permission được flat và gắn tại jwt
