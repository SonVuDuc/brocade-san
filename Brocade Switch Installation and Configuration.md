# Brocade Switch Installation and Configuration

## Switch Installation and Configuration

### Installation Concern

Khi cài đặt switch, cần quan tâm đến những vấn đề sau:
- Power: Nguồn điện
- Air: luồng khi tản nhiệt
- Cables: dây cáp
- Monitor Switch Envirnoment: môi trường giám sát switch. Một số câu lệnh như: 
  - psshow: hiển thị trạng thái nguồn
  - fanshow: hiển thị trạng thái quạt
  - tempshow: hiển thị nhiệt độ
  - ...


### Brocade Management Interface and Tools

Một số công cụ quản lý switch, bao gồm

- CLI
  - Serial (PuTTY,...)
  - Telnet
  - SSH
- SANnav
- Webtool
  - HTTP
  - HTTPS
- SNMP

### CLI Shortcut

- Có thể sử dụng phím mũi tên trái phải để di chuyển con trỏ
- Phím mũi tên lên xuống để xem câu lệnh đã chạy trước đó
- Dùng phím Tab để tự hoàn thiện câu lệnh
- Xem lịch sử câu lệnh bằng lệnh **h** (h command)
- Nhiều câu lệnh có thể nhập trên cùng 1 dòng

## Initial Configuration

Các bước cài đặt và cấu hình Switch

![image](https://user-images.githubusercontent.com/32956424/131083080-0f42314f-848c-4e04-942f-69bbfe634b6c.png)

### Connect the Serial Cable

- Kết nối tới switch bằng cáp serial, cắm vào cổng COM. Hoặc sử dụng dây cáp để kết nối đến cổng console 
- Cấu hình địa chỉ IP
- Sau khi cấu hình địa chỉ IP, nên sử dụng SSH hoặc Web Tools

![image](https://user-images.githubusercontent.com/32956424/131083174-31984bf8-16b4-433f-8b57-342991c84426.png)

### Change Default Password

Khi kết nối được với switch thông qua serial port. Đăng nhập vào switch với username/password mặc định

Khi đăng nhập với user admin, hệ thống sẽ yêu cầu đổi password.

### Set Management IP Address

Sử dụng lệnh **ipaddrset** để cấu hình IP 


Với Brocade X7 và X6, cần phải set thêm 2 IP tương ứng với 2 Control Processor. Tổng là 3 IP 

VD: Default IP management: 10.77.77.77, IP CP0: 10.77.77.75, IP CP1: 10.77.77.76


### Log in through the Ethernet Interface

Có thể có nhiều telnet session cùng lúc

Telnet có thể bị disable bằng lệnh **ipfilter** để kết nối thông qua SSH,

### Set switch configuration parameters

#### Setting the Domain ID

Giá trị mặc định của DID là 1, (1-239)

Mặc dù DID của switch sẽ tự động được gán khi switch enable, tuy nhiên nên thay đổi giá trị DID của switch trước khi kết nối nó vào fabric.

Nếu switch tự động có DID khi bật mà DID bị trùng với một DID khác, sẽ gây ra hiện tượng conflict


#### Active Licsensed Feature

Licsensed để kích hoạt các tính năng của switch

Hiển thị licsense ID bằng lệnh **licsense -show -lid**

Để thêm licsense key, dùng lệnh **licsense -install -key <lic_key>**

#### Set Fabric wide clock






























