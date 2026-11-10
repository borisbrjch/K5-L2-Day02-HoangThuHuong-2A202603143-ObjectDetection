# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Phương tiện di chuyển cá nhân | Sedan, SUV, hatchback |
| `truck` | Phương tiện chở hàng | Có khoang chở hàng hoặc cấu trúc xe tải |
| `bus` | Phương tiện chở hành khách công cộng | Có khoang hành khách lớn, thường có nhiều ghế |
| `van` | Phương tiện hình hộp chữ nhật có thể chở người hoặc hàng hoá | Thân xe kín, thường có khoang thân kéo dài |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: bbox liền sát với vật thể, không bao gồm background không cần thiết. Vật thể nằm trong bbox cần rõ ràng tuyệt đối.
- Vật thể bị che khuất: Vẫn gán nhãn những phương tiện có thể nhận diện được dù bị che khuất một phần. Vẽ bounding box bao quanh phần phương tiện nhìn thấy được.
- Vật thể bị cắt bởi rìa ảnh: Vẫn gán nhãn những phương tiện có thể nhận diện được dù một phần nằm ngoài khung hình. Giới hạn bounding box tại ranh giới của ảnh, không vẽ box vượt ra ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: Không tự suy đoán class khi phương tiện không thể được nhận diện một cách đáng tin cậy. Không gán nhãn những vật thể nhỏ, mờ, ở xa. Ghi nhận những trường hợp không rõ ràng để xem xét lại.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_006.jpg` | có nhiều vật thể nhỏ chồng lấn nhau ở phía xa | label những vật thể còn nguyên ven / không quá chồng lấn, bỏ qua những vật thể đủ 3 tiêu chí xa, chồng lấn nhiều, không rõ hình dáng | Đủ chi tiết nhưng không từ bỏ chất lượng nhãn dán |
| `drive_013.jpg` | Vật thể bị blur (nhoè) | Vẫn gán nhãn | Muốn có data training đa dạng trong từng khung hình / tình huống |
| `drive_019.jpg` | Có nhiều vật thể chồng lấn nhưng rõ hình dạng và ở tầm gần với camera | Gán nhãn tất cả vật thể ở phía gần camera, bỏ qua những vật thể ở quá xa, không rõ hình dáng, và chồng lấn nhiều | Train model ở những tình huống khó, đảm bảo chất lượng ở những vùng quan sát quan trọng |
