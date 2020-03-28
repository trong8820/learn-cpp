# C++ Style

Nguồn tham khảo:
- https://developer.lsst.io/cpp/style.html
- https://github.com/mortennobel/cpp-cheatsheet

## 1. The #define Guard

Sử dụng `#define` thay vì `#pragma once`. Tuy rằng đa số các trình biên dịch đã hỗ trợ pragma cũng như cách dùng đơn giản và giảm thời gian biên dịch. Tuy nhiên hiện nay define vẫn được khuyến cáo. Sử dụng define linh hoạt hơn trao quyền quản lý cho lập trình viên điều này sẽ có lợi trong một số trường hợp phức tạp, gặp hệ thống tệp lạ trong khi pragma thì cố định. 

```cpp
#ifndef <PROJECT>_<PATH>_<FILE>_H_
#define <PROJECT>_<PATH>_<FILE>_H_
...
#endif // <PROJECT>_<PATH>_<FILE>_H_
```

## 2. File name

- Một số hệ thống nhận biết tên file hoa thường khác nhau vì vậy tên file nên viết thường. VD: `tenfile.cpp`. 
- Một số nguồn có khuyến cáo dùng phần mở rộng `*.cc` tuy nhiên đa số các dự án và điển hình như QT, Unreal Engine 4, ... sử dụng `*.cpp`

```
tenduan/tenfile.h
tenduan/tenfile.cpp
tenduan/tenfile_test.cpp
```

## 3. Naming Rules

Ví dụ:

```cpp
namespace ten                           // (1)
{
    const int HANG_SO = 0;
    class TenLop                        // (2)
    {
    private:
        const int kHangSo;
        int <prefix>tenBien;            // (3)
    public:
        TenLop();                       
        TenLop(const TenLop& tenLop);
        TenLop& operator=(const TenLop& tenLop)
        ~TenLop();

        int getTenBien() const;         // (4)
        void setTenBien(int tenBien);   // (5)
    };
}

// (1) Viết thường, ngắn gọn
// (2) PascalCase
// (3) <prefix>camelCase
// (4) Get camelCase
// (5) Set camelCase
```

Việc sử dụng prefix là không cần thiết vì sẽ khiến code dài hơn cũng như các ide đã hỗ trợ để xác định kiểu dữ liệu khá rõ ràng rồi. Tuy nhiên không phải bất cứ môi trường nào cũng có ide hỗ trợ cũng như để rõ ràng tường minh hơn có thể sử dụng một số gợi ý sau:
|Prefix |Description                        |
|-------|:--------------------------------- |
|g_     |global                             |
|m_     |data member of class               |
|p      |point                              |
|a      |Array                              |
|is     |bool                               |
|c      |char                               |
|i      |interger                           |
|u      |unsigned interger                  |
|l      |long                               |
|s      |string                             |
|n      |number of objects                  |
|f      |flags                              |
|fn     |function                           |
|x,y    |x,y coordinates                    |

Đối với hàm thì một số tên phổ biến bao gồm: `get/set`, `add/remove`, `create/destroy`, `start/stop`, `insert/delete`, `increment/decrement`, `old/new`, `begin/end`, `first/last`, `up/down`, `min/max`, `next/previous`, `open/close`, `show/hide`, `suspend/resume`, etc...

Ngoài ra một số tên phổ biến như: `is`, `has`, `can`, `should`, `command`, `copy`, `compute`, `initialize`, `exists`, `read`, `make`, `build`, `find`, `expects`, etc...

## 4. Khai báo

### Preprocessor
```cpp
#include  <stdio.h>     // Insert standard header file
#include "myfile.h"     // Insert file in current directory
#define X some text     // Replace X with some text
#define F(a,b) a+b      // Replace F(1,2) with 1+2
#define X \
 some text              // Multiline definition
#undef X                // Remove definition
#if defined(X)          // Conditional compilation (#ifdef X)
#if !defined(X)         // Conditional compilation (#ifndef X)
#else
#endif                  // Required after #if, #ifdef
```

### Literals
```cpp
255, 0377, 0xff         // decimal, octal, hex
1l, 0xffl               // long
1.2f                    // float
1.2, 1.2e3              // double
'a', "a"                // char, string
```