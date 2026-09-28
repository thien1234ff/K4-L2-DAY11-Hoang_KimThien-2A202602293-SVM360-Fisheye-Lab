# Escalation ticket

## Ticket 1

- **Frame:** adasind_086220.jpg (Object ref: L1)
- **Ảnh chụp:** submission/screenshots/escalation_adasind_086220_car.jpg
- **Expected impact:** Nếu reference bỏ sót phương tiện di chuyển hợp lệ trên đường, mô hình AI khi học sẽ bị phạt sai (False Positive giả) và dẫn đến suy giảm độ nhạy phát hiện xe con ở làn đường đối diện.
- **Owner:** guideline
- **Recommendation:** Bổ sung box `Car` cho đối tượng này vào Teaching Reference / Gold Set, hoặc làm rõ ngưỡng H=40px và độ tương phản tối thiểu trong guideline để tránh bất đồng giữa annotator, mô hình và người duyệt ground truth.
