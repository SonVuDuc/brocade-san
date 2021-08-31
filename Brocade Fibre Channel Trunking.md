# Brocade Fibre Channel Trunking Fundamentals

## Brocade ISL Trunking Overview

Trunking là một tính năng giúp tối ưu lượng băng thông bằng cách nhóm các đường ISL vật lý thành một đường ISL logic, được gọi là trunk group

Mục đích của trunking:
- Giảm tắc nghẽn trên từng đường link ISL
- Tăng khả năng chịu lỗi, tăng băng thông

Các frame được truyền và được ghép lại với trong tất cả ISL của trunk group

Một port duy nhất của trunk group, được gọi là trunk master, sẽ đại diện cho toàn bộ trunk trong bảng định tuyến

![image](https://user-images.githubusercontent.com/32956424/131546122-70062846-63f0-4d59-9aa3-1e262d3ac2f6.png)

Tự động ghép 8 đường ISL khi switch được kết nối
- Condor 5 ASICs: 512Gbps tổng băng thông
- Condor 4 ASICs: 256Gbps tổng băng thông

Tất cả port trunk phải có chung speed. 

Condor 5 ASICs hỗ trơ 8/10/16/32/64 Gbps trunk

Condor 4 ASICs hỗ trơ 4/8/10/16/32 Gbps trunk

## Trunking Requirement

- Phải có license Trunking
- Trunking phải được enable. Nếu nó bị disable thì dùng lệnh ```portcfgtrunkport``` để enable
- Port trunk phải có cùng tốc độ và chung setting về độ dài khoảng cách
- Port trunk phải bắt đầu về kết thúc ở số port hợp lệ

![image](https://user-images.githubusercontent.com/32956424/131547109-5d118635-a1dc-4335-8044-6d3c131143e6.png)

## Trunk Frame Allocation

ASICs sẽ chia đều các frame vào các đường trunk nếu như băng thông đang được sử dụng nhiều 

Trong lúc băng thông còn trống nhiều thì các frame sẽ không bị chia 

Không bị gián đoạn khi trunk master offline

![image](https://user-images.githubusercontent.com/32956424/131547487-0daf2df5-c143-4ce5-865f-3c4272198fce.png)

## Routing over Trunk

Trunk group được coi như là 1 đường logic ISL duy nhất trong bảng định tuyến

Lượng băng thông của trunk group bằng tổng băng thông tất cả đường link tạo nên nó

![image](https://user-images.githubusercontent.com/32956424/131551220-72e81e98-ff27-40b7-a065-4649a93ee93a.png)

## One port group with Multiple ISL Trunk

Lệnh ```switchshow``` dưới đây hiển thị 2 trunk group:
- 32Gbps Trunk group port 0-5: master port 4
- 16Gbps Trunk group port 6-7: master port 7

![image](https://user-images.githubusercontent.com/32956424/131552147-740123f6-4638-4272-8c3e-c62c3e7b0e79.png)

## Masterless Trunking

Brocade sử dụng cơ chế masterless trunking để ngăn chặn lỗi xảy ra khi trunk master offline

Vì trunk master đại diện cho cả trunk group khi định tuyển, nên khi trunk master offline, trunk group lập tức sẽ chọn ra trunk master mới

![image](https://user-images.githubusercontent.com/32956424/131552685-96085083-477a-4ed3-a404-f35dab2c8c78.png)

![image](https://user-images.githubusercontent.com/32956424/131552694-a219741d-ad69-4e53-9e10-62e906a88987.png)

Tiêu chí chọn trunk master phụ thuộc vào giá trị **deskew**

## Deskew Value

Deskew là một giá trị dùng để duy trì khả năng truyền frame theo yêu cầu, nó được set cho mỗi port dựa trên khoảng cách và chất lượng đường link

Hệ thống sẽ luôn set giá trị deskew thấp nhất là 1 cho ISL có độ trễ thấp nhất

Giá trị deskew của các ISL còn lại được tính toán dựa trên deskew của ISL có độ trễ thấp nhất

Giá trị deskew cũng đại diện cho khả năng truyền tải dữ liệu

Lệnh ```trunkshow``` để hiển thị deskew

## Trunking and Cable Length

Chênh lệch khoảng cách tối đa giữa ISL ngắn nhất và dài nhất trong trunk group là 400 mét

Chênh lệch 2 mét tương đương với 1 deskew 

Nếu chênh lệch lớn hơn 30 mét thì có khả năng perfomance sẽ bị giảm

Nếu ISL ngắn nhất có deskew là 1, suy ra ISL có 30 mét chênh lệch thì sẽ có deskew là 16

## Trunking Related Commands

Một số câu lệnh giúp hiển thị thông tin trunk:
- ```trunkshow```
- ```switchshow```
- ```islshow```
- ```portcfgshow```

Câu lệnh cấu hình tham số trunk:
- ```portcfgtrunkport```
- ```switchcfgtrunk```

Câu lệnh troubleshoot trunk: ```trunkdebug <start port> <end port>```

























