
Lưu ý : Các file cấu hình được đặt trong thư mục  `ANDROID/version_3_yml`
### 1. Cấu hình CI SUN
Để bắt đầu sử dụng SunCI, bổ sung file `framgia-ci.yml` ở thư mục gốc của dự án.

Lưu ý  :  trong file `framgia-ci.yml` mặc định sẽ check các coding convention `check_style` ,`check_pmd` ,`check_lint`  . Có thế xoá phần này nếu không cần thiết cho dự án .

Ngược lại để chạy được command `check_style` ,`check_pmd` ,`check_lint` cần cầu hình file `checkci.gradle`  ( xem tại **Cấu hính checkci.gradle** ) 

### 2. Cấu hình Fastlane 
1) Bổ sung thư mục `fastlane` vào thư mục gốc của dự án 
2) Cấu hình các `KEY` , `API_TOKEN` cần thiết  
Trong thư mục `fastlane/.env.secret` thêm các thông tin cần thiết theo bảng giải thích bên dưới 

| KEY | Ý Nghĩa |  Tham khảo |
| -------- | -------- | -------- |
| `FABRIC_API_TOKEN`     | FABRIC_API_TOKEN của  Fabric , để đẩy các bản build APK lên Fabric     |  [API_TOKEN ](https://docs.fabric.io/android/fabric/settings/api-keys.html)     |
| `FABRIC_BUILD_SECRET`     | FABRIC_BUILD_SECRET của Fabric     | [KEY SECRET ](https://docs.fabric.io/android/fabric/settings/api-keys.html)     |
| `GROUP_TESTER`     | Tên Alias của Manage Group trên Fabric | [Manage Testers ](https://docs.fabric.io/apple/beta/tester-management.html)     |
| `CHATWORK_API_TOKEN`     | TOKEN của Account boot Chat Work , để thông báo mỗi khi có bản build mới trên Fabric     |      |
| `CHATWORK_ROOM_ID`     | Room ID của Chat Work muốn thông báo   |      |
| `SEND_TO`     | Danh sách ID Chat word  muốn TO  trực tiếp |      |

3) Cập nhật thông tin mỗi khi tạo Pull Request 

+ Trong thư mục `fastlane/.env.info` thêm các thông tin cần thiết theo bảng giải thích bên dưới

| KEY | Ý Nghĩa |  Tham khảo |  
| -------- | -------- | -------- | 
| `BUILD_TYPE`     | Tên của `build variants`  trong  dự án | [Build Variants](https://developer.android.com/studio/build/build-variants)     | 
| `TICKET_NUMBER`     | Refer theo number ticket  `task/function/fixbug` trong dự án . Mục đích cho việc tracking |    
| `DISTRIBUTE_FABRIC`     | + Nếu `true` khi tạo PR sẽ tạo ra file APK từ PR đó , đẩy lên Fabric và thông báo qua CW  . Ngược lại `false` sẽ bỏ qua phần trên   |    

### 3. Cấu hình checkci.gradle 
1) Bổ sung file `checkci.gradle` ở thư mục gốc của dự án
2) Bổ sung thư mục `config` vào thư mục gốc của dự án 
3) Trong file `app/build.gradle`  thêm 
`apply from: '../checkci.gradle'`

###     4. Bổ sung
1) Thêm vào file  `.gitignore`
```
fastlane/report.xml
fastlane/README.md
.framgia-ci-reports
```

