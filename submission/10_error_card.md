# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | MISSING | 8 |
| center | B2 | SPURIOUS | 13 |
| center | C0 | SPURIOUS | 2 |
| center | C0 | WRONG_CLASS | 1 |
| edge | B2 | IGNORE_SCOPE | 4 |
| edge | B2 | MISSING | 3 |
| edge | B2 | SPURIOUS | 3 |
| edge | B2 | WRONG_CLASS | 2 |
| edge | C0 | WRONG_CLASS | 1 |
| mid | B2 | ATTRIBUTE | 2 |
| mid | B2 | MISSING | 5 |
| mid | B2 | SPURIOUS | 15 |
| mid | B2 | WRONG_CLASS | 2 |
| mid | C0 | WRONG_CLASS | 1 |

## Top defects
- SPURIOUS: 33 (ví dụ frame adasind_019560.jpg)
- MISSING: 16 (ví dụ frame adasind_060000.jpg)
- WRONG_CLASS: 7 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:
  + Lỗi SPURIOUS (33 ca) và MISSING (16 ca): Phần lớn thuộc về `E4_model_domain` do model YOLO26m pre-trained chưa thích ứng hoàn toàn với hệ tọa độ cong và méo hình học cực đoan của thấu kính fisheye, dẫn đến ảo giác sinh box thừa trên chi tiết mặt đường hoặc bỏ sót các vật bị nén ở rìa. Một phần thuộc về `E1_annotator_error` khi người gán nhãn vẽ các vật thể quá nhỏ dưới ngưỡng H=40px hoặc phân tách nhầm người ngồi trên xe máy.
  + Lỗi WRONG_CLASS (7 ca): Nổi bật ở vùng `edge` (ví dụ `adasind_102750.jpg` L1), nơi biến dạng quang học làm biến đổi tỷ lệ chiều dài/rộng của phương tiện, khiến người gán nhãn nhầm lẫn giữa Truck (xe tải nhỏ/pickup) và Car.
- Cách sửa và ai nhận việc (`owner`):
  + `annotator`: Cần sử dụng công cụ đo kiểm pixel để loại bỏ các box dưới H=40px (R01); kiểm tra kỹ đặc điểm thùng hàng/kết cấu để phân biệt Truck vs Car theo R04; đảm bảo rider ngồi trên xe gom về 1 box Bike duy nhất (R03).
  + `ai_team`: Cần bổ sung tập dữ liệu huấn luyện fisheye đa dạng (domain adaptation) và áp dụng cơ chế attention cho các vật thể méo ở biên ngoài thấu kính.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):
  + Ảnh minh chứng: `submission/screenshots/case_adasind_102750_edge.jpg` và `submission/screenshots/escalation_adasind_086220_car.jpg`.
  + Dòng findings: Các dòng round `r1_craft` và `r3_diag` cho frame `adasind_102750.jpg` (L1, L5) và `adasind_086220.jpg` (L1).
  + Quy tắc áp dụng: R01 (ngưỡng chiều cao), R02 (vẽ trên ảnh gốc), R04 (ánh xạ class), R06 (phạm vi ignore_region).
