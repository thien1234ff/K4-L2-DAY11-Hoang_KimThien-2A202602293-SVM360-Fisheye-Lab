# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Ngược sáng cực mạnh, mưa phùn bám giọt nước trên kính, vật cắt ngang đầu xe cự ly < 1.5m | Thấu kính fisheye hình cầu tán xạ ánh sáng mạnh gây lóa (flare); vật quá gần bị giãn hình học nén sát viền | Tọa độ pixel trên ảnh fisheye gốc (không nắn phẳng), kèm ma trận nội suy camera intrinsics/distortion | 2 annotator gán nhãn độc lập (double-blind) + 1 Senior Adjudicator duyệt nếu IoU < 0.7 |
| rear | Ban đêm chỉ có đèn lùi xe rọi, chướng ngại vật thấp (trụ chắn, gờ giảm tốc, trẻ em bò) | Tỷ lệ tín hiệu trên nhiễu (SNR) thấp, nhiễu hạt ISO cao, vật thể thấp bị lẫn vào bóng tối mặt đường | Hệ trục camera sau hướng nhìn xuống mặt đất, giữ nguyên thông số extrinsic pitch/roll gắn cản sau | Review chéo kết hợp tín hiệu cảm biến siêu âm (Ultrasonic sensor) để xác thực sự tồn tại của vật |
| left | Xe máy lách qua gương chiếu hậu ở góc chéo mù, bóng đổ dài ngang thân xe | Góc quan sát hướng xuống và nghiêng lớn, thấu kính gắn dưới gương xe chịu rung lắc cơ học | Không gian ảnh fisheye gốc kết hợp mask vùng thân xe `ego_body` sườn trái | Soát độc lập bởi QA chuyên trách, kiểm tra kỹ ranh giới bóng đổ vs bánh xe thực tế |
| right | Người đi bộ từ lề đường cao bước xuống mặt đường, xe đạp điện áp sát sườn phải | Chênh lệch độ cao giữa vỉa hè và lòng đường làm biến dạng hình học 3D; vật thể bị che khuất một phần | Hệ tọa độ fisheye camera sườn phải, giữ nguyên calibration và góc xoay yaw | Review chéo mù 2 vòng, đối chiếu với video chuỗi thời gian trước/sau để xác định chuyển động |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):
  + Cần làm mới (refresh) Gold Set khi: (1) Thay đổi phần cứng camera (đổi độ phân giải cảm biến, góc FOV, bước sóng hồng ngoại); (2) Hiệu chuẩn lại (re-calibration) vị trí/góc đặt rig xe (thay đổi ma trận ngoại suy extrinsics làm dịch chuyển vùng seam); (3) Thay đổi hoặc bổ sung định nghĩa trong Guideline (ví dụ điều chỉnh ngưỡng H=40px, định nghĩa lại class Rider hay ranh giới `ego_body`).
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:
  + Tình huống: Một người đi bộ đang băng qua góc chéo phía trước bên phải xe, xuất hiện đồng thời trên cả camera Front và camera Right tại vùng chồng lấn (seam overlap).
  + Policy & Evidence cần thiết: Tuyệt đối không tự ý gộp (merge) hay gán cùng một Track ID trên không gian 2D riêng lẻ từng ảnh. Việc hợp nhất đối tượng chỉ được thực hiện ở tầng Bird's-Eye-View (BEV) hoặc không gian 3D thế giới khi thỏa mãn đồng thời: (a) Timestamp giữa 2 camera được đồng bộ cứng (hardware sync, sai lệch < 5ms); (b) Ma trận Extrinsic/Intrinsic của 2 camera đã được calibrate chính xác; (c) Khoảng cách hình học 3D của 2 box khi chiếu xuống mặt phẳng đường (ground plane) nằm trong ngưỡng dung sai cho phép (sai lệch < 20cm).
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:
  + Báo cáo chất lượng (quality report) trên một camera trước (như slice ADASIND) chỉ phản ánh độ đồng thuận cục bộ trong một điều kiện cảm biến đơn lẻ. 4 camera quanh xe có góc nghiêng (pitch), độ cao lắp đặt, cường độ ánh sáng và hướng di chuyển của vật thể hoàn toàn khác nhau. Đặc biệt, các hiện tượng ở vùng giao thoa (seam distortion, chênh lệch đo sáng giữa 2 camera, ghép nối track) hoàn toàn không xuất hiện trên camera đơn lẻ, do đó không thể dùng kết quả của 1 camera để chứng minh tính chuẩn xác (Ground Truth) cho toàn bộ hệ thống SVM 360.
