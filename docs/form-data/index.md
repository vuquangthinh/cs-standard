# Input Controller
Input Field Interface (Antd Design Format)

PropTypes: {
  value: any,
  onChange: (eventOrValue: any | Event) => void
}



# Data Format
- Các thông tin gửi lên cần được validate tương đối (validate: required, format, number).
- Các thông tin gửi lên nên được chuẩn hoá đúng khuôn dạng / kiểu dữ liệu nhằm tránh sai sót (do shit của server :v). Các dữ liệu cần chuẩn hoá như boolean / number / string / datetime.

| Dữ liệu          | JSON          |
| ---------------- |:-------------:|
| Number           | 1234          |
| Number / Integer | 1234          |
| String           | 'string'      |
| Boolean          |  true / false |
| DateTime         |  [ISO](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString) |

_
Lưu ý với DateTime / Date nên chuẩn hoá về string YYYY-MM-DDTHH:mm:ss.sssZ
Kiểu Boolean một số server có thể tự động ép kiểu falsy (1 -> True, "0" -> True =)) ) do vậy cần kiêm tra kĩ và chuẩn hoá dữ liệu trước khi gửi request lên server.
_

# Data Serialize
- Sử dụng JSON format và các serialize tương thích (messagepack, protobuf).
- Object dữ liệu được xử lý trung gian với kiểu dữ liệu File. Nếu value của 1 field bất kỳ có instanceof File thì chuyển Content Type từ json qua multipart và chuẩn hoá dữ liệu tương ứng. (hạn chế sử dụng cờ như isFile vì làm phức tạp tham số request).

```javascript
function containsFile(obj) {
  return Object.keys(obj).some(field => {
    if (obj[field] instanceof File) {
      return true;
    }

    return false;
  });
}

function objectAsFormData(obj) {
  const fm = new FormData();
  Object.keys(obj).forEach((field) => {
    fm.add(field, obj[field]);
  })
}

```