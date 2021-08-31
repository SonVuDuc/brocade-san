# Brocade Firmware Management Fundamentals

## Brocade Switch Firmware Download Process Overview

### Firmware Download Best Practice

Cần đọc những release note của firmware mới để xem có update nào liên quan đến firmware của mình hay ko

Tham khảo tài liệu Brocade Fabric OS Software Upgrade Guide, 9.0.x để xem thông tin:
- Tải firmware
- Upgrade và downgrade firmware
- Độ tương thích của firmware

Kết nối với switch và chạy lệnh ```firmwareshow``` để xem phiên bản hiện tại của Fabric OS

Chạy lệnh ```configupload``` để backup lại file config của switch

Trước khi tiến hành download firmware, cần kết nối vào switch và chạy lệnh ```supportsave``` thu hồi lại các core files. Điều này giúp quá trình kiểm tra lỗi trong trường hợp upgrade firmware xảy ra vấn đề

Đối với mỗi switch trong fabric, cần phải hoàn tất toàn bộ sự thay đổi liên quan đến quá trình download firmware trên switch hiện tại. Sau đó mới được chạy ```firmwaredownload``` trên switch khác. Điều này đảm bảo traffic giữa các switch ko bị gián đoạn

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

Để display EULA, dùng lệnh ```firmwaredownload -showEULA```

![image](https://user-images.githubusercontent.com/32956424/131384423-c900d833-2d6e-499f-a06b-ad629e87d30f.png)

Khi chạy lệnh ```firmwaredownload```, hệ thống sẽ yêu cầu người dùng accept EULA

![image](https://user-images.githubusercontent.com/32956424/131384515-1c4e22e6-9409-454d-b1ca-fcf0f10abafa.png)

## Firmware Download on Departmental Switches

### Firmware Download Internal Process

![image](https://user-images.githubusercontent.com/32956424/131384611-447413a5-d56f-4f8d-8b84-19a33115676b.png)

### Check current Firmware Status

Trong bộ nhớ flash của switch, được chia làm 2 phân vùng giống nhau: primary và secondary partition.

Phân vùng Primary chạy phiên bản firmware hiện tại của Switch. Phân vùng Secondary có thể chạy firmware giống Primary hoặc khác

![image](https://user-images.githubusercontent.com/32956424/131434561-226128db-3e01-4f52-bb58-7e59371a6585.png)


Để kiểm tra firmware của 2 phân vùng, sử dụng lệnh ```firmwareshow```

![image](https://user-images.githubusercontent.com/32956424/131434403-623d760d-a620-4289-9908-6820d4a01b38.png)

### The firmwaredownload Command

Lệnh ```firmwaredownload``` dùng để download firmware 

![image](https://user-images.githubusercontent.com/32956424/131435420-2c64a4ec-aa1c-4424-9784-f17aa8009361.png)

Firmware khi được download, nó sẽ được lưu ở một phân vùng

Mặc định lệnh ```firmwaredownload``` sẽ tự động reboot và tự động commnit để nhân bản firmware vào phân vùng còn lại

### Firmware Download 

Firmware khi được download, nó sẽ được lưu ở phân vùng Secondary

![image](https://user-images.githubusercontent.com/32956424/131435804-25eb7324-a8dd-44a2-976e-56501c8f0c45.png)

Sau khi download , pointer (con trỏ) của 2 phân vùng sẽ swap với nhau. Tức là phân vùng Secondary sẽ trở thành Primary và ngược lại

![image](https://user-images.githubusercontent.com/32956424/131435835-8218c023-30a4-4e1b-a08e-a4d1a15a6852.png)

Sau đó Switch sẽ được reboot và load firmware vừa download ở trong Primary (trước đó nó là Secondary), lúc này switch sẽ chạy phiên bản firmware mới download.

![image](https://user-images.githubusercontent.com/32956424/131436080-e9516c2f-8157-4563-9be5-92a551f3f23f.png)

Sau đó, firmware mới sẽ được commit vào phân vùng còn lại (tức Secondary)

![image](https://user-images.githubusercontent.com/32956424/131436096-64eb3f2d-750a-4d2b-a309-0b1d5bc9e055.png)

Quá trình download Firmware hoàn tất

![image](https://user-images.githubusercontent.com/32956424/131436158-982ea718-4e77-4122-84b3-34b3b229cc5f.png)

![image](https://user-images.githubusercontent.com/32956424/131436165-6467f114-ec43-43b3-bdce-7c6d9ad7c334.png)

Lệnh ```firmwaredownloadstatus``` để kiểm tra trạng thái download firmware 

![image](https://user-images.githubusercontent.com/32956424/131437118-1c4fbd55-a7d4-40d4-a124-27d69bd2025e.png)

## Firmware Download on Director

### Non-disruptive Firmware Download Requirments

Một số yêu cầu trước khi download Firmware trên Director nhằm tránh những lỗi rủi ro:
- Control Processor Card phải có cùng phiên bản firmware 
- CP Card phải trong trạng thái healthy
- HA Monitor đồng bộ giữa 2 CP cards
- Mỗi CP phải có kết nối mạng

Chạy lệnh ```hashow``` để kiếm ra một số thông số:
- CP đang active hiện tại, CP đang standby
- CP standby có trong trạng thái healthy không ? Có ready để chuyển sang active không ?
- HA có enable giữa 2 CP card không ?
- Heartbeat có up không ?
- Trạng thái của HA 

![image](https://user-images.githubusercontent.com/32956424/131438040-9cf35446-faa4-4717-afac-8d308c2ffa20.png)

### Director Firmware Upgrade Overview 

![image](https://user-images.githubusercontent.com/32956424/131438243-8fd30617-8313-46d5-a086-c884d57c4b4c.png)

### Pre-steps: hashow and firmwareshow Commands

Kiểm tra CP, HA bằng ```hashow```

![image](https://user-images.githubusercontent.com/32956424/131439268-cf5b8187-9d67-4c27-a9f8-8c10c9b3f785.png)

Trong đó:
- Cold Recovered: CP trở thành active thông qua power on hoặc hard reboot và disruptive
- Warm Recovered: CP trở thành active thông qua failover và non-disruptive
- Healthy: Standby CP đang chạy ở background và không phát hiện ra lỗi
- Failed: Standby CP đang chạy ở background và có vấn đề xảy ra. Failover sẽ bị disabled cho đến khi CP này được repair
- Unknown: Không thể xác định trạng thái healthy của CP. Có thể do heartbeat bị down, standby CP không tồn tại...

Kiểm tra firmware của CP bằng ```firmwareshow```

![image](https://user-images.githubusercontent.com/32956424/131439235-77c1bbe8-171d-4ed0-9033-62c2632affeb.png)

### Pre-steps: Management Interfaces

Cần đảm bảo rằng mỗi management interface trên mỗi CP phải có kết nối mạng

Trong quá trình download firmware, CP sẽ reboot thay đổi trạng thái giữa active và standby

Management interfaces giữ CP active và standby có kết nối với mạng

Nếu không có management interface, quá trình download sẽ fail

![image](https://user-images.githubusercontent.com/32956424/131441201-8d2ed743-8a1f-41da-a3af-4e27ea0bbad1.png)

### Director Firmware Upgrade

Initial State: tất cả partition của 2 CP đều chạy cùng một phiên bản firmware

![image](https://user-images.githubusercontent.com/32956424/131441447-0bed0955-27a7-4333-8c50-6a93e80ce97f.png)

Quá trình download firmware của Director phức tạp hơn trên Switch. Vì nó có 2 CP card, một CP active và một CP standby

Mỗi CP lại có bộ nhớ flash của riêng chúng, được chia làm 2 phân vùng giống nhau: Primary và Secondary như trên Switch

Lệnh ```firmwaredownload``` phải được chạy trên CP active

![image](https://user-images.githubusercontent.com/32956424/131441851-56e778d0-072b-440d-b6e9-d525b3fa43a2.png)

Firmware được download về phân vùng Secondary của CP standby (thông qua kết nối ở management port)

![image](https://user-images.githubusercontent.com/32956424/131441971-716c9693-dcb6-4bb5-a7ba-cba99a1f6028.png)

Trong CP standby, tương tự như ỏ switch, phân vùng Primary và Secondary được swap với nhau. Khi này firmware mới sẽ nằm ở Primary

![image](https://user-images.githubusercontent.com/32956424/131442216-833cc88e-024b-4be5-a801-d77875c55350.png)

Sau đó CP standby sẽ reboot và load firmware mới từ Primary, lúc này CP standby đang chạy firmware mới

![image](https://user-images.githubusercontent.com/32956424/131443162-415a31c6-8742-4fa4-864d-719ff6eedaea.png)

Sau khi reboot CP standby xong, nó sẽ resync với CP active. Nó gửi trạng thái cho CP active là quá trình reboot đã hoàn tất. Heartbeat khi này sẽ up và 2 CP sẽ kết nối được với nhau 

![image](https://user-images.githubusercontent.com/32956424/131444379-f09ba751-28fc-4c7e-91a2-991e62a65125.png)

Do lúc này CP standby đang chạy firmware mới, quá trình HA failover sẽ diễn ra. CP standby (CP1) sẽ trở thành active CP, còn CP0 sẽ trở thành CP standby

![image](https://user-images.githubusercontent.com/32956424/131444464-9ba0c436-2f9b-4f2f-a2ed-658175b37901.png)

Firmware Fabric OS B được copy từ Primary CP Active sang Secodary CP Standby

![image](https://user-images.githubusercontent.com/32956424/131446033-66d97921-b003-4268-bf64-1dd9be1ce04b.png)

Trên CP Standby (CP0), 2 phân vùng sẽ được swap với nhau, do đó phân vùng Primary sẽ chứa firmware Fabric OS B

![image](https://user-images.githubusercontent.com/32956424/131446105-c6a447ca-3610-40ef-9d33-b9db3615b9fc.png)

CP Standby sẽ reboot và load firmware Fabric OS B từ Primary. Lúc này CP0 sẽ chạy firmware mới. Khi hoàn tất, nó sẽ resync lại với CP1, thông báo là reboot đã hoàn tất

Lúc này cả 2 CP đều đang chạy firmware Fabric OS B. Tuy nhiên phân vùng Secondary vẫn chưa firmware cũ

![image](https://user-images.githubusercontent.com/32956424/131446351-39b057d0-37ae-4285-9e09-dd8f63a59eb0.png)

Bước cuối cùng, là commit firmware, Fabric OS B sẽ được copy vào phân vùng Secondary của cả 2 CP 

![image](https://user-images.githubusercontent.com/32956424/131446464-6dada27e-60ba-4576-9187-c0016d40b0cb.png)

Quá trình download firmware hoàn tất

![image](https://user-images.githubusercontent.com/32956424/131446565-91ece9f7-3f8d-4cfa-8b7e-6782385e9bd3.png)

Kiểm tra firmware bằng ```firmwareshow```

![image](https://user-images.githubusercontent.com/32956424/131446826-2838d606-b733-42f7-803a-35d208a2e347.png)

## USB Device Firmware Upgrade

Cắm USB vào cổng USB 

Trên Director, chỉ có CP Active mới có thể mount được USB, nhưng USB drive có thể cài trên cả 2 CP

Chạy lệnh ```usbstorage -e``` để mount USB

Lệnh ```usbstorage -l``` để xem bên trong USB

![image](https://user-images.githubusercontent.com/32956424/131447388-48492519-309e-4d0a-890d-df9cb38bc83b.png)

Để download firmware từ USB drive, chạy lệnh ```firmwaredownload -U <fileName>```

![image](https://user-images.githubusercontent.com/32956424/131447518-a689f9e7-0760-4589-9469-6fe3356dbeca.png)

## Firmware Staging and Testing

Mặc định thì lệnh ```firmwaredownload``` sẽ kèm theo chức năng auto reboot và auto commit, tuy nhiên nếu thêm tham số ```-s```  thì chế độ auto reboot và commit có thể được tuỳ chỉnh

Lúc này nếu auto reboot bị OFF, thì firmware sẽ ở trạng thái staged, tức là download về phân vùng Secondary nhưng switch vẫn chạy phiên bản firmware cũ

![image](https://user-images.githubusercontent.com/32956424/131448067-331d88f6-f922-431a-ab16-cb7991ba1549.png)

Khi download firmware xong thì sẽ thấy 2 phân vùng với 2 phiên bản firmware khác nhau

![image](https://user-images.githubusercontent.com/32956424/131448121-677d14a5-5708-4266-833c-5d38b2f8beca.png)

Nếu chạy lệnh ```reboot``` switch thì sẽ thấy 2 phân vùng đổi chỗ cho nhau, và switch sẽ chạy firmware mới

![image](https://user-images.githubusercontent.com/32956424/131448164-7eaeb427-cc38-4e45-a47b-0e50b9843cf5.png)

Lúc này người dùng có thể:
- Để cho switch chạy firmware mới bằng cách dùng lệnh ```firmwarecommit``` để commit firmware mới
- Quay lại phiên bản trước đó bằng lệnh ```firmwarerestore```
- Giữ nguyên như vậy. Tuy nhiên cách này là không nên

## Firmware History

Để hiển thị lịch sử firmware, chạy lệnh ```firmwareshow --history```

![image](https://user-images.githubusercontent.com/32956424/131447844-42829063-63ec-4d87-9676-88c732576b3a.png)

















































