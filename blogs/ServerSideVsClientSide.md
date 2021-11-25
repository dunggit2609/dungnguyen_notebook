# khác nhau giữa SSR và CSR

## SSR

### ưu điểm
* Initial load nhanh, dễ otpimize, vì toàn bộ dữ liệu đã được
* SEO tốt vì khi bot của Google, Bing vào web sẽ thấy toàn bộ dữ liệu dưới dạng HTML

### nhược điểm
* Mỗi lần người dùng chuyển trang là site phải load lại nhiều lần, gây khó chịu
* Tương tác không tốt như Client Side rendering vì trang phải refresh, load lại nhiều lần.

## CSR

### ưu
* Page chỉ cần load một lần duy nhất. Khi user chuyển trang hoặc thêm dữ liệu, call data. User có thể thấy dữ liệu mới mà không cần chuyển trang.

## nhược
* init chậm
* không chạy được js nếu bị disable