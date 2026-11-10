
# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- **Họ tên:** Tạ Xuân Anh Vũ
- **MSSV:** 2A202603173
- **Email:** anhvudepzai211104@gmail.com
- **Lớp:** 2A
- **Link repo bài làm:** https://github.com/AnhVuTaXuan/K5-L2A-Day02-Ta-Xuan-Anh-Vu-2A202603713-ObjectDetection
- **Link Colab đã chạy:** https://colab.research.google.com/drive/1aIGoYjSzQ2SKcnbTsjmdNh87xjJ6YxEz?usp=drive_link

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

### 3.1. Kết quả annotation

- **Số file nhãn train:** 50 file `.txt`, tương ứng với 50 ảnh trong `dataset/images/train/`.
- **Tổng số bounding box:** 1.037 box.
- **Phân bố nhãn theo class:**

| Class | Class ID | Số bounding box |
|---|---:|---:|
| `car` | 0 | 874 |
| `truck` | 1 | 21 |
| `bus` | 2 | 98 |
| `van` | 3 | 44 |
| **Tổng** | | **1.037** |

### 3.2. Kiểm tra cấu trúc nhãn

Tôi đã chạy script `tools/check_yolo_labels.py` để kiểm tra các file nhãn YOLO.

**Kết quả checker:**

ĐẠT cấu trúc: 50/50 file nhãn hợp lệ.

Chờ ground truth để chấm chất lượng.

### 3.3. Danh sách ít nhất 10 ảnh đã rà bằng mắt:

- drive_001.jpg
- drive_002.jpg
- drive_003.jpg
- drive_004.jpg
- drive_005.jpg
- drive_006.jpg
- drive_007.jpg
- drive_008.jpg
- drive_009.jpg
- drive_010.jpg

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 420 | |
| FP | 617 | |
| FN | 233 | |
| Precision | 0.4050 | |
| Recall | 0.6432 | |
| F1 | 0.4970 | |
| mean_iou_of_matches | 0.7242 | |
| passed | false | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

### 5.1. Môi trường và cấu hình huấn luyện

Thông tin được lấy từ args.yaml trong output của lần chạy YOLO.

- Runtime: Google Colab.
- Thiết bị huấn luyện: CPU (device: cpu).
- Model: yolo26n.pt.
- Task: Object Detection.
- Epochs: 50.
- Image size: 640.
- Batch size: -1 (AutoBatch).
- Optimizer: auto.
- Pretrained: true.
- Seed: 20260909.
- Validation: Có sử dụng tập val gồm 10 ảnh eval.
- Checkpoint tốt nhất: outputs/yolo26_runs/vehicle_annotation/weights/best.pt.

Các tham số chính được cấu hình trong notebook gồm epochs=50, imgsz=640, batch=-1, device=DEVICE, seed=20260909 và plots=True. Không có bằng chứng cho thấy tôi đã chỉnh sửa notebook nguồn.

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4458 |
| Recall | 0.2538 |
| mAP50 | 0.2852 |
| mAP50–95 | 0.1718 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

**Trả lời bốn câu hỏi cuối notebook:**

**1. Lớp nào có recall thấp nhất? Ảnh nào minh họa?**

Theo confusion_matrix_normalized.png trong thư mục vehicle_eval, class bus có tỷ lệ nhận diện đúng trên đường chéo bằng 0.00 trong ma trận này. Điều đó cho thấy model chưa nhận diện đúng các đối tượng bus trong tập được biểu diễn ở ma trận.

Có thể sử dụng drive_058.jpg làm ảnh quan sát trực quan cảnh có nhiều xe bus và các phương tiện khác. Tuy nhiên, ảnh prediction riêng lẻ không thay thế được phép tính recall theo từng class; cần đối chiếu prediction với ground truth của ảnh để xác định chính xác từng trường hợp đúng/sai.

**2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm tra điều gì trên box?**

Cần kiểm tra độ chính xác của vị trí và kích thước bounding box:

- Box có bao sát phần thân xe nhìn thấy được hay không.
- Box có bị rộng hoặc cao quá mức, lấy nhiều background hay không.
- Box có bị lệch khỏi tâm vật thể hay không.
- Box có bỏ sót một phần thân xe hay không.

mAP50 sử dụng ngưỡng IoU 0.50, trong khi mAP50–95 lấy trung bình kết quả ở nhiều ngưỡng IoU từ 0.50 đến 0.95. Vì vậy, box có thể đạt yêu cầu ở ngưỡng IoU thấp nhưng không đủ chính xác ở các ngưỡng cao hơn.

**3. Lỗi class nào có thể liên quan tới guideline? Dữ kiện nào hỗ trợ nhận xét?**

Trường hợp đáng chú ý là nhầm lẫn giữa car và van.

Trong ảnh drive_051.jpg, một chiếc sedan màu đỏ bị dự đoán thành van với confidence khoảng 0.94. Theo GUIDELINE_MINI.md:

- car là phương tiện đường bộ có bốn bánh, chủ yếu chở người, ví dụ sedan, SUV và hatchback.
- van có thân dạng hộp, kích thước trung bình, thường có khoang cabin và khoang chứa hàng liền nhau.

Trường hợp này cho thấy ranh giới phân biệt car và van cần được áp dụng nhất quán. Tuy nhiên, đây là lỗi prediction của model, không tự động chứng minh nhãn ground truth do người gán bị sai. Cần kiểm tra lại ảnh và nhãn tương ứng trước khi kết luận guideline hoặc annotation có vấn đề.

**4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?**

Không sử dụng 10 ảnh eval để train nhằm tránh đưa dữ liệu đánh giá vào quá trình học của model. Nếu model được huấn luyện trực tiếp trên các ảnh này, kết quả đánh giá sẽ không còn phản ánh khách quan khả năng tổng quát hóa trên dữ liệu chưa được dùng để học.

Trong notebook, 10 ảnh eval được khai báo ở split val và được sử dụng để:

- Theo dõi kết quả validation trong quá trình huấn luyện.
- Hỗ trợ lựa chọn checkpoint best.pt.
- Tính Precision, Recall, mAP50 và mAP50–95.
- Kiểm tra trực quan kết quả prediction trên các ảnh eval.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
