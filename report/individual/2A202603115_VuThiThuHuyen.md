# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Vu Thi Thu Huyen
- MSSV: 2A202603115
- Email: vuthithuhuyen126204@gmail.com
- Lớp: 2A
- Link repo bài làm: https://github.com/thuhuyen126/K5-L2A-Day02-VuThiThuHuyen-2A202603115-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/github/thuhuyen126/K5-L2A-Day02-VuThiThuHuyen-2A202603115-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb

## 2. Schema và mini guideline

Tham khảo [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

- Schema gồm 4 class: car (0), truck (1), bus (2), van (3).
- Bounding box sử dụng định dạng YOLO: class_id, x_center, y_center, width, height; tọa độ được chuẩn hóa trong khoảng [0, 1].
- Mỗi phương tiện được nhận diện có một bounding box riêng. Box bao quanh phần thân xe nhìn thấy được.
- Xe đang đỗ vẫn được annotate.
- Xe bị cắt bởi mép ảnh: annotate phần nhìn thấy, box chạm mép ảnh.
- Xe bị che khuất: annotate phần nhìn thấy nếu vẫn nhận diện được class.

Ba trường hợp cần chú ý:
1. drive_004: Ảnh tối, có ánh sáng chói và xe ở xa; chỉ annotate khi có thể nhận diện class.
2. drive_006: Xe bị cắt bởi mép ảnh; box bao quanh phần nhìn thấy và chạm mép ảnh.
3. drive_030: Giao thông dày đặc, nhiều xe nhỏ hoặc bị che khuất; tách box nếu nhận diện được, không đoán class.

Trường hợp chưa chắc chắn về van/minivan, bus và car cần được giảng viên làm rõ để bảo đảm tính nhất quán giữa annotator và guideline.

## 3. Annotation và self-QC

- Số file nhãn train: 50 (drive_001.txt đến drive_050.txt).
- Công cụ annotation: CVAT.
- Hỗ trợ tạo proposal: YOLO26n pretrained; các proposal đã được rà soát và chỉnh sửa thủ công trên CVAT.
- Kết quả checker: ĐẠT cấu trúc 50/50 file nhãn hợp lệ.
- Định dạng: 5 cột YOLO, class ID 0–3, tọa độ normalized.
- Đã rà soát trực quan toàn bộ 50 ảnh trong CVAT. Một số ảnh đại diện: drive_001, drive_002, drive_003, drive_004, drive_005, drive_006, drive_007, drive_008, drive_009, drive_010.
- Sau khi rà soát, xuất lại 50 file nhãn từ CVAT; tổng số bounding box là 691.
- Commit khóa nhãn ban đầu: c865a1e31daa461e07f7feeba4ac9c6b26585dbc.
- Commit message: Add initial train annotations and mini guideline.

## 4. Gold QC và rework

Đã nhận gold_train_labels.rar sau khi khóa và push nhãn ban đầu. Gold được giải nén bên ngoài repository tại /tmp/gold_train_labels. Không đưa gold vào repository.

Đã đối chiếu đủ 50 file Student và 50 file Gold; basename khớp hoàn toàn.

Kết quả đánh giá lần đầu, IoU matching threshold = 0.50:

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 405 | Chưa thực hiện |
| FP | 286 | Chưa thực hiện |
| FN | 248 | Chưa thực hiện |
| Precision | 0.5861 | Chưa thực hiện |
| Recall | 0.6202 | Chưa thực hiện |
| F1 | 0.6027 | Chưa thực hiện |
| mean_iou_of_matches | 0.8515 | Chưa thực hiện |
| passed | false | Chưa thực hiện |

Ngưỡng yêu cầu: F1 >= 0.80 và mean IoU >= 0.70.

Kết luận:
- Mean IoU đạt ngưỡng.
- F1 chưa đạt ngưỡng.
- Kết quả được lưu tại [annotation_evaluation.json](../../outputs/annotation_evaluation.json).
- Kết quả Before Rework được giữ riêng tại /tmp/annotation_evaluation_before_rework.json.

Đã tạo ảnh đối chiếu Student và Gold cho một số ảnh ưu tiên kiểm tra, gồm drive_001, drive_008, drive_009, drive_018, drive_024, drive_027, drive_041 và drive_050.

Ví dụ drive_001: Student có 15 box, Gold có 5 box, evaluator ghi nhận 5 matches. Đây là khác biệt cần kiểm tra thêm theo guideline; không thể kết luận tất cả box không match đều là lỗi nếu chưa xác minh từng đối tượng.

Chưa thực hiện vòng rework hoàn chỉnh và chưa có kết quả After Rework. Không chỉnh sửa gold labels.

## 5. Kết quả YOLO

- Runtime: Google Colab.
- GPU: NVIDIA Tesla T4.
- Model: YOLO26n.
- Ultralytics: 8.4.208.
- PyTorch: 2.14.1+cu130.
- Epochs: 50.
- Image size: 640.
- Batch size: 1.
- Seed: 20260909.
- Best checkpoint trong phiên Colab: outputs/yolo26_runs/vehicle_annotation/weights/best.pt.
- Best checkpoint không được đưa vào repository theo quy định không nộp file .pt.
- Tập đánh giá: 10 ảnh eval, drive_051.jpg đến drive_060.jpg.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.7947 |
| Recall | 0.2926 |
| mAP50 | 0.3003 |
| mAP50–95 | 0.2028 |

Kết quả được lấy từ model_eval_metrics.json.

Assets:
- [10 ảnh eval predictions](assets/eval_predictions/visuals/)
- [YOLO26 training và evaluation runs](assets/yolo26_runs/)
- Training log: assets/yolo26_runs/vehicle_annotation/results.csv
- Confusion matrix: assets/yolo26_runs/vehicle_eval/confusion_matrix.png
- PR curve: assets/yolo26_runs/vehicle_eval/BoxPR_curve.png

Nhận xét:
- Recall tổng thể còn thấp (0.2926), cho thấy mô hình còn bỏ sót nhiều đối tượng.
- Theo kết quả đánh giá theo class trong output Colab, truck và van có recall thấp nhất (0).
- Các chỉ số phản ánh kết quả của mô hình trên tập eval được notebook sử dụng làm validation; không phải kết quả trên một test set độc lập.

Trả lời câu hỏi cuối notebook:

1. Truck và van có recall thấp nhất, đều bằng 0 theo output đánh giá theo class. Cần tiếp tục đối chiếu confusion matrix và ảnh validation predictions để xác định trường hợp cụ thể; chưa xác định chắc chắn một ảnh riêng lẻ chỉ từ file metrics tổng hợp.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm tra độ chặt và vị trí bounding box ở các ngưỡng IoU cao hơn, đặc biệt các box bao quá rộng hoặc chưa sát thân xe.
3. Lỗi class có thể liên quan đến sự mơ hồ giữa van/minivan, car và bus. Dữ kiện cần đối chiếu là confusion matrix, ảnh dự đoán và quy tắc phân loại trong GUIDELINE_MINI.md. Chưa kết luận nguyên nhân nếu chưa xác minh từng ảnh.
4. Không dùng 10 ảnh eval để train nhằm tránh rò rỉ dữ liệu và giữ vai trò đánh giá/validation theo thiết kế notebook. Trong notebook, tập này được dùng để validation trong quá trình huấn luyện và đánh giá checkpoint, nên không phải test độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Chạy evaluator trên WSL | Lệnh python không được nhận diện khi virtualenv chưa kích hoạt | Kích hoạt ~/venvs/yolo và dùng python | Chạy evaluator thành công |
| Push GitHub | GitHub không chấp nhận tài khoản password | Sử dụng Personal Access Token | Push commit thành công; không lưu token trong repo |
| Tải Colab ZIP | Giao diện tải file báo Failed to fetch; Google Drive mount lỗi credential propagation | Dùng google.colab.files.download | Tải ZIP 16.45 MB thành công |
| Gold QC | F1 ban đầu thấp hơn ngưỡng | Lưu kết quả, kiểm tra ảnh đối chiếu | Chưa hoàn thành rework; cần tiếp tục xác minh khác biệt Student/Gold |

## 7. Tự đánh giá theo rubric

- Đã hoàn thành: 50 file nhãn YOLO, checker cấu trúc đạt, mini guideline, commit và push nhãn ban đầu, Gold evaluation lần đầu.
- Đã hoàn thành: chạy notebook YOLO26n trên Colab, lưu 10 ảnh eval predictions, training logs và các biểu đồ đánh giá.
- Chưa đạt ngưỡng Gold QC: F1 = 0.6027 < 0.80; mean IoU = 0.8515 >= 0.70.
- Chưa hoàn thành: vòng rework và đánh giá After Rework.
- Model metrics đã được ghi nhận; recall và mAP còn thấp, cần phân tích thêm nguyên nhân.
- 10 ảnh eval đã được lưu trong assets; checkpoint .pt không được đưa vào repository.
- Các giới hạn và phần chưa hoàn thành được ghi rõ, không tự nhận đã đạt những tiêu chí chưa kiểm chứng.

## 8. Công cụ AI và tài liệu tham khảo

Công cụ đã sử dụng:
- ChatGPT: hỗ trợ giải thích quy trình, hướng dẫn thao tác WSL, Git, CVAT, kiểm tra cấu trúc nhãn, đọc kết quả evaluator và hỗ trợ trình bày báo cáo.
- YOLO26n pretrained: tạo bounding box proposals để hỗ trợ annotation; các proposal được người học rà soát và chỉnh sửa thủ công.
- CVAT: công cụ gán nhãn và xuất dữ liệu YOLO.
- Google Colab: chạy notebook huấn luyện và đánh giá YOLO26n.

Người học trực tiếp thực hiện và kiểm tra việc annotation, rà soát 50 ảnh, chạy checker, chạy evaluator, chạy notebook Colab và kiểm tra các file đầu ra.

Tài liệu tham khảo:
- RULES.md
- GUIDE.md
- CHECKPOINTS.md
- SUBMISSION.md
- RUBRIC.md
- GUIDELINE_MINI.md
- Notebook: notebooks/day2_annotation_to_yolo.ipynb

Không đưa gold train, tài liệu nội bộ, token, mật khẩu hoặc checkpoint .pt vào repository.
