# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?
   - Đây **KHÔNG PHẢI** là lỗi `DUPLICATE`, mà là hiện tượng vật lý hoàn toàn hợp lệ tại vùng chồng lấn (seam) giữa hai trường nhìn của hai cảm biến quang học độc lập.
   - Vì mỗi camera chụp vật thể từ một góc chiếu (perspective) và khoảng cách khác nhau, chịu các ma trận biến dạng quang học (lens distortion) khác nhau. Tại mỗi camera, bounding box 2D phản ánh đúng phần hình ảnh chiếu của vật thể lên mặt cảm biến đó.
   - Việc coi đây là `DUPLICATE` trên ảnh 2D là sai lầm; hệ thống cần một quy tắc riêng ở tầng trích xuất không gian 3D/BEV (Bird's-Eye-View) hoặc Sensor Fusion để hợp nhất hai quan sát 2D này thành một đối tượng thực tế duy nhất ngoài đời thực dựa trên thông số calibration.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   - **Giữ cùng Track ID:** Khi vật thể liên tục xuất hiện trong tầm nhìn và duy trì được tính liên tục về vị trí, hình dáng hoặc chuyển động mượt mà (chỉ bị che khuất tạm thời rất ngắn rồi xuất hiện lại trong cùng quỹ đạo).
   - **Thêm Keyframe:** Thêm keyframe tại các thời điểm vật thể thay đổi trạng thái hình học đột ngột (đổi hướng rẽ, tăng tốc, biến dạng do chuyển từ tâm ra mép fisheye, hoặc bị che khuất từng phần bởi vật cản).
   - **Trạng thái Outside:** Đánh dấu Outside khi vật thể đi hoàn toàn ra khỏi trường nhìn của thấu kính fisheye hoặc đi vào vùng tối viền đen `lens_border`. Nếu sau đó vật quay lại, cần đánh giá xem có đủ bằng chứng tái nhận dạng (Re-ID) không trước khi tái sử dụng track ID cũ.
   - **Bằng chứng cần thiết trước khi nối track qua 2 camera:** (a) Timestamp được đồng bộ ở mức phần cứng (hardware genlock); (b) Ma trận Extrinsic/Intrinsic calibration chính xác giữa 2 camera; (c) Quỹ đạo di chuyển 3D dự đoán (Kalman filter / motion model) khớp với vị trí xuất hiện ở camera tiếp theo tại vùng seam; (d) Thuộc tính đặc trưng ngoại hình (Re-ID feature embedding) trùng khớp.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   - **Ca thực tế:** Tại frame `adasind_086220.jpg` đối tượng `L1` (ô tô con Car màu bạc di chuyển ở làn giữa-trái, tọa độ y: 962–1011px). Cả tôi (Annotator) và mô hình AI đóng băng (YOLO26m M8) đều nhận diện đây là một phương tiện Car di chuyển rõ ràng trên đường với chiều cao đo được H=49.1px (thỏa mãn H≥40px theo R01). Tuy nhiên, Teaching Reference của bài học lại bỏ sót không gán nhãn cho xe này (`LM_noR` - SPURIOUS đối với Reference).
   - **Cách xử lý:** Thay vì tự ý xóa bỏ box để "ép số" cho khớp với Reference, tôi đã ghi nhận đây là `E0_reference_defect` với hành động `action=escalate` trong `findings.csv`, lập `submission/30_escalation_ticket.md` kèm ảnh chụp màn hình minh chứng và ghi vào `submission/40_decision_log.csv`.
   - **Nếu làm lại:** Tôi sẽ chủ động đo đạc kỹ chiều cao pixel bằng công cụ đo trước khi gán nhãn, đồng thời đối chiếu sớm với các frame lân cận để xác nhận vận tốc di chuyển của xe, giúp lập luận bảo vệ nhãn hoặc chuyển cấp (escalation) càng chặt chẽ và thuyết phục hơn.
