# Fibre Channel Fundamental

## Fibre Channel Basic

### What is SAN ?

<Đã có trong phần Introduce>

### Connectivity

<Đã có trong phần Introduce>


### Domain Identification (Domain ID)

Mọi thiết bị trong SAN đều được định danh bằng ID. Mỗi switch/director trong fabric đều có một domain ID (DID)
- DID này dùng để xác định switch/director trong fabric
- Mỗi DID trong cùng 1 fabric là duy nhất và không bị trùng lặp
- Giá trị DID từ 1 - 239

![image](https://user-images.githubusercontent.com/32956424/130006621-b9fae520-6b7c-48bc-954e-4b7f30a8633c.png)

### Port Identification (Port ID)

Mỗi node port trong fabric đều có một ID 24 bit (PID) để định danh

PID chia làm 3 trường, mỗi trường 8 bit dưới dạng thập lục phân. Thể hiện node port đó đang kết nối với cái gì và thuộc domain nào:
- Domain ID: Domain ID của switch/director mà node port này đang kết nối tới 
- Area ID: Port index của node mà node port này đang kết nối tới
- Node ID: địa chỉ của N_Port

![image](https://user-images.githubusercontent.com/32956424/130007695-828e5970-3a9a-4a2c-9e9f-56e50e761b44.png)

### Fabric Identification (Fabric ID)

Fabric ID dùng để xác định từng switch/director trong Virtual Fabric, đồng thời cũng để sử dụng trong Fibre Channel Routing

Giá trị FID từ 1 - 128

- Trong VF, FID được gán cho tất cả logic switch/director được tạo từ switch/directo vật lý. Logic switch/director trong cùng VF sẽ có FID giống nhau.
- Trong FC Routing, Firbe Channel Router sẽ tự đánh FID cho từng fabric để định tuyến và xác định kết nối tới fabric, chỉ có FC Router mới có thể hiểu FID này.

![image](https://user-images.githubusercontent.com/32956424/130027737-5367ecf7-0af4-46b4-9c13-1659c8e4d8c8.png)

### Common Port Types

<Đã có trong phần Introduce>

### Switch Port Logic

Các port này chỉ có trên Switch/Director

- U_Port: universal port, port này đang trong trạng thái chờ để trở thành loại port khác 
- G_Port: generic port, port này đang trong trạng thái chờ để trở thành F_Port hoặc E_Port
- F_Port: Fabric port, port ở trên switch/director để kết nối với N_Port
- E_Port: Expansion port, port sử dụng cho ISL giữa các switch/director

![image](https://user-images.githubusercontent.com/32956424/130029285-02840151-21c1-4edc-98aa-af0bbdb1436c.png)

### Additonal Port Types

- EX_Port: một loại E_Port, kết nối từ FC Router đến fabric
- VE_Port: Virtual E_Port (dùng trong FCIP Fabric)
- VEX_Port: giống như EX_Port, nhưng truyền dữ liệu qua IP thay vì FC
- D_Port: port cấu hình để kiểm tra kết nối với một D_Port khác
- M_Port: port cấu hình sử dụng tính năng Flow Mirror

![image](https://user-images.githubusercontent.com/32956424/130031845-8b4384ce-f847-44a3-bb59-2176ee6b749a.png)

## Fibre Channel Protocol Fundamentals

### Fibre Channel Networking Model

FC được chia thành các layer, mỗi layer đảm nhiệm một vai trò riêng

- FC-0: bao gồm thông tin về kênh truyền vật lý như tốc độ, loại cáp truyền
- FC-1: mã hoá và giải mã 
- FC-2: định nghĩa cấu trúc frame, flow control
  - Cách chia dữ liệu ra từng frame
  - Kiểm soát lượng dữ liệu gửi
  - Vị trí mà frame được gửi đi
- FC-3: dịch vụ chung của fabric
  - Striping dữ liệu qua tới nhiều link
  - Multicast
  - Hunt group: mapping nhiều port vào một node
- FC-4: định nghĩa các giao thức được carry bởi FC

![image](https://user-images.githubusercontent.com/32956424/130032038-bc4d41a5-29c5-4c91-85be-b74ec2ff73e1.png)


...

## World Wide Name and Port IDs

### Node World Wide Name - NWWN 

Là địa chỉ 8 bytes được gán cho thiết bị, để định danh node trong SAN.

Theo chuẩn IEEE, định dạng của Brocade WWN có dạng 10:00:xx:xx:xx:yy:yy:yy

Trong đó 10:00 luôn là 2 bytes đầu tiên, 3 bytes xx:xx:xx là giá trị đại diện cho nhà sản xuất, được quy định bởi IEEE (OUI code), 3 bytes yy:yy:yy là do nhà sản xuất tự chỉ định.

VD: OUI code của Brocade là 00:27:F8 

Định dạng này được áp dụng cho Switch, Storage controller, HBA

![image](https://user-images.githubusercontent.com/32956424/130041145-76750158-710e-43d4-807a-8a7244c00154.png)


### Port World Wide Name - PWWN 

Khá giống với NWWN, cũng là địa chỉ 8 bytes được gán cho port. Định dạng của PWWN khác với NWWN

Định dạng PWWN bao gồm:

2p:pp:xx:xx:xx:yy:yy:yy

Trong đó, p:pp là thể hiện số port trên thiết bị. xx:xx:xx là OUI code của nhà sản xuất, yy:yy:yy là do nhà sản xuất tự chỉ định.

VD: port số 9 sẽ là 0:09

![image](https://user-images.githubusercontent.com/32956424/130051524-de350422-97b2-4243-bc82-6b6db9c561fb.png)

### Port ID Overview

Khi một thiết bị như server hoặc storage được kết nối vào một fabric, nó sẽ được gán một PID 24 bit. PID nhằm xác định vị trí của thiết bị trong fabric. 

PID chia làm 3 trường, mỗi trường 8 bit dưới dạng thập lục phân. Thể hiện node port đó đang kết nối với cái gì và thuộc domain nào:
- Domain ID: Domain ID của switch/director mà node port này đang kết nối tới 
- Area ID: Port index của node mà node port này đang kết nối tới
- Node ID: địa chỉ của N_Port

![image](https://user-images.githubusercontent.com/32956424/130076975-dcf4db08-e401-455b-9a7a-ccf58fcd3d66.png)

### Port ID: Gen 7 Switches and Directors

Gen 7 Switch/Director sử dụng cơ chế Unified Addressing Mode (UAM) khi gán địa chỉ 24 bit PID

UAM sử dụng 10 bit cho Area ID và 6 bit cho Node Address. Cho phép số lượng port lớn hơn 256

![image](https://user-images.githubusercontent.com/32956424/130077727-d98971de-bb6a-4916-a6ff-200a2b57f168.png)


## Fabric Services

### Fabric Services and Well-Known Address

FC cũng định nghĩa ra các service dùng để quản lý FC network, ví dụ như fabric port login, nameserver registration,...

Thường dùng trong fabric topology

Mỗi service lại có một địa chỉ riêng được gọi là Well-Known Address

![image](https://user-images.githubusercontent.com/32956424/130089423-1e22c9dd-d1fd-47c3-844a-a36ad0c4ccdd.png)

### Common Fabric Service Well-Known Address

Một số service thông dụng và Well-Known Address của nó

Chúng cung cấp dịch vụ cho node hoặc ứng dụng quản lý trong fabric

- FFFFFE (Fabric Login Server): gán địa chỉ 24 bit cho node khi tham gia vào fabric
- FFFFFD (Fabric Controller): tạo thông báo thay đổi trạng thái tới register node khi có sự thay đổi trong fabric
- FFFFFC (Directory Server): là nơi mà node register và query để phát hiện các thiết bị trong fabric
- FFFFFA (Management Server): cho phép các ứng dụng quản lý FC SAN lấy dữ liệu và quản lý fabric 

### Fabric Login

FFFFE

Khi node tham gia vào fabric, nó sẽ tạo fabric login request (FLOGI), trao đổi các tham số dịch vụ với fabric và nhận lại địa chỉ 24 bit

![image](https://user-images.githubusercontent.com/32956424/130095121-0eb8cd0b-5693-491d-8d8f-e756d8c9623b.png)


### Name Server

FFFFFC 

Mỗi brocade switch đều chứa một Nameserver để quản lý các thông tin local. 

Name Server cung cấp dịch vụ Name Service cho các thiết bị local.

Thông tin local được chia sẻ giữa các switch và lưu trong Name Server cache.

Khi có thiết bị được kết nối với switch, thông tin sẽ được chia sẻ cho toàn bộ switch trong Fabric. 

Name Server quản lý tất cả thông tin về những thiết bị có kết nối tới fabric

![image](https://user-images.githubusercontent.com/32956424/130099943-e4a1d5e7-59d5-4b37-aa09-56febe053061.png)

Một số lệnh Fabric OS

**nsshow**: hiển thị chi tiết thông tin về các thiết bị kết nối tới switch (Local Name Server)

Các trường bao gồm:
- Type: loại port (U là unknown, N là N_Port)
- PID: địa chỉ 24 bit FC
- COS: Class of Service
- PortName: PWWN
- NodeName: NWWN
- Permanent Port Name: NWWN của port vật lý
- SCR: State Change Registration

![image](https://user-images.githubusercontent.com/32956424/130116188-377c3bae-1491-417d-af26-bf0d53ac7a81.png)


**nscamshow**: hiển thị chi tiết thông tin về các thiết bị kết nối tới switch khác (Remote Name Server)

![image](https://user-images.githubusercontent.com/32956424/130116562-04e62c18-4ff5-45bb-b8dd-9e6d9982c2df.png)

**nsallshow**: hiển thị địa chỉ 24bit của toàn bộ thiết bị trong Fabric

![image](https://user-images.githubusercontent.com/32956424/130117115-1930f9ff-3b50-4f8b-a9b4-d57b4aed668d.png)

### Fabric Controller

SCR được sử dụng bởi initiator để request thông báo từ fabric khi có trạng thái thay đổi

Server gửi SCR cho Fabric Controller, controller trả về SCR ACC (Accept). Khi nó nếu server thay đổi trạng thái online hay offline thì Fabric Controller sẽ biết được sự thay đổi này.

![image](https://user-images.githubusercontent.com/32956424/130117618-e312e6af-c512-4223-8a63-3374b86d3cc9.png)


## N_Port Virtualization (NPIV)

- Virtual server cũng có những chức năng và kết nối tới storage như một server vật lý
- NPIV cho phép một server vật lý có thể tạo ra nhiều kết nối riêng biệt cho nhiều virtual server, tăng tính năng bảo mật. Nếu không thì  tất cả LUN  và storage port sẽ exposed và tất cả VM sẽ nhìn thấy được.
- Các thiết bị NPIV kết nối tới cùng 1 port trên switch đều phải có địa chỉ 24 bit riêng biệt


Với NPIV, một HBA có thể tạo ra 255 PWWN riêng biệt

Switch hỗ trợ NPIV có thể gán fabric PID cho từng virtual server khi chúng kết nối vào fabric

Quá trình zoning virtual server hoạt động bình thường như server vật lý

![image](https://user-images.githubusercontent.com/32956424/130165264-6fb74be5-ffd9-4301-9af6-2eef557a1846.png)

Lệnh **nsshow**

Permanent Port Wide Name của các port ở đây là giống nhau, tức là chúng có chung một port vật lý. Nhưng là virtual server nên có PID khác nhau

![image](https://user-images.githubusercontent.com/32956424/130165638-5426713a-cb3f-415e-97c7-c1183aa84f09.png)

Lệnh **portcfgnpivport** có tác dụng enable hoặc disable tính năng NPIV (mặc định enable), đồng thời cũng có thể giới hạn số lầnlogin với mỗi virtual port

Lệnh **portshow*** để xem những thuộc tính của NPIV và tất cả PWWN của N_Port (cả vật lý lẫn virtual) 

![image](https://user-images.githubusercontent.com/32956424/130165890-3c48de1d-3e3c-4a0e-8f79-692c53dedf2a.png)

Lệnh **switchshow** hiển thị F_Port và số lượng thiết bị NPIV đang kết nối với nó 

![image](https://user-images.githubusercontent.com/32956424/130165958-0e1b0792-ad4a-44b2-a5e8-85c57d5f00a2.png)





















