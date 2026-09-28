# Sensor context

- Rig: Camera fisheye đơn góc siêu rộng (ultra-wide FOV, tầm nhìn ~180°), được gắn ở vị trí phía trước xe ego (khu vực cản trước/lưới tản nhiệt hoặc mép nắp ca-pô), hướng nhìn trực diện về phía trước bao quát toàn cảnh mặt đường và vật thể xung quanh đầu xe.
- `ego_body`: Nhìn thấy một phần cản trước / nắp ca-pô của xe ego ở chính giữa mép đáy phía dưới của khung hình (chiếm một vùng cong nhỏ ở đáy của 46/48 frame ADASIND; ngoại trừ 2 frame 006840 và 271039 không thấy thân xe). Vùng này cần được gán polygon `ignore_region` với reason là `ego_body`.
- Vòng kính (lens circle): Vòng tròn quang học của ống kính fisheye nằm ở trung tâm khung hình, chiếm khoảng 80%–85% diện tích ảnh. Bốn góc ngoài vòng tròn quang học là vùng viền đen (vignetting do thấu kính) được bao phủ bởi 2 polygon `ignore_region` với reason là `lens_border`.
