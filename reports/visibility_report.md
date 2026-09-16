# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 31 skeleton, trung bình 15.45 khớp có v > 0 mỗi người
- Tổng: v=2 415 | v=1 64 | v=0 48

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 4 | 3 | 13% |
| 1 | left_eye | 21 | 5 | 5 | 16% |
| 2 | right_eye | 22 | 2 | 7 | 6% |
| 3 | left_ear | 20 | 7 | 4 | 23% |
| 4 | right_ear | 23 | 6 | 2 | 19% |
| 5 | left_shoulder | 31 | 0 | 0 | 0% |
| 6 | right_shoulder | 31 | 0 | 0 | 0% |
| 7 | left_elbow | 25 | 6 | 0 | 19% |
| 8 | right_elbow | 26 | 5 | 0 | 16% |
| 9 | left_wrist | 25 | 5 | 1 | 16% |
| 10 | right_wrist | 26 | 2 | 3 | 6% |
| 11 | left_hip | 29 | 2 | 0 | 6% |
| 12 | right_hip | 29 | 1 | 1 | 3% |
| 13 | left_knee | 23 | 5 | 3 | 16% |
| 14 | right_knee | 23 | 5 | 3 | 16% |
| 15 | left_ankle | 19 | 5 | 7 | 16% |
| 16 | right_ankle | 18 | 4 | 9 | 13% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
