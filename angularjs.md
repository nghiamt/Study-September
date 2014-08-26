Như các bạn đã biết, AngularJS là một front-end MVC framework rất mạnh và tiện dụng. 
Hôm nay mình sẽ giới thiệu một chút về Directives, Factories, Services và cách sử dụng chúng trong AngularJS.

##DIRECTIVE
Đầu tiên là về Directives.
Những thứ như thuộc tính, class, tên,... của 1 DOM element gọi chung là directive. AngularJS sẽ dựa vào chúng để attach các chỉ thị hoặc các sự kiện tới DOM element đó.

Một số directive có sẵn trong Angularjs như: ng-bind, ng-model, ng-click, ng-controller,...

Việc sử dụng directive sẽ giảm thiểu được số lượng thẻ HTML, làm cho code HTML nhìn sẽ gọn gàng và sáng sủa hơn.

AngularJS cung cấp cho chúng ta 3 loại directive đó là :
- Directive dạng element (là 1 thẻ HTML) viết tắt là E.
```Javascript
<my-directive></my-directive>
```

- Directive dạng attribute (thuộc tính của 1 thẻ HTML) viết tắt là A, đây là dạng mặc định.
```Javascript
<div my-directive></div>
```

- Directive dạng class (CSS class) viết tắt là C.
```Javascript
<div class="my-directive"></div>
```

####Cách tạo một directive:
Ta có thể tạo một directive như sau.
Trước tiên ta định nghĩa một module. Module này sẽ được sử dụng trong toàn bộ bài viết.
```Javascript
var app = angular.module( "myApp", [] );
```

Sau đó ta định nghĩa một directive như sau:
```Javascript
app.directive('myDirective', function() {
  return {
    restrict : 'C',
    template: 'Name: {{customer.name}} Address: {{customer.address}}'
  };
});
```

Khai báo controller:
```Javascript
app.controller('myCtrl', function($scope) {
  $scope.customer = {
     name: 'Nghiamt',
     address: 'Hanoi'
  };
});
```
        
Ta có thể sử dụng derective như một element, 1 class hoặc 1 attribute (tùy thuộc vào restrict)
```html
<!DOCTYPE html>
<html ng-app="myApp">
<head>
  <title>Angular</title>   
  <script type="text/javascript" src="js/angular.min.js"></script>
</head>
<body>
  <div ng-controller="myCtrl">
    <my-directive></my-directive>
    <div my-directive></div>
    <div class="my-directive"></div>
  </div>
</body>
</html>
```

##SERVICE
Tiếp theo ta sẽ tìm hiểu về services trong AngularJS.
AngularJS service là một object hoặc một function phụ trách một tác vụ nào đó. Việc viết service chỉ là việc gom các đoạn xử lý logic vào object hoặc function để dễ quản lý hơn. Cũng giống như mô hình MVC tách phần model, view và controller riêng biệt nhau.

Trong AngularJS service sẽ chứa tất cả các phần xử lý, Controller nhận yêu cầu và gọi các service cần thiết để xử lý, Model thao tác với dữ liệu, View hiển thị dữ liệu, Route điều hướng request tới controller.

Một số service trong AngularJS như: $http, $scope, $window, $routeProvider ...

####Cách tạo 1 service:

Ta sử dụng lại module đã định nghĩa ở trên:
```var app = angular.module( "myApp", [] );```

Định nghĩa 1 service:
```Javascript
app.service('ContactService', function () {
  this.contacts = [];
  this.addContact = function (contact) {
    this.contacts.push(contact);
  }

  this.getContacts = function () {
    return this.contacts;
  }
});
```

Để sử dụng service, ta chỉ việc truyền tên service vào function của controller như 1 tham số.
```Javascript
app.controller('contactController', function ($scope, ContactService) {
  $scope.emails = ContactService.getContacts();
  $scope.addContact = function (contact) {
  ContactService.addContact(contact);
  }
});
```

##FACTORY
Factory trong AngularJS cũng tương tự như Service, tuy nhiên có một điểm khác biệt là Service xử lý chung 1 Object, tức là Object chỉ được tạo một lần trong toàn bộ quá trình xử lý (khi hàm khởi tạo được gọi). Còn Object định nghĩa bằng Factory sẽ được tạo các Instance mỗi lần service chứa nó được gọi đến.

Cách định nghĩa Factory như sau:
```Javascript
app.factory('userService', function(){
  var fac = {};
  fac.users = ['Nghia', 'Trong', 'Vu'];
  return fac;
});
```

Để sử dụng factory, ta cũng chỉ việc truyền tên factory vào function của controller như 1 tham số (giống Service).

Trên đây mình chỉ giới thiệu sơ bộ về Directives, Factories, Services trong AngularJS. Thế giới AngularJS rất rộng lớn và còn rất rất nhiều điều thú vị nữa để các bạn khám phá.



