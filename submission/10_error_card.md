# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | MISSING | 3 |
| center | B1 | SPURIOUS | 7 |
| edge | B1 | ATTRIBUTE | 2 |
| edge | B1 | BOX_GEOMETRY | 2 |
| edge | B1 | SPURIOUS | 1 |
| mid | B1 | BOX_GEOMETRY | 1 |
| mid | B1 | MISSING | 4 |
| mid | B1 | SPURIOUS | 7 |
| mid | B1 | STRUCTURE | 2 |

## Top defects
- SPURIOUS: 15 (ví dụ frame adasind_014670.jpg)
- MISSING: 7 (ví dụ frame adasind_014670.jpg)
- BOX_GEOMETRY: 3 (ví dụ frame adasind_034080.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: 
  Lỗi chiếm số lượng áp đảo là SPURIOUS (15 trường hợp, tập trung 7 ca ở center và 7 ca ở mid) cùng MISSING (7 trường hợp), chủ yếu phát sinh từ mô hình suy luận đóng băng YOLO26m (`why = E4_model_domain`). Mô hình gốc được train trên ảnh thông thường (perspective camera), khi áp dụng trực tiếp lên ảnh fisheye có độ méo góc rộng (radial distortion) mạnh tại zone mid và edge, các đặc trưng hình học (aspect ratio, đường thẳng xe) bị biến dạng cong khiến detector dự đoán nhầm các vệt bóng râm, mái che, biển hiệu ven đường thành phương tiện (SPURIOUS). Đồng thời, trong bối cảnh giao thông hỗn hợp với mật độ cao, các đối tượng bị che khuất đan xen (occlusion) khiến mô hình bỏ sót phương tiện thật (MISSING, ví dụ các xe ThreeWheeler `L3`, `L6` ở mid/center zone). Ngoài ra, ca `adasind_014670.jpg` `L5` (Truck mép trái) xuất phát từ khoảng trống quy tắc (`why = E2_guideline_gap`) do vật bị viền ảnh cắt chỉ còn lại một phần rất nhỏ.
- Cách sửa và ai nhận việc (`owner`):
  + Đối với lỗi mô hình (`owner = ai_team`): Cần thu thập tập dữ liệu huấn luyện đặc thù cho camera fisheye góc rộng để fine-tune detector (fisheye transfer learning), kết hợp các kỹ thuật data augmentation mô phỏng độ méo thấu kính mắt cá (radial distortion augmentation) và tinh chỉnh ngưỡng confidence threshold / NMS IoU để giảm false positives (SPURIOUS).
  + Đối với khoảng trống hướng dẫn (`owner = guideline`): Cần ban hành bản vá quy chuẩn (guideline patch) làm rõ quy định gán nhãn cho các phương tiện bị cắt rìa (truncated ở mép khung hình) với tỷ lệ thấy được dưới 30% để đảm bảo tính nhất quán giữa các annotator.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):
  + Frame `adasind_014670.jpg` đối tượng `L5+R5` (Truck mép trái, x: 0–70, y: 845–1100), đối chiếu quy tắc R04 và R05; ảnh minh chứng tại `submission/screenshots/01_adasind_014670_truck_edge.png`.
  + Frame `adasind_034080.jpg` đối tượng `L8+R8` (Bike xa lề trái, x: 124–178, y: 1022–1106), đã được rework tinh chỉnh bám sát hình học ảnh cong; ảnh minh chứng tại `submission/screenshots/02_adasind_034080_k12_bike.png`.
  + Dữ liệu định lượng trích xuất từ `submission/findings.csv`, `zone_table.md` và `rework/delta.md`.
