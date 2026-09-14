# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** NGÔ VĂN HƯNG<br>
**MSSV:** 2A202602094<br>
**Hình thức:** cá nhân <br>
**Mã cặp:** `SOLO` 

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: `f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33`
- Bốn mã ảnh:`drive_008, drive_022, drive_033, drive_038`
- Số vật thể thực tế: `103`
- Mã SHA-256 của gói YOLO của bạn: `f19f5ef42ccb882f8cb0972b8f91ec258f0a51c660b8eb0460def1026098a38f`
- Mã SHA-256 của gói CVAT gốc của bạn: `83de179a417e4ae2478275c5407f7ac46df56f62eca2fee870e6f677e01fb2a1`
- Nguồn đối chiếu: bộ nhãn tham chiếu do người hướng dẫn thực hành cung cấp (teaching_reference)
- Mã SHA-256 của gói đối chiếu:`c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b`
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: không có mã lần phát , lấy bộ tham chiếu trên discord , Lab Coach cung cấp lúc 15h - 14/9/2026

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu: Việc làm độc lập trước khi đối chiếu giúp kết quả phản ánh đúng khả năng tự đọc ảnh và áp dụng guideline của tôi, không bị ảnh hưởng bởi đáp án tham chiếu. Sau khi hoàn thành, tôi mới dùng kết quả tham chiếu để so sánh và tìm ra những điểm cần cải thiện.



## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp      | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| ----------- | -------- | ------------------ | --------------- |
| drive_022/mine_row 1    | bus | Thân xe dài, nhiều ô cửa sổ và ghế | Gán `bus` khi có thân xe khách dài, nhiều cửa sổ hoặc nhiều hàng ghế        |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

Ví dụ 1 xe dài nhiều cửa sổ và ghế bị che khuất bởi một vật thể khác , thì ở đây lớp là `bus` dựa vào đặc điểm nhận dạng của xe còn thuộc tính là `occluded` mô tả trạng thái quan sát được vật thể

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi                        | Cách phát hiện | Sau khi sửa và quy tắc |
| ------------- | ------------------------------- | -------------- | ---------------------- |
| Gán van cho một xe có phần thùng hàng phía sau, nhưng hình dáng bị che khuất nên dễ nhầm với van     | lớp |Khi tự kiểm tra, tôi zoom 100% và nhận ra xe có khoang/thùng hàng riêng phía sau cabin, đây là dấu hiệu của truck theo guideline.      | Sửa thành truck, truck phải có dấu hiệu rõ của thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng |

- Số hộp `needs_review` trước và sau khi kiểm:
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ:

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: `2 0.384328 0.722922 0.435813 0.351969`
- Tên lớp và tọa độ điểm ảnh `xyxy`: `bus xyxy=[106.5, 350.0, 385.4, 575.3] `
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?
Vì đúng định dạng YOLO chỉ đảm bảo các giá trị được ghi đúng cấu trúc và tọa độ được chuẩn hóa, không đảm bảo người gán nhãn đã chọn đúng lớp hoặc vẽ đúng vị trí/kích thước của object. Ví dụ, dòng trên có thể đúng format nhưng vẫn có thể gán nhầm bus thay vì van, hoặc box không bao sát phần xe nhìn thấy.

## 5. Huấn luyện và dự đoán thử
- Ba mã ảnh huấn luyện: `drive_022, drive_033, drive_038`
- Mã ảnh thẩm định: `drive_008`

- Mô tả một dự đoán trong `detect_result.jpg`:
  Model tạo một bounding box quanh chiếc xe buýt lớn ở phía bên phải ảnh. Đây là một vật thể có thân xe dài và nhiều cửa sổ, phù hợp với các dấu hiệu nhận biết lớp `bus` trong guideline.

- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
  Dự đoán này gợi ý cần tiếp tục kiểm tra việc các đặc điểm nhìn thấy như cấu trúc thân xe dài và nhiều cửa sổ có được áp dụng nhất quán khi phân biệt `bus` với các lớp khác hay không.

- Minh chứng nào có thể bác bỏ nhận định của bạn?
  Có thể kiểm tra thêm các xe tương tự trong tập dữ liệu và xem nhãn đã được gán có nhất quán với guideline hay không. Nếu các trường hợp tương tự đều được phân loại nhất quán thì nhận định về vấn đề ở quy tắc hoặc dữ liệu không có đủ cơ sở.

- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?
  Chỉ có bốn ảnh và một ảnh được dùng để thẩm định nên tập dữ liệu quá nhỏ, không đủ đại diện cho các tình huống thực tế. Kết quả này chỉ dùng để kiểm tra đường ống dữ liệu và tìm vấn đề cần xem lại, không dùng để kết luận khả năng sử dụng mô hình trong thực tế.

## 6. Đối chiếu nhãn

- Số hộp ghép được: 48
- IoU trung bình và trung vị:`0.846461, 0.885453`
- Mức đồng thuận lớp:`0.729167`
- Số hộp phía bạn không ghép được:`55`
- Số hộp phía đối chiếu không ghép được:`2`
- Một điểm khác biệt cụ thể:`drive_022, mine_row 1: tôi gán obj này là bus nhưng bộ reference lại là van `
- Quy tắc hoặc hành động sửa phát sinh: `tôi đã check lại guideline quy tắc cho bus là thân xe dài , nhiều cửa sổ và ghế cho khách , tôi thấy đúng với đặc điểm nhận dạng nên tôi vẫn giữ nguyên lớp`
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?
`IoU chỉ đo mức độ chồng khít giữa hai bounding box đã được ghép, không đánh giá trực tiếp việc hai bên có phân loại đúng object hay không.`

## 7. Kiểm tra kho GitHub cá nhân

- [✅] Có phiếu quy tắc với ba tình huống mơ hồ.
- [✅] Có kết quả kiểm hai gói xuất.
- [✅] Có thông tin lần huấn luyện và ảnh dự đoán.
- [✅] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [✅] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [✅] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:

