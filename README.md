# Brocade SAN


## 1. Introduction to Fibre Channel SANs (FCSAN-101)

### What is a SAN

SAN - Storage Area Network là mạng được thiết kế với mục đính là lưu trữ.
- Hầu hết các SAN đều sử dụng Fibre Channel switch và director.
- Fabric là một mạng lưu trữ ở trong SAN.
- Một SAN có thể chứa nhiều fabric.

![image](https://user-images.githubusercontent.com/32956424/129702432-7e9241b8-1081-4982-bf24-e5bd99d31c7a.png)

### Standard Components of Fibre Channel SAN

Các thành phần cơ bản của hệ thống SAN bao gồm

- Server
- HBA
- SAN Switch
- Diretor
- Storage

### Connectivity

SAN dùng để kết nối server tới thiết bị lưu trữ và chia sẻ tài nguyên.

Một hệ thống SAN thường bao gồm nhiều fabric để đảm bảo tính redundancy.

![image](https://user-images.githubusercontent.com/32956424/129834615-cc32fcf1-9204-42ec-b9f0-d06c557d4650.png)

Cả server và storage device đều kết nối đến fabric.



![image](https://user-images.githubusercontent.com/32956424/129834801-5030e525-4ff7-4aa4-a38d-25971c57c85c.png)

![image](https://user-images.githubusercontent.com/32956424/129867599-a4a14d49-3e80-43da-afd8-97a559773419.png)


### World Wide Name

World Wide Name (WWN) là một địa chỉ 8 bytes hexadecimal dùng để xác định fibre channel port và node port. Node có thể là HBA, switch, storage controller.  

![image](https://user-images.githubusercontent.com/32956424/129868543-efca5812-b7c8-4d18-a0db-270c91637cd4.png)

Mỗi node đêu có một World Wide Node Name (WWNN) được định danh bởi nhà sản xuất. Mỗi port trên từng node cũng được gán một World Wide Port Name (WWPN) duy nhất.

Ví dụ, một HBA 2 port sẽ có 3 WWN, bao gồm 1 WWNN và 2 WWPN.

Byte thứ 2 trong WWPN thể hiện số thứ tự của port đó trên node.

### Zoning

Zoning là quá trình phân vùng các thiết bị trong một fabric thành các logical group được gọi là zones

Các thiết bị trong zone được gọi là *zone members*, các thiết bị chỉ có thể kết nối với thiết bị khác thuộc cùng zone. 

Thiết bị ngoài zone không thể kết nối với thiết bị trong zone (bị coi là isolated)

Một thiết bị có thể thuộc nhiều zone cùng lúc

![image](https://user-images.githubusercontent.com/32956424/129882237-518c1474-3606-4ac4-9b00-a4df24d36f16.png)

Nhiều thiết bị tạo thành zone, nhiều zone tạo thành zone configuration. Một fabric có thể có nhiều zone configuration nhưng chỉ có một configuration chạy tại một thời điểm. 

### Zone membership

Có 2 cách để định nghĩa một thiết bị thuộc zone
- WWN: có thể dùng WWNN hoặc WWPN của thiết bị để thêm vào zone. Khi zone bằng WWN, thiết bị có thể kết nối vào bất kỳ port nào của switch mà vẫn hoạt động
- Domain/Port: bằng cách chọn switch domain ID và port index trên switch để thêm vào zone. Bất kỳ thiết bị nào kết nối vào port đã chọn trên switch sẽ thành trở thành zone membership. Domain ID của mỗi switch phải khác nhau trong cùng một fabric

![image](https://user-images.githubusercontent.com/32956424/129910580-187069d4-1afa-47a8-b5e9-fb7f22f01ce3.png)

### Advanced Switch Feature and Tools

#### Access Gateway

Access Gateway là một mode đặc biệt chỉ hỗ trợ trên một số loại Brocade switch. Khi một switch đang ở mode Access Gateway mà được kết nối với một fabric, nó sẽ không merge vào fabric đó, mà sẽ hoạt động như một gateway, cho phép các host bên ngoài kết nối vào fabric thông qua nó.

#### Fabric Extension

Là một công nghệ trong storage networking, cho phép mở rộng phạm vi của fabric. Có 2 giải pháp:

- Long Distance ISLs (LD ISLs): 
- Fibre Channel over IP (FCIP):
















