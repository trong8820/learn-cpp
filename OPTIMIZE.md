# Optimize

## 1. Tính toán đơn giản

- Các biểu thức nên được rút ngắn để giảm số bước tính toán của CPU. `a*b + a*c + a*d` nên được viết lại là `a*(b+c+d)`
- Trong mốt số trường hợp có thể sử dụng dịch bit để tăng tốc độ tính toán. Một số trình biên dịch có khả năng tối ưu tự động chuyển đổi thành phép dịch bit.

## 2. Tối ưu việc sử dụng biến tạm

Sử dụng biến tạm để chứa tạm thời kết quả để tăng tốt độ thay vì phải tính đi tính lại nhiều lần cũng như giảm số cấp phát.

## 3. Tối ưu các biểu thức điều kiện

- Ưu tiên việc đặt các biểu thức điều kiện cho kết quả sớm VD: `((a || b) && c)` có thể viết thành `(c && (a || b))` vì `(a || b)` cần đến hai phép kiểm tra.
- Tránh các tính toán lặp lại trong các biểu thức điều kiện. Khi đó cần dùng biến tạm (`2.`).

## 4. if-else và switch-case

Hoàn toàn có thể chuyển luồng chương trình từ việc dùng `if-else` sang `switch-case` và ngược lại. Tuy nhiên theo thống kê thì lời khuyên là nếu `switch-case` có dưới 5 case thì nên dùng `if-else` thay vì `switch-case`

## 5. Tỗi ưu vòng for

- Việc dùng vòng lặp quá nhiều cũng là nguyên nhân làm giảm tốc độ chương trình. Vì vậy đối với những vòng lặp nhỏ ta có thể trải code ra mà không cần dùng đến vòng lặp
- Đối với những vòng lặp số lượng lớn cần hạn chế việc cấp phát nội bộ. Tính toán lặp đi lập lại nhiều lần, khi đấy có thể sử dụng biến tạm (`2.`)

## 6. So sánh

- Trong quá trình tính toán việc so sánh với 0 luôn nhanh hơn so với so sánh với các số nguyên khác, tuy nhiên tốc độ không đáng kể
- Các phép so sánh `<` hay `<=` có tốc độ là như nhau. Còn nếu có thì việc tối ưu này trình biên dịch nên làm :D

## 7. Toán tử prefix vs postfix

Trong vòng lặp, các toán tử prefix `--i` hoặc `++i` thực hiện nhanh hơn so với toán tử postfix `i--` hoặc `i++`. Nguyên nhân do postfix cần lưu lại giá trị để sử dụng sau. Tuy nhiên bây giờ tất cả các trình biên dịch hiện đại đã tối ưu điều này

## 8. Tối ưu con trỏ

- Hạn chế di chuyển pointer nhiều cấp nhiều lần như gọi lệnh `a->b->c->d();` này trong vòng for thay vì thế có thể sử dụng biến tạm `tmp=a->b->c;` ngoài vòng for.
- Nên sử dụng các con trỏ thông minh để thay thế. Tuy rằng tốc độ là chậm hơn tuy nhiên không đáng kể và nhất là dễ dành quản lý giảm thiểu rò rỉ bộ nhớ.

## 9. Tối ưu hóa bộ nhớ

Tránh phân mảnh vùng nhớ, việc liên tục cấp phát và giải phóng bộ nhớ sẽ làm giảm tốc độ chương trình cũng như phân mảnh và chiếm dụng bộ nhớ. Có một số giải pháp sau:
- Sử dụng bộ nhớ tĩnh sẽ cho tốt độ truy xuất cao.
- Khi dùng bộ nhớ động tránh cấp phát và giải phóng những vùng nhớ có kích thước nhỏ.
- Xin cấp phát một vùng nhớ lớn và quản lý để có thể tái sử dụng. Có thể tìm hiểu thêm với từ khóa object-pool.

## 10. Hạn chế dùng try-catch

Việc sử dụng `try-catch` sẽ làm tăng kích thước và giảm tốc độ của chương trình. Vì vậy nên hạn chế sử dụng try-catch.

## 11. Tận dụng CPU

- Thứ tự khai báo dữ liệu sẽ làm thay đổi kích thước đối tượng. VD: các bộ vi xử lý hiện nay tính toán theo từng offset 4byte. vì thế với khai báo dưới thì `sizeof(Test1)` sẽ là 16 còn `sizeof(Test2)` sẽ là 12
```cpp
class Test1
{
    bool a;
    int c;
    int d;
    bool b
}
class Test2
{
    int c;
    int d;
    bool a;
    bool b
}
```
- Ngoài ra có thể tận dụng có thể tận dụng bộ nhớ case của CPU. Khi lớp có nhiều thuộc tính khác nhau. Tuy nhiên cần duyệt và xử lý chỉ một hoặc ít thuộc tính của đối tượng một cách liên tục. Ta có thể tác thuốc tính đó ra khỏi lớp để tránh việc tải dữ liệu liên tục không cần thiết của cả đối tượng trong vòng for cũng như làm vậy sẽ tăng tốc độ đẩy dữ liệu cho cpu xử lý

## 12. So sánh với số real

Trục số trong khoa học máy tính là tròn, cũng gần như vậy với số thực trong việc máy tính tính toán sẽ có sai số. Vì vậy khi cần so sánh với độ chính xác cao nên so sánh với bù trừ của epsilon.
```cpp
std::fabs(x - y) <= std::numeric_limits<double>::epsilon();
```