# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 14.76 khớp có v > 0 mỗi người
- Tổng: v=2 348 | v=1 80 | v=0 65

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 1 | 6 | 3% |
| 1 | left_eye | 20 | 3 | 6 | 10% |
| 2 | right_eye | 23 | 0 | 6 | 0% |
| 3 | left_ear | 13 | 13 | 3 | 45% |
| 4 | right_ear | 18 | 9 | 2 | 31% |
| 5 | left_shoulder | 26 | 3 | 0 | 10% |
| 6 | right_shoulder | 29 | 0 | 0 | 0% |
| 7 | left_elbow | 22 | 3 | 4 | 10% |
| 8 | right_elbow | 24 | 4 | 1 | 14% |
| 9 | left_wrist | 18 | 6 | 5 | 21% |
| 10 | right_wrist | 21 | 6 | 2 | 21% |
| 11 | left_hip | 20 | 8 | 1 | 28% |
| 12 | right_hip | 23 | 5 | 1 | 17% |
| 13 | left_knee | 18 | 6 | 5 | 21% |
| 14 | right_knee | 20 | 4 | 5 | 14% |
| 15 | left_ankle | 16 | 4 | 9 | 14% |
| 16 | right_ankle | 15 | 5 | 9 | 17% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
