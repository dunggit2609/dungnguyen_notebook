# hook userRef

* Sẽ trả về 1 object với key là `current` và value là giá trị mình mong muốn
* VD 
```
a = useRef(1) // a = {current: 1}
```

## hữu ích trong việc
* do object được tạo ra từ useRef sẽ tồn tại trong suốt life cycle của component nên những lần re-render của component sẽ không init lại object này
* tương tác với DOM dễ hơn