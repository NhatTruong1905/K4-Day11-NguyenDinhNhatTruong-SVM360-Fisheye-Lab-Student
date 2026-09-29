# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?
   - **Trả lời:** Đây KHÔNG phải là lỗi `DUPLICATE`, mà là một trường hợp hoàn toàn hợp lệ bắt buộc phải có **quy tắc riêng (cross-camera seam policy)**.
   - **Vì sao:** Trong hệ thống Surround View Monitoring (SVM), mỗi camera fisheye gắn ở một vị trí vật lý độc lập (trước, sau, hai bên sườn) với góc nhìn (perspective), độ cao và trường méo quang học khác nhau. Khi một đối tượng (ví dụ xe hơi hoặc người đi bộ) di chuyển vào vùng quan sát chồng lấn (seam) giữa hai camera kề cận (như Front và Left), việc cả hai camera cùng quan sát thấy và ghi nhận bounding box bám sát phần thân hiển thị trên ảnh 2D gốc của từng camera là hoàn toàn chính xác theo nguyên lý đo lường cảm biến. Nếu quy chụp đây là lỗi `DUPLICATE` rồi xóa một trong hai box trên ảnh 2D, ta sẽ làm mất dữ liệu huấn luyện cục bộ của camera bị xóa, khiến mạng nơ-ron trên camera đó mất khả năng nhận diện vật thể ở vùng rìa. Quy tắc đúng đắn là: giữ nguyên hai box độc lập ở tầng gán nhãn 2D raw camera; việc hợp nhất (fusion) hoặc khử trùng lặp định danh sẽ do tầng 3D BEV / Multi-Camera Tracking phía sau thực hiện dựa trên tính toán hình học không gian.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   - **Giữ cùng track ID:** Khi cùng một đối tượng vật lý liên tục xuất hiện qua chuỗi frame và duy trì được đặc tính nhận dạng (identity) nhất quán, kể cả khi đối tượng tạm thời bị che khuất một phần (occluded nhẹ trong vài frame ngắn) nhưng vẫn xác định được quỹ đạo chuyển động logic.
   - **Thêm keyframe:** Khi đối tượng có sự thay đổi đột ngột về hình thái hình học (ví dụ xe quay đầu từ nhìn thẳng sang nhìn nghiêng), kích thước thay đổi lớn do di chuyển từ vùng tâm ra rìa méo quang học của ống kính mắt cá, hoặc khi thuộc tính trạng thái thay đổi (chuyển từ không bị che sang bị che khuất).
   - **Gán trạng thái Outside:** Khi đối tượng hoàn toàn rời khỏi vùng trường nhìn hữu ích của camera (di chuyển vượt qua mép khung hình hoặc đi vào vùng vành đen `lens_border`), hoặc bị vật thể khác che khuất hoàn toàn 100% trong thời gian dài. Trạng thái Outside giúp thuật toán tracking đóng track cũ một cách chuẩn xác, tránh việc nội suy tuyến tính sai lệch qua các vùng mù.
   - **Bằng chứng cần thiết trước khi nối track qua hai camera:** Bắt buộc phải có đủ 4 yếu tố:
     1) *Timestamp đồng bộ:* Khung hình của cả hai camera phải được chụp tại cùng một thời điểm với độ trễ cực thấp (hardware trigger mili-giây).
     2) *Ma trận hiệu chuẩn ngoại vi (Extrinsics):* Thông số vị trí, góc xoay của 2 camera so với gốc tọa độ xe (ego vehicle frame) đã được căn chỉnh chính xác.
     3) *Quỹ đạo động học liên tục (Kinematic continuity):* Vị trí 3D, hướng di chuyển và vận tốc của vật thể khi rời camera A phải tương thích logic với vị trí xuất hiện trên camera B.
     4) *Đặc trưng ngoại hình tương đồng (Visual Re-ID features):* Màu sắc, chủng loại, kiểu dáng phương tiện phải đồng nhất giữa hai góc nhìn sau khi đã bù trừ sai lệch ánh sáng.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   - **Dẫn chứng ca cụ thể:** Frame `adasind_014670.jpg`, đối tượng `L5` (`Truck`, tọa độ x: 0–70 px, y: 845–1100 px ở mép cực trái của khung hình). Đây là phần đầu cabin của một chiếc xe tải bị cắt ngang bởi biên ảnh dọc. Chiều cao H đạt tới 255 px (vượt xa ngưỡng R01 H ≥ 40 px), nhưng chiều rộng chỉ có 70 px (chỉ thấy một vệt mỏng ca-pô/bội xe). Trong khi người gán nhãn căn cứ theo quy tắc chiều cao để vẽ box và gán `truncated = true`, reference hoặc người soát có thể coi đây là vật thể không đủ đặc trưng để định danh.
   - **Cách đã xử lý:** Tôi quyết định không tự ý xóa bỏ hay nhượng bộ một cách thiếu căn cứ. Thay vào đó, tôi giữ nguyên box theo đúng tiêu chí R01 và R05, ghi nhận trường hợp này vào `findings.csv` với chẩn đoán `why = E2_guideline_gap` (khoảng trống quy chuẩn), mở Escalation Ticket 1 (`30_escalation_ticket.md`) và soạn thảo bản vá Guideline Patch R12 (`20_guideline_patch.md`) nhằm đề xuất bổ sung ngưỡng tỷ lệ nhìn thấy tối thiểu 20% cho các vật bị cắt rìa. Cách làm này vừa bảo vệ tính logic của dữ liệu hiện tại, vừa giải quyết triệt để vấn đề ở cấp độ quy trình cho cả đội ngũ.
   - **Nếu làm lại slice này:** Tôi sẽ chủ động rà soát sớm các đối tượng nằm sát viền kính và mép ảnh ngay từ pha P0/P1 trước khi xuất bản khóa; đồng thời chủ động trao đổi với QA reviewer về các trường hợp ranh giới (borderline cases) nhằm thống nhất cách hiểu trước khi tiến hành gán nhãn hàng loạt, giúp tiết kiệm thời gian tranh luận và hạn chế tối đa các bước rework không cần thiết.
