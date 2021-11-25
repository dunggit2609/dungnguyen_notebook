# React refs

## Khái niệm
* Tương tự như các keys lưu trữ tham chiếu đến các nút trong DOM và dùng để truy cập trực tiếp đến DOM

## Khi nào sử dụng
* Các phép đo DOM: quản lý tiêu điểm,  lựa chọn văn bản, media playback
* Sử dụng kích hoạt các hoạt ảnh bắt buộc (imperative animations)
* Tích hợp với thu viện DOM bên thứ 3

## Khi nào không nên dùng
* Tránh sử dụng trong mọi trường hợp
* Do refs trỏ trực tiếp vào DOM thật nên sẽ gây giảm performance

## Cách tạo refs
### Dùng React.createRef() và gắn vào các phần tử thông qua `ref`
```
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.myRef = React.createRef()
  }
  render() {
    return <div ref={this.myRef} />
  }
}
```
### ref callback
```
class SearchBar extends Component {
   constructor(props) {
      super(props);
      this.txtSearch = null;
      this.state = { term: '' };
      this.setInputSearchRef = e => {
         this.txtSearch = e;
      }
   }
   onInputChange(event) {
      this.setState({ term: this.txtSearch.value });
   }
   render() {
      return (
         <input
            value={this.state.term}
            onChange={this.onInputChange.bind(this)}
            ref={this.setInputSearchRef} />
      );
   }
}
```

## Forward refs
* là tính năng cho phép 1 số component chuyển tiếp tham chiếu mà nó nhận được cho component con
