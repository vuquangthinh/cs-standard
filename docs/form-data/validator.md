
- Cú pháp chung cho kết quả trả về khi báo lỗi của từ server cho client.
- Mã lỗi luôn là 400 / 422 (tuỳ thuộc ý chí của người code :v )


Status: 422 / 400
{
  [field:string]: [message: string]
  ...
}

Kết quả báo lỗi luôn là 1 object json với property là tên của field chứa lỗi, value là nội dung của báo lỗi.
Trong nhiều trường hợp thông tin gửi lên có cấu trúc phức tạp, property có thể là [json path](https://lodash.com/docs/4.17.15#get) tới field chứa lỗi.

Một số API có sẵn nếu không hỗ trợ sẵn cấu trúc này cần được xử lý để quy về dạng thống nhất.

