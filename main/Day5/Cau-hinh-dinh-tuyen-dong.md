<h1> Cấu hình định tuyến động - distance vector </h1>

<h2> Mục lục </h2>

- [I. RIP](#i-rip)
  - [1. RIPv1](#1-ripv1)
  - [2. RIPv2](#2-ripv2)
  - [3. Bảng so sánh RIPv1,v2](#3-bảng-so-sánh-ripv1v2)
  - [4. Chứng thực trong RIPv2](#4-chứng-thực-trong-ripv2)
  - [5. Các lệnh kiểm tra cấu hình](#5-các-lệnh-kiểm-tra-cấu-hình)
- [II. OSPF](#ii-ospf)
  - [1. Quá trình xây dựng bảng định tuyến (hoạt động của OSPF)](#1-quá-trình-xây-dựng-bảng-định-tuyến-hoạt-động-của-ospf)
    - [Bước 1: Thiết lập neighbor (hàng xóm)](#bước-1-thiết-lập-neighbor-hàng-xóm)
    - [Bước 2: Cập nhật thông tin định tuyến](#bước-2-cập-nhật-thông-tin-định-tuyến)
  - [2.Cấu hình OSPF](#2cấu-hình-ospf)
  - [3. Chứng thực trong OSPF:](#3-chứng-thực-trong-ospf)
    - [3.1 Plain text](#31-plain-text)
    - [3.2 MD5](#32-md5)
- [III. EIGRP](#iii-eigrp)
  - [1. Các tính Metric của EIGRP](#1-các-tính-metric-của-eigrp)
  - [2. Hoạt động](#2-hoạt-động)
  - [3. EIGRP chống "routing loop"](#3-eigrp-chống-routing-loop)
  - [4. Cấu hìnhh EIGRP](#4-cấu-hìnhh-eigrp)
  - [5. Chứng thực trong EIGRP](#5-chứng-thực-trong-eigrp)
  - [6. Ví dụ](#6-ví-dụ)
- [IV. Redistribution giữa các giao thức định tuyến](#iv-redistribution-giữa-các-giao-thức-định-tuyến)
- [Tài liệu tham khảo](#tài-liệu-tham-khảo)

# I. RIP
- Là 1 giao thức định tuyến theo kiểu distance vector
- Sử dụng `Hop count` (đếm số router) làm `metric` cho việc chọn đường (có nhiều đường cùng đến 1 đích thì chọn đường có `hop count` ít nhất).
- Nếu `hop count > 15` thì packet bị loại bỏ
- thời gian update: 30s (mặc định)
- AD : 120
- Có 2 phiên bản: RIPv1 và RIPv2

## 1. RIPv1
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

## 2. RIPv2
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

## 3. Bảng so sánh RIPv1,v2
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

## 4. Chứng thực trong RIPv2
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

## 5. Các lệnh kiểm tra cấu hình
```
R#debug ip rip
R#show ip route
```

# II. OSPF
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
  - Cost của toàn tuyến được tính theo cách cộng dồn cost dọc theo tuyến đường đi của packet.
  - Được tính dựa trên băng thông sao cho tốc độ kết nối của đường link càng cao thì cost càng thấp
  - `cost =10^8/bandwidth`. Trong đó: bandwidth được cấu hình trên interface với đơn vị bps.
  - có thể thay đổi giá trị cost.
  - Nếu router có nhiều đường đến đích mà cost bằng nhau thì router sẽ cân bằng tải trên các tuyến đường đó (tối đa là 16 đường).

## 1. Quá trình xây dựng bảng định tuyến (hoạt động của OSPF)
Khi các Router OSPF bật lên:

### Bước 1: Thiết lập neighbor (hàng xóm)

- Gửi các bản tin `hello` (với nhau) để thiết lập mối quan hệ hàng xóm
- Bản tin `hello`:
  - Được gửi theo chu kỳ 10s/lần
  - Gửi đến địa chỉ Multicast dành riêng cho OFPF là 224.0.0.5 đến tất cả router chạy OSPF
  - Mục đích: tìm kiếm hàng xóm, thiết lập và duy trì mối quan hệ này.
  - Chứa các điều kiện router thiết lập mối quan hệ láng giềng bao gồm:
    - Router_ID: Giá trị định danh duy nhất cho Router khi chạy OSPF. Có định dạng của 1 địa chỉ IP (A.B.C.D)
    - Area-ID:
      - Có giá trị từ 0-65535
      - Được định nghĩa các miền Router chạy OSPF
      - Các Router trong cùng miền OSPF mới có thể thiết lập mối quan hệ với nhau.
      - Area 0: back bone area
    - Subnetmask: 2 router cùng kết nối với nhau trên cùng subnetmask
    - Tham số xác thực(nếu có)
> Điều kiện bắt buộc để kết nối với nhau:
> - Tham số xác thực giống nhau
> - Router-id khác nhau
> - Subnet mask giống nhau
> - ...

- Hiện thị thông tin hàng xóm:
```
R#Show ip OSPF neighbor
```

### Bước 2: Cập nhật thông tin định tuyến
- Các router gửi các bản tin (link state update) gửi tới 224.0.0.5
- Khi Router nhận được bản tin
  - phản hồi lại bằng bản tin LSACK - Link state ACK, đồng thời tạo bản sao gửi cho các router còn lại trong miền OSPF
  - Đồng bộ cơ sở dữ liệu
- Để hiển thi CSDL:
```
R#Show ip OSPF database
```

## 2.Cấu hình OSPF
- Khởi tạo tiến trình định tuyến OSPF:
```
R(config)# router ospf <process-id>
```
- Chọn cổng tham gia voà quá trình trao đổi thông tin định tuyến
```
R(config-router)#network <address> <wildcard-mask> area <area-id>
```

Trong đó:
- Process-id: chỉ số tiến trình của OSPF (giá trị từ 1-65535)
- Address: địa chỉ cổng tham gia định tuyến
- Wildcard mask: điều kiện kiểm tra giữa địa chỉ cầu hình trong address và địa chỉ các cổng trên router, tương ứng bít 0-phải so khớp, bit 1- không cần kiểm tra.
- Area-id: vùng mà cổng tương ứng thuộc về trong miền OSPF

Ví dụ:

![Imgur](https://i.imgur.com/Ej9fpjI.png)

- Các câu lệnh kiểm tra cấu hình OSPF:
```
R# show ip protocol
R# show ip route
R# show ip ospf interface
R# show ip ospf neighbor
R# debug ip ospf events
R# debug ip ospf packet
```

## 3. Chứng thực trong OSPF:
- Tương tự RIP cũng hỗ trợ 2 dạng:
  - Plain text
  - MD5
### 3.1 Plain text
- Cấu hình 2 cổng của 2 router nối trực tiếp với nhau để chứng thực giữa chúng trước khi trao đổi thông tin định tuyến
- Mật khẩu gửi đi không được mã hoá

```
R(config)#interface <interface>
R(config-if)#ip ospf authentication
R(config-if)#ip ospf authentication-key <password>
```

### 3.2 MD5
- Trên cổng của router gửi thông tin chứng thực, cấu hình:
```
R(config)#interface <interface>
R(config-if)#ip ospf authentication message-digest
R(config-if)#ip ospf messages-digest-key 1 md5 <password>
```

# III. EIGRP

EIGRP
- Là giao thức định tuyến do Cisco tạo ra
- Chỉ hoạt động trên thiết bị của Cisco
- là 1 giao thức định tuyên lai
  - Vừa mang đặc điểm của "distance vector"
  - Vừa mạng một số đặc điểm của "link-state"
- là dạng định tuyến "classless"
- Hỗ trợ VLSM và CIDR nên sử dụng hiệu quả không gian địa chỉ
- Sử dụng địa chỉ Multicast (224.0.0.10) để trao đổi thông tin định tuyến.

## 1. Các tính Metric của EIGRP

![Imgur](https://i.imgur.com/GSxVcoA.png)

- Với K1, K2, K3, K4, K5 là hằng số
- Mặc định: K1=1, K2=0, K3=1, K4=0, K5=0

Do đó, ta có:

`metric = bandwith + delay`

## 2. Hoạt động
- Các router tìm kiếm các láng giềng của nó và lưu giữ trong **neighbor table**.
- Mỗi router sẽ trao đổi các thông tin về cấu trúc mạng với các láng giềng của nó , lưu trữ vào cơ sở dữ liệu về cấu trúc mạng (topology table)

- Router chạy thuật toán DUAL với cơ sở dữ liệu đã thu thập được ở bước trên để tính toán tìm ra đường đi tốt nhật đến mỗi một mạng trong cơ sở dữ liệu.
- Router đặt các đường đi tốt nhất đến mỗi mạng đích vào bảng định tuyến.


Trong EIGRP có hai tuyến ta cần quan tâm là “successor route” và “fessible successor route”
- `Successor route`: 
  - Là tuyến đường đi chính được sử dụng để chuyển dữ liệu đến đích
  - Được lưu trong bảng định tuyến. 
  - EIGRP cho phép chia tải tối đa trên 16 đường (mặc định là 4 đường) đến mỗi mạng đích.
- Fessible successor route: 
  - Là đường đi dự phòng cho đường đi chính
  - Được lưu trong bảng cấu trúc mạng (topology table).

## 3. EIGRP chống "routing loop"
- “Routing loop” là một trở ngại rất lớn trong các giao thức định tuyến dạng “distance vector”. 
- Các giao thức định tuyến dạng “link-state” vượt qua vấn đề này bằng cách mỗi router đều nắm giữ toàn bộ cấu trúc mạng. 
- Trong giao thức EIGRP, khi tuyến đường đi chính gặp sự cố, router có thể kịp thời đặt đường đi dự phòng vào bảng định tuyến đóng vai trò như đường đi chính.
- Trường hợp không có đường đi dự phòng, sử dụng thuật toán DUAL cho phép router gửi các yêu cầu và tính toán lại các đường đi đến đích.

## 4. Cấu hìnhh EIGRP
- **Bước 1.** Kích hoạt giao thức định tuyến EIGRP
```
Router(config)#router eigrp <autonomous-system>
```
Trong đó: `autonomous-system`: có giá trị từ 1 đến 65535, giá trị này phải giống nhau ở tất cả các router trong hệ thống chạy EIGRP
- **Bước 2.** Chọn cổng tham gia vào quá trình trao đổi thông tin định tuyến
```
Router(config-router)#network <network-number>
```
Trong đó: `network-number` là địa chỉ cổng theo đúng lớp mạng của nó.

Để quảng bá các mạng con và hỗ trợ mạng không liên tục, chúng ta phải sử dụng lệnh sau:
```
Router(config-router)#no auto-summary
```

## 5. Chứng thực trong EIGRP
Chỉ hỗ trợ MD5
Cac bước cấu hình:
```
R(config)#key chain <keychain>
R(config-keychain)#key <key-id>
R(config-keychain-key)#key-string <password>

R(config)#interface <interface>
R(config-if)#ip authentication mode eigrp <AS> md5
R(config-if)#ip authentication key-chain eigrp <AS> <keychain>
```

## 6. Ví dụ
Cấu hình chứng thực cho giao thức định tuyến EIGRP giữa hai router R1 và R2.

![Imgur](https://i.imgur.com/XGVjHKY.png)

**Các bước cấu hình:**
- Cấu hình cơ bản: hostname, địa chỉ IP cho các cổng trên các router.
- Cấu hình định tuyến EIGRP AS 100
```
R1(config)#router eigrp 100
R1(config-if)#network 192.168.12.0
R1(config-if)#network 172.16.0.0
R1(config-if)#no auto-summary
R2(config)#router eigrp 100
R2(config-if)#network 192.168.12.0
R2(config-if)#network 192.168.10.0
R2(config-if)#no auto-summary
```
- Cấu hình chứng thực
```
R1(config)#key chain my_keychain1
R1(config-keychain)#key 1
R1(config-keychain-key)#key-string cisco

R1(config)#interface fa0/0
R1(config-if)#ip authentication mode eigrp 100 md5
R1(config-if)#ip authentication key-chain eigrp 100 my_keychain1

R2(config)#key chain my_keychain2
R2(config-keychain)#key 1
R2(config-keychain-key)#key-string cisco

R2(config)#interface fa0/0
R2(config-if)#ip authentication mode eigrp 100 md5
R2(config-if)#ip authentication key-chain eigrp 100 my_keychain2
```
- Kiểm tra cấu hình: Dùng các lệnh sau
```
show ip eigrp neighbors
show ip eigrp interfaces details
show key chain
```

# IV. Redistribution giữa các giao thức định tuyến
- Nếu một hệ thống mạng chạy nhiều hơn một giao thức định tuyến, người quản trị cần một vài phương thức để phân phối các đường đi của một giao thức này vào một giao thức khác. 
- Quá trình đó gọi là phân phối giữa các giao thức định tuyến (redistribution).

![Imgur](https://i.imgur.com/SAPZILc.png)

- Mỗi giao thức định tuyến có cách tính toán “metric” khác nhau, do đó khi thực hiện quá trình phân phối thì dạng `metric` sẽ được chuyển đổi sao cho phù hợp với giao thức định tuyến đó để các giao thức đó có thể quảng bá các đường đi cho nhau.

Phân phối định tuyến giữa RIP và OSPF: Quảng bá các tuyến học được từ OSPF vào RIP
```
Router(config)#router rip
Router(config-router)#redistribute ospf 1 metric <number>
```
# Tài liệu tham khảo

1. https://drive.google.com/file/d/1-1vF6vYwSv9J-SrHjqtXsllXJqjvknTe/view?usp=sharing
2. https://vnpro.vn/thu-vien/cau-hinh-dinh-tuyen-dong-ospf-2351.html