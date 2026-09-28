# Guideline patch

- **Rule mới đề xuất:** R12 — Phân định ranh giới vật thể đè mép `ego_body`: Bất kỳ phương tiện hoặc người nào có phần diện tích tiếp xúc với vùng `ego_body` nhưng phần thân trên nhìn thấy được trên mặt đường có chiều cao H ≥ 40px thì chỉ được vẽ bounding box ôm phần thân trên nằm ngoài `ego_body`; nếu diện tích vật thể nằm bên trong polygon `ego_body` ≥ 50% thì áp dụng quy tắc R09 (không vẽ box, coi như don't-care).
- **Áp dụng cho:** Tất cả các class (`Bike`, `Pedestrian`, `Car`, `ThreeWheeler`) xuất hiện ở khu vực mép đáy khung hình tiếp giáp với phần mũi xe ego.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật hiện hành R06 và R07 chỉ quy định vẽ polygon `ego_body` và R09 cấm box nằm ≥50% trong ignore, nhưng chưa hướng dẫn cụ thể cho annotator cách xử lý hình học (crop box hay vẽ trùm) đối với các xe máy/người ở tiền cảnh bị mũi xe ego che khuất một phần bánh/chân (như trường hợp L1 ở frame `adasind_060000.jpg` và L4 ở `adasind_102750.jpg`), dẫn đến tranh cãi giữa annotator và QA về lỗi `IGNORE_SCOPE`.
- **`rules_version` mới:** v1.1.0 (nâng cấp từ v1.0.0)
- **Hiệu lực từ:** Round `rework` trở đi.
