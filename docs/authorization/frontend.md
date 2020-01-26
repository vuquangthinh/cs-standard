

# Authorize in UI

- JWT sau khi nhận được từ API, cần được giải mã -> array of permission_code
- Tại UI cần phân quyền: được hiển thị hay không tuỳ thuộc vào việc có hay không permission (với các permission không cần context).
- Với các permission cần context cần được kiểm tra thông qua 1 hàm callback với tham số là object chứa giá trị context tương ứng.

# UseCase:
ReactJS


```javascriptreact
<Authorize role={["permission", user => true ]} args={[...params]}>
{/* ... children component */ }
</Authorize>
```

