# Chap 1
* Viết business logic trong `<script>` và view logic trong `<template>`

* Không xử lý dữ liệu trong `<template>`
```
<div id="app">
 <p v-if="hours < 12">Good morning!</p>
 <p v-if="hours >= 12 && hours < 18">Good afternoon!</p>
 <p v-if="hours >= 18">Good evening!</p>
</div>
<script>
 new Vue({
 el: '#app',
 data: {
 hours: new Date().getHours()
 }
 });
</script>
```

* Có thể pass data dạng array và object vào template thông qua expressions nhờ Vue đã JSON-encoded

### **v-if vs v-show**

* v-if: không generate DOM => không load dữ liệu
* v-show: genrate DOM và ẩn/ hiện bằng css display none => load dữ liệu bên trong

```
 <div v-show="user">
 <p>User name: {{ user.name }}</p>
 </div>
<script>
 new Vue({
 el: '#app',
 data: {
 user: undefined
 }
 });
</script>
```

* VD trên sẽ error vì v-show sẽ load user.name, nhưng nó undefined
* VD trên dùng v-if sẽ không có error

### *Tại sao dùng v-show trong khi v-if trong sịn hơn* ?
* v-if tốn performance, cần generate lại DOM khi element được thêm, xóa => tốn tài nguyên
* v-show chỉ tốn trong lần init 
* **Cần show element thay đổi nhiều => Dùng v-show**
* Tuy nhiên, nếu element chứa image và có ý định là hide lại => download xong mới show thì sẽ không work. Vì nó sẽ không bắt đầu download trừ khi nó được display

### **Looping in templates**
* thứ tự arg trong v-for(value, key)
```
 <ul>
 <li v-for="n in 10">{{ n }}</li>
 </ul>
```
* Đoạn code trên sẽ return n có giá trị từ (0 -> 9), nghĩa là chạy từ 0 -> n-1

### **Binding Arguments**
* v-bind dùng bind value vào HTML attribute
```
 <button v-bind:type="buttonType">Test button</button>
```
* v-bind: tên directives, type: button attribute, buttonType: value của attribute
* v-bind có thể dùng bind properties, vd như disabled, checked
* viết ngắn gọn hơn, nên dùng cách này
```
<button :type="buttonType">Test button</button>
```

### **Reactivity**
**Cách hoạt động**
* Nó theo dõi sự thay đổi của các object được thêm vào object data
* Mọi property trong object được thay thế bằng getter and setter => sử dụng bình thường
* Nhưng khi thay đổi property, Vue biết được sự thay đổi
* Vue support cho các array methods như là .slice() , cho phép method lắng nghe khi nào nó được gọi. Vì vậy, Vue biết khi nào chúng ta cập nhật array và thay đổi các view cần thiết
**Warning**
#### **Adding new properties to an object**
* Khi init các value trong object data, các value đã được add sẵn getter và setter nên Vue có thể theo dõi sự thay đổi
* Nhưng đối với các value được add vào sau => Vue sẽ không theo dõi sự thay đổi => **Không reactive khi value này thay đổi**
```
const vm = new Vue({
 data: {
 formData: {
 username: 'someuser'
 }
 }
});
vm.formData.name = 'Some User';
```
##### **Cách khắc phục**
* Define trước các biến sẽ sử dụng ở trong object data với init value là undefined nếu không muốn gán giá trị init cho nó
```
formData: {
 username: 'someuser',
 name: undefined
}
```
* Dùng object.assign() để thêm 1 property
```
vm.formData = Object.assign({}, vm.formData, {
 name: 'Some User'
});
```
* Dùng Vue.set(), trong component có thể dùng this.$set()
```
Vue.set(vm.formData, 'name', 'Some User');
```
#### **Setting items on an array**
* Set value cho 1 item thuộc array trong object data bằng index => không reactive
```
const vm = new Vue({
 data: {
 dogs: ['Rex', 'Rover', 'Henrietta', 'Alan']
 }
});
vm.dogs[2] = 'Bob'
```
##### **Cách khắc phục**
* Dùng .slice() để remove item cũ và add item mới
```
vm.dogs.splice(2, 1, 'Bob);
```
* Dùng Vue.set()
```
Vue.set(vm.dogs, 2, 'Bob');
```
#### **Setting items on an array**
* Nếu set length of array bằng cách truyền thống => không reactive
##### **Cách khắc phục**
* Dùng slice(). Nhưng chỉ dùng giảm độ dài mảng, không dùng tăng độ dài mảng
```
vm.dogs.splice(newLength);
```
### TWO WAY DATA BINDING
* Khi dùng **v-model** , nếu set value, checked hoặc thuộc tính cho thẻ HTML thì sẽ đều bị bỏ qua.
* Nếu cần set giá trị mặc định thì nên set trong object **data**

### Thiết lập HTML tự động
* Khi muốn hiển thị đoaạn HTML từ API trả về có thể dùng **directives v-html**
```
<div v-html="yourHtml"></div>
```
* Hãy cần thận vì khi viết HTML từ biến, sẽ dê bị người từ bên ngoài thực bất kì đoạn code nào trên web của chúng ta.

### METHODS
### this
* Trong một method, nó tham chiếu đến component chứa method đó. Chúng ta có thể truy cập thuộc tính trong **data object** và những phương thức khác bằng cách sử dụng **this**

```
<div id="app">
 <p>The sum of the positive numbers is {{ getPositiveNumbersSum() }}</p>
</div>
<script>
 new Vue({
 el: '#app',
 data: {
 numbers: [-5, 0, 2, -1, 1, 0.5]
 },
 methods: {
 getPositiveNumbers() {
 // Note that we're now using this.numbers
 // to refer directly to the data object.
 return this.numbers.filter((number) => number >= 0);
 },
 getPositiveNumbersSum() {
 return this.getPositiveNumbers().reduce((sum, val) => sum + val);
 }
 }
 });
</script>
```

### Computed properties
* Computed properties nằm giữa thuộc tính của **data object** và **phương thức**
* Khi dùng **computed**, các biến sử dụng bên trong **computed property** thay đổi thì kết quả return cũng sẽ thay đổi tương ứng

#### Khác biệt của **computed properties** và **methods**
* Computed properties có tính **lưu trữ**: khi method được gọi, nó sẽ chạy method đó mỗi lần được gọi. Đối với **computed properties** khi được gọi nhiều lần, nó sẽ chỉ chạy một lần và những lần sau **cached value** sẽ được sử dụng. **Computed properties** chỉ chạy lại khi mà các biến mà nó phụ thuộc bị thay đổi.

* Ngoài việc get data từ **computed properties** chúng ta còn có thể **set data** trong **computed properties**
```
<div id="app">
 <p>Sum of numbers: {{ numberTotal }}</p>
</div>
<script>
 new Vue({
 el: '#app',
 data: {
 numbers: [5, 8, 3]
 },
 computed: {
 numberTotal: {
 get() {
 return numbers.reduce((sum, val) => sum + val);
 },
 set(newValue) {
 const oldValue = this.numberTotal;
 const difference = newValue - oldValue;
 this.numbers.push(difference).
 }
 }
 }
 });
</script>
```
### Dùng **Data object**, **methods** hay **computed properties** ?
* Data object là nơi chứa dữ liệu các dạng như **string**, **arrays**, và **objects**
* Methods là nơi chứa các **hàm** và gọi các hàm này từ **templates tag** 
* Computed properties là nơi chứa các hàm và gọi chúng như là các properties trong **data object**. 
* Nhưng nên sử dụng cái gì và khi nào?
* Mỗi loại đều có sự hữu dụng riêng. Nhưng mỗi loại sẽ tốt hơn với từng loại nhiệm vụ riêng của nó.
* Ví dụ, khi muốn truyền vào các **argument**, hãy sử dụng **method** mà không nên là **data** hay **computed property**
* **data object** tốt nhất cho **data ảo**: Khi bạn muốn đặt data để sử dụng trong **template** hoặc trong **method**, **computed properties**, và bạn muốn đặt nó ở đây. Bận có thể cập nhật nó vào 1 lúc khác
* Phương thức sẽ tốt nhất khi bạn muốn thêm một hàm vào trong **templates**
* **Computed properties** tốt nhất cho việc xử lý các biểu thức phức tạp mà bạn không thê để trong **template** bởi vì chúng rất dài và bạn cần sử dụng nó lại nhiều lần. Đây chính là một phiên bản mạnh mẽ hơn của **data object**

### Watchers
* Cho phép chúng ta xem sự thay đổi của 1 thuộc tính trong `data object` hoặc `computed property` 
* Sử dụng `computed property` kèm với `setter` sẽ tốt hơn là sử dụng `data object` và phải lắng nghe sự thay đổi của các properties trong đó
```
<script>
 new Vue({
 el: '#app',
 data: {
 count: 0
 },
 watchers: {
 count() {
 // this.count has been changed!
 }
 }
 });
</script>
```
### Watching properties of Objects in the Data Object
```
new Vue({
 data: {
 formData: {
 username: ''
 }
 },
 watch: {
 'formData.username'() {
 // this.formData.username has changed
 }
 }
});
```
### Getting the ola value
```
watch: {
 inputValue(val, oldVal) {
 console.log(val, oldVal);
 }
}
```
### Deep watching
```
watch: {
 formData: {
 handler() {
 console.log(val, oldVal);
 },
 deep: true
 }
}
```
### Filters
* Dễ dàng format những gì hiển thị ra DOM
```
<div id="app">
 <p>Product one cost: {{ productOneCost | formatCost }}</p>
 <p>Product two cost: {{ productTwoCost | formatCost }}</p>
 <p>Product three cost: {{ productThreeCost | formatCost }}</p>
</div>
<script>
 new Vue({
 el: '#app',
 data: {
 productOneCost: 998,
 productTwoCost: 2399,
 productThreeCost: 5300
 },
 filters: {
 formatCost(value) {
 return '$' + (value / 100).toFixed(2);
 }
 }
 });
</script>
```
* Giúp ít lặp lại code, dễ đọc và dễ bảo trì
* Có thể sử dụng nhiều filter cùng một lúc bằng cách viết 
` {{ productOneCost | round | formatCost }} `. Thứ tự filter từ trái qua phải. Filter bằng hàm `round` sau đó return về kết quả rồi filter bằng hàm `formatCost`
* Filters có thể nhận vào các `tham số`
```
<div id="app">
 <p>Product one cost: {{ productOneCost | formatCost('$') }}</p>
</div>
<script>
 new Vue({
 el: '#app',
 data: {
 productOneCost: 998,
 },
 filters: {
 formatCost(value, symbol) {
 return symbol + (value / 100).toFixed(2);
 }
 }
 });
</script>
```
* Khai báo filters để dùng ở toàn app
```
Vue.filter('formatCost', function (value, symbol) {
 return symbol + (val / 100).toFixed(2);
});
```
* **Không** dùng **filters** thay thế `methods` hay `data object` vì `filters` chỉ nhận input và trả về `output` tương ứng mà không phụ thuộc vào các biến bên ngoài. Có thể truyền vào các argument.
* Vue2 chỉ support dùng `filters` trong `{{}}` và `v-bind`

### Accessing Elements Directly Using ref
* Dùng `ref` để tương tác `DOM` mà không cần dùng tới các thư viện khác
* Để dùng `ref` chỉ cần set thuộc tính `ref` cho thẻ HTML với value là tên dùng để truy cập phần tử đó
```
<canvas ref="myCanvas"></canvas>
```
* Sau đó để truy cập chỉ cần dùng `this.$refs.myCanvas`
* Nó đặt biệt hữu dụng trong 1 component
* `this.refs` ***chỉ chứa*** những `ref` của component hiện tại

### Inputs and Events
* Sử dụng `v-on` directive để binding `event` vào thẻ HTML
```
<div id="app">
 <button v-on:click="increase">Click to increase counter</button>
 <p>You've clicked the button {{ counter }}</p> times.
</div>
<script>
 new Vue({
 el: '#app',
 data: {
 counter: 0
 },
 methods: {
 increase(e) {
 this.counter++;
 }
 }
 });
</script>
```
* Sự khác biệt dễ thấy giữa việc để code `inline` event và sử dụng `method` trong event: khi dùng method, `object event` sẽ được truyền vào là tham số `đầu tiên` 

### v-on Shortcut
* thay vì viết `v-on:click`, có thể viết `@click`
```
<button @click="increase">Click to increase counter</button>
```

### Evemt modifiers
* Để chặn những hành động mặc định, ví dụ như dừng việc `page navigate` từ việc `click` vào link, ta có thể dùng `.prevent` modifier
```
<a @click.prevent="handleClick">...</a>
```
* Chặn sự kiện kích hoạt trên component cha, dùng `.stop` modifier
```
<button @click.stop="handleClick">...</button>
```
* Để sự kiện chỉ thực hiện đúng 1 lần dùng `.once`
```
<button @click.once="handleFirstClick">...</button>
```
* Để event được thực hiện trên chính element được trigger trước khi được gửi lên DOM dùng `.capture`
```
<div @click.capture="handleCapturedClick">...</div>
```
* Để trigger event trên chính element đó mà không phải con của nó dùng `.self`
```
<div @click.self="handleSelfClick">...</div>
```
* Có thể dùng các modified built in như `@keyup.enter, @keyup.shift-left ... `
* `.exact` sẽ trigger sự kiện chỉ khi các phím đặc biệt được nhấn
```
<input @keydown.enter.exact="handleEnter">
```

### Life-cycle hooks
* `beforeCreate` được dùng trước khi Vue instance được khởi tạo
* `created` được dùng sau khi Vue instance đã khởi tạo nhưng được thêm vào DOM
* `beforeMount` được dùng sau khi các element sẵn sàng để được thêm vào DOM nhưng vẫn chưa được thêm vào
* `mounted` được dùng sau khi các phần tử đã được tạo (nhưng không nhất thiết được thêm vào DOM: dùng nextTick trong TH này)
* `beforeUpdate` được dùng khi có sự thay đổi tới kết quả của DOM
* `updated` được dùng sau khi những thay đổi đã được ghi vào DOM
* `beforeDestroy` được dùng khi component sắp bị hủy và xóa khỏi DOM
* `destroyed` được dùng sau khi component đã được hủy

### Custom directives
* Tạo ra 1 directive
```
Vue.directive('blink', {
 bind(el) {
 let isVisible = true;
 setInterval(() => {
 isVisible = !isVisible;
 el.style.visibility = isVisible ? 'visible' : 'hidden';
 }, 1000);
 }
});
```
#### Lưu ý:
* Hook được gọi khi `directive` được ràng buộc vào trong element
* Hook `inserted` được gọi khi element bị ràng buộc được thêm vào `parent node` và đã `mounted`. Nhưng vẫn chưa đảm bảo rằng element đó đã được thêm vào DOM. ùng `this.$nextTick` trong TH này
* Hook `update` được gọi khi cha của các component directive bị ràng buộc được update. Nhưng có thể trước khi con của component được update
* Hook `componentUpdated` tương tự với hook `update`, nhưng được gọi sau khi con của component đã được update
* Hook `unbind` được dùng để hủy bỏ và được gọi khi `directive` bị xóa ràng buộc khỏi element
* Không nhất thiết phải call hết các hook ở trên, hoặc có thể không call

### Hook Arguments
* VD nếu dùng `v-my-directive:example.one.two="someExpression", thì sẽ như sau
    * Name của thuộc tính là tên của directive bỏ đi `v-`, vd: `my-directive`
    * Value sẽ là `someExpression`
    * Thuộc tính `oldValue` là value được truyền vào lần trước đó. Nó có ở `update` và `componentUpdated`. Nghĩa là nếu ta có các giá trị khác nhau của `someExpression`, thì hook `update` sẽ được gọi giá trị mới as `value` và giá trị cũ as `oldValue`
    * `expression` là giá trị của trạng thái ở dạng `string` trước khi được tính toán
    * `arg` property là đối số được truyền vào directive, trong VD này là `example`
    * `modifiers` là object chưa các modifiers truyền vào directive. Trong TH này nó là `{one: true, two: true}`

* Để tìm hiểu các dùng đối số, xem ví dụ:
```
Vue.directive('blink', {
 bind(el, binding) {
 let isVisible = true;
 setInterval(() => {
 isVisible = !isVisible;
 el.style.visibility = isVisible ? 'visible' : 'hidden';
 }, binding.value || 1000);
 }
});
```
* Trong ví dụ này, tác giả đã dùng `binding.value` để nhận giá trị truyền vào tư `argument`
    * Chúng ta cần phải hủy `setInterval` trong `update` hook trước khi tạo 1 cái mới

### Transitions and Animations
* Vue cung cấp nhiều function mà chúng ta có thể dùng để thiết lập animations và transitions
* Nó sẽ giúp chúng ta với CSS transitions và JS animation khi những elements xuất hiện hoặc biến mất khỏi trang hoặc được thiết lập, để chuyển đổi giữa các components hoặc thậm chí là tự `animating` data.
* Chúng ta sẽ bắt đầu với component `<transition>`

### CSS Transitions
* CSS transition sử dụng tốt cho các `animation` đơn giản
* Nó phù hợp khi ta `transition` một hoặc nhiều thuộc tính CSS từ value này sang value khác
* Ví dụ như chúng ta có thể `transition` màu từ ` blue` sang `red`, `opacity` từ 1 sang 0.
* Vue cung cấp `<transition>` component để thêm các class vào element bên trong `<transition>` với `v-if`, khi đó chúng ta có thể áp dụng CSS transition khi element được thêm vào hoặc xóa đi

# dừng ở trang 50

