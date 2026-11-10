# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Phương tiện giao thông đường bộ có 4 bánh, chủ yếu dùng để chở người. | Sedan, SUV, hatchback. Thường có 4–5 chỗ ngồi, kích thước nhỏ hơn bus và truck. |
| `truck` | Xe tải được thiết kế chủ yếu để vận chuyển hàng hóa, vật liệu hoặc thiết bị. | Xe tải thùng, xe ben, xe đầu kéo. Đặc trưng là thùng chở hàng hoặc khả năng kéo rơ-moóc. |
| `bus` | Phương tiện giao thông đường bộ có sức chứa lớn, được thiết kế để chở nhiều hành khách. | Xe bus công cộng, xe khách, xe bus trường học. Thường có nhiều hàng ghế và sức chứa lớn hơn car hoặc van. |
| `van` | Xe có thân dạng hộp, kích thước trung bình, được thiết kế để chở hành khách hoặc hàng hóa. | Minivan, cargo van, passenger van. Thường có khoang cabin và khoang chứa hàng liền nhau, nhỏ hơn bus và thường lớn hơn car. |
## Quy tắc chung

- Box bám sát vật thể nghĩa là: Box bao quanh sát phần thân xe nhìn thấy được, không lấy thêm nền, mặt đường, bầu trời hoặc vật thể bên cạnh.
- Vật thể bị che khuất: Vẫn gán nhãn nếu nhận diện được lớp xe; box chỉ bao quanh phần thân xe còn nhìn thấy được.
- Vật thể bị cắt bởi rìa ảnh: Box kéo sát đến đúng rìa ảnh tại vị trí vật thể bị cắt, không tự đoán hoặc vẽ thêm phần nằm ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: Nếu phần nhìn thấy quá nhỏ, không thể xác định chắc chắn lớp xe thì không gán nhãn, ghi lại trường hợp đó vào mini guideline và hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_016` | Xe màu cam nằm gần phía bên trái ảnh, kích thước nhỏ và khó quan sát rõ hình dáng tổng thể | Gán nhãn `van` | Dựa trên hình dáng thân xe tương đối cao, dạng hộp và cấu trúc khoang xe phù hợp với định nghĩa `van`. Phân loại dựa trên đặc điểm cấu trúc phương tiện thay vì chỉ dựa vào kích thước. |
| `drive_025` | Xe ô tô màu xanh đậm bị dải cây xanh che khuất một phần thân xe | Giữ nhãn `car`, vẽ box bao quanh phần xe nhìn thấy được | Vẫn nhận diện được phương tiện là ô tô con thông qua hình dáng tổng thể và phần thân xe còn quan sát được. Theo guideline, vật thể bị che khuất một phần vẫn được gán nhãn nếu xác định được lớp. |
| `drive_026` | Phương tiện nằm gần rìa phải ảnh, kích thước nhỏ và khó quan sát rõ cấu trúc thân xe | Gán nhãn `truck` theo quyết định hiện tại | Theo schema, `truck` là phương tiện chủ yếu dùng để vận chuyển hàng hóa, có đặc trưng như thùng chở hàng hoặc khả năng kéo rơ-moóc. |
