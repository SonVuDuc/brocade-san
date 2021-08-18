# 1. Introduction to Fibre Channel SANs (FCSAN-101)

## What is a SAN

SAN - Storage Area Network: là một mạng được thiết kế với mục đính là lưu trữ.
- Hầu hết các SAN đều sử dụng Fibre Channel switch và director.
- Fabric là một mạng lưu trữ ở trong SAN.
- Một SAN có thể chứa nhiều fabric.

![image](https://user-images.githubusercontent.com/32956424/129702432-7e9241b8-1081-4982-bf24-e5bd99d31c7a.png)

## Standard Components of Fibre Channel SAN

Các thành phần cơ bản của hệ thống SAN bao gồm

- Server
- HBA
- SAN Switch
- Diretor
- Storage

## Connectivity

SAN dùng để kết nối server tới thiết bị lưu trữ để cấp phát và chia sẻ tài nguyên.

Một hệ thống SAN thường bao gồm nhiều fabric để đảm bảo tính redundancy.

![image](https://user-images.githubusercontent.com/32956424/129834615-cc32fcf1-9204-42ec-b9f0-d06c557d4650.png)

Cả server và storage device đều kết nối đến fabric.

Host server kết nối đến SAN và client. 

![image](https://user-images.githubusercontent.com/32956424/129834801-5030e525-4ff7-4aa4-a38d-25971c57c85c.png)

Trong Fibre Channel, port được chia ra làm nhiều loại dựa theo cách chúng được sử dụng. Một số port thông dụng bao gồm:

- N_Port: Node port, nằm trên HBA hoặc storage controller, kết nối đến port của switch/director
- F_Port: Fabric port, nằm trên switch/director và được kết nối đến N_Port
- E_Port: Expansion port, port dùng để kết nối các switch/director với nhau

![image](https://user-images.githubusercontent.com/32956424/129867599-a4a14d49-3e80-43da-afd8-97a559773419.png)


## World Wide Name

World Wide Name (WWN) là một địa chỉ 8 bytes hexadecimal dùng để xác định fibre channel port và node port. Node có thể là HBA, switch, storage controller.  

![image](https://user-images.githubusercontent.com/32956424/129868543-efca5812-b7c8-4d18-a0db-270c91637cd4.png)

Mỗi node đêu có một World Wide Node Name (WWNN) được định danh bởi nhà sản xuất. Mỗi port trên từng node cũng được gán một World Wide Port Name (WWPN) duy nhất.

Ví dụ, một HBA 2 port sẽ có 3 WWN, bao gồm 1 WWNN và 2 WWPN.

Byte thứ 2 trong WWPN thể hiện số thứ tự của port đó trên node.

## Zoning

Zoning là quá trình phân vùng các thiết bị trong một fabric thành các logical group được gọi là zones

Các thiết bị trong zone được gọi là *zone members*, các thiết bị chỉ có thể kết nối với thiết bị khác thuộc cùng zone. 

Thiết bị ngoài zone không thể kết nối với thiết bị trong zone (bị coi là isolated)

Một thiết bị có thể thuộc nhiều zone cùng lúc

![image](https://user-images.githubusercontent.com/32956424/129882237-518c1474-3606-4ac4-9b00-a4df24d36f16.png)

Nhiều thiết bị tạo thành zone, nhiều zone tạo thành zone configuration. Một fabric có thể có nhiều zone configuration nhưng chỉ có một configuration chạy tại một thời điểm. 

## Zone membership

Có 2 cách để định nghĩa một thiết bị thuộc zone
- WWN: có thể dùng WWNN hoặc WWPN của thiết bị để thêm vào zone. Khi zone bằng WWN, thiết bị có thể kết nối vào bất kỳ port nào của switch mà vẫn hoạt động
- Domain/Port: bằng cách chọn switch domain ID và port index trên switch để thêm vào zone. Bất kỳ thiết bị nào kết nối vào port đã chọn trên switch sẽ thành trở thành zone membership. Domain ID của mỗi switch phải khác nhau trong cùng một fabric

![image](https://user-images.githubusercontent.com/32956424/129910580-187069d4-1afa-47a8-b5e9-fb7f22f01ce3.png)

## Advanced Switch Feature and Tools

### Access Gateway

Access Gateway là một mode đặc biệt chỉ hỗ trợ trên một số loại Brocade switch. Khi một switch đang ở mode Access Gateway mà được kết nối với một fabric, nó sẽ không merge vào fabric đó, mà sẽ hoạt động như một gateway, cho phép các host bên ngoài kết nối vào fabric thông qua nó.

### Fabric Extension

Là một công nghệ trong storage networking, cho phép mở rộng phạm vi của fabric. Có 2 giải pháp:

- Long Distance ISLs (LD ISLs): bằng cách sử dụng kết nối bằng long distance optic (dây cáp khoảng cách xa) hoặc Wave Division Multiplexer (ghép các kênh sợi quang theo bước sóng) để kết nối Fibre Channel ISLs giữa 2 switch hoặc director ở khoảng cách lên tới 100km 
- Fibre Channel over IP (FCIP): là một giao thức sử dụng Fibre Channel và IP để kết nối SAN ở khoảng cách cực xa. Gói tin Fibre Channel được FCIP đóng gói và truyền chúng đi qua mạng IP. FCIP dựa trên giao thức TCP/IP để thiết lập kết nối giữa các SANs ở khoảng cách xa, hình thành kết nối peer-to-peer.

### Fibre Channel Routing

Là một tính năng cao cấp của FC routing, được dùng khi có 2 hoặc nhiều hơn fabric cần chia sẻ tài nguyên lưu trữ mà không muốn merge chúng lại với nhau.

### Trunking

Dùng để tối ưu lượng băng thông của ISL bằng cách nhóm nhiều đường ISL vật lý thành một đường ISL logic

### Virtual Fabric

Tính năng cho phép ảo hoá một switch hoặc director vật lý thành nhiều switch logic


![image](https://user-images.githubusercontent.com/32956424/129965456-1f0a0eae-9503-429a-8105-000ea734f678.png)


## SAN Fabric Topologies

### Single switch

Mô hình SAN đơn giản nhất

- Ưu:
  - Dễ triển khai
  - Thích hợp với switch có số port cao
- Nhược:
  - Khả năng mở rộng bị giới hạn do số lượng port hạn chế
  - Tính availability bị hạn chế
  - Khả năng chống lỗi thấp

![image](https://user-images.githubusercontent.com/32956424/129968059-a02b3966-5c91-4191-9f83-5e74101d061a.png)

### Cascade

Switch và director kết nối với nhau tạo thành chuỗi

- Ưu:
  - Giảm số lượng port cần dùng cho ISL
  - Quản lý switch dễ dàng

- Nhược:
  - Hiệu suất giảm khi có nhiều hơn 2 switch/director trong fabric
  - Khả năng scalablility và availability không cao

Một node bị lỗi thì cả hệ thống sẽ không hoạt động được 

![image](https://user-images.githubusercontent.com/32956424/129968067-1ffb21b9-cad5-4981-86c3-47b2c61982b8.png)

### Ring 

Mô hình khá giống với Cascade, tuy nhiên switch/director cuối cùng trong chuỗi sẽ kết nối với với switch/diretor đầu tiên tạo thành mô hình vòng tròn

- Ưu:
  - Giống như Cascade nhưng tính availability cao hơn
- Nhược:
  - Khả năng scalablility thấp

Khác với Cascade, khi một node bị lỗi ở trong Ring, hệ thống vẫn hoạt động bình thường

![image](https://user-images.githubusercontent.com/32956424/129968214-97c78898-6537-47f6-9f2d-cf6c78c375bd.png)

### Mesh

Mỗi switch/director đều có kết nối đến mọi switch/director khác trong mạng

- Ưu:
  - Khả năng chịu lỗi cao
  - Khoảng cách tối đa 1 hop tới mọi thiết bị
- Nhược:
  - Khả năng scalablility thấp
  - Số lượng port giảm do phải sử dụng nhiều ISL
  
Sử dụng swtich/director sẽ tăng khả năng mở rộng
  
![image](https://user-images.githubusercontent.com/32956424/129968613-9b430099-56d0-443b-ad55-3f5301dc5a64.png)

### Core/Edge

Mô hình này chia switch/director thành 2 vai trò

Switch kết nối với server và storage nằm ở phần rìa ngoài (Edge switch)

Switch kết nối nằm ở trung tâm (Core switch), dùng để kết nối các switch với nhau

Host và storage có thể kết nối trực tiếp đến core switch tuỳ theo trường hợp thiết kế

- Ưu:
  - Hiệu suất cao
  - Tính scalablility và availability cao
  - Khi cần thêm switch mới vào hệ thống, chỉ cần kết nối với core switch
  - Sử dụng 2 core switch tăng khả năng chịu lỗi
- Nhược:
  - Chi phí rất cao

![image](https://user-images.githubusercontent.com/32956424/129969298-804184e6-7592-4f60-a0f1-63bae64ddbb9.png)


## Fabric Redundancy and Resilientcy

Thiết kế dual faric cung cấp khả năng redundancy cao

![image](https://user-images.githubusercontent.com/32956424/129971209-581e98f1-844b-4f82-87fe-c5bc6ad2ff60.png)




























