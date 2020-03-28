# Modern C++

Nguồn tham khảo:
- https://github.com/changkun/modern-cpp-tutorial
- https://github.com/AnthonyCalandra/modern-cpp-features

## 1. Constants
- Sử dụng `nullptr` thay vì `NULL` hay `0`. VD: khi gọi `ham(int)` và `ham(char*)` sẽ dễ bị hiểu nhầm nếu không có ép kiểu rõ ràng.
- Sử dụng linh hoạt `const` và `constexpr`. Việc sử dụng `constexpr` chặt chẽ hơn vì từ khóa này sẽ yêu cầu trình biên dịch tính toán toán tại thời điểm biên dịch.

## 2. Khai bái biến trong if-switch
Từ phiên bản C++17, đã có thể khai báo biến bên trong các lệnh `if` hoặc `switch`
```cpp
if (int a = 1; a == 1) {}
```

## 3. Initalizer list
Sử dụng `std::initializer_list` để truyền tham số như danh sách
```cpp
void ham(std::initalizer_list<int> list);
// Sử dụng: ham({1, 2, 3});
```

## 4. Initialization
Sử dụng `{}` để khởi tạo giá trị thay cho `=` hoặc `()`. Việc dùng `{}` sẽ kiểm soát chặt về kiểu dữ liệu hơn là dùng phép gán `=`.
```cpp
int a{1};   // good

double b{7.9};
int c = b;  // c = 7 và điều này là không mong muốn
int c{b};   // error
```

## 5. Structured binding
Structured binding cho phép hàm trả về nhiều gia trị.
```cpp
std::tuple<int, double, std::string> f() {
    return std::make_tuple(1, 2.3, "456");
}
// Sử dụng: auto [x, y, z] = f();
```

## 6. auto
Dùng `auto` để trình biên dịch tự động xác định kiểu dữ liệu. Giảm số lượng mã. Tuy nhiên một số trường hợp cân nhắc việc khai báo dữ liệu cụ thể nhằm tránh nhầm lẫn và tăng khả năng đọc
```cpp
auto a = 1; // biến a sẽ có kiểu dữ liệu int
```

## 7. decltype
Từ khóa `decltype` dùng để xác định kiểu dữ liệu trong việc khai báo.
```cpp
auto x = 1;
auto y = 2;
decltype(x + y) z;
```

## 8. decltype(auto)
Dùng để lấy ra kiểu dữ liệu trả về của hàm hoặc chuyển tiếp.
```cpp
std::string lookup1();
std::string lookup2();
decltype(auto) look_up_1() {
    return lookup1();
}
decltype(auto) look_up_2() {
    return lookup2();
}
```

## 9. if constexpr
Dùng `if constexpr` nhằm xác định ngay việc khối lệnh nào sẽ được gọi ngay trong quá trình biên dịch
```cpp
if constexpr (a) {
    // block1    
} else {
    // block2
}
// Khi đó a được xác định trước trong quá trình biên dịch suy ra trình biên sẽ bỏ câu lệnh if và xác định luôn block nào sẽ được gọi.
```

## 10. Range-based for loop
```cpp
for (auto e : vec) {}
```

## 11. using - typedef
Việc sử dụng `using` không tạo ra kiểu dữ liệu mới vì vậy nó sẽ không có type-id. Ngoài ra dùng để làm bí danh cho templates trong khi typedef không dùng được.
```cpp
template<typename T>
using MyTemplate = Temp<T>;
```

## 12. Default template parameters
```cpp
template<typename T = int, typename U = int>
auto add(T x, U y);
```

## 13. Fold expression
```cpp
template<typename ... T>
auto sum(T ... t) {
    return (t + ...);
}
// Sử dụng: sum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
```

## 14. Delegate constructor
Có thể sử dụng hàm tạo khác để hộ trợ một hàm tạo trong cùng một lớp
```cpp
class Base {
public:
    Base();
    Base(int a) : Base () {}
};
```

## 15. Inheritance constructor
Sử dụng `using` để kế thừa hàm tạo từ lớp cha
```cpp
class Base {
public:
    Base();
    Base(int a);
};
class SubClass : public Base {
    using Base::Base;
};
```

## 16. Explicit delete default function
Sử dụng `delete` và `default` để chỉ định cho trình biên dịnh chính xác việc loại bỏ hay sử dụng mặc định các hàm do trình biên dịch tạo ra
```cpp
class Lop {
public:
    Lop() = defalut;
    Lop& operator=(const Lop&) = delete;
}
```

## 17. Enum
Sử dụng `enum class` thay vì `enum` thông thường đồng thời xác định kiểu dữ liệu cho `enum` nằm tạo tính tường minh rõ ràng
```cpp
enum class MyEnum : unsigned int {}
```

## 18. Lambda
Lambda hay còn gọi là hàm nặc danh, là hàm không tên.
```cpp
[]      // Khai báo lambda
()      // Đối số
{}      // Thân hàm
();     // Chạy!
```
Capture, lamda là function object và truy cập được biến số ngoài scope của nó. Truy cập theo `&` reference và `=` copy.
```cpp
[&] {};
[=] {};
[a, &b] {};
```
Khi dùng `=` thì không thể thay đổi giá trị biến số bên ngoài scope. Để làm việc đó cần dùng mutable. Và khi ra ngoài giá trị lại trở lại như trước.
```cpp
int a = 5;
[=]() mutable {a=a+1};
```
Generic lambda, lambda không phải là hàm thông thường nên không thể sử dụng cùng với template để xác định rõ kiểu tham số. Vì vậy hàm lambda kết hợp với từ khóa auto
```cpp
auto generic = [](auto x, auto y) {
    return x+y;
};
// generic(1, 2);
// generic(1.1, 2.2);
```
Sử dụng `std::bind` và `std::placeholder` để liên kết các tham số gọi hàm trước
```cpp
int foo(int a, int b, int c);
auto bindFoo = std::bind(foo, std::placeholders::_1, 1,2);
// bindFoo(1);
```

## 19. Value category
Mỗi một biểu thức sẽ có một trong ba thành phần chính: `lvalue`, `xvalue`, `prvalue`
```
       expression
        /      \
    glvalue   rvalue
    /    \     /   \
lvalue   xvalue    prvalue
```
Ngay cả tài liệu về vấn đề này cũng rất chung chung khó hiểu. Tuy nhiên có thể hiểu đơn giản như sau:
- `lvalues` (`L`ocator values): Đại diện cho một đối tượng, một vị trí trong vùng nhớ
- `prvalue` (`P`ure rvalues): Đại diện cho một giá trị cụ thể
- `xvalues` (e`X`piringval): Đại diện cho một đối tượng có thời gian tồn tại

## 20. std::array
Sử dụng `std::array` thay vì dùng mảng truyền thống. Chúng về bản chất giống nhau tuy nhiên thì `std::array` hiện đại hơn và cung cấp nhiều tiện ích.
```cpp
std::array<int, 4> arr = {1, 2, 3, 4};
int* p1 = &arr[0];
int* p2 = arr.data();
```

## 21. Tuples
`std::get` dùng để lấy giá trị từ tuples. Tuy nhiên `std::get<index>()` thì index phải được xác định trong quá trình biên dịch. Vì vậy cần sử dụng `std::variant<>`

## 22. Smart pointers
Là những con trỏ thông minh tự động gọi `delete` vùng nhớ sau khi không còn tham chiệu đến nó.
```cpp
std::unique_ptr<int> p1 = std::make_unique<int>(1);
std::shared_ptr<int> p2 = std::make_shared<int>(2);std::make_shared<int> p3 = p2;
```
- `unique_ptr` cấm việc chia sẻ đối tượng. Nó tự huỷ sau khi rời khỏi phạm vi scope. Tuy không thể chia sẻ copy nhưng có thể chuyển toàn bộ sang con trỏ khác bằng `std::move`
- `shared_ptr` cho phép việc chia sẻ đối tượng. Dùng `use_cout()` để lấy về số lượng con trỏ đang tham chiếu đến vùng nhớ và `reset()` để giảm số lượng tham chiếu.
- `make_shared` giống như `shared_ptr` tuy nhiên nó yếu hơn và có tác dụng trong việc tránh cyclic references (tham chiếu vòng tròn). a chứa b, b lại chứa a.

## 23. Regular expression
Sử dụng `std::regex` để làm việc với biểu thức chính quy.

## 24. Raw string literal
```cpp
std::string str = R"(D:\Data)";
char x = u8'x'; // UTF-8 character literals
//u8R               
//u U              UTF-16   UTF-32
```

## 25. Custom literal
```cpp
std::string operator"" _wow(int i) {
    return std::to_string(i) + "wow";
}
// Sử dụng: auto num = 1_wow;
```