
Thành phần chức thực là phần không thể thiếu trong hệ thống thông tin. Frontend nên sử dụng các API theo mô tả tại authentication/index.md để đảm bảo tránh sai sót.

Thành phần quản lý đăng nhập và phiên truy cập cần đảm bảo tính trong suốt. Các thông tin ngữ cảnh nên được loại bỏ (hoặc sử dụng như là tham số tuỳ chọn) nhằm tránh rườm rà trong quá trình gọi API bất kỳ.

Để quản lý phiên làm việc, hệ thống sẽ sử dụng token để nhận dạng người dùng.
Token có thể sử dụng JWT để làm mã hoá với thời gian hữu dụng ngắn.
Việc sinh token mới sẽ cần (nên) được gọi tự động nếu hàm kiểm tra expiresIn + issuedAt > now và nên block toàn bộ các request cho tới khi request lấy token mới được thực hiện thành công. Một số hệ thống có xử lý ràng buộc chặt sẽ đảm bảo không tồn tại 2 request với tham số refresh_token giống nhau (trả về Http GONE).

Có nhiều cách cài đặt để đảm bảo việc này với ReactJS (các nền tàng khác cũng xử lý tương đương):
- Dùng while chờ cờ request refresh_token được đổi.
- Dùng Saga để watch và block các request cho tới khi request lấy refresh_token được thực hiện thành công (nếu access_token còn hiệu lực thì put sự kiện báo cho việc take action được thông).
- Sử dụng EventEmitter (tương tự như redux saga). Đăng ký sự kiện lấy refresh_token hoàn thành (với once) và đồng thời gọi sự kiện lấy refresh_token. Tại thời điểm gọi, nếu sự kiện lấy refresh_token đang được thực hiện thì chờ cho tới khi nhận được token mới thành công thì emit các sự kiện được đăng ký.[Token Management](https://www.npmjs.com/package/token-management)
- Một số thư viện đã cài đặt 1 phần việc block request cho tới khi cài đặt thành công thao tác (VD [axios với interceptor](https://www.npmjs.com/package/axios#interceptors), [APISauce](https://github.com/infinitered/apisauce)).

