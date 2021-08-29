# Brocade Zoning

## Zoning Overview

Khái niệm zoning 

Khi zoning được enable:
- Thiết bị chỉ có thế kết nối với nhau trong khi zone
- Thiết bị ngoài zone không thể kết nối với thiết bị khác
- Một thiết bị có thể thuộc nhiều zone

![image](https://user-images.githubusercontent.com/32956424/131257985-de0ea59a-fbbb-4679-b42a-115d7fa76cbe.png)

Đặc điểm của zoning truyền thống:
- Chỉ có một initiator mỗi zone
  - Tránh trường hợp các initiator trong cùng zone tự query lẫn nhau
  - Một initiator có thể zone với nhiều target port trong cùng zone

Nếu một thiết bị được zone với 2 initiator thì sẽ có 2 zone được tạo ra

## Default Zone

Ở những phiên bản Fabric OS đầu, chưa có zone, các thiết bị chung fabric hoàn toàn có thể nhìn thấy nhau -> security ko đảm bảo

Default zone là một zone mặc định của switch, dùng để kiểm soát thiết bị trong fabric khi không có zone khác enable. 

Default zone có 2 mode:
- allaccess (mặc định): các thiết bị chung fabric sẽ thấy nhau
- noaccess: các thiết bị chung fabric sẽ ko thấy nhau

Default zone sẽ chạy khi không có cấu hình zone nào khác đang chạy và tự disable khi có cấu hình zone khác enable

Để hiển thị default zone, dùng lệnh **defzone --show**

![image](https://user-images.githubusercontent.com/32956424/131258198-d6431d01-fdaf-47db-8b6a-e6828689e324.png)

Ngoài ra lệnh **cfgshow** cũng hiển thị cấu hình zone hiện tại

![image](https://user-images.githubusercontent.com/32956424/131258209-7fd89d41-44c8-4e1f-ab28-699690137832.png)

## Types of Zones

Có nhiều loại zone với chức năng khác nhau:
- Regular Zone
- Peer Zones
- Target Driven Zones
- QoS Zones
- LSAN Zones


### Peer Zones

Xuất hiện từ Fabric OS v7.4 trở lên, cho phép nhiều initiator và một target trong cùng một zone

Khác với Regular Zone, các initiator trong Peer Zone chỉ có thể kết nối với target

Trong Peer Zone, các zone membership được chia là 2 loại: principal member và peer member

Chỉ cho phép kết nối giữa principal member và peer member. 

Active config có thể bao gồm regular zone và peer zone

Trong single peer zone, một thiết bị chỉ có thể là principal member hoặc peer member

Event trên peer member chỉ được truyền đến principal member, không truyền đến peer member

Event trên principal member được truyền đến tất cả peer member, ko truyền đến principal member

Tuy nhiên chỉ có Fabric OS v7.4 trở lên là hỗ trợ Peer Zone, các OS đời thấp hơn sẽ coi Peer Zone là Regular Zone

Hầu hết thường dùng Regular Zone

### Target Drive Zones

Target Drive Zones cũng có đặc điểm như Peer Zone, như đều có principal member và peer member, hỗ trợ alias, WWN.

Tuy nhiên TDZ không thể tạo ra hay chỉnh sửa, mà nó được định nghĩa bởi principal device (thường là storage device). TDZ quản lý zoning bằng cách sử dụng giao diện của bên thứ ba 

TDZ không thể bị sửa hay tạo ra bởi SANnav hay các công cụ khác, tuy nhiên vẫn có thể xem chúng, hoặc xoá đi

TDZ mode phải được enable ở trên F_Port của switch mà principal device đang kết nối tới

### QoS Zones

Zone này ưu tiên traffic giữa host và target

![image](https://user-images.githubusercontent.com/32956424/131259706-521b2cd1-56b3-4e6a-bb94-4f6632461293.png)

Cấu hình QoS zone để chỉ định tốc độ high, medium hay low giữa từng cặp host/target

Mặc định các zone thông thường thì tốc độ là medium. Tuỳ theo tốc độ mà sử dụng % băng thông

- High: 60%
- Medium: 30%
- Low: 10%

### LSAN Zones

Là một zone bao gồm nhiều routed fabric

Cho phép các thiết bị ở rìa fabric có thể kết nối với nhau

![image](https://user-images.githubusercontent.com/32956424/131259861-72cb066d-8da4-4d80-acfb-4b72df596151.png)

## Zones Configuration Overview

### Process to implement Zoning

Quá trình zoning:

- Tạo sơ đồ chi tiết về topology của fabric
- Quy ước đặt tên
- Xác định từng thiết bị bằng PWWN, NWWN
- Set default zone
- Chọn loại zone
- Tạo alias, zone, zone config
- Loại trừ E_Port
- Review zone config (bằng CLI, SANnav,...)
- Enable zone config
- Verify

### Zone Management

Có thể quản lý zone bằng nhiều công cụ khác nhau như:
- CLI
- SANnav
- Web tools
- REST API


![image](https://user-images.githubusercontent.com/32956424/131260114-9714878f-d39a-45f0-b164-ac3645a17acb.png)

Các bước zoning
- Tạo alias
- Tạo zone
- Tạo config
- Enable config

Tạo alias là 1 bước tuỳ chọn, nhưng tạo alias giúp quản lý zone dễ dàng hơn, đỡ tốn công sức maintain

## Zoning with the CLI

Các lệnh liên quan đến alias, zone và zone config

![image](https://user-images.githubusercontent.com/32956424/131260247-9d932b28-d96b-4e71-82f6-0a543edfbf2c.png)

Lệnh delete sẽ xoá toàn bộ

Lệnh remove chỉ xoá một hoặc hoặc nhiều

#### Zoning sử dụng PWWN
- Cho phép thiết bị kết nối với thiết bị có WWN trong cùng zone
- Cần thay đổi zone khi WWN thay đổi
- Không cần thay đổi zone khi thiết bị cắm vào port khác

#### Zoning sử dụng domain, index
- Cho phép thiết bị kết nối với port được xác định
- Cần thay đổi zone khi thiết bị di chuyển sang port khác ngoài zone
- Không cần thay đổi zone khi WWN thay đổi

### Regular Zone with CLI

![image](https://user-images.githubusercontent.com/32956424/131260956-e532d007-a42e-42bb-95ac-68f6b3763d81.png)


![image](https://user-images.githubusercontent.com/32956424/131261043-bf6c0b36-cc5a-4090-8425-31814256b1cd.png)


![image](https://user-images.githubusercontent.com/32956424/131261031-011bfc52-b68c-4ba0-8ed3-c5575148a9c6.png)


Lệnh **cfgenable** sẽ lưu cấu hình vào bộ nhớ flash, đồng thời cấu hình cũng được phân phối cho toàn bộ switch trong fabric

### Peer Zone with CLI

![image](https://user-images.githubusercontent.com/32956424/131261051-46743725-6869-423f-b605-37ea3f8291f0.png)

![image](https://user-images.githubusercontent.com/32956424/131261060-a04b1add-7ce3-4a3f-a8b0-3bd484719f25.png)

## Zone Configuration Management

### Saving Zoning

Dùng lệnh **cfgsave** để lưu cấu hình zone  

Save chỉ lưu cấu hình chứ ko enable chúng

![image](https://user-images.githubusercontent.com/32956424/131261092-a803340d-6861-40cd-bfa6-d5d9716eba95.png)

Khi save thì cấu hình zone được lưu vào bộ nhớ flash của toàn bộ switch trong fabric

Nếu file cấu hình mà ko hơp lệ thì sẽ ko được lưu, đồng thời sẽ có thông báo lỗi hiện lên

### Enable a defined configuration

Lệnh **cfgenable [cfgName]** để enable zone config

Không nên disable zone config hiện tại trước khi enable config mới, vì như thế switch sẽ quay lại default zone. 

Nếu default zone ở mode allaccess, các thiết bị lại có thể nhìn tháy nhau

Zone config có thể hiển thị bằng lệnh **switchshow**

### Clear Zoning

Clear zone sẽ ko disable zone config hiện tại và không lưu bất cứ thông tin gì vào flash

Muốn xoá toàn bộ zone config, chạy lệnh theo thứ tự
- **cfgdisable**
- **cfgclear**
- **cfgsave**


## Zone Merging

Trước khi thêm switch mới vào Fabric:
- Switch mới không có zone
- Switch mới phải có cấu hình default zone giống với fabric
- Kết nối switch vào fabric
- Zone config của fabric sẽ có hiệu lực trên switch mới

![image](https://user-images.githubusercontent.com/32956424/131262012-e61122ed-4d25-4577-9d88-b3aae71a734d.png)

## Zoning Configuration Size

Là kích thước bộ nhớ flash dùng để lưu config

![image](https://user-images.githubusercontent.com/32956424/131262086-f45817a3-6338-4636-b147-238a1122f9d4.png)

Lệnh **cfgsize** để hiển thị size

![image](https://user-images.githubusercontent.com/32956424/131262108-bdacc6d6-f44d-4b8f-ad15-87fe35065975.png)


## Related Zoning Commands

![image](https://user-images.githubusercontent.com/32956424/131262140-32909199-a519-4b9d-ba47-b6809934eb4e.png)

Lệnh **nsaliasshow** hiển thị alias và server name

![image](https://user-images.githubusercontent.com/32956424/131262166-78379e93-30ba-464e-9328-428f6c0ae267.png)

Lệnh **nodefind [name]** tìm kiếm server phù hợp với WWN, PID hoặc alias cho trước

![image](https://user-images.githubusercontent.com/32956424/131262183-c129ef8a-259a-4450-91b0-dcd02acdd012.png)

Lệnh **nszonemember [WWN]** hiển thị thông tin của tất cả thiết bị zone với WWN hoặc PID cho trước

![image](https://user-images.githubusercontent.com/32956424/131262244-9153e053-252d-4fc4-83d4-a54ce2e89546.png)

Lệnh **nszonemember -u** hiển thị những thiết bị chưa được zone trong fabric 

![image](https://user-images.githubusercontent.com/32956424/131262264-42295276-d7e5-49b3-aa2b-70a9cc5b583d.png)



























