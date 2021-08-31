# Brocade Fabric Shortest Path First Fundamentals

## Layer 2 Fabric Routing - Overview

Là giao thức định tuyến thuộc kiểu link-state. Tức là switch sẽ biết hết toàn bộ topo mạng và lưu trong database

Switch sử dụng database để xây dựng nên bảng định tuyến của riêng nó, tính toán đường đi ngắn nhất tới mọi node trong fabric

![image](https://user-images.githubusercontent.com/32956424/131576812-28fe52d6-c679-4af6-b6f2-2174d9a0860e.png)

## Fabric Terminology

- Inter-Switch Link (ISL): E_Port to E_Port
- Principal Switch: 
  - Switch này được fabric chọn ra trước khi bắt đầu quá trình định tuyến
  - Quản lý Domain ID
  - Đồng bộ thời gian tất cả switch trong fabric
- Principal ISL: ISL kết nối từ principal switch tới switch khác

## Principal Switch Path

![image](https://user-images.githubusercontent.com/32956424/131577149-2275a91c-6ae8-4e44-90a6-f743e45e5fd2.png)


## Principal Switch Commands

Set mức độ ưu tiên principal cho switch bằng lệnh ```fabricprincipal```

Quá trình chọn switch principal:
- Nếu ko có switch nào được ưu tiên, switch có WWN nhỏ nhất sẽ trở thành principal
- Nếu có nhiều switch được set mức ưu tiên, thì chỉ có chúng tham gia vào quá trình chọn principal switch
- Switch có mức ưu tiên thấp nhất sẽ trở thành principal
- Nếu có nhiều switch có mức ưu tiên thấp như nhau thì switch có WWN thấp hơn sẽ thành principal

![image](https://user-images.githubusercontent.com/32956424/131577716-e688bb59-1b31-423f-8201-ff42e631387a.png)

## Routing Terminology

Oversubscription: tranh chấp băng thông dự kiến

Congestion: tranh chấp băng thông thực tế

ISL oversubscription ration calculation methods:
- By port count
- By bandwidth

## ISL Oversubscription

Trong hình, tổng băng thông là 256 nhưng lại chỉ có 1 đường 64

![image](https://user-images.githubusercontent.com/32956424/131578412-e10dcf48-672d-43e9-aea0-89fe0a8534a9.png)


## Virtual Channel

## Fabric Shortest Path First

Tính đoán đường đi switch-to-switch với chi phí thấp nhất

Download bảng định tuyến vào ASICs

Path và Route:
- Path: tất cả những đường đi có thể đến được đích
  - Mỗi ISL đều có trọng số metric cost
  - Chi phí đường đi được tính bằng tổng chi phí của các ISL tới đích
- Route: là path với chi phí đường đi thấp nhất và được đưa vào bảng định tuyến

## FSPF Link Cost

Giá trị metric được gán cho phía Tx (Transfer)

Default link cost của Brocade cho các giá trị 2, 4, 8, 10, 16, 32, 64 Gbps là 500

![image](https://user-images.githubusercontent.com/32956424/131579041-8bf2b10e-d598-4051-afd5-3768c38b1706.png)
















































