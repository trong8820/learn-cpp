# Note C++

## Con trỏ hằng

Con trỏ hằng có thể thay đổi được đi chỉ vùng nhớ lưu trữ nhưng không thể thay đổi được giá trị của vùng nhớ thông qua con trỏ đó.
```cpp
const int* a = &b;
```

## Hằng con trỏ

Hằng con trỏ không thể thay đổi được đia chỉ vùng nhớ mà nó trỏ đến nhưng có thể thay đổi được giá trị của vùng nhớ thông qua con trỏ đó.
```cpp
int* const const a = &b;
```