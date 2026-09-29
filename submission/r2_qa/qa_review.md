# QA review · B1-mid

- **Reviewer độc lập (Vai B):** Nguyễn Quang Huy (MSSV: 2A202602243)
- **Chủ nhãn / Người vẽ (Vai A):** Nguyễn Đình Nhật Trường (MSSV: 2A202602321 - truong)
- **Slice:** B1-mid
- **Mã khóa bản P2 khóa:** 4B57-BF25

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_014670.jpg | L1 | R05 | **Điều nhìn thấy:** Xe ô tô (`Car`) ở rìa phải (edge zone) bị cắt bởi biên khung hình và vành kính fisheye. **Điều cần kiểm lại:** Thuộc tính `truncated=true` và `occluded=false` đã được gán chính xác; bounding box bám sát phần vỏ xe thực tế nhìn thấy trên ảnh fisheye gốc, không nắn thẳng theo R02 và R05. Đạt yêu cầu. |
| adasind_014670.jpg | L5 | R04 | **Điều nhìn thấy:** Một phần đầu ca-pô xe tải (`Truck`) ở mép cực trái (x: 0–70 px), bị cắt xén gần như toàn bộ phần thân sau bởi biên ảnh. **Điều cần kiểm lại:** Chiều cao H=255 px đạt ngưỡng R01 (H ≥ 40 px), annotator gán Truck với `truncated=true`. Tuy nhiên bề ngang chỉ 70 px lộ quá ít đặc trưng nhận dạng cấu trúc. Cần chuyển giao sang pha P4 để đối chiếu reference/model và xem xét đề xuất Guideline Patch R12 về ngưỡng nhận diện vật cắt rìa. |
| adasind_032280.jpg | L1 | R03 | **Điều nhìn thấy:** Người điều khiển xe hai bánh ở tiền cảnh trung tâm (center zone) đang ngồi lái phương tiện. **Điều cần kiểm lại:** Tuân thủ đúng quy tắc R03: toàn bộ xe máy và người lái được gộp chung thành một bounding box `Bike` duy nhất, không vẽ tách box `Pedestrian` riêng biệt. Không có box trùng lặp. Đạt yêu cầu. |
| adasind_034080.jpg | L3 | R01 | **Điều nhìn thấy:** Người đi bộ (`Pedestrian`) đứng ở lề đường bên phải (edge zone), cơ thể nhìn thấy trọn vẹn theo chiều dọc. **Điều cần kiểm lại:** Chiều cao H thực tế ~195 px (vượt xa ngưỡng tối thiểu H ≥ 40 px của R01). Bounding box bao bọc chính xác từ đỉnh đầu tới chân nhìn thấy trên ảnh mắt cá theo R01 và R02. Đạt yêu cầu. |
| adasind_034080.jpg | L8 | R01 | **Điều nhìn thấy:** Xe hai bánh (`Bike`) nhỏ ở xa mép lề trái, viền box ban đầu còn hơi rộng so với viền cong của xe do biến dạng méo quang học. **Điều cần kiểm lại:** Cần ghi nhận vào pha chẩn đoán P4 để phân xử và thực hiện rework tinh chỉnh lại tọa độ box ôm khít hơn vào phần nhìn thấy của xe. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
