# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm
- Khóa/lớp: Khóa 4 (K4) - AI Engineer Lab
- Tên nhóm: Nhóm K4-Day11-NguyenDinhNhatTruong
- Repo Public: https://github.com/NhatTruong1905/K4-Day11-NguyenDinhNhatTruong-SVM360-Fisheye-Lab-Student
- Máy giữ hồ sơ chính / người quản lý: Nguyễn Đình Nhật Trường
- Slice chung lấy từ mode.json: B1-mid
- Tên định danh vai A dùng cho --self: truong
- Kênh trao đổi nội bộ: Discord / Telegram K4 AI Lab
- Đại diện nộp (vai C): Bùi Việt Nam, MSSV: 2A202602272
- Commit chốt bài: be698ff5a9f6346c548d86f668f24b6cb18972e4

## 2. Ba vai chính
| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Nguyễn Đình Nhật Trường | 2A202602321 | truong | Tạo task, vẽ parking/C0/slice B1-mid, self-QC, export, lock, rework | `submission/parking/annotations.xml`, `submission/r1_craft/annotations.xml`, `submission/r1_craft/lock.txt` (mã 4B57-BF25), `submission/r1_craft/selfqc.md`, `submission/rework/annotations-v2.xml` |
| B · QA độc lập | Nguyễn Quang Huy | 2A202602243 | huy | Review trước reference theo ảnh và guideline, finding QA r2_qa, kiểm lại ca sửa | `submission/r2_qa/qa_review.md`, `submission/r2_qa/qa_overlay.html`, các dòng round r2_qa trong `submission/findings.csv`, ảnh minh chứng trong `submission/screenshots/` |
| C · Chẩn đoán & điều phối | Bùi Việt Nam | 2A202602272 | nam | Chọn/cố định slice, theo dõi mốc P0-P6, chạy báo cáo r3_diag sau QA, phân xử L/R/M, lập hồ sơ kế hoạch và nộp | `submission/r3_diag/`, `submission/10_error_card.md`, `submission/20_guideline_patch.md`, `submission/30_escalation_ticket.md`, `submission/40_decision_log.csv`, `submission/45_review_plan.md`, `submission/45_sampling_plan.csv`, `submission/46_gold_set_plan.md`, `submission/50_exit_ticket.md`, `submission/manifest.json` |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha
| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | `mode.json`, slice `B1-mid`, bảng phân vai | A, B xác nhận cấu hình slice B1-mid, môi trường CVAT local sẵn sàng | Đạt mốc, không có vướng mắc môi trường |
| P2 · Khóa bản đầu | A → B, C | `r1_craft/annotations.xml`, `lock.txt`, mã khóa `4B57-BF25` | B, C kiểm tra đúng mã khóa 4B57-BF25, đủ 3 frame, đủ 9 mục self-QC và 4 đối tượng fill K12 | Hoàn thành gán nhãn slice chính đúng hạn |
| P3 · Chốt QA mù | B → C, A | `r2_qa/qa_review.md`, dòng finding r2_qa, ảnh screenshots | C kiểm tra đủ frame/object_ref/rule_id, bằng chứng rõ ràng, tuân thủ QA mù chưa mở reference | Chốt QA độc lập, chuyển giao sang chẩn đoán |
| P4 · Quyết định sửa | C → A, B | `local_quality.md`, `zone_table.md`, `findings.csv`, `40_decision_log.csv` | A, B đối chiếu từng xung đột L/R/M, thống nhất nguyên nhân why, severity, owner và action | Thống nhất phân xử 4 ca; 1 ca escalate lên guideline |
| P5 · Kiểm bản sửa | A → B → C | `rework/annotations-v2.xml`, `lock2.txt`, `delta.md` | B kiểm tra lại đúng ca sửa geometry L8+R8, C kiểm tra bảng số delta trước/sau | Hoàn thành rework, không sửa nhãn đúng |
| P6 · Chốt nộp | A, B → C | Toàn bộ thư mục `submission/`, `manifest.json`, commit chốt | Cả ba thành viên kiểm tra chéo, chạy `lab11.py check` exit code 0 | Hồ sơ hoàn chỉnh, failed_gates rỗng |

## 4. Bất đồng và phối hợp
- **Một ca đã phân xử:** Frame `adasind_014670.jpg` đối tượng `L5` (xe tải Truck mép trái, x: 0–70 px, y: 845–1100 px). Annotator A gán nhãn Truck với thuộc tính `truncated=true` vì chiều cao H=255 px (thỏa mãn quy tắc R01 H ≥ 40 px). Tuy nhiên QA Reviewer B nhận định bề rộng chỉ 70 px nhìn thấy quá ít đặc trưng nhận dạng cấu trúc của xe tải. Người điều phối C đã chủ trì đối chiếu ảnh gốc và quy tắc, đi đến quyết định giữ nguyên nhãn hiện tại của A để không bỏ sót nguy cơ va chạm, đồng thời mở Escalation Ticket 1 (`30_escalation_ticket.md`) và đề xuất Guideline Patch R12 (`20_guideline_patch.md`) quy định ngưỡng diện tích nhìn thấy tối thiểu 20% cho các vật bị cắt rìa khung hình.
- **Ca còn mở:** Không còn ca tồn đọng chưa phân xử; tất cả 4 quyết định đã được ghi nhận đầy đủ trong `submission/40_decision_log.csv`.
- **Đóng góp của A/B/C vào kế hoạch và exit ticket:**
  - Thành viên A (gán nhãn - Nguyễn Đình Nhật Trường): Đóng góp kinh nghiệm thực tế về thao tác bounding box và polygon trên CVAT, chỉ ra các vùng méo rìa thấu kính fisheye gây khó khăn khi xác định biên vật thể để đóng góp vào mục ca khó của kế hoạch gold set.
  - Thành viên B (QA độc lập - Nguyễn Quang Huy): Đóng góp các tiêu chuẩn kiểm tra chéo độc lập, đề xuất ngưỡng IoU thẩm định 0.85 cho kế hoạch gold set và phương pháp phát hiện khoảng trống quy chuẩn.
  - Thành viên C (điều phối - Bùi Việt Nam): Xây dựng ma trận phân bổ 200 frame cho 4 camera theo rủi ro vận hành, thiết lập kế hoạch gold set 4 camera, và tổng hợp câu trả lời sâu sắc cho 3 câu hỏi trong exit ticket (`50_exit_ticket.md`).
- **Thay đổi phân công nếu có:** Phân công ba vai A, B, C được giữ nguyên vẹn trong suốt toàn bộ quá trình thực hiện từ P0 đến P6 nhằm đảm bảo tính khách quan và độc lập của vòng QA.

## 5. Xác nhận trước khi nộp
- [x] A xác nhận nhãn và export đúng phiên bản: Nguyễn Đình Nhật Trường (MSSV: 2A202602321) / Đã kiểm tra `r1_craft/annotations.xml` và `rework/annotations-v2.xml` export đúng định dạng CVAT for images 1.1, mã khóa khớp.
- [x] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Nguyễn Quang Huy / Đã QA mù độc lập trên overlay `r2_qa/qa_overlay.html`, ghi nhận finding r2_qa và kiểm lại ca sửa `adasind_034080.jpg L8` tại `rework/delta.md`.
- [x] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: Bùi Việt Nam / Đã chạy `lab11.py triage`, `lab11.py status`, `lab11.py check` xác nhận đạt exit code 0.
- [x] manifest.json tại commit chốt có failed_gates rỗng.
- [x] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [x] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
