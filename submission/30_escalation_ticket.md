# Escalation ticket

## Ticket 1

- **Frame:** adasind_014670.jpg (đối tượng Truck L5, tọa độ x: 0–70 px, y: 845–1100 px)
- **Ảnh chụp:** submission/screenshots/01_adasind_014670_truck_edge.png
- **Expected impact:** Ảnh hưởng tới 3–5% đối tượng nằm ở vùng rìa ngoài (edge zone) tiếp giáp viền cắt của camera mắt cá. Nếu không có tiêu chuẩn rõ ràng về tỷ lệ phần thân nhìn thấy tối thiểu, annotator và QA sẽ liên tục xung đột giữa việc gán nhãn hay bỏ qua, dẫn đến việc mô hình AI học phải các mẫu dữ liệu khuyết thiếu đặc trưng trầm trọng, làm tăng tỷ lệ cảnh báo sai (False Positive) hoặc bỏ sót xe thật ở rìa quan sát SVM.
- **Owner:** guideline
- **Recommendation:** Phê duyệt đề xuất Guideline Patch R12: Quy định rõ các phương tiện bị viền khung hình cắt xén (truncated) chỉ được vẽ box nếu phần thân nhìn thấy đạt ít nhất 20% hoặc có tối thiểu 2 đặc trưng nhận diện rõ ràng; các trường hợp dưới ngưỡng này phải được bao bằng polygon `ignore_region` với `reason = unreadable` thay vì ép vẽ box phương tiện.
