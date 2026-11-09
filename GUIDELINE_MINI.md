# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con phục vụ vận chuyển hành khách cá nhân. | Sedan, hatchback, SUV, taxi. |
| `truck` | Xe tải hoặc xe đầu kéo phục vụ vận chuyển hàng hóa. | Xe tải thùng, xe tải lớn, xe đầu kéo. |
| `bus` | Phương tiện vận tải hành khách dạng xe buýt. | City bus, coach. |
| `van` | Phương tiện có kiểu dáng van hoặc minivan. | Cargo van, passenger minivan. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bao quanh phần thân xe thực sự nhìn thấy, hạn chế lấy thêm mặt đường, bầu trời hoặc phương tiện bên cạnh.
- Vật thể bị che khuất: Vẫn gán nhãn nếu có thể nhận diện được class. Bounding box chỉ bao quanh phần phương tiện nhìn thấy.
- Vật thể bị cắt bởi rìa ảnh: Vẫn gán nhãn nếu nhận diện được class. Box chạm đúng rìa ảnh, không ước lượng phần nằm ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: Chỉ gán nhãn khi đủ thông tin xác định class. Nếu không chắc chắn thì không tự đoán, ghi nhận và hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_004` | Ảnh ban đêm, ánh sáng yếu và đèn xe gây lóa, khiến một số phương tiện ở xa khó nhận diện. | Gán nhãn phương tiện nếu nhận diện được class; không đoán đối tượng chỉ còn vùng sáng hoặc bóng mờ. | Chỉ annotation phương tiện thuộc bốn class khi có đủ thông tin xác định loại phương tiện. |
| `drive_006` | Một số phương tiện bị cắt bởi rìa ảnh. | Vẫn gán nhãn nếu nhận diện được class; box bao quanh phần nhìn thấy và chạm rìa ảnh. | Không ước lượng phần phương tiện nằm ngoài khung hình. |
| `drive_030` | Mật độ phương tiện cao, nhiều xe ở xa, kích thước nhỏ hoặc bị che khuất. | Gán từng xe riêng nếu nhận diện được class; không gộp box. Nếu không đủ thông tin phân loại thì không tự đoán, ghi nhận và hỏi giảng viên. | Mỗi xe hợp lệ có một box riêng và phải xác định được class. |
