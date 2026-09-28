# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 9 | 2 | 3 | 5 | 8 | SPURIOUS (3) |
| mid | 8 | 1 | 4 | 4 | 10 | SPURIOUS (2) |
| edge | 3 | 2 | 2 | 2 | 0 | WRONG_CLASS (2) |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên:
  + Đối với người gán nhãn (L): Vùng `mid` và `center` có số lượng box thừa (spurious) nhiều nhất (4 ở mid, 3 ở center). Vùng `edge` dù chỉ có 3 reference objects nhưng tỷ lệ lỗi rất cao (2 missing, 2 spurious) với lỗi nổi bật là `WRONG_CLASS (2)`.
  + Đối với model (M): Gãy nặng nhất ở vùng `mid` (10 box thừa, 4 missing) và `center` (8 box thừa, 5 missing). Ở vùng `edge`, model có 2 missing trên tổng số 3 ref objects.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame:
  + Vùng `edge`: Biến dạng quang học (radial distortion) rất lớn của thấu kính fisheye làm các vật thể bị kéo cong và giãn tỷ lệ méo mó, khiến cả người và mô hình gặp khó khăn trong việc nhận diện đúng class (ví dụ nhầm giữa Truck và Car/ThreeWheeler) cũng như xác định biên box.
  + Vùng `center` và `mid`: Giao thông mật độ dày, các vật thể kích thước trung bình và nhỏ đan xen nhau (Rider và xe, người đi bộ). Người gán nhãn dễ bị thừa box do phân tách nhầm người ngồi trên xe máy thành box riêng, trong khi model thường sinh box trùng lặp (duplication) hoặc nhận nhầm các chi tiết đường phố thành vật thể.
  + Giới hạn: Tập dữ liệu slice chỉ gồm 3 frame trên một camera trước (front fisheye) của tập ADASIND tại một tuyến đường đô thị Ấn Độ, cỡ mẫu nhỏ nên các chỉ số đo lường mang tính khảo sát cục bộ của buổi học, không đại diện cho hiệu năng tổng thể của toàn hệ thống SVM 360 4 camera.
