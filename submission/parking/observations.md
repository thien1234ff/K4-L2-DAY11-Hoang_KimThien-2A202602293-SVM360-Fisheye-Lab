# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):
  + Vạch 1: Đoạn vạch sơn trắng phân chia ô đỗ ở hàng tiền cảnh (khu vực giữa-trái bãi đỗ, tọa độ y khoảng 520-565px, x khoảng 168-248px), tạo ranh giới ngăn cách trực tiếp giữa 2 ô đỗ xe cạnh nhau.
  + Vạch 2: Đoạn vạch sơn trắng phân chia ô đỗ liền kề ở hàng tiền cảnh (khu vực trung tâm, tọa độ y khoảng 518-556px, x khoảng 278-424px). Các polyline được vẽ bám sát theo phần vệt sơn nhìn thấy trên mặt đường, dừng lại tại nơi vạch sơn bị đứt đoạn hoặc mờ khuất.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao:
  + Vạch mép ngoài cùng giáp lề đường và các dải sơn giới hạn lối lưu thông xe ở phía xa: Không gán nhãn `parking_line` vì chúng chỉ đóng vai trò phân luồng giao thông hoặc phân định ranh giới mép đường/vỉa hè (curb), không tạo thành ranh giới ngăn cách của một ô đỗ xe riêng lẻ (theo quy chuẩn trong docs/11-parking-lines-vi.md).
- Polygon `free_space` dừng ở đâu; có phần bị che nào không:
  + Polygon `free_space` bao trọn vùng mặt đường nhựa trống nhìn thấy được của lối xe chạy chính (driving aisle) giữa các dãy ô đỗ.
  + Biên của polygon dừng lại chính xác tại mép đầu của các ô đỗ và gờ vỉa hè (curb); không đi xuyên qua thân xe, vật cản hay các vùng bị che khuất tầm nhìn.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”):
  + Các đoạn vạch sơn ở phía hậu cảnh xa bị mờ và suy giảm độ phân giải do góc nhìn perspective nghiêng, ranh giới kết thúc của ô đỗ không hoàn toàn rõ nét. Đã xử lý bằng cách chỉ gán nhãn các vạch nhìn thấy rõ ràng ở tiền cảnh và trung cảnh.
