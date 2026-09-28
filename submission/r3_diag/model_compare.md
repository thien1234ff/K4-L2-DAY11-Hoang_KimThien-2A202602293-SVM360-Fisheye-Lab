# L / R / M

L/R/M là thứ tự box in-scope theo XML của từng frame.

## adasind_060000.jpg
- L5+R4: LR_noM (edge)
- L8+R6+M8: LRM (mid)
- L9+R10+M5: LRM (mid)
- L6+R3: LR_noM (center)
- L2+R2: LR_noM (mid)
- L10+R1: LR_noM (center)
- L7+R8: LR_noM (center)
- L4+R7+M6: LRM (center)
- L3+R5+M4: LRM (mid)
- R9: R_only (center)
- M2: M_only (mid)
- M3: M_only (mid)
- M7: M_only (mid)
- M9: M_only (center)
- M10: M_only (center)
- M11: M_only (center)
- M12: M_only (center)
## adasind_086220.jpg
- L7+R1: LR_noM (mid)
- L8+R5: LR_noM (mid)
- L2+R2+M1: LRM (center)
- L3+R3+M4: LRM (center)
- L4+R4: LR_noM (center)
- L1+M8: LM_noR (center)
- L5: L_only (center)
- L6: L_only (edge)
- L9: L_only (mid)
- M2: M_only (center)
- M3: M_only (center)
- M5: M_only (mid)
- M6: M_only (mid)
- M7: M_only (mid)
## adasind_102750.jpg
- L3+R3: LR_noM (mid)
- L1: L_only (edge)
- L2: L_only (center)
- L5+M3: LM_noR (mid)
- L6: L_only (mid)
- L7: L_only (mid)
- R1+M1: RM_noL (mid)
- R2+M2: RM_noL (edge)
- R4: R_only (edge)
- R5+M8: RM_noL (center)
- M4: M_only (mid)
- M5: M_only (mid)
- M6: M_only (center)
- M7: M_only (mid)

## Zone × cell
| zone | LRM | LR_noM | LM_noR | L_only | RM_noL | R_only | M_only |
|---|---|---|---|---|---|---|---|
| center | 3 | 4 | 1 | 2 | 1 | 1 | 7 |
| mid | 3 | 4 | 1 | 3 | 1 | 0 | 9 |
| edge | 0 | 1 | 0 | 2 | 1 | 1 | 0 |
