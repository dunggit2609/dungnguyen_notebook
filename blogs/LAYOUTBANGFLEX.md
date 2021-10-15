## Chia các div bằng nhau dùng flex
```
<div class="container">
    <div class="item">
    <div class="item">
    <div class="item">
</div>

.container {
    display: flex;
}
.item {
    flex-grow: 1; // tỉ lệ của các item so với các item khác (các item tự dàn ra cho khít với container)
}
```