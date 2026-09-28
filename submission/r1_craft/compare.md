# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_060000.jpg
- L1 edge IGNORE_SCOPE
- R9 center MISSING
## adasind_086220.jpg
- L7+R1 mid ATTRIBUTE
- L1 center SPURIOUS
- L5 center SPURIOUS
- L6 edge SPURIOUS
- L9 mid SPURIOUS
## adasind_102750.jpg
- L4 edge IGNORE_SCOPE
- L1+R2 edge WRONG_CLASS
- L2 center SPURIOUS
- L5+R4 edge WRONG_CLASS
- L6 mid SPURIOUS
- L7+R1 mid WRONG_CLASS
- R5 center MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 9 | 7 | 2 | 3 |
| mid | 8 | 7 | 1 | 4 |
| edge | 3 | 1 | 2 | 2 |
