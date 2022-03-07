<h1> Định tuyến</h1>

<h2> Mục lục </h2>

- [Tổng quan](#tổng-quan)
  - [Khái niệm](#khái-niệm)
  - [Phân loại định tuyến](#phân-loại-định-tuyến)
    - [Định tuyến tĩnh - static routing](#định-tuyến-tĩnh---static-routing)
    - [Định tuyến động](#định-tuyến-động)
      - [Distance vector](#distance-vector)
      - [Link state](#link-state)
      - [Ngoài ra](#ngoài-ra)
        - [Classfull routing protocol](#classfull-routing-protocol)
        - [Classless routing protocol](#classless-routing-protocol)
    - [Các tham số quan trọng](#các-tham-số-quan-trọng)
  - [Cấu hình định tuyến động - distance vector](#cấu-hình-định-tuyến-động---distance-vector)
    - [RIP](#rip)
      - [RIPv1](#ripv1)
      - [RIPv2](#ripv2)
    - [OSPF](#ospf)

# Tổng quan
## Khái niệm

- Định tuyến là chức năng của router giúp xác định quá trình tìm đường đi cho các gói tin từ nguồn tới đích thông qua hệ thống mạng.
- Để xác định đường đi cho các gói tin, Router dựa vào:
  - Địa chỉ IP đích (Destination IP)
  - Bảng định tuyến (routing table)

## Phân loại định tuyến
Có 2 loại định tuyến:
- Định tuyến tĩnh
- Định tuyến động
### Định tuyến tĩnh - static routing
- Là loại định tuyến mà trong đó router sử dụng các tuyến đường đi tĩnh để vận chuyển dữ liệu đi.
- Do người quản trị cấu hình thủ công vào router.
- Cú pháp:
```
R(config)#ip route <destination-net> <subnet-mask> <NextHop|Outport>
```

Trong đó:
- destination-network: là địa chỉ mạng cần đi tới
- subnet-mask: subnet-mask của destination-network
- next-hop: địa chỉ IP của router kế tiếp kết nối trực tiếp với router đang xét.
- OutPort: cổng của router đang xét mà packet sẽ đi ra.

**Default route**
- Nằm ở cuối bảng định tuyến.
- Sử dụng để gửi các gói tin đi trong trường hợp mạng đích không tìm thấy trong bảng định tuyến.

Ví dụ:

![Imgur](https://i.imgur.com/K7Kd7Zl.png)

Định tuyến:
- Next Hop:
```
R1(config)#ip route 0.0.0.0 0.0.0.0 203.113.115.1
```
- Hoặc Outport:
```
R1(config)#ip route 0.0.0.0 0.0.0.0 S0/0/0
```

### Định tuyến động
- Là loại định tuyến mà trong đó router sử dụng các tuyến đường đi động để vận chuyển dữ liệu
- Các tuyến đường này do các router sử dụng các giao thức định tuyến để trao đổi thông tin định tuyến với nhau.
- Một số giao thức định tuyến phổ biến:
  - RIP
  - OSPF
  - BGP
  - ...
- Giao thức định tuyến động chia làm 2 loại:
  - Distance-route
  - link-state


#### Distance vector
![Imgur](https://i.imgur.com/nSgCJyV.png)
- Giao thức thuộc loại này: RIP,...
- Các router định tuyến:
  - thực hiện gửi định kỳ toàn bộ bảng định tuyến của mình và gửi cho các router láng giềng kết nối trực tiếp với mình.
  - Không biết được đường đi đến đích một cách cụ thể
  - Không biết về các router trung gian trên đường đi
  - Không biết cấu trúc kết nối giữa các router
- Bảng định tuyến là nơi lưu kết quả chọn đường tốt nhất của mỗi router.
=> Khi các router trao đổi bảng định tuyến với nhau, chúng chọn đường dựa trên kết quả đã chọn của router láng giềng.
- Mỗi router nhìn hệ thống mạng theo sự chi phối của các router láng giềng.
- Nhược điểm: Tốn nhiều băng thông (do thực hiện cập nhật thông tin định tuyến theo định kỳ)

#### Link state
![Imgur](https://i.imgur.com/YbylhGg.png)

- Giao thức thuộc loại này: OSPF, IS-IS
- Các router định tuyến:
  - sẽ trao đổi các LSA (Link state advertisement) với nhau để xây dựng và duy trì CSDL về trạng thái các đường liên kết.
  - Các thông tin trao đổi được gửi dưới dạng Multicast
  - Nó không thực hiện cập nhật định tuyến định kỳ mà chỉ khi có sự thay đổi xảy ra. => Nhanh, tốn ít băng thông.
  - Hỗ trợ CIDR, VLSM nên là lựa chọn tốt cho các mạng lớn và phức tạp. => Đòi hỏi dung lượng bộ nhớ lớn và khả năng xử lý mạnh của CPU router.

- Để database luôn cập nhật thông tin mới: Trong LSA được đánh thêm chỉ số `sequense`:
  - Được bắt đầu từ giá trị `initial` đến `Max-age`.
  - Khi một router tạo ra 1 LSA, `sequense`=`initial`.
  - Giá trị `sequense` càng cao thì LSA update càng mới (VÌ khi gửi ra 1 phiên bản LSA update khác, `sequense` + 1).
  - Nếu `sequense`=`Max-age`, router sẽ flood (Ngập lụt) LSA ra cho tất cả ác router còn lại, sau đó set về `initial`.


#### Ngoài ra
Còn được phân thành 2 loại:
- Classfull routing protocol
- Classless routing protocol

##### Classfull routing protocol
- Trong các gói tin cập nhật (routing update) nhóm classfull không quảng bá: subnet-mask cùng với địa chỉ đích. Router phải lấy giá trị network-mask mặc định có cùng với địa chỉ lớp mạng của IP đích.
- Nếu  địa chỉ đích:
  - Kết nối trực tiếp với route, network-mask được lấy trên interface kết nối đến mạng đó.
  - Không nối trực tiếp (disconnected), sẽ lấy địa chỉ subnetmask default của địa chỉ đích.

##### Classless routing protocol
- Trong các gói tin cập nhật định tuyến, nhóm classless sẽ quảng bá subnet-mask cùng với địa chỉ đích.

### Các tham số quan trọng
- Metric:
  - sử dụng để chọn đường đi tốt nhất cho việc định tuyến.
  - All protocol cũng phải dùng để tính toán đường đi đến mạng đích.
  - Khi có nhiều đường đi đến đích, đường có `Metric` thấp sẽ được đưa vào bảng định tuyến.
  - Mỗi giao thức định tuyến có 1 kiểu `metric` khác nhau.

- AD:
  - Administrative Distance
  - Là giá trị quy ước dùng để chỉ độ tin cậy của giao thức định tuyến.
  - Giao thức có AD nhỏ hơn được xem là đáng tin cậy hơn.
  - Giao thức định tuyến nào có AD nhỏ nhất sẽ được lựa chọn và đưa vao bảng định tuyến.

## Cấu hình định tuyến động - distance vector

### RIP
- Là 1 giao thức định tuyến theo kiểu distance vector
- Sử dụng `Hop count` (đếm số router) làm `metric` cho việc chọn đường (có nhiều đường cùng đến 1 đích thì chọn đường có `hop count` ít nhất).
- Nếu `hop count > 15` thì packet bị loại bỏ
- thời gian update: 30s (mặc định)
- AD : 120
- Có 2 phiên bản: RIPv1 và RIPv2

#### RIPv1
- Là 1 giao thức định tuyến theo kiểu **distance-vector** và theo lớp **classfull routing protocol**.
- `Metric` là `hop-count`.
- Chu kỳ cập nhật: 30s
- `Hop-count` max : 15
- Không hỗ trợ:
  - VLSM
  - mạng không liên tục (discontigous network)

Câu lệnh cấu hình:
```
Router(config)#router rip
Router(config-router)#network network_number
```

#### RIPv2
- Là phiên bản cải tiến của RIPv1
- Là giao thức định tuyến dạng classless (có gửi thông tin subnet-mask qua cập nhật định tuyến)
- Hỗ trợ:
  - VLSM
  - Chứng thực trong các routing update
- Cập nhật định tuyến dạng Multicast, sử dụng địa chỉ lớp D 224.0.0.9

- `Metric`: hop-count

Cấu hình RIPv2:
```
R(config)#router rip
R(config-router)#version 2
R(config-router)#network network-number
```
Trong đó: `network-number` là địa chỉ mạng láng giềng

**Bảng so sánh**
|Đặc điểm|RIPv1|RIPv2|
|---|---|---|
Loại định tuyến|Classfull|Classless|
Hỗ trợ VLSM và mạng không liên tục|không|có|
gửi kèm subnet-mask trong bản tin routing update|không |có|
Quảng bá thông tin định tuyến |Broadcast|Multicast|
Hỗ trợ chứng thực|không|có|
Định nghĩa trong RFC|RFC 1058|RFC 1721, 1722, 2453|

- Mạng không liên tục (discontiguous network) là mạng mà trong đó các mạng con (subnet) của cùng một mạng lớn (major network - mạng theo đúng lớp) bị ngăn cách bởi "major-network" khác.

![Imgur](https://i.imgur.com/frARUQO.png)

Trong hình: 2 mạng con lớp B bị ngăn cách bởi mạng con lớp C.

**Chứng thực trong RIPv2**
- Chứng thực trong định tuyến là cách thức bảo mật trong việc trao đổi thông tin định tuyến giữa các router.
- Các router phải vượt qua quá trình chứng thực này trước khi trao đổi định tuyến được thực hiện.
- RIPv2 hỗ trợ 2 kiểu định tuyến:
  - `Plain text`
  - `MD5`

- Chứng thực `Plain text` (hay Clear text)
  - các router được cấu hình 1 khoá (password) và trao đổi chúng để so khớp.
  - password được gửi dưới dạng không mã hoá.
  - Cấu hình:
    - Bước 1: Tạo bộ khoá
    ```
    R(config)#key chain <name>
    ```
    - Bước 2: Tạo các khoá
    ```
    R(config-keychain)#key <key-id>
    R(config-keychain)#key-string <password>
    ```
    - Bước 3: Áp đặt vào các cổng gửi chứng thực
    ```
    R(config)#interface <interface>
    R(config-if)#ip rip authentication key-chain <name>
    ```
- Chứng thực dạng `MD5`:
  - Password được mã hoá khi gửi đi (an toàn hơn).
  - Cấu hình tương tự `Plain text`
  - Bước 3 thêm dòng lệnh:
  ```
  R(config-if)#ip rip authentication mode md5
  ```

**Các lệnh kiểm tra cấu hìnu**
```
R#debug ip rip
R#show ip route
```

### OSPF
- Open Shortest Path First
- Định tuyến dạng `link-state`
- Sử dụng thuật toán Dijkstra `Shortest Path Firs(SPF)` để xây dựng bảng định tuyến.

- Ưu điểm:
  - Hội tụ nhanh
  - hỗ trợ được mạng có kích thước lớn
  - Không xảy ra routing loop.
  - Định tuyến dạng classless nên hỗ trợ VLSM và mạng không liên tục.
  - Sử dụng địa chỉ Multicast 224.0.0.5 và 224.0.0.6 (DR và BDR router) để gửi các thông điệp `hello` và `update`.

- Sử dụng area để giảm yêu cầu về CPU, memory của OSPF router và lưu lượng định tuyến.
- Hỗ trợ chứng thực `Plain text` và `MD5`.
- `Metric` : cost
  - Co
