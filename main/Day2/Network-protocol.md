# Network Protocol

- Định nghĩa cấu trúc và định dạng của message
- Định nghĩa các tiến trình chia sẻ thông tin với nhau ntn
- Có các cơ chế phát hiện lỗi xảy ra giữa các thiết bị trên mạng
- Thiết lập và khởi tạo phiên kết nối.

# Các lớp mô hình OSI
- Aplication: Cung cấp giao diện truy cập trực tiếp với người sử dụng.
- Presentation: Mã hoá, nén dữ liệu và giải mã dữ liệu nhận được
- Sesion: Khởi tạo phiên kết nối, duy trì và kết thúc 1 phiên kết nối.
- Transport: Phân đoạn dữ liệu thành các segmentation (nhiều khúc. tại sao vì kích cỡ của dữ liệu ảnh hưởng đến chất lượng của 1 phiên truyền thông ). Sử dụng TCP/UDP để thêm thông tin điều khiển vào các segment.
    - Bắt đầu gửi đi, thế gửi cho ai?
- Network: mỗi segmet được đóng gói thành gói tin (tiến trình này là encapsulation) đưỢc gán tem nhãn đi từ đâu đến đâu. Sử dụng giao thức IP, gói tin này sẽ đc truyền trên mạng nên người ta gọi mạng này là mạng IP là vì thế.
- Datalink: đóng gói tin thành khung (frame) dữ liệu
- Phyciscal: xuống card mạng truyền đi (nó chỉ là tín hiệu điện thôi).

- Mỗi dịch vụ sử dụng các giao thức khác nhau.
- Mô hình osi để tham chiếu thôi, chứ người ta sử dụng mô hình TCP/IP để tối ưu quá trình.

# Protocol Data Units (PDUs)
là đơn vị dữ liệu của giao thức.
là tên gọi của dữ liệu khi đi qua các lớp khác nhau.
Ví dụ: ;ớp 7 là data, lớp 4 là segmets, lớp 3 là packet, lơp2 là frame, lớp 1 là bít.

# Encapsulation (đóng gói dữ liệu) >< De-encapsulation (Mở gói ra)

## Tầng ứng dụng
## 
- Mã hoá và nén dữ liệu lớP ứng dụng
- Giải mã dữ liệu
## Lớp phiên 