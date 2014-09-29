Ở bài viết trước, mình đã đề cập qua về Directives trong AngularJS. Trong bài biết này, mình sẽ giới thiệu sâu hơn về các Directives có sẵn trong AngularJS (built-in directives). Nắm bắt và sử dụng một cách hợp lý những directives này sẽ giúp ta tiết kiệm được rất nhiều thời gian code, đồng thời cũng làm code của ta thêm rõ ràng, sạch sẽ hơn.

Trước tiên là những directives thuộc tính cơ bản.
##Attribute directives cơ bản:
- ng-href
- ng-src
- ng-disable
- ng-readonly
- ng-selected
- ng-class

Những directives này rất gần với các thuộc tính trong các thẻ HTML cơ bản, nên chúng rất dễ nhớ (thêm ng vào trước các thuộc tính của thẻ HTML là xong).

####ng-href
Khi ta muốn tạo một phần tử HTML link với url động, ta sử dụng ng-href thay cho href.

Ta có ví dụ như sau:
```HTML
<!-- Luôn sử dụng ng-href nếu thuộc tính href chứa {{}} -->
<!-- Link được tự động active sau 2s -->
<a ng-href="{{myHref}}">Google</a>
<!-- Link không được load trước khi người dùng click -->
<a href="{{myHref}}">404</a>
```

```Javascript
angular.module('myApp', [])
.run(function($rootScope, $timeout) {
  $timeout(function() {
    $rootScope.myHref = 'http://google.com';
  }, 2000);
});
```

####ng-src
Cũng giống như ng-href, ta cần thiết phải sử dụng ng-src, thay cho src khi mà url trong thẻ ```<img>``` chứa {{}}
```HTML
<h1>Wrong Way</h1>
<img src="{{imgSrc}}" />
<h1>Right way</h1>
<img ng-src="{{imgSrc}}" />
```

```Javascript
angular.module('myApp', [])
.run(function($rootScope, $timeout) {
  $timeout(function() {
    $rootScope.imgSrc = 'https://www.google.com/images/srpr/logo11w.png';
  }, 2000);
});
```

####ng-disabled
Ta sử dụng ng-disabled để bind dữ liệu của thuộc tính disable trong các input field của form
```
- <input> (text, checkbox, radio, number, url, email, submit)
- <textarea>
- <select>
- <button>
```

Khi ta viết một field input HTML, giá trị của thuộc tính disabled sẽ quyết định có input field này có disable hay không.
Ở ví dụ sau, ta sẽ disble button cho đến khi người dùng bắt đầu nhập vào text field
```HTML
<input type="text" ng-model="someProperty" placeholder="Type to Enable">
<button ng-model="button" ng-disabled="!someProperty">A Button</button>
```

Ở ví dụ tiếp theo, ta sẽ disable text field trong 5 giây.
```HTML
<textarea ng-disabled="isDisabled">Wait 5 seconds</textarea>
```

```Javascript
angular.module('myApp', [])
.run(function($rootScope, $timeout) {
  $rootScope.isDisabled = true;
  $timeout(function() {
    $rootScope.isDisabled = false;
  }, 5000);
});
```

####ng-readonly
Cũng giống như những thuộc tính boolean khác, giá trị của readonly quyết định trạng thái của phần tử input (cho edit hay không).
Ta sử dụng ng-readonly thay cho readonly khi muốn cập nhật giá trị này một cách động.

```HTML
<input type="text" ng-model="someProperty"><br/>
<input type="text" ng-readonly="someProperty" value="Some text here"/>
```

####ng-selected
Ta sử dụng ng-selected để set trạng thái thuộc tính selected của tag ```<option>```:

```HTML
<label>Select Two Fish:</label>
<input type="checkbox" ng-model="isTwoFish"><br/>
<select>
  <option>One Fish</option>
  <option ng-selected="isTwoFish">Two Fish</option>
</select>
```

####ng-class
Ta sử dụng ng-class khi muốn set động giá trị class của một phần tử HTML. Class lặp lại sẽ không được thêm vào.
Khi biểu thứ trong ng-class thay đổi, những class cũ sẽ bị xóa đi, thay bởi những class mới.
Ta có ví dụ sau: class .red sẽ được thêm vào khi số random lớn hơn 5
```HTML
<div ng-controller="LotteryController">
<div ng-class="{red: x > 5}" ng-if="x > 5">
  You won!
</div>
<button ng-click="x = generateNumber()" ng-init="x = 0">
  Draw Number
</button>
<p>Number is: {{ x }}</p>
</div>
```

```CSS
.red {
  background-color: red;
}
```

```Javascript
angular.module('myApp', [])
.controller('LotteryController', function($scope) {
  $scope.generateNumber = function() {
    return Math.floor((Math.random()*10)+1);
  }
})
```

Trên đây là một số  directives thuộc tính cơ bản được built sẵn trong AngularJS. Cần nắm chắc những directives này để có thể sử dụng một cách hợp lý khi viết AngularJS code.












