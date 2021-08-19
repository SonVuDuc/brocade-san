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










































