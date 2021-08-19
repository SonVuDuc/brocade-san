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
- Trong VF, FID 



























