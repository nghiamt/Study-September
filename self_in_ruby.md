Trong bài viết trước, chúng ta đã tìm hiểu sơ bộ về Metaprogramming trong Ruby và cách viết code tạo ra code, một trong những khía cạnh nổi bật của Ruby.
Trong bài viết này, mình sẽ giới thiệu về **self** trong Ruby.

Tại mọi thời điểm khi chúng ta chạy chương trình, tồn tại 1 và chỉ 1 **self**. Nó được gọi là "the current/default object".
Dưới đây chúng ta sẽ tìm hiểu sâu hơn về **self**.

###Top level context
Context nằm bên ngoài mọi class hoặc module được gọi là top level context.
VD1: Tạo 1 biến local x ở top level context
```Ruby 
x = 1
```

VD2: Định nghĩa method ở top level context. (Cần chú ý **self** không phải là 1 Object).
```Ruby
def method
end
```

Method ở top level context luôn luôn là private.
Khi bạn gọi **self** ở top level context:
```Ruby
puts self
```
Kết quả in ra màn hình sẽ là **main** - Đây được gọi là **default self object**, sử dụng để tham chiếu đến chính nó.
Class của **main** là Object.


###Self nằm trong các class và module
Trong các class hoặc module, **self** chính là các class object hoặc module object đó.
Ta có ví dụ như sau:

```Ruby
class S
  puts 'Class S'
  puts self
  module M
    puts 'Module S::M'
    puts self
  end
  puts 'in outer level of class S'
  puts self
end
```

Kết quả in ra màn hình sẽ là:
```
Class S
S
Module S::M
S::M
In outer level of class S
S
```

###Self trong các instance method

Khi thực thi một method, **self** là những object được sinh ra hoặc có thể access từ method đó.
Ví dụ:
```Ruby
class S
  def m
    puts 'Class S method m:'
    puts self
  end
end
s = S.new
s.m
```

Kết quả in ra như sau:
```
Class S method m:
#<S:0x2835908>  
```

```#<S:0x2835908>``` là một instance của class S.

###Self trong các singleton-method và class-method

Singleton methods là những method được gắn liền với những object cụ thể. Khi thực thi một singleton method , **self** chính là object cha của method đó.

Ví dụ:
```Ruby
obj = Object.new
def obj.show
  print 'I am an object: '
  puts "here's self inside a singleton method of mine:"
  puts self
end
obj.show
print 'And inspecting obj from outside, ' 
puts "to be sure it's the same object:"
puts obj
```

Kết quả hiển thị trên màn hình như sau:
```
I am an object: here's self inside a singleton method of mine: 
#<Object:0x2835688> 
And inspecting obj from outside, to be sure it's the same object:
#<Object:0x2835688>  
```

Class methods được định nghĩa như một singleton methods cho class objects. 

Ví dụ:
```Ruby
class S
  def S.x
    puts "Class method of class S"
    puts self
  end
end
S.x
```

Kết quả sẽ là:
```
Class method of class S
S
```
**self** nằm trong singleton method (trong trường hợp này là một class method) chính là object của method đó.
