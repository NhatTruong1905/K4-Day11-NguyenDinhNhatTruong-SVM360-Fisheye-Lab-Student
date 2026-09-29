# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `adasind_014670.jpg` (Edge & Mid zone) | 2 ca MISSING (Truck L5 cắt rìa E2_guideline_gap, ThreeWheeler L3 do model domain) và 3 ca SPURIOUS từ model | Vùng rìa mắt cá (edge zone) chịu độ méo quang học tối đa, vật thể bị cắt xén (truncated) giáp viền ảnh dễ gây xung đột quy tắc và sai số detector nặng nhất | Screenshot `01_adasind_014670_truck_edge.png`, XML coordinates box L5 (x: 0..70), overlay compare và checklist self-QC R05 |
| `adasind_034080.jpg` (Center & Mid zone) | 4 ca MISSING (Bike L5, ThreeWheeler L7, L9), 6 ca SPURIOUS, 1 ca BOX_GEOMETRY (Bike L8 đã rework thành công) | Mật độ giao thông hỗn hợp đông đúc nhất, hiện tượng che khuất (occlusion) đan xen phức tạp giữa xe máy, xe ba bánh và người đi bộ gây rủi ro an toàn cao nhất | Screenshot `02_adasind_034080_k12_bike.png`, báo cáo delta `rework/delta.md`, bảng confusion matrix và zone table |

Giới hạn của kết luận từ ba frame ADASIND: Bộ dữ liệu chỉ gồm 3 frame tĩnh trích xuất từ 1 camera đơn hướng phía trước trong điều kiện ban ngày khô ráo tại Ấn Độ. Dữ liệu hoàn toàn thiếu camera sau và hai camera sườn (hông gương), không có thông tin góc nhìn mặt đất sát thân xe, không có chuỗi thời gian để đánh giá tracking hay bài toán ghép ảnh Bird's-Eye View (BEV). Do đó, tỷ lệ lỗi ở đây mang tính định tính phục vụ chẩn đoán quy chuẩn, không đại diện cho năng lực tổng thể của hệ thống 4 camera SVM.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
- **Kiểm soát độ phủ và chống trùng lặp mẫu:** 200 frame được phân bổ có chủ đích trên 4 camera (Front, Rear, Left, Right) × 2 phân loại (Normal, Hard). Để tránh hiện tượng lấy các frame liên tiếp trong cùng một chuỗi video làm méo mó thống kê (video correlation bias), quy trình lấy mẫu áp dụng bước nhảy thời gian tối thiểu 3 giây (tương đương 30–90 frame) giữa các cảnh. Đồng thời thiết lập ma trận kiểm tra độ phủ (coverage matrix) trên các trục: điều kiện ánh sáng (nắng chói, ngược sáng, đêm tối), thời tiết (mưa đọng giọt trên thấu kính fisheye, sương mù), và kịch bản vận hành (đỗ xe song song, lùi vào chuồng hẹp, điểm mù sát sườn xe).
- **Ý nghĩa của tập lấy mẫu:** Kế hoạch 200 frame này là phương pháp lấy mẫu phân tầng hướng tới ca khó (stratified edge-case stress sampling). Mục đích cốt lõi là chủ động "săn tìm" các điểm gãy hệ thống (boundary edge cases, seam stitching failures, occluded vulnerable road users) để rà soát và khắc phục quy chuẩn. Tập mẫu này không phải là mẫu ngẫu nhiên đồng nhất (uniform random sampling) từ không gian 50.000 frame, nên không dùng để suy diễn thống kê tỷ lệ lỗi trung bình (general error rate) mà là công cụ rà soát rủi ro chất lượng nghiêm ngặt trước khi đóng gói sản phẩm.
