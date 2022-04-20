<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Redis</h1>
</div>

## Main concepts.

- ### Replica
- ### Persistence

  - RDB (Redis DataBase file): RDB thực hiện tạo và sao lưu snapshot của DB vào ổ cứng sau mỗi khoảng thời gian nhất định (ví dụ 5p).
  - AOF (Append Only File): AOF lưu lại tất cả các thao tác write mà server nhận được, các thao tác này sẽ được chạy lại khi restart server hoặc tái thiết lập dataset ban đầu.

  |                                                                                                                                               RDB                                                                                                                                                |                                            AOF                                             |
  | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------: |
  |                                                   Thông thường người dùng sẽ set up để tạo RDB snapshot 5 phút 1 lần (hoặc nhiều hơn). Do vậy, trong trường hợp có sự cố, Redis không thể hoạt động, dữ liệu trong những phút cuối sẽ bị mất.                                                    |           Sử dụng AOF sẽ giúp đảm bảo dataset được bền vững hơn so với dùng RDB.           |
  |                                                                                    Bằng việc lưu trữ data vào 1 file cố định, người dùng có thể dễ dàng chuyển data đến các data centers, máy chủ khác nhau.                                                                                     |                                           Không                                            |
  | RDB cần dùng fork() để tạo tiến trình con phục vụ cho thao tác disk I/O. Trong trường hợp dữ liệu quá lớn, quá trình fork() có thể tốn thời gian và server sẽ không thể đáp ứng được request từ client trong vài milisecond hoặc thậm chí là 1 second tùy thuộc vào lượng data và hiệu năng CPU. | Redis cung cấp tiến trình chạy nền, cho phép ghi lại file AOF khi dung lượng file quá lớn. |
  |                                                                                                    Khi restart server, dùng RDB làm việc với lượng data lớn sẽ có tốc độ cao hơn là dùng AOF.                                                                                                    |                                                                                            |

    <p align="right">(<a href="#top">Back to top</a>)</p>
