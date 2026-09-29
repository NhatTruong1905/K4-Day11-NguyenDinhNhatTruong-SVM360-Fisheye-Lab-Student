# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 7 | 0 | 0 | 3 | 7 | — |
| mid | 11 | 0 | 0 | 5 | 7 | — |
| edge | 2 | 0 | 0 | 0 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên:
  + Phía người gán nhãn (L): Đạt độ khớp 100% với reference ở cả 3 zone (L missing = 0, L spurious = 0 trên toàn bộ 20 đối tượng n_ref: 7 center, 11 mid, 2 edge) nhờ tuân thủ nghiêm ngặt rule H=40 và quy tắc phân loại 6 class.
  + Phía mô hình (M): Bị gãy nhiều nhất ở hai zone `mid` và `center`. Cụ thể:
    * Zone `mid`: M bỏ sót (missing) tới 5/11 đối tượng (chiếm 45.5%) và phát hiện thừa 7 box giả (false positives).
    * Zone `center`: M bỏ sót 3/7 đối tượng (chiếm 42.9%) và sinh ra 7 box thừa.
    * Zone `edge`: Do số lượng mẫu tham chiếu ít (n_ref=2), M không bỏ sót đối tượng nào (M missing = 0) nhưng vẫn xuất hiện 1 box thừa.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame:
  + Giả thuyết nguyên nhân: Mô hình YOLO26m pre-trained được huấn luyện chủ yếu trên ảnh rectilinear thông thường; khi áp dụng lên ảnh fisheye với độ méo xuyên tâm (radial distortion) mạnh ở zone mid, các đường nét hình học bị uốn cong làm feature representation bị lệch khiến model gãy box hoặc phân loại nhầm. Ở vùng center/mid, mật độ giao thông hỗn hợp đông đúc khiến xe hai bánh (Bike), xe ba bánh (ThreeWheeler) và người đi bộ bị che khuất (occluded) đan xen nhau, dẫn tới NMS của model kích hoạt sai sinh ra nhiều box trùng lặp/thừa (15 box thừa tổng cộng trên 3 frame).
  + Giới hạn của slice 3 frame: Kích thước mẫu rất nhỏ (chỉ 3 frame, 20 objects), phân bố đối tượng không đồng đều giữa các zone (edge chỉ có 2 objects), do đó các tỷ lệ đo lường mang tính chất định tính học tập cục bộ trên slice được giao, không đại diện đầy đủ cho phân phối dữ liệu toàn bộ dataset hay hệ thống camera 360 thực tế.
