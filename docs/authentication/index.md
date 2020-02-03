
Xử lý đăng nhập (dựa trên passport.js)

```mermaid
sequenceDiagram
  participant C as Client
  participant A as AuthServer

  C->>A: POST /auth/basic { username, password }
  alt success
    A->>C: { access_token, refresh_token }
  else exception
    A->>C: Exception (422, 400, 500...)
  end
```

Xử lý đăng nhập với kênh đăng nhập bên thứ 3
Authenticate with <provider> (VD: facebook)
```mermaid
sequenceDiagram
  participant C as Client
  participant A as AuthServer

  C->>A: POST /auth/facebook { token }
  alt success
    A->>C: { access_token, refresh_token }
  else exception
    A->>C: Exception (422, 400, 500...)
  end
```

Refresh token
```mermaid
sequenceDiagram
  participant C as Client
  participant A as AuthServer

  C->>A: POST /auth/refresh { refresh_token }
  alt success
    A->>C: { access_token, refresh_token }
  else exception
    A->>C: Exception (422, 400, 500...)
  end
```

Lấy thông tin của đối tượng đăng nhập (thường là người dùng)
Thông tin ngữ cảnh (Authorization token được truyền thông qua Header của giao thức Http)
```mermaid
sequenceDiagram
  participant C as Client
  participant R as Server

  C->>A: GET /users/me Authorization: Bearer {access_token}
  alt success
    A->>C: { ...userinfo }
  else exception
    A->>C: Exception (422, 400, 500...)
  end
```
