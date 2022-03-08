<h1> Định tuyến</h1>

<h2> Mục lục </h2>

- [I. Tổng quan](#i-tổng-quan)
- [II. Cấu trúc bảng định tuyến](#ii-cấu-trúc-bảng-định-tuyến)
- [III. Phân loại định tuyến](#iii-phân-loại-định-tuyến)
  - [1. Định tuyến tĩnh - static routing](#1-định-tuyến-tĩnh---static-routing)
  - [2. Định tuyến động](#2-định-tuyến-động)
    - [2.1. Distance vector](#21-distance-vector)
    - [2.2. Link state](#22-link-state)
    - [2.3. Ngoài ra](#23-ngoài-ra)
      - [a. Classfull routing protocol](#a-classfull-routing-protocol)
      - [b. Classless routing protocol](#b-classless-routing-protocol)
    - [2.4. Các tham số quan trọng](#24-các-tham-số-quan-trọng)
      - [a. Metric](#a-metric)
      - [b. AD](#b-ad)
- [Tài liệu tham khảo](#tài-liệu-tham-khảo)

# I. Tổng quan
- Định tuyến là chức năng của router giúp xác định quá trình tìm đường đi cho các gói tin từ nguồn tới đích thông qua hệ thống mạng.
- Để xác định đường đi cho các gói tin, Router dựa vào:
  - Địa chỉ IP đích (Destination IP)
  - Bảng định tuyến (routing table)

Trong hình, 2 bộ định tuyến R1 và R2 đang kết nối 3 mạng khác nhau.

![Imgur](https://i.imgur.com/RiWgBCx.png)
# II. Cấu trúc bảng định tuyến
- Các Router kiểm kiểm tra địa chỉ IP đích của gói tin đã nhận và đưa ra tuyến đường phù hợp.
- Để xác định gói tin sẽ được gửi qua đường nào, các router phải sử dụng Bảng định tuyến (Routing table).
- Bảng định tuyến liệt kê tất cả các mạng từ các tuyến đường đã biết cho router
- Bảng định tuyến của mỗi router là duy nhất và được lưu trữ trong RAM của thiết bị.
- Lệnh ở chế độ EXEC được sử dụng để xem bảng định tuyến trên router Cisco:
```
R#show ip route
```

Ví dụ:
```
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
E1 - OSPF external type 1, E2 - OSPF external type 2
i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
ia - IS-IS inter area, * - candidate default, U - per-user static route
o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
a - application route
+ - replicated route, % - next hop override, p - overrides from PfR
Gateway of last resort is 209.165.200.226 to network 0.0.0.0
S* 0.0.0.0/0 [1/0] via 209.165.200.226, GigabitEthernet0/0/1
10.0.0.0/24 is subnetted, 1 subnets
O 10.1.1.0 [110/2] via 209.165.200.226, 00:02:45, GigabitEthernet0/0/1
192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.10.0/24 is directly connected, GigabitEthernet0/0/0
L 192.168.10.1/32 is directly connected, GigabitEthernet0/0/0
209.165.200.0/24 is variably subnetted, 2 subnets, 2 masks
C 209.165.200.224/30 is directly connected, GigabitEthernet0/0/1
L 209.165.200.225/32 is directly connected, GigabitEthernet0/0/1
R1#
```

Ở đầu mỗi mục là 1 mã để xác định loại tuyến đường hoặc cách tuyến đường được học. Các mã phổ biến:
- L - Địa chỉ IP giao diện cục bộ được kết nối trực tiếp
- C - Mạng kết nối trực tiếp
- S - Tuyến đường tĩnh được cấu hình thủ công
- O - OSPF
- D - EIGRP
# III. Phân loại định tuyến
Có 2 loại định tuyến:
- Định tuyến tĩnh
- Định tuyến động
## 1. Định tuyến tĩnh - static routing
- Là loại định tuyến mà trong đó router sử dụng các tuyến đường đi tĩnh để vận chuyển dữ liệu đi.
- Do người quản trị cấu hình thủ công vào router.
- Cú pháp:
```
R(config)#ip route <destination-net> <subnet-mask> <NextHop|Outport>
```

Trong đó:
- `destination-network`: là địa chỉ mạng cần đi tới
- `subnet-mask`: subnet-mask của destination-network
- `next-hop`: địa chỉ IP của router kế tiếp kết nối trực tiếp với router đang xét.
- `OutPort`: cổng của router đang xét mà packet sẽ đi ra.

**Default route**
- Nằm ở cuối bảng định tuyến.
- Sử dụng để gửi các gói tin đi trong trường hợp mạng đích không tìm thấy trong bảng định tuyến.

Ví dụ:

![Imgur](https://i.imgur.com/K7Kd7Zl.png)

**Câu hình định tuyến tĩnh:**
- Next Hop:
```
R1(config)#ip route 0.0.0.0 0.0.0.0 203.113.115.1
```
- Hoặc Outport:
```
R1(config)#ip route 0.0.0.0 0.0.0.0 S0/0/0
```

## 2. Định tuyến động
- Là loại định tuyến mà trong đó router sử dụng các tuyến đường đi động để vận chuyển dữ liệu
- Các tuyến đường này do các router sử dụng các giao thức định tuyến để trao đổi thông tin định tuyến với nhau.
- Một số giao thức định tuyến phổ biến:
  - RIP (Routing Information Protocol)
  - OSPF (Open Shortest Path First)
  - BGP (Border Gateway Protocol)
  - ...
- Giao thức định tuyến động chia làm 2 loại:
  - Distance-route
  - link-state


### 2.1. Distance vector
![Imgur](https://i.imgur.com/nSgCJyV.png)
- Giao thức thuộc loại này: RIP (Routing Information Protocol),...
- Các router định tuyến:
  - thực hiện gửi định kỳ toàn bộ bảng định tuyến của mình và gửi cho các router láng giềng kết nối trực tiếp với mình.
  - Không biết được đường đi đến đích một cách cụ thể
  - Không biết về các router trung gian trên đường đi
  - Không biết cấu trúc kết nối giữa các router
- Bảng định tuyến là nơi lưu kết quả chọn đường tốt nhất của mỗi router.
=> Khi các router trao đổi bảng định tuyến với nhau, chúng chọn đường dựa trên kết quả đã chọn của router láng giềng.
- Mỗi router nhìn hệ thống mạng theo sự chi phối của các router láng giềng.
- Nhược điểm: Tốn nhiều băng thông (do thực hiện cập nhật thông tin định tuyến theo định kỳ)

### 2.2. Link state
![Imgur](https://i.imgur.com/YbylhGg.png)

- Giao thức thuộc loại này: OSPF (Open shortest Path First), IS-IS (Intermediate System to Intermediate System)
- Các router định tuyến:
  - sẽ trao đổi các LSA (Link state advertisement) với nhau để xây dựng và duy trì CSDL về trạng thái các đường liên kết.
  - Các thông tin trao đổi được gửi dưới dạng Multicast
  - Nó không thực hiện cập nhật định tuyến định kỳ mà chỉ khi có sự thay đổi xảy ra. => Nhanh, tốn ít băng thông.
  - Hỗ trợ CIDR (Classless Inter Domain Routing), VLSM (Variable Length Subnet Masking) nên là lựa chọn tốt cho các mạng lớn và phức tạp. => Đòi hỏi dung lượng bộ nhớ lớn và khả năng xử lý mạnh của CPU router.

> CIDR (Classless Inter-Domain Routing) là 1 phương pháp tóm tắt địa chỉ IP. Format địa chỉ IP CIDR: 192.168.1.0/24
> VLSM (Variable Length Subnet Masking) là kỹ thuật phân chia không gian địa chỉ IP thành các subnet có kích thước khác nhau.


- Để database luôn cập nhật thông tin mới: Trong LSA được đánh thêm chỉ số `sequense`:
  - Được bắt đầu từ giá trị `initial` đến `Max-age`.
  - Khi một router tạo ra 1 LSA, `sequense`=`initial`.
  - Giá trị `sequense` càng cao thì LSA update càng mới (VÌ khi gửi ra 1 phiên bản LSA update khác, `sequense` + 1).
  - Nếu `sequense`=`Max-age`, router sẽ flood (Ngập lụt) LSA ra cho tất cả ác router còn lại, sau đó set về `initial`.


### 2.3. Ngoài ra
Còn được phân thành 2 loại:
- Classfull routing protocol
- Classless routing protocol

#### a. Classfull routing protocol
- Trong các gói tin cập nhật (routing update) nhóm classfull không quảng bá: subnet-mask cùng với địa chỉ đích. Router phải lấy giá trị network-mask mặc định có cùng với địa chỉ lớp mạng của IP đích.
- Nếu  địa chỉ đích:
  - Kết nối trực tiếp với route, network-mask được lấy trên interface kết nối đến mạng đó.
  - Không nối trực tiếp (disconnected), sẽ lấy địa chỉ subnetmask default của địa chỉ đích.

#### b. Classless routing protocol
- Trong các gói tin cập nhật định tuyến, nhóm classless sẽ quảng bá subnet-mask cùng với địa chỉ đích.

### 2.4. Các tham số quan trọng

#### a. Metric
- sử dụng để chọn đường đi tốt nhất cho việc định tuyến.
- All protocol cũng phải dùng để tính toán đường đi đến mạng đích.
- Khi có nhiều đường đi đến đích, đường có `Metric` thấp sẽ được đưa vào bảng định tuyến.
- Mỗi giao thức định tuyến có 1 kiểu `metric` khác nhau.

#### b. AD
- Administrative Distance
- Là giá trị quy ước dùng để chỉ độ tin cậy của giao thức định tuyến.
- Giao thức có AD nhỏ hơn được xem là đáng tin cậy hơn.
- Giao thức định tuyến nào có AD nhỏ nhất sẽ được lựa chọn và đưa vao bảng định tuyến.

Bảng mức độ tin cậy được dùng trong router Cisco:
|Protocol|AD|
|---|---|
Nối trực tiếp|0|
Static route|1
EIGRP summary route|5
External BGP|20
Internal EIGRP |90
IGRP|100
OSPF|110
IS-IS|115
RIP|120
EGP|140
ODR|160
External EIGRP|170
Internal BGP|200
Không xác định|255|


# Tài liệu tham khảo

1. https://drive.google.com/file/d/1-1vF6vYwSv9J-SrHjqtXsllXJqjvknTe/view?usp=sharing
2. https://vi.wikipedia.org/wiki/%C4%90%E1%BB%8Bnh_tuy%E1%BA%BFn
3. https://ccna-200-301.online/introduction-to-routing/
4. https://geek-university.com/routing-table-explained/
