# Chapter1: Internet

Kiến thức cơ bản về mạng

- [Chapter1: Internet](#chapter1-internet)
    - [Truyền thông là gì? - Communication](#truyền-thông-là-gì---communication)
    - [Chất lượng truyền thông](#chất-lượng-truyền-thông)
    - [Các dịch vụ viễn thông truyền thống](#các-dịch-vụ-viễn-thông-truyền-thống)
    - [Planning for the Future](#planning-for-the-future)
  - [Thành phần của Network](#thành-phần-của-network)
    - [End devices](#end-devices)
    - [Network Infrastructure Devices](#network-infrastructure-devices)
    - [Network Media](#network-media)
    - [Network Topology Diagrams - Mô hình mạng](#network-topology-diagrams---mô-hình-mạng)
  - [Types of Network](#types-of-network)


- Internet có từ năm 1995, về VN từ 1997
- Internet of Everything: Tất cả mọi thứ đều kết nối với nhau Internet.

### Truyền thông là gì? - Communication
Nguyên tắc truyền thông:
- Xác định được bên gửi và bên nhận
  - Muốn giao tiếp thì phải có người nói người nghe
- Chấp nhận giao thức truyền thông (face to face, telephone, letter)
  - Cần phương thức truyền tải: ví dụ khi nói chuyện chúng ta phải có không khí để truyền âm thanh đi
  - Thống nhất được giao thức truyền thông: cáp đồng, cáp quang...
- Thiết lâp quy tắc chung: format,...
  - Ngôn ngữ và ngữ pháp chung
  - Thống nhất về ngôn ngữ: ví dụ cùng dùng Tiếng Việt
- Kích cơ của thông điệp
  - Khi chuyển nhà, có cái ngõ rất hẹp, mà rất nhiều lần ta phải chuyển nhiều lần hoặc thuê xe vừa với cái ngõ.
  - Trong truyền thông trên thiết bị nguồn, thông điệp sẽ được tách ra thành mảnh nhỏ, đảm bảo đc không gian xử lý của các thiết bị nhận, bên nhận sẽ tái cấu trúc lại thành thông điệp ban đầu.
- Message Timing: 3 nguyên tắc
  - Access Method: Khi gọi điện không thể 2 bên cùng nói đc, k ai sẽ nghe được bên kia nói gì, nên kẻ nói phải có người nghe=> nó là phương thức quyết định xem thiết bị nào sẽ bắt đầu phiên truyền và gửi trước.
  - Flow Control: hình dung giống như tốc độ nói, nói với tốc độ nhanh người nghe cũng không kịp xử lý. Trong không gian mạng cũng vậy.
  - Response Timeout: Chờ lâu quá k trả lời, tbi sẽ gửi lại...
- Cơ chế xác nhận khi truyền
  - Ví dụ gửi đi bên kia nhận được sẽ trả lời: anh ơi em nhận được rồi.

### Chất lượng truyền thông

Yếu tố tác động đến chất lượng:
1. **Tác động bên ngoài:**
- Tuyến đường giữa bên gửi và bên nhận
  - Khi đi từ A-B ta có rất nhiều 
- Số lần bản tin thay đổi cấu trúc dữ liệu khi truyền
  - Khi đi từ A-B phải trải qua các đoạn đường khác nhau với cấu trúc khác nhau(hay môi trường)
- Số lần bản tin chuyển hướng khi truyền
  - Đi từ quê ra hn: đi cao tốc- ok; đi trong thành phố: đèn, rẽ trái phải,...
  - Quá nhiều hop, route,... nó cũng ảnh hưởng đến quality
- Nhiều bản tin cùng truyền đồng thời trên đường truyền.
- Thời gian truyền
  - Thời gian dài cũng ảnh hưởng, ví dụ như độ trễ,...
2. **Tác động bên trong:**
- Kích thước bản tin
  - Kích thước lớn thì độ rớt cũng cao
- Độ phức tạp (complexity) của bản tin (message)
  - Càng phức tạp, mã hoá nhiều cũng ảnh hưởng đến chất lượng truyền thông
- Độ quan trọng (importance) của bản tin

### Các dịch vụ viễn thông truyền thống
3 hạ tần mạng viễn thông:
- Computer Networks - mạng máy tính
- Telephone Networks - mạng điện thoại cố định
- Broadcast Networks - mạng ti vi
### Planning for the Future
Gộp chung lại 3 mạng truyền thống thành mạng Internet (mạng IP)

## Thành phần của Network
- End devices - Thiết bị đầu cuối
- Intermediary devices - Các thiết bị trung gian
- Network media - Môi trường mạng

### End devices
- là các thiết bị đầu cuối tham gia các kết nối vào mạng:
  - Máy tính
  - Máy in
  - Camera bảo mật
  - Các thiết bị cầm tay (smartphones,...)
### Network Infrastructure Devices
- Network Access Devices (switches) - kết nối thiết bị với mạng
- Internetwork Devices (routers) - kết nối các các mạng
- Security Devices (firewalls)
### Network Media
- Copper: cáp đồng
- Fiber Optic: cáp quang
- Wireless
### Network Topology Diagrams - Mô hình mạng
Có 2 loại Topology:
- Physical Topology: mô tả chi tiết thiết kế, thi công các thiết bị mạng kết nối với nhau như thế nào.
- Logical Topology: mô tả các thành phần mạng kết nối với nhau.

## Types of Network
Cơ sở hạ tần mạng phổ biến:
- Local Area Network (LAN)
  - Hệ thống mạng kết nối các máy tính trong cùng 1 công ty, doanh nghiệp trong hạ tầng switch
- Wide Area Network (WAN)
  - là hệ thống mạng  nội bộ kết nối giữa các chi nhánh của 1 công ty, tổ chức nhưng ở vị trí địa lý khác nhau.

Một số loại mạng khác:
- Metropolitan Area Network(MAN)
  - là hệ thống mạng đô thị:
    - Máy tính kết nối modem
    - modem kết nối tới các nhà mạng
    - Thiết bị thu gom (OLT) của nhà mạng (Viettel,..) gom các kết nối từ modem
    - Các OLT gom lên Switch (có rất nhiều switch kết nối với nhau)
    - gom các switch thành mạng Metro area network (63 tỉnh thì 63 cái metro)
    - Mỗi Metro area network gom tới các Router Biên (PE - Provider Edge)
    - Router Biên Gom tới Router Core (Route P - kết nối trục bắc nam) (route trung tâm) của nhà mạng
    - Từ router core của nhà cung cấp dịch vụ kết nới tới nhà cung cấp khác trên toàn cầu (các nhà cung cấp khác nhau qua cáp quang chung- cáp quang trên biển)
    - Server đặt ở IDC (Trung tâm dữ liệu)
    - IDC kết nối tới PE,...
- Wireless LAN (WLAN)
  - Là hệ thống mạng không dây kết nối bằng cách truyền sóng điẹn từ trong không gian
- Storage Area Network (SAN)
  - Là hệ thống kết nối máy chủ với hệ thống lưu trữ dữ liệu tập trung. (Chỉ có trong trung tâm dữ liệu)