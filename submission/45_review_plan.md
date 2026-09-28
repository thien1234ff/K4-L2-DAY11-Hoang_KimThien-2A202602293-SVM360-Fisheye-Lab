# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| adasind_102750.jpg (Vùng Edge / Left border) | 3 ca WRONG_CLASS (L1, L5, L7) và 1 ca IGNORE_SCOPE (L4) | Vùng biên mép ngoài thấu kính fisheye chịu độ biến dạng quang học (radial distortion) cực đại, khiến hình dạng xe tải nhỏ và ô tô bị bẻ cong; đồng thời tiếp giáp với mũi xe ego nên rất dễ vi phạm lỗi phạm vi nhãn. | Screenshot submission/screenshots/case_adasind_102750_edge.jpg và mã lỗi trong compare.html |
| adasind_086220.jpg (Vùng Mid & Center) | 4 ca SPURIOUS (L1, L5, L6, L9) và 1 ca ATTRIBUTE (L7) | Mật độ giao thông đông đúc, nhiều đối tượng nhỏ ở xa dưới ngưỡng H=40px gây sai số box thừa, và xe ThreeWheeler lớn bị che khuất một phần cần chốt rõ quy tắc occluded. | Screenshot submission/screenshots/escalation_adasind_086220_car.jpg và bảng local_quality_conflicts.csv |

Giới hạn của kết luận từ ba frame ADASIND: Ba frame dữ liệu chỉ được ghi nhận từ một camera đơn góc rộng gắn phía trước (Front Camera) tại một điều kiện ánh sáng ban ngày và một cung đường đô thị cụ thể của Ấn Độ. Không thể suy diễn trực tiếp các phát hiện này cho toàn bộ hệ thống SVM 360 gồm 4 camera (đặc biệt là camera hông và camera sau có góc đặt, độ cao và điểm mù hoàn toàn khác biệt).

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
- Để kiểm soát độ phủ cho 200 frame, cần lấy mẫu phân tầng (stratified sampling) trên 50.000 frame giả lập dựa trên các trục: 4 vị trí camera (`front`, `rear`, `left`, `right`) × 2 mức độ khó (`normal`, `hard`).
- Tránh trùng lặp chuỗi thời gian (temporal correlation): Không chọn các frame liền kề trong cùng 1 clip video ngắn; mỗi frame lấy mẫu phải cách nhau tối thiểu một khoảng thời gian (stride ≥ 5 giây hoặc ≥ 50 frame) để đảm bảo tính độc lập thống kê về bối cảnh giao thông.
- Kế hoạch 200 frame tập trung vào phát hiện các ca lỗi biên (edge cases, hard cases) và thẩm định guideline, không thể đại diện hoàn toàn cho phân phối thực tế của 50.000 frame để tính toán tỷ lệ lỗi tuyệt đối (error rate) của toàn hệ thống trong môi trường sản xuất.
