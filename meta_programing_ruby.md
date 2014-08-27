Một trong những tính năng ấn tượng của Ruby là nó cho phép metaprogramming.

Trước hết, ta sẽ tìm hiểu xem metaprogramming là gì.

Theo định nghĩa trên wiki: Metaprogramming là việc tiến hành một trong hai (hay cả hai) thao tác sau:

- Viết một chương trình máy tính mà chương trình này lại điều chỉnh hay soạn thảo một chương trình khác (hay điều chỉnh chính nó) như là dữ liệu của metaprogramming
- Viết một chương trình máy tính mà một phần của công việc này chỉ hoàn tất trong thời gian dịch mã.
Trong đa số các trường hợp thì, sử dụng metaprogramming có thể giúp ta hoàn tất nhiều việc hơn trong cùng một thời gian so với làm điều đó bằng tay.

Tiếp theo ta sẽ tìm hiểu sơ bộ về metaprogramming trong Ruby.
Như các bạn đã biết, mọi thứ trong Ruby đều là Object. Mọi Object trong Ruby đều có những method hoặc biến instance mà có thể thêm, sửa, xóa ngay trong khi đang chạy chương trình.
Ví dụ:
######Tạo một instance của class Object
```Ruby
my_object = Object.new
```

######Định nghĩa một method của my_object để set giá trị cho biến @my_instance_variable
```Ruby
def my_object.set_my_variable=(var)
  @my_instance_variable = var
end
```

######Định nghĩa một method của my_object trả về giá trị biến @my_instance_variable
```Ruby
def my_object.get_my_variable
  @my_instance_variable
end
```

Khi đó ta có thể gọi các method như sau:
```Ruby
my_object.set_my_variable = "Hello"
my_object.get_my_variable # => Hello
```

Ở ví dụ trên, ta tạo một instance của class Object và định nghĩa 2 method dùng để đọc và ghi trong nó. 2 method này chỉ có thể được sử dụng trong object my_object và bị vô hiệu hóa ở các instance khác của class Object.

Ta có thể chứng minh điều này qua ví dụ sau:
```Ruby
my_object = Object.new
my_other_object = Object.new

def my_object.set_my_variable=(var)
  @my_instance_variable = var
end

def my_object.get_my_variable
  @my_instance_variable
end

my_object.set_my_variable = "Hello"
my_object.get_my_variable # => Hello

my_other_object.get_my_variable = "Hello" # => NoMethodError
```

Khi ta gọi get_my_variable() từ object my_other_object, lỗi NoMethodError sẽ xảy ngay.


Tiếp theo, chúng ta sẽ tìm hiểu về một khía cạnh của metaprogramming ruby: Viết code tạo ra code.

Một trong những triết lý quan trọng của Ruby đó là DRY (Don’t Repeat Yourself). Việc lặp lại một đoạn code quá nhiều lần không chỉ khiến chúng ta tiêu tốn thời gian vô ích mà còn ảnh hưởng đến hiệu suất cũng như khả năng bảo trì, update trong tương lai. Trong nhiều trường hợp, chúng ta có thể giải quyết được vẫn đề lặp code này bằng cách viết ra đoạn code để tạo ra code mà mình mong muốn.

Ví dụ như sau:

Giả sử chúng ta làm một app cho nhà máy sản suất oto. Trong app, ta định nghĩa một class CarModel:

```Ruby
class CarModel
  def engine_info=(info)
    @engine_info = info
  end

  def engine_info
    @engine_info
  end

  def engine_price=(price)
    @engine_price = price
  end

  def engine_price
    @engine_price
  end

  def wheel_info=(info)
    @wheel_info = info
  end

  def wheel_info
    @wheel_info
  end

  def wheel_price=(price)
    @wheel_price = price
  end

  def wheel_price
    @wheel_price
  end

  def airbag_info=(info)
    @airbag_info = info
  end

  def airbag_info
    @airbag_info
  end

  def airbag_price=(price)
    @airbag_price = price
  end

  def airbag_price
    @airbag_price
  end

  def alarm_info=(info)
    @alarm_info = info
  end

  def alarm_info
    @alarm_info
  end

  def alarm_price=(price)
    @alarm_price = price
  end

  def alarm_price
    @alarm_price
  end

  def stereo_info=(info)
    @stereo_info = info
  end

  def stereo_info
    @stereo_info
  end

  def stereo_price=(price)
    @stereo_price = price
  end

  def stereo_price
    @stereo_price
  end
end
```

Mỗi car model có các thuộc tính như “stereo”, “alarm”, .... Ta có các method dùng để get và set các thuộc tính này của car. Mỗi thuộc tính cần có info vaf price. Vì vậy mỗi khi thêm một thuộc tính nào đó của car, ta cần phải định nghĩa 2 method mới là: feature_info và feature_price.

Vì việc định nghĩa các method này tương tự nhau, nên ta có thể viết đơn giản như sau:
```Ruby
class CarModel
  FEATURES = ["engine", "wheel", "airbag", "alarm", "stereo"]

  FEATURES.each do |feature|
    define_method("#{feature}_info=") do |info|
      instance_variable_set("@#{feature}_info", info)
    end

    define_method("#{feature}_info") do
      instance_variable_get("@#{feature}_info")
    end

    define_method "feature_price=" do |price|
      instance_variable_set("@#{feature}_price", price)
    end

    define_method("#{feature}_price") do
      instance_variable_get("@#{feature}_price")
    end
  end
end
```

Ở ví dụ trên, ta khai báo một array FEATURES chứa các thuộc tính của car mà ta muốn thêm method. Sau đó, với mỗi thuộc tính, ta sử dụng Module#define_method của Ruby để khai báo các method getter, setter cho các thuộc tính này. 

Ngoài ra, Ruby cũng cung cấp cho ta một cách làm khác đó là sử dụng attr_accessor như sau:
```Ruby
class CarModel
  attr_accessor :engine_info, :engine_price, :wheel_info, :wheel_price, :airbag_info, :airbag_price, :alarm_info, :alarm_price, :stereo_info, :stereo_price
end
```
Tuy nhiên, ở ví dụ trên, với mỗi thuộc tính, ta lại cần khai báo feature_info và feature_price cho chúng.
Ta có một cách viết khác:
```Ruby
class CarModel
  def self.features(*args)
    args.each do |feature|
      attr_accessor "#{feature}_price", "#{feature}_info"
    end
  end

  features :engine, :wheel, :airbag, :alarm, :stereo
end
```
Cách làm trên trông có vẻ phức tạp hơn ví dụ trước nhưng lại có lợi cho ta khi muốn thay đổi hoặc thêm các thuộc tính sau này. Khi đó ta chỉ cần truyền tham số vào CarModel#feature là OK.

Trên đây là mình chỉ điểm qua một số kiến thức cơ bản về meta programming ruby. Mình sẽ tìm hiểu sâu hơn và cung cấp cho các bạn những bài viết khác về chủ đề này trong thời gian tiếp theo nhé.

