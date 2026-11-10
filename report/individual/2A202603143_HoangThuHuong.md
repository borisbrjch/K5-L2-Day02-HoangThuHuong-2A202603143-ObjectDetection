# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Hoàng Thu Huong
- MSSV: 2A202603143
- Email: huonghoang.work18237@gmail.com
- Lớp: K5 Level 2
- Link repo bài làm: https://github.com/borisbrjch/K5-L2-Day02-HoangThuHuong-2A202603143-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1RZXdNX39cVqcNcE2eXfCRT9u8o-7wlr_#scrollTo=6f78b880

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](https://github.com/borisbrjch/K5-L2-Day02-HoangThuHuong-2A202603143-ObjectDetection/blob/main/GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định:
- `drive_006.jpg` — nhiều vật thể nhỏ chồng lấn ở phía xa: chỉ g
án nhãn vật thể còn nguyên, không quá chồng lấn; bỏ qua vật thể v
ừa xa, vừa chồng lấn nhiều, vừa không rõ hình dáng.
- `drive_013.jpg` — vật thể bị blur: vẫn gán nhãn nếu nhận diện
được lớp, để có dữ liệu training đa dạng.
- `drive_019.jpg` — nhiều vật thể chồng lấn nhưng rõ hình dạng,
tầm gần: gán nhãn tất cả vật thể gần camera; bỏ qua vật thể quá xa/không rõ/chồng lấn nhiều.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file YOLO
- Kết quả checker và lỗi đã xử lý: 50/50 file hợp lệ, không có lỗi cấu trúc
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
  - drive_001.jpg (xe con)
  - drive_015.jpg (xe tải)
  - drive_022.jpg (xe buýt)
  - drive_037.jpg (van)
  - drive_045.jpg (xe con)
  - drive_051.jpg (xe tải)
  - drive_058.jpg (xe buýt)
  - drive_060.jpg (xe con)
  - drive_054.jpg (van)
  - drive_056.jpg (xe tải)

- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
  - Lỗi bỏ sót: drive_012.jpg - bỏ 1 box van khi xe bị che 50% (sửa: ghi chú "case ID: van-012" và hỏi giảng viên)
  - Lỗi class: drive_008.jpg - ghi nhãn car khi thực tế là van (sửa: ghi chú "case ID: van-008" và sửa nhãn)

- Commit khóa nhãn ban đầu: `05f5748` (Add 50 YOLO labels for training dataset)

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 405 | |
| FP | 301 | |
| FN | 248 | |
| Precision | 0.5737 | |
| Recall | 0.6202 | |
| F1 | 0.5960 | |
| mean_iou_of_matches | 0.8418 | |
| passed | false | |

Nguồn: `outputs/annotation_evaluation.json` (IoU threshold = 0.50, required F1 = 0.80, required mean IoU = 0.70).

Nhận xét: mean IoU đạt (0.84 > 0.70) — box vẽ chuẩn. F1 chưa đạt (0.60 < 0.80) do FP cao (301) và FN cao (248). Các ảnh lệch nhiều nhất: drive_028 (3 gold / 19 student), drive_041 (24 gold / 7 student), drive_018 (2 gold / 11 student), drive_005 (24 gold / 9 student). Rework tập trung vào việc điều chỉnh tiêu chí gán nhãn vật thể xa/nhỏ cho nhất quán với gold.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: 
  - Runtime: 12 phút 34 giây
  - GPU: NVIDIA T4 (Colab)
  - Phiên bản Ultralytics: yolov8n.pt
  - Torch: 2.3.0
- Các tham số thay đổi so với notebook nếu có và lý do:
  - Thêm `--conf 0.25` để lọc nhãn thấp độ tin cậy
  - Thêm `--iou 0.5` để điều chỉnh ngưỡng IoU
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.5522 |
| Recall | 0.2961 |
| mAP50 | 0.2962 |
| mAP50–95 | 0.2093 |

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Self-QC | Nhãn van bị nhầm với bus khi bị che 60% | Ghi chú case ID, hỏi giảng viên | Vẫn cần xác nhận lại với gold labels |
| Self-QC | Box rìa không bao trọn xe con khi bị che 40% | Kiểm tra lại ảnh gốc | Cần xác nhận xem xe con thực tế có bao trọn không |
| Self-QC | Đánh giá F1 thấp do nhiều FN | Thêm confidence threshold 0.25 | Kết quả chưa rõ, cần kiểm lại |

## 7. Tự đánh giá theo rubric

- Đã có 50 file nhãn YOLO, checker chạy đạt
- Có 50 file nhãn, cùng basename với ảnh train
- Có JSON đánh giá annotation
- Có 2 folder assets (eval_predictions và yolo26_runs) trong báo cáo
- Không có gold train, tài liệu giảng viên, token, mật khẩu hoặc model .pt trong commit
- Đã nộp link trên LMS trước deadline và người chấm xem được bài

## 8. Công cụ AI và tài liệu tham khảo

- Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

GPT, Claude 