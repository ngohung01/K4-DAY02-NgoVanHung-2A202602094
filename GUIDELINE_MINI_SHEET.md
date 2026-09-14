# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** NGÔ VĂN HƯNG<br>
**MSSV:** 2A202602094<br>
**Hình thức:** cá nhân p<br>
**Mã cặp:**  `SOLO` 
 
## 1. Phạm vi

- Chỉ gán phương tiện thuộc bốn lớp bên dưới.
- Mỗi phương tiện là một hộp; không gộp nhiều xe.
- Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
- Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp | Gán khi nhìn thấy | Không gán vào lớp này |
| ---: | --- | --- | --- |
| 0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
| 1 | `truck` (xe tải) | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối |
| 2 | `bus` (xe buýt) | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế | xe van nhỏ; xe tải; ô tô con |
| 3 | `van` (xe van) | thân hộp nhỏ, kín, dùng chở người hoặc hàng | thân xe buýt; khoang hàng tách biệt như xe tải |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

- Vẽ sát phần vật thể nhìn thấy.
- Không ước lượng phần bị xe khác che.
- Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
- Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
| --- | --- | --- |
| `visibility` (mức nhìn thấy) | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy |
| `boundary` (quan hệ mép ảnh) | `inside` (trong ảnh), `truncated` (bị cắt) | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại) | đánh dấu quyết định cần quay lại |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

Hoàn thành trước khi xem bài của người khác hoặc bộ nhãn tham chiếu.

### Tình huống A — xe buýt hay xe van?

- Ảnh và mã vật thể: drive_022 và mine_row 1
- Dấu hiệu nhìn thấy: Thân xe dài và cao , có nhiều cửa số và ghế
- Quy tắc áp dụng: Gán `bus` khi thấy thân xe khách dài, nhiều cửa sổ hoặc hàng ghế 
- Quyết định: `bus`
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Đặt `review_state=needs_review`, ghi lại lý do và xin xem lại thay vì đoán giữa `bus` và `van`

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: drive_022 và mine_row 2
- Dấu hiệu nhìn thấy: Phía sau cabin có một khoang/thùng kín lớn, tách biệt với phần cabin; hình dạng của xe cho thấy đây là phương tiện chở hàng.
- Quy tắc áp dụng: Gán `truck` khi nhìn thấy thùng, ben, sàn chở hàng hoặc thiết bị công vụ rõ ràng. Không gán `truck` cho xe van kín một khối hoặc ô tô con.
- Quyết định: `truck`
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? đặt `review_state=needs_review` và ghi rõ lý do thay vì đoán giữa `truck` và `van`.

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: drive_033 và mine_row 1
- Dấu hiệu nhìn thấy khi phóng 100%: Đuôi xe dài , có nhiều ô kính 
- Giá trị `visibility`: `clear`
- Giá trị `boundary`: `truncated`
- Trạng thái `review_state`: `confident`
- Lý do: Đuôi xe dài đồng nghĩa thân xe sẽ dài, tôi nghĩ đuôi xe bởi vì tôi không thấy bánh xe mà phần chìa ra đã rất dài , và có 4 ô kính liền nhau

## 6. Xác nhận tự kiểm tra

- [✅] Đã rà đủ bốn ảnh.
- [✅] Đã kiểm vật thể thiếu và trùng.
- [✅] Đã kiểm lớp và hình học từng hộp.
- [✅] Mỗi hộp có đủ ba thuộc tính.
- [❌] Đã xử lý mọi hộp `needs_review`.
- [✅] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [❌] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [✅] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
- [✅] Số vật thể thực tế: 103, 40–60 là mục tiêu khối lượng, không phải điểm cắt.
