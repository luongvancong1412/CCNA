# Cấu hình định tuyến động - distance vector

## RIP
- Là 1 giao thức định tuyến theo kiểu distance vector
- Sử dụng `Hop count` (đếm số router) làm `metric` cho việc chọn đường (có nhiều đường cùng đến 1 đích thì chọn đường có `hop count` ít nhất).
- Nếu `hop count > 15` thì packet bị loại bỏ
- thời gian update: 30s (mặc định)
- AD : 120
- Có 2 phiên bản: RIPv1 và RIPv2

### RIPv1
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

### RIPv2
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

## OSPF
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