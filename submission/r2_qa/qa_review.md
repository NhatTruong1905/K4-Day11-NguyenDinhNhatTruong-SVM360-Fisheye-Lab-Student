# QA review · B1-mid

Mã khóa: 4B57-BF25

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_014670.jpg | L1 | R05 | Xe ô tô ở rìa phải (edge zone) bị cắt bởi biên khung hình và vòng kính fisheye, attribute truncated=true là chính xác theo R05. Box ôm sát phần vỏ xe nhìn thấy. |
| adasind_032280.jpg | L1 | R03 | Người điều khiển xe hai bánh ở tiền cảnh trung tâm; theo quy tắc R03 toàn bộ xe máy và người lái được gộp thành một box Bike duy nhất, không vẽ tách Pedestrian. |
| adasind_034080.jpg | L3 | R01 | Người đi bộ ở lề phải (edge zone), chiều cao H > 40 px (thực tế ~195 px), box bám sát phần cơ thể nhìn thấy trên ảnh fisheye gốc theo R01 và R02. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
