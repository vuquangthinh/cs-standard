
Authenticate with username / password
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

Authenticate with <provider> (facebook)
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

  C->>A: POST /auth/refresh { token }
  alt success
    A->>C: { access_token, refresh_token }
  else exception
    A->>C: Exception (422, 400, 500...)
  end
```

Request with credentials
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