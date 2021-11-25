# Sự khác biệt giữa Controlled component và Uncontrolled component

## Controlled component

* Là 1 component native (`<input>`) nhận giá trị thông qua props và thông báo sự thay đổi lên component cha
* Component cha sẽ xử lý callback và update state của nó sau đó truyền prop mới xuống cho component con

## UnControlled component

* Là 1 component lưu trữ state riêng trong nội bộ và tự truy vấn DOM bằng tham chiếu để lấy giá trị hiện tại