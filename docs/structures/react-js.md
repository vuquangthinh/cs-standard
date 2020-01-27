

# Cấu trúc thư mục (dựa trên umijs)

```
/src/index.js (bootstrap)
    /config/
    /components/
    /pages/
    /services/
    /utils (tuỳ chọn)
```

Trong đó:
  - config: chứa các file cấu hình, hắng số như API_URL ...
  - components: chứa các component sử dụng chung trong hệ thống.
  - pages: các trang của web
  - services: các file chứa hàm service. Hàm service là 1 hàm Promise phục vụ việc gọi API / gọi internal API ...


_Best practice:_
- Mỗi file jsx chỉ nên define 1 component duy nhất. Nếu define nhiều hơn 1 component thì chỉ được export default 1 component duy nhất. Việc này nhằm tránh code bị dính và khó tái sử dụng. (1 file được require nhiều file khác nhau sẽ làm tăng sự phụ thuộc của chương trình vào file đó.)
- Mỗi file jsx chỉ nên import 1 file less. (Nhằm đảm bảo tính đóng gói)
- File less nên cùng tên với file jsx import nó. (Nhằm đảm bảo tính duy nhất, việc kế thừa có thể thực hiện nhưng không nên phức tạp - Nếu có hơn 2 file jsx cùng sử dụng 1 file less sẽ làm tăng tính kết dính -> khó sửa đổi vì không kiểm soát được mức độ ảnh hưởng đến các Component).
- File jsx chứa component nên có tên trùng với tên Component chứa nó (phục vụ thuận lợi cho việc tìm kiếm component).
- Tên file cần tối giản.
- Với Component có mức độ phức tạp nhỏ thì có tạo file với tên <Component>.js(x?). Khi độ phức tạp của file Component tăng và cần chia nhỏ thành các component nhỏ hơn (không nên để 1 file code có kích thước lớn > 600 - 800 dòng vì khó kiểm soát được chất lượng code cũng như dễ nát code).
- File <Component>.js(x?) khi chia nhỏ có thể tạo thư mục với tên Component, file index.js(x?) chứa code của file cũ. Các component, chức năng tách nhỏ từ file sẽ được đặt cùng cấp với file index. Các file có sử dụng component cũ sẽ không cần sửa đổi.
- Hạn chế tối đa việc import chéo component nhằm đảm bảo tính độc lập