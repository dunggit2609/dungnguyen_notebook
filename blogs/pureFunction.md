# Pure function
* Là hàm không phụ thuộc vào các biến bên ngoài, luôn trả về cùng kết quả nếu input giống nhau
* Không tạo ra các side effect

## Observable side effect
* Là các sự tương tác từ bên ngoài vào trong hàm như thay đổi giá biến bên ngoài hàm, dùng callback ở trong hàm

## VD pure function
```
function priceAfterTax(productPrice) {
 return (productPrice * 0.1) + productPrice;
}
```

## VD không phải pure function
* Vì kết quả của hàm sẽ phụ thuộc vào biến tax bên ngoài hàm
```
var tax =  10;

function calculateTax(productPrice) {
 return (productPrice * (tax/100)) + productPrice; 
}
```