# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): 
  1) Vạch chia ô đỗ ở tiền cảnh trung tâm (tọa độ từ x=400, y=652 đến x=530, y=720), phân định ranh giới giữa hai ô đỗ xe phía trước.
  2) Vạch chia ô đỗ ở tiền cảnh bên phải (tọa độ từ x=720, y=622 đến x=955, y=686), tạo ranh giới cho ô đỗ góc phải của hàng đỗ tiền cảnh.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: Dải sơn mờ phân làn/chỉ hướng xe chạy ở hậu cảnh xa phía sau và vạch mép lề gần hàng rào cây xanh. Không vẽ vì chúng là vạch hướng dẫn lưu thông của lối đi chung trong bãi đỗ, không phải vạch phân chia từng ô đỗ riêng lẻ theo quy tắc phân định tại docs/11-parking-lines-vi.md.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: Polygon `free_space` bao phủ vùng mặt đường nhựa phẳng trống nhìn thấy được của lối xe chạy giữa hai dãy ô đỗ (khoảng x: 120-750, y: 560-640). Polygon dừng lại ngay trước ranh giới vạch ô đỗ, không chạy xuyên qua lề cỏ, cây cối, hàng rào hay chiếc xe ô tô đỏ ở xa. Không có vật cản che khuất trong diện tích polygon này.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): Các đoạn vạch ô đỗ ở hậu cảnh rất xa (gần xe ô tô màu đỏ) bị suy giảm độ tương phản do ánh sáng và khoảng cách lớn, cần xác nhận ngưỡng chiều dài tối thiểu hoặc mức độ hiển thị để quyết định có gán nhãn hay bỏ qua.
