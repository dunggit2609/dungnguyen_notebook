# Memoization là gì
* Tính toán và lưu kết quả cho từng bộ input.
* Khi gặp lại bộ input đã từng làm thì không tính toán lại, mà trả về kết quả sẵn có.

## useCallback()
* Return `memoized callback`
* tính toán lại khi dependencies thay đổi
* dùng dependencies là `[]`
```
// Mỗi lần App re-render
// --> tạo ra một function mới
// --> Chart bị re-render
function App() {
 const handleChartTypeChange = (type) => {}
 return <Chart onTypeChange={handleChartTypeChange} />;
}
```
```
// Mỗi lần App re-render
// --> nhờ có useCallback() nó chỉ tạo function một lần đầu
// --> Nên Chart ko bị re-render.
function App() {
 const handleChartTypeChange = useCallback((type) => {}, [])
 return <Chart onTypeChange={handleChartTypeChange} />;
}
```

## useMeMo()
* return `memoized value`
* tính toán lại khi dependencies thay đổi
* dùng dependencies là `[]`
```
// Mỗi lần App re-render
// --> tạo ra một mảng mới
// --> Chart bị re-render do props thay đổi
function App() {
 const data = [{}, {}, {}];
 return <Chart data={data} />;
}
```
```
// Mỗi lần App re-render
// --> nhờ có useMemo() nó chỉ tạo ra cái mảng 1 lần đầu
// --> Nên Chart ko bị re-render.
function App() {
 const data = useMemo(() => [{}, {}, {}], [])
 return <Chart data={data} />;
}
```

## Lưu ý
* Không nên dùng cho tất cả components
* Chỉ nên dùng cho đồ thị, biểu đồ, animations, component nặng phần render
