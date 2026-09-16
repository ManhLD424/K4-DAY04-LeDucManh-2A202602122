# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lê Đức Mạnh   Nhóm: Nhóm 02   Ngày: 16/09/2026

> Báo cáo đánh giá chất lượng gán nhãn tư thế người (COCO-17), kiểm chứng OKS với Gold Reference và đánh giá hiệu năng mô hình YOLO26-Pose sau fine-tune.

---

## 1. Nhãn của tôi

*Số liệu trích xuất trực tiếp từ [outputs/visibility_report.json](file:///d:/Project/VinPrj/K4-DAY04-LeDucManh-2A202602122/outputs/visibility_report.json) và [reports/visibility_report.md](file:///d:/Project/VinPrj/K4-DAY04-LeDucManh-2A202602122/reports/visibility_report.md) sau khi hoàn tất gán nhãn 20 ảnh core.*

| Chỉ số | Giá trị |
| :--- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 348 / 80 / 65 |
| Thời gian trung bình mỗi ảnh | ~4.5 phút / ảnh |

Ba khớp có `%v=1` cao nhất:

1. **`left_ear`**: 45% (13 khớp `v=1`, 13 khớp `v=2`, 3 khớp `v=0`)
2. **`right_ear`**: 31% (9 khớp `v=1`, 18 khớp `v=2`, 2 khớp `v=0`)
3. **`left_hip`**: 28% (8 khớp `v=1`, 20 khớp `v=2`, 1 khớp `v=0`)

**Nhận xét:**
Ba khớp trên phản ánh rất chính xác hai nhóm nguyên nhân che khuất khác nhau trong thực tế. Khớp tai (`left_ear`, `right_ear`) là khớp **hay bị che vật lý** bởi tóc dài, mũ đội đầu hoặc góc quay nghiêng của khuôn mặt; tuy nhiên vị trí giải phẫu của tai rất dễ xác định dựa trên mốc đuôi mắt và góc xương hàm dưới. Ngược lại, khớp hông (`left_hip`, `right_hip`) vừa **bị che khuất** bởi trang phục (áo dài, áo khoác, váy) vừa là khớp **khó xác định vị trí giải phẫu nhất** vì cơ thể người không có mốc bề mặt nhìn thấy trực tiếp tại ổ cối xương chậu, buộc người gán phải suy luận dựa trên trục cột sống và hướng xương đùi.

---

## 2. Chấm với gold

*Số liệu so sánh đối chiếu giữa [outputs/eval_vs_gold_before_rework.json](file:///d:/Project/VinPrj/K4-DAY04-LeDucManh-2A202602122/outputs/eval_vs_gold_before_rework.json) và [outputs/eval_vs_gold.json](file:///d:/Project/VinPrj/K4-DAY04-LeDucManh-2A202602122/outputs/eval_vs_gold.json):*

| Chỉ số | Trước rework | Sau rework |
| :--- | ---: | ---: |
| OKS trung bình | 0.9195 | **0.9642** |
| OKS@0.50 | 0.9655 | **1.0000** |
| OKS@0.75 | 0.9310 | **1.0000** |
| Lỗi `dao_trai_phai` | 0 | **0** |
| Lỗi `nham_nguoi` | 0 | **0** |
| Lỗi `xoa_khop_bi_che` | 9 | **0** |
| Lỗi `thieu_khop` | 5 | **0** |
| Lỗi `thieu_nguoi` | 1 | **0** |

**Tôi đã sửa gì giữa hai lần chạy:**

- `train_13.jpg`: Bổ sung người thứ 3 ở góc trái bức ảnh (`bbox: [0.093, 0.612, 0.128, 0.548]`) bị bỏ sót do người này đứng khuất ở rìa ảnh.
- `train_02.jpg`: Người 1, khớp `left_ear` bổ sung ước lượng cờ `v=1` do bị tóc che phủ.
- `train_06.jpg`: Người 1, khớp `left_ear` và `right_ear` chuyển từ `v=0` sang `v=1` (ước lượng giải phẫu theo trục đối xứng hộp sọ).
- `train_09.jpg`: Người 1, khớp `left_eye` chuyển từ `v=0` sang `v=1` (mặt nghiêng che khuất mắt trái nhưng đầu vẫn nằm trọn trong khung hình).
- `train_10.jpg`: Người 1, khớp `left_hip` và `right_hip` chuyển từ `v=0` sang `v=1` (áo khoác che phủ vùng xương chậu).
- `train_11.jpg`: Người 1, khớp `right_wrist`, `left_hip`, `right_hip` chuyển từ `v=0` sang `v=1` (cổ tay sau thân và hông mặc quần áo tối màu).
- `train_13.jpg`: Người 2, khớp `left_knee` chuyển từ `v=0` sang `v=1` (đầu gối bị vật cản che một phần).
- `train_16.jpg`: Người 2, khớp `nose` và `right_eye` chuyển từ `v=0` sang `v=1` do góc quay 3/4.
- `train_19.jpg`: Người 2, khớp `left_ear` và `right_ear` chuyển từ `v=0` sang `v=1`.
- `train_20.jpg`: Người 1, khớp `right_wrist` hiệu chỉnh dịch tâm chấm về đúng khớp cổ tay theo trục cẳng tay (khắc phục lỗi `lech_nhe`).

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào:**
Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh. Toàn bộ các khớp đối xứng (vai, khuỷu, cổ tay, hông, gối, cổ chân) đều được kiểm tra nghiêm ngặt theo quy tắc cơ thể người ngay từ Chặng 2 và Chặng 4 qua công cụ `visualize_pose.py`.

---

## 3. Kiểm chéo

Bạn cùng nhóm: **Nguyễn Văn An (Nhóm 02)**

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| :--- | ---: | ---: | ---: | :--- |
| `left_hip` | 28% | 14% | 14% | **Guideline chưa rõ ràng**: Bạn cùng nhóm có xu hướng để `v=0` khi hông mặc quần thụng tối màu, trong khi luật của lớp yêu cầu còn trong khung hình thì phải đặt chấm ước lượng `v=1`. |
| `left_ear` | 45% | 27% | 18% | **Thao tác**: Bạn cùng nhóm bấm phím `o` (Outside) khi tóc phủ kín tai thay vì bấm `q` (Occluded). |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Bất kỳ khớp nào bị che khuất bởi trang phục, tóc, phụ kiện hoặc vật cản nhưng phần cơ thể liên đới vẫn nằm trong biên giới ảnh thì **bắt buộc chọn cờ `v=1` và đặt chấm tại vị trí ước lượng giải phẫu hợp lý**. Cờ `v=0` chỉ được dùng duy nhất khi tâm khớp đã thực sự nằm vượt ra ngoài 4 mép ảnh.

---

## 4. Model

*Số liệu trích xuất từ [outputs/eval_model.json](file:///d:/Project/VinPrj/K4-DAY04-LeDucManh-2A202602122/outputs/eval_model.json) sau khi fine-tune 80 epoch trên Google Colab GPU T4:*

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| :--- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | **+0.0055** |
| pose_precision | 0.9734 | 0.9792 | **+0.0058** |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**
   - Chỉ số `pose_mAP50-95` tăng từ `0.6853` lên `0.6908` (tăng **+0.0055**, tương đương tăng **+0.55%**), đồng thời `pose_precision` tăng từ `0.9734` lên `0.9792` (+0.58%).
   - Mặc dù tập train chỉ gồm 20 ảnh (rất nhỏ so với quy mô COCO), kết quả không hề bị suy giảm (catastrophic forgetting) mà còn tăng nhẹ về độ chính xác vị trí khớp. Điều này chứng minh chất lượng nhãn sau rework đạt độ nhất quán giải phẫu cao ($OKS = 0.9642$), cờ `v=1` được áp dụng chuẩn mực giúp mô hình củng cố khả năng dự đoán các khớp bị che khuất trên tập test mà không làm xáo trộn các trọng số đặc trưng hình học sẵn có.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?**
   - `box_mAP50-95` đạt `0.8041` trong khi `pose_mAP50-95` đạt `0.6908`, chênh lệch **0.1133** (khoảng **11.3%**).
   - Mô hình tìm **người** (Bounding Box) dễ hơn rất nhiều so với tìm **khớp** (Keypoints). Nguyên nhân là vì việc phát hiện hộp bao người chỉ yêu cầu trích xuất đặc trưng ngữ nghĩa vùng ở cấp độ vĩ mô (tỷ lệ chiều cao/rộng, hình bóng đầu-thân-chân, sự tách biệt với nền). Ngược lại, xác định 17 keypoint đòi hỏi sự chính xác vi mô đến từng pixel, phải chống chịu tốt trước các biến dạng co gập phức tạp của tứ chi, các hiện tượng tự che khuất (self-occlusion) và góc xoay 3D của cơ thể người.

3. **Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):**
   - Trong ảnh `test_03.jpg`, người ở hậu cảnh đang di chuyển nhanh, mô hình xuất hiện lỗi **"lệch nhẹ" (slight offset)** ở khớp cổ tay phải (`right_wrist`). Do bàn tay bị mờ chuyển động (motion blur) và lẫn vào phông nền phía sau, điểm dự đoán bị lệch ra ngoài khớp cổ tay khoảng 8-10 pixel về phía đầu ngón tay.

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**
   - Bức ảnh có OKS thấp nhất giữa nhãn của tôi và mô hình là `train_13.jpg` (OKS đạt ~0.88).
   - **Nhãn của tôi đúng.** Tôi dựa vào bằng chứng giải phẫu học và ngữ cảnh không gian: ở người đứng bên trái bị che khuất một phần chân bởi bàn/ghế, con người có thể liên kết hướng đi của cẳng chân và đùi để định vị tâm khớp gối với cờ `v=1`. Trong khi đó, mô hình thị giác chỉ bắt các điểm ảnh có độ tương phản cao nên bị nhiễu bởi các cạnh thẳng của đồ vật, dẫn đến việc dự đoán khớp gối bị tụt xuống thấp hơn so với thực tế.

5. **Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**
   - **Có**, ảnh `train_13.jpg` vừa là bức ảnh trước rework tôi bị sót 1 người ở rìa ảnh, vừa là ảnh mô hình đạt OKS so sánh thấp nhất.
   - Điều này chỉ ra rằng `train_13.jpg` là một bức ảnh có **độ phức tạp trực quan cực kỳ cao (Hard Sample)**: độ phân giải cục bộ thấp, ánh sáng yếu, mật độ đối tượng dày đặc chen lấn nhau và người bị cắt ngang mép ảnh. Đối với các mẫu khó như vậy, cả người gán nhãn lẫn mô hình máy học đều gặp thử thách lớn, đòi hỏi phải có hướng dẫn gán nhãn (guideline) tường minh và cơ chế chú ý (attention) mạnh hơn.

---

## 5. Một rule evidence bạn đã dùng

**Tình huống phân tích:** Ảnh `train_10.jpg`, người thứ 1, khớp **`left_hip` (hông trái)**.

- **Bằng chứng nhìn thấy:** Người trong ảnh mặc áo khoác len dài trùm qua mông và quần ống rộng sẫm màu. Bề mặt trực quan hoàn toàn không để lộ mấu chuyển lớn xương đùi hay nếp gấp bẹn.
- **Căn cứ quyết định:** Dựa vào hai mốc giải phẫu nhìn thấy rõ là vai trái (`left_shoulder`, `v=2`) và đầu gối trái (`left_knee`, `v=2`), tôi dựng trục chịu lực thẳng đứng của thân trên và trục xương đùi. Toàn bộ cơ thể người này nằm gọn bên trong khung hình (khoảng cách tới mép ảnh gần nhất > 15% bề rộng ảnh), do đó khớp háng chắc chắn 100% không thể rơi ra ngoài biên ảnh.
- **Quyết định trạng thái:** Tôi quyết định chọn **`v = 1` (Occluded)** và đặt chấm tại giao điểm ước lượng giải phẫu giữa trục thân và trục đùi (ngay dưới mép áo khoác), kiên quyết **không chọn `v = 0` (Outside)** vì không vi phạm đường biên khung hình.
