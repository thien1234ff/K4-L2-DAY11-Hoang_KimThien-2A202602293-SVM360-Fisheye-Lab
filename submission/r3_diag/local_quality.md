# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `f2f7562fa22d958a039dff88a5f3446413b670b701b494c846107bffb4c638cd`; slice `B2-mid`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_060000.jpg, adasind_086220.jpg, adasind_102750.jpg. Frame thiếu trong export: không.
TP=15; FP=9; FN=5; số lần đối chiếu=26; mean IoU của TP=0.852.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.577 | 0.892 | 0.808 |
| precision | 0.625 | 0.489 | 0.000 |
| recall | 0.750 | 0.556 | 0.000 |
| jaccard | 0.517 | 0.461 | 0.000 |
| dice | 0.682 | 0.516 | 0.000 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 2 | 0 | 0.923 | 0.667 | 1.000 | 0.667 | 0.800 |
| Car | 0 | 5 | 0 | 0.808 | 0.000 | 0.000 | 0.000 | 0.000 |
| Pedestrian | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 7 | 2 | 2 | 0.846 | 0.778 | 0.778 | 0.636 | 0.778 |
| Truck | 0 | 0 | 3 | 0.885 | 0.000 | 0.000 | 0.000 | 0.000 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_060000.jpg | 9 | 0 | 1 | 0.900 | 1.000 | 0.900 |
| adasind_086220.jpg | 5 | 4 | 0 | 0.556 | 0.556 | 1.000 |
| adasind_102750.jpg | 1 | 5 | 4 | 0.143 | 0.167 | 0.200 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 0 | 0 | 0 | 0 | 0 |
| Car | 0 | 0 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 4 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 1 | 0 | 7 | 0 | 1 |
| Truck | 0 | 2 | 0 | 0 | 0 | 1 |
| <extra> | 2 | 2 | 0 | 2 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
