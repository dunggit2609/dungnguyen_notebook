# React router

## Khái niệm
* Là thư viện hỗ trợ cung cấp các URL để app truy cập đến các page của app

## Lưu ý cảnh báo `Router may have only one child element`

* là vì chưa khai báo `Switch` bên ngoài các `Route`
```
<Router>
  <Switch>
    <Route {/* ... */} />
    <Route {/* ... */} />
  </Switch>
</Router>
```

## Khai báo trang mặc định Not Found

* khai báo route của Not Found cuối cùng, Switch sẽ duyệt các route từ trên xuống dưới, nếu không match các route bên trên thì sẽ vào not found