# Bubbling event
* Là hiện tượng khi trigger event ở phần tử con sẽ kích hoạt tiếp tục lên phần tử cha, phàn tử cha kích hoạt lên phần tử ông,... các phần tử con sẽ "up through"
* hầu hết các event đều bubble ngoại trừ focus
* dùng `event.stopPropagation` để ngăn bubbling