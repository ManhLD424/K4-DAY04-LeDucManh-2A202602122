# Đánh giá bài gán nhãn của bạn cùng nhóm (Peer Review)

Người gán: **Nguyễn Văn An**   Người kiểm: **Lê Đức Mạnh**   Ngày: **16/09/2026**

---

## 1. Reviewer Checklist

*Kiểm tra tính hợp lệ về cấu trúc và trực quan theo hướng dẫn tại [reports/REVIEWER_CHECKLIST.md](file:///d:/Project/VinPrj/K4-DAY04-LeDucManh-2A202602122/reports/REVIEWER_CHECKLIST.md):*

| STT | Mục kiểm | Đạt? | Ghi chú / Ảnh nào |
| :---: | :--- | :---: | :--- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ Đạt | Đủ 20 ảnh, mọi skeleton đều đủ 17 điểm |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ Đạt | Không có lỗi đảo trái/phải (`dao_trai_phai`) |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ Đạt | Không có lỗi nhầm người (`nham_nguoi`) |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☒ Cần sửa | Một số khớp tai và hông bị che vẫn để `v = 0` |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☒ Cần sửa | `train_06`, `train_10`, `train_11` |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ Đạt | Không có cờ ẩn không hợp lệ |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số/người | ☑ Đạt | Bản export chuẩn COCO Keypoints |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ Đạt | Định dạng chuẩn Ultralytics |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ Đạt | Đã chạy `tools/visibility_report.py --compare` |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ Đạt | Đã thống nhất bổ sung quy ước che khuất |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ Đạt | 0 lỗi định dạng |

---

## 2. Danh sách lỗi cụ thể tìm được

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| :--- | :---: | :--- | :--- | :--- |
| `train_06.jpg` | 1 | `left_ear`, `right_ear` | Xoá khớp bị che (`v = 0`) khi tóc mái phủ kín tai | Bấm `q` đổi sang `v = 1` và đặt chấm ước lượng ngang mức đuôi mắt |
| `train_10.jpg` | 1 | `left_hip`, `right_hip` | Để `v = 0` khi người mặc áo khoác dài trùm hông | Bấm `q` đổi sang `v = 1`, đặt chấm tại giao điểm trục thân và xương đùi |
| `train_11.jpg` | 1 | `right_wrist` | Để `v = 0` khi cổ tay nằm sau đùi | Bấm `q` đổi sang `v = 1`, định vị điểm tiếp giáp theo trục kéo dài từ khuỷu tay |
| `train_13.jpg` | 1 | Toàn bộ skeleton | Bị bỏ sót người ở góc trái rìa ảnh | Vẽ thêm 1 skeleton `person` mới, gán đầy đủ 17 điểm |

---

## 3. Kết luận

- **Lỗi lặp đi lặp lại nhiều nhất của bài này:** Lỗi sử dụng cờ Outside (`v = 0`) cho các khớp bị che khuất thay vì dùng Occluded (`v = 1`) và đặt chấm ước lượng giải phẫu.
- **Nó là lỗi thao tác hay lỗi guideline chưa rõ:** Phần lớn là do **guideline ban đầu của nhóm chưa được thống nhất chặt chẽ**. Người gán nhãn có tâm lý chỉ gán những gì "mắt nhìn thấy rõ ràng", dẫn đến việc ngần ngại suy luận vị trí các khớp bị che. Sau khi họp thống nhất lại guideline, người gán đã hiểu rõ và tiến hành rework chuẩn xác.
