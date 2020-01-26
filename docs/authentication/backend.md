
Để đảm bảo ẩn thông tin (tương đối) với các trình log, sử dụng các phương thức như POST / PUT / PATCH với thông tin nhạy cảm được truyền qua Body.

# Structure

POST /auth/basic
{
  username,
  password
}

// google, facebook ...
POST /auth/<provider>
{
  provider params ...
}


Cài đặt trên NestJs:
// TODO: Link to nestjs module