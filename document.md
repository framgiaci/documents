
# FramgiaCI Documents
`FramgiaCI` là hệ thống CI/CD do Framgia nghiên cứu và phát triển, ngoài những tính năng chính của một hệ thống CI/CD cần có, FramgiaCI còn cũng cấp thêm nhiều tính năng nhằm giúp người dùng dễ dàng hơn trong quá trình thực hiện dự án, dưới đây là hướng dẫn sử dụng bạn cần biết khi làm việc với FramgiaCI.

## 1. Permission
Hiện tại `framgiaCI` đang support login bằng tài khoản github, sau khi login user sẽ có các quyền như sau:

<table>
    <tr>
        <td rowspan="2" colspan='2'>Page</td>
        <td colspan="3">Permission</td>
    </tr>
    <tr>
        <td>Admin</td>
        <td>Mode</td>
        <td>User</td>
    </tr>
    <tr>
        <td colspan='2'>Home page</td>
        <td>+</td>
        <td>+</td>
        <td>+</td>
    </tr>
    <tr>
        <td colspan='2'>Available repository</td>
        <td>+</td>
        <td>+</td>
        <td>-</td>
    </tr>
    <tr>
        <td rowspan='5'>Admin page</td>
        <td>Add user</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Nodes</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Statistics</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Active repository</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Build list</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    
</table>

## 2. Home page (Active Repositories)
Là page hiển thị danh sách các project mà user đang login đã được add vào, hoặc do user đó tự tạo và đã active trên hệ thống framgiaCI.

![home](https://raw.githubusercontent.com/phucqd/documents/master/images/Selection_010.png)

<table>
    <tr>
        <td>#</td>
        <td>Chức năng</td>
        <td colspan='2'>Mô tả</td>
    </tr>
    <tr>
        <td rowspan='12'>1</td>
        <td rowspan='12'>Thống kê</td>
        <td>Active project</td>
        <td>Tổng số project đang active</td>
    </tr>
    <tr>
        <td>Buid success</td>
        <td>Tổng số bản build success, khi đã pass qua hết các câu lệnh</td>
    </tr>
    <tr>
        <td>Build error</td>
        <td>Tổng số bản build error, khi bị failue một trong các câu lệnh</td>
    </tr>
    <tr>
        <td>Build killed</td>
        <td>Tổng số build bị killed. Nếu trong một khoảng thời gian đã config mà không có log được ghi ra</td>
    </tr>
    <tr>
        <td>Build skipped</td>
        <td>Tổng số build đã skipped, do trong quá trình build , user push force </td>
    </tr>
    <tr>
        <td>Build killed</td>
        <td>Tổng số build bị killed. Nếu trong một khoảng thời gian đã config mà không có log được ghi ra</td>
    </tr>
    <tr>
        <td>Build timeout</td>
        <td>Tổng số build đã vượt quá thời gian cho phép, sẽ bị timeout</td>
    </tr>
    <tr>
        <td>Build running</td>
        <td>Tổng số bản build đang running trong thời điểm hiện tại</td>
    </tr>
    <tr>
        <td>Total User</td>
        <td>Tổng số user tính đến thời điểm hiện tại</td>
    </tr>
    <tr>
        <td>Max build time</td>
        <td>Thời gian build nhanh nhất của các bản build</td>
    </tr>
    <tr>
        <td>Min build time</td>
        <td>Thời gian build lâu nhất</td>
    </tr>
    <tr>
        <td>Average build time</td>
        <td>Thời gian build trung bình</td>
    </tr>
    <tr>
        <td>2</td>
        <td>Tìm kiếm</td>
        <td>Nhập tên project</td>
        <td>Tìm kiếm project có tên thỏa mãn</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Danh sách repository/ project</td>
        <td></td>
        <td></td>
    </tr>
</table>

## 3. Available repositories
![available](https://raw.githubusercontent.com/phucqd/documents/master/images/Selection_012.png)
 
 <table>
    <tr>
        <td>#</td>
        <td>Mô tả</td>
    </tr>
    <tr>
        <td>1</td>
        <td>Khi user có project mới trên github thì cần fetch về để cập nhật vào database của hệ thống</td>
    </tr>
    <tr>
        <td>2</td>
        <td>Phân loại project theo các trạng thái all/ personal / Organization</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Tìm kiếm project theo tên</td>
    </tr>
    <tr>
        <td>4</td>
        <td>Danh sách project tương ứng với project trên github</td>
    </tr>
</table>

> Click vào 1 project, nếu có quyền tạo sẽ active được project và tạo được hook  trên github
> 
![available](https://raw.githubusercontent.com/phucqd/documents/master/images/Selection_013.png)

> Trường hợp nếu đã active thì có thể deactive project
> 
![available](https://raw.githubusercontent.com/phucqd/documents/master/images/Selection_014.png)

## 4. Overview
Khi click vào một project đã được active tại trang home, ta sẽ đến page overview. Là page hiển thị thông tin tổng quan, thống kê các bản build của project.

![available](https://raw.githubusercontent.com/phucqd/documents/master/images/Selection_015.png)
![available](https://raw.githubusercontent.com/phucqd/documents/master/images/Selection_016.png)

>Trong đó:
>1. Thống kê đếm tổng số build, số build success, error ...
>2. Chi tiết của bản build gần nhất
>3. Biểu đồ phân tích các bản build trong 10 ngày gần nhất.
>4. Biểu đồ hiển thị lỗi của 10 bản build gần nhất
## 5. Build
Tại trang home khi click vào từng bản build, chúng ta sẽ đến page chi tiết của bản build, gồm các tabs:
> Logs: hiển thị log của những command đã được config trong project
> Coverage: Là kết quả chạy unit test của project
> Violations: Trang thống kê chi tiết, chỉ ra cho bạn các lỗi convention để tiện theo dõi và fix
> Metrics: với các dự án PHP, các dự án Ruby sẽ là các chuẩn : reek, rspec, brakeman ...
> 
![available](https://raw.githubusercontent.com/phucqd/documents/master/images/Selection_023.png)

<table>
    <tr>
        <td>#</td>
        <td>Mô tả</td>
    </tr>
    <tr>
        <td>1</td>
        <td>Là các tab chức năng hiển thị log, coverage, violation ...</td>
    </tr>
    <tr>
        <td>2</td>
        <td>Nếu là người tạo ra bản build, bạn có thể restart bản build đó để chạy build lại</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Chi tiết của bản build, người tạo ra bản build ...</td>
    </tr>
    <tr>
        <td>4</td>
        <td>Khi build đang running, bạn có thể để cho từng dòng log chay từng dòng và auto scroll xuống cuối</td>
    </tr>
    <tr>
        <td>5</td>
        <td>Chi tiết của log sau khi các command được chạy</td>
    </tr>
    <tr>
        <td>6</td>
        <td>Khi log quá dài có thể click vào đây để scroll xuống cuối</td>
    </tr>
    <tr>
        <td>7</td>
        <td>Scroll lên dòng log đầu tiên</td>
    </tr>
</table>

## 6. Setting & Notification
Sau khi active project để bắt đầu sử dụng hệ thống, hoặc trong quá trình sử dụng bạn có thể vào setting để lựa chọn những điều kiện chạy build mà bạn muốn hoặc vào notification để lựa chọn hình thức gửi message là chatwork, email hoặc slack.
![available](https://raw.githubusercontent.com/phucqd/documents/master/images/Selection_021.png)

`FramgiaCI` cung cấp tính năng comment trên github khi gặp lỗi convention sẽ có một bot chỉ cho bạn dòng nào của file nào đang bị lỗi, và lỗi đó là gì. Để thực hiện điều đó đơn bản bạn chỉ cẩn vào setting và thêm bot vào project của bạn bằng cách nhấn button  `Add github bot to this repo `
Nếu trong quá trình làm việc, dự án được thêm member trên github thì bạn cần vào đồng bộ member cho project và quyền cho từng member bằng cách nhất button `Re-sync repo's member`.

![available](https://raw.githubusercontent.com/phucqd/documents/master/images/Selection_022.png)

Khi lựa chọn hình thức notification, sau mỗi bản build sẽ có message gửi tới chatwork, email hoặc slack để báo cho bạn biết trạng thái của bản build.
> Với chatwork:
> 1. Add key cho bot
> 2. Add room id, lưu ý là bot được add ở trên phải đang join room này.

> Với Email
> Chỉ cần add một mail hợp lệ 

> Với slack
> 1. Add slack hook bao gồm: `slack webhook` và `tên hiển thị cho slack` là gì.
> 2. Add room 

Trên đây là hướng dẫn tổng quan về cách sử dụng FramgiaCI, Kết hợp với cách config được mô tả trong file `README.md` hy vọng bạn đã có thể áp dụng thành công FramgiaCI vào project của mình.
