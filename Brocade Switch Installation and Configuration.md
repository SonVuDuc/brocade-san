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

Sử dụng lệnh **tsclockserver** để switch đồng bộ thời gian với NTP server. Câu lệnh **tsclockserver {IP NTP Server}**

Sử dụng lệnh **date** để set thời gian bằng tay cho switch, với cú pháp **date "mmddhhmmyy"**. Trong đó **mm** đầu tiên là tháng, **mm** thứ hai là phút.

Chạy lệnh **date** không đi kèm tham số sẽ hiển thị thời gian hiện tại trên switch

Không thể set thời gian bằng lệnh **date** nếu như lệnh **tsclockserver** đang hoạt động. Do đó nếu muốn dùng lệnh date phải disable tsclockserver bằng **tsclockserver LOCL**

#### Set Switch Time Zone

Sử dụng lệnh **tstimezone** để set time zone cho từng switch 

VD: **tstimezone +7**

Để hiển thị time zone menu, dùng lệnh **tstimzone --interactive**

#### Configure Zoning

Mặc định, khi không có cấu hình zone nào đang chạy, thì switch sẽ ở chế độ default zone. 

Default zone có 2 mode: no access và all access

Nên cấu hình switch về chế độ default no access trước khi thêm switch vào fabric

Vì nếu ở mode all access, khi thêm vào fabric thì các thiết bị kết nối với switch mới sẽ nhìn thấy được nhau và truy cập được dữ liệu. 

![image](https://user-images.githubusercontent.com/32956424/131251214-2f5fa37d-4fce-4107-98dc-abeeb1b0d558.png)


### Install optics and attach cables

#### Install SFPs and attach cables

Cài đặt module SFP

Cắm dây cáp 

![image](https://user-images.githubusercontent.com/32956424/131251240-f8dc08a1-a24e-471c-ae27-ee2a259ac5e7.png)

### Verify operation

#### Port Status LEDs

![image](https://user-images.githubusercontent.com/32956424/131251268-d6367615-e4f6-45b6-821a-3cee6726d6cb.png)


#### Verifying Switch Operation

Dùng lệnh **switchshow** để hiển thị thông tin, trạng thái của switch và port. Bao gồm:
- switchName: tên switch
- switchType: model
- switchState: trạng thái, có thể là Online, Offline, Testing, Faulty
- switchMode: chế độ của switch
- switchRole: vai trò của switch (Principal, Subordinate,...)
- switchDomain
- switchId 

![image](https://user-images.githubusercontent.com/32956424/131251715-0097ac59-ab6b-49eb-9352-691559a670e0.png)

#### Port Status

Để hiển thị trạng thái riêng của từng port, dùng lệnh: **portshow [port slot]**

![image](https://user-images.githubusercontent.com/32956424/131253268-db3edc25-7941-4989-9109-b225a4ac0aa7.png)

#### FRU Status Commands

Lệnh **sensorshow** để hiển thị nhiệt độ, tốc độ quạt và trạng thái nguồn trên switch 

![image](https://user-images.githubusercontent.com/32956424/131253333-bd8e060c-b299-47c0-8f92-4792e7f97c7b.png)

## Additional Switch Configuration Parameters

### Configuration Parameters

Bên cạnh nhưng tham số cơ bản, còn những tham số khác của switch cũng cần phải cấu hình

Sử dụng lệnh **configure** 

Nhiều tham số yêu cầu switch phải ở chế độ offline (disabled). Chạy lệnh **switchdisable; configure**

Khi muốn reset lại cấu hình gốc, dùng lệnh **switchdisable; configdefault**

Tuy nhiên một số tham số ko bị reset như:
- IP address, subnetmask, gateway, MAC address 
- Licsense key
- Product ID và vendor ID
- SNMP config
- System name
- Chassis name
- WWN
- Zoning config
- Username / password
- Ethernet link mode


### Fabric Name

Dùng lệnh **fabricname --set [FabricName]** để set fabric name cho switch

Lệnh này có thể chạy được cả online và offline mode

Có fabric name dễ dàng hơn trong quản lý fabric

### Set the chassis name

Dùng để phân biệt switch vật lý và switch logic trong Virtual Fabric mode

Lệnh **chassisname [ChassisName]**

### Set the switch name

Switch name có thể được trùng lặp trong fabric

Đặt tên cho switch giúp quản lý switch dễ hơn

Lệnh **switchname [SwitchName]**

### Configure Registered Organization Name (RON)

Dùng lệnh **ron -set [Name]** để set Organization Name cho switch

RON chỉ có thể được set bởi customer và không thể bị thay đổi

Tính năng này chỉ có trên director

Để hiển thị RON, dùng lệnh **ron --show**

### Set banner

Có 2 loại banner:
- Message of the day (MOTD): banner này hiển thị trước khi login và set bằng lệnh **motd --set "Please login"**. Hiển thị bằng lệnh **motd --show**
- Login banner: banner này hiển thị sau khi login thành công và set bằng lệnh **bannerset "Welcome"**. Hiển thị bằng lệnh **bannershow**

### Port Speed

Có thể cấu hình tốc độ cụ thể cho từng port trên switch với lệnh: **portcfgspeed [slot]portspeed**

Các tốc độ của port: 
- 0 - auto negotiate
- 4 - 4Gbps
- 8 - 8Gbps
- 10 - 10Gbps
- 16 - 16Gbps
- 25 - 25Gbps
- 32 - 32Gbps
- 40 - 40Gbps
- 53 - 53Gbps
- 64 - 64Gbps

Lệnh **switchshow** và **portshow** cũng sẽ hiển thị tốc độ port

### Configuring 10Gbps Fibre Channel

Để set tốc độ 10Gbps cho port, cần cấu hình octet combo

- 1: auto negotiate hoặc tốc độ cố định 32, 16, 8 và 4 Gbps (Gen 6)
- 2: auto negotiate hoặc tốc độ cố định 10, 8 và 4 Gbps (Gen 6)
- 3: auto negotiate hoặc tốc độ cố định 16, 10 Gbps (Gen 5)

Port phải được disabled trước khi set. 

Set combo cho port bằng lệnh **portcfgoctetspeedcombo [slot/]port combo**

### Port setting and Commands

Dùng lệnh **portshow | more** để hiển thị chi tiết cấu hình của port

![image](https://user-images.githubusercontent.com/32956424/131254786-025d5a60-1803-4043-b474-e9e021867c18.png)

### Enabling / Disabling Ports

Lệnh **portdisable [slotnumber/]port1-port2** để disable port

Lệnh **portenable [slotnumber/]port1-port2** để disable port

Có thể enable/disable một khoảng port bằng: **portenable [slotnumber/]port1-port2  [slotnumber/]port3-port4**

### PortName

Thay đổi port name bằng lệnh: **portname [portnumber] -n [portName]**

Cấu trúc portname default của switch và director

![image](https://user-images.githubusercontent.com/32956424/131255578-184ef304-b76a-42b6-a208-1ae8d60326db.png)

Dùng lệnh **portcfgdefault** để reset port name về mặc định

### Set Syslog server

Lệnh **syslogadmin [IP server]** để cấu hình gửi log về server

Gắn tag facility cho syslog thì dùng lệnh **syslogadmin --set -facility [name]**

## Configuration Backup

### Configuration File Overview

File config của switch được chia làm 3 section
- Header
- Chassis
- Switch (bao gồm switch và logic switch)

![image](https://user-images.githubusercontent.com/32956424/131256449-93510c1d-b9ea-4b12-9550-ddd7b70980e8.png)


### Chassis section

Chỉ có duy nhất một chassic section trong một file config

![image](https://user-images.githubusercontent.com/32956424/131256500-5562000c-9304-4e11-8ea5-00a53ae339d2.png)


### Switch section

Luôn có ít nhất một switch section chứa:
- Switch vật lý khi VF bị disable
- Default logical switch khi VF enable

Nếu có nhiều logcial switch thì sẽ có thêm switch section

Switch section chứa thông tin về:
- Switch name
- Fabric ID
- Boot parameter
- Config
- Zoning
- Banner
- ...

### Configuration Backup Interactive

Chạy lệnh **configupload** để upload file config đến server 

![image](https://user-images.githubusercontent.com/32956424/131256883-b0becdb4-d359-48c5-9fa7-df4f6e90d3fa.png)

Download config logical switch từ server bằng lệnh **configdownload -ftp [IP], user, [fileName].txt, password**

### Backup and Restore 

- Backup
  - Upload config với lệnh **configupload -vf**. Chỉ dùng lệnh này khi switch có cấu hình VF
  - Sau đó upload bình thường bằng lệnh **configupload -all**

- Restore
  - Disable switch bằng lệnh **switchDisable**
  - Download VF config với lệnh **configdownload -vf**
  - Download config thường bằng lệnh **configdownload -all**

































































