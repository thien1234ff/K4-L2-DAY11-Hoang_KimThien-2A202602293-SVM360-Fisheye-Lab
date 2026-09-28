# QA review · B2-mid

Mã khóa: F2F7-562F

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_060000.jpg | L1 | R03 | Rider ngồi trên xe máy ở tiền cảnh trái được gộp chung thành một box Bike; box ôm sát phần nhìn thấy và đánh dấu truncated=true do mép kính cắt. |
| adasind_086220.jpg | L7 | R05 | ThreeWheeler kích thước lớn bên phải khung hình có một phần bị người/vật khác che khuất nhẹ; cần kiểm tra lại thuộc tính occluded đã đặt đúng chưa. |
| adasind_102750.jpg | L1 | R05 | Xe ở mép trái ngoài cùng bị vòng kính và biên khung hình cắt một phần thân; cần đảm bảo thuộc tính truncated=true theo định nghĩa hình học R05. |
| adasind_102750.jpg | L4 | R02 | Box Bike tiền cảnh góc dưới trái ôm sát viền nhìn thấy trên ảnh fisheye gốc, không nắn thẳng theo coordinate perspective. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
