# Brocade Firmware Management Fundamentals

## Brocade Switch Firmware Download Process Overview

### Firmware Download Best Practice

Cần đọc những release note của firmware mới để xem có update nào liên quan đến firmware của mình hay ko

Tham khảo tài liệu Brocade Fabric OS Software Upgrade Guide, 9.0.x để xem thông tin:
- Tải firmware
- Upgrade và downgrade firmware
- Độ tương thích của firmware

Kết nối với switch và chạy lệnh **firmwareshow** để xem phiên bản hiện tại của Fabric OS

Chạy lệnh **configupload** để backup lại file config của switch

Trước khi tiến hành download firmware, cần kết nối vào switch và chạy lệnh **supportsave** thu hồi lại các core files. Điều này giúp quá trình kiểm tra lỗi trong trường hợp upgrade firmware xảy ra vấn đề

Đối với mỗi switch trong fabric, cần phải hoàn tất toàn bộ sự thay đổi liên quan đến quá trình download firmware trên switch hiện tại. Sau đó mới được chạy **firmwaredownload** trên switch khác. Điều này đảm bảo traffic giữa các switch ko bị gián đoạn

Không được thay đổi config switch khi đang download firmware

### Download Firmware

Truy cập vào trang chủ của Brocade Fabric OS tại địa chỉ https://www.broadcom.com

- Đăng nhập hoặc đăng ký tài khoản customer
- Click vào Product

![image](https://user-images.githubusercontent.com/32956424/131382835-85c71c7e-e238-402e-86a8-c702194bc355.png)

<><><><><><><><><><><><><><><><><><><><><><><><><><><>

## End User License Agreement Acceptance

Ở trên Brocade SAN switch, để upgrade được firmware lên Fabric OS v9.0.0 hoặc hơn cần phải chấp nhận EULA

Bao gồm cả quá trình upgrade hoặc downgrade

Để display EULA, dùng lệnh **firmwaredownload -showEULA**

![image](https://user-images.githubusercontent.com/32956424/131384423-c900d833-2d6e-499f-a06b-ad629e87d30f.png)

Khi chạy lệnh **firmwaredownload**, hệ thống sẽ yêu cầu người dùng accept EULA

![image](https://user-images.githubusercontent.com/32956424/131384515-1c4e22e6-9409-454d-b1ca-fcf0f10abafa.png)

## Firmware Download on Departmental Switches

### Firmware Download Internal Process

![image](https://user-images.githubusercontent.com/32956424/131384611-447413a5-d56f-4f8d-8b84-19a33115676b.png)




































