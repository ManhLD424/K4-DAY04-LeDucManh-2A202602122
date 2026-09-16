# Mini guideline - nhóm: Nhóm 02  |  người gán: Lê Đức Mạnh  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Chọn `v = 1`. Ước lượng vị trí mấu chuyển lớn xương đùi dựa trên giao điểm của đường trục thân thẳng đứng (từ vai cùng bên) và trục đùi hướng lên từ đầu gối. | Khớp háng nằm sâu dưới cơ và trang phục, không có mốc da nhìn thấy trực tiếp. Nếu để `v = 0`, mô hình sẽ học mất khớp thân dưới dù người đứng trọn trong ảnh. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Chọn `v = 1`. Đặt chấm tại vị trí lỗ tai giải phẫu (ngang mức với đuôi mắt và nằm trên đường nối từ đuôi mắt tới chân tóc sau gáy). | Tóc hoặc vành mũ chỉ che bề mặt ngoài, hộp sọ vẫn định hình rõ ràng vị trí giải phẫu của tai. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp từ hông trở lên nếu thấy rõ để `v = 2`, bị che để `v = 1`. Toàn bộ khớp đầu gối và cổ chân nằm ngoài mép ảnh để `v = 0` (phím `o`) và không đặt toạ độ. | Khớp đã đi ra khỏi trường nhìn của ống kính thì không còn bằng chứng thị giác nào. Đoán bừa sẽ làm loãng và sai lệch loss hàm hồi quy toạ độ. |
| Cổ tay nằm sau tay lái / sau thân mình | Chọn `v = 1`. Đặt chấm ước lượng tại vị trí khớp cổ tay dựa trên hướng kéo dài của trục xương cẳng tay (từ khuỷu tay). | Trục cẳng tay cho ta vector định hướng rất chính xác đến khớp cổ tay, giúp mô hình học được mối quan hệ động học (kinematics). |
| Hai người chồng lên nhau | Gán dứt điểm từng người một. Khớp của người phía sau bị người phía trước che khuất thì đánh dấu `v = 1` và đặt chấm tại vị trí suy luận. Tuyệt đối không kéo nhầm sang khớp của người phía trước. | Tránh lỗi nhầm người (`nham_nguoi`), đây là nguyên nhân khiến mô hình nối xương chéo giữa hai đối tượng đứng gần nhau. |
| Người nhỏ đến mức nào thì không gán nữa | Mọi người có chiều cao bounding box $\ge 40$ pixel đều phải gán đầy đủ 17 điểm. | Trong bộ 20 ảnh core của lab, mọi người đều đủ độ phân giải để nhận diện dáng người, không bỏ sót bất kỳ ai. |

---

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_10.jpg`, người thứ `1`, khớp `left_hip`

- **Mơ hồ ở chỗ nào:** Người trong ảnh mặc áo khoác dài trùm qua mông, vạt áo buông thõng hoàn toàn che kín vùng xương chậu và khớp háng bên trái.
- **Bạn quyết thế nào:** Gán `v = 1` (Occluded), đặt chấm tại vị trí mấu chuyển lớn ước lượng (nằm trên đường dóng thẳng từ vai trái xuống và thẳng góc với trục đùi trái).
- **Vì sao:** Người này đứng thẳng trọn vẹn trong ảnh, cách mép ảnh rất xa nên khớp không thể là `v = 0`. Khung chậu người trưởng thành có tỷ lệ sinh trắc học cố định so với bề ngang của vai.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Nếu chọn `v = 0`, mô hình sẽ học rằng khi con người mặc áo dài/áo khoác thì họ "không có khớp hông", dẫn đến việc khi suy luận mô hình sẽ triệt tiêu độ tự tin ở thân dưới.

### Ca 2 - ảnh `train_06.jpg`, người thứ `1`, khớp `right_ear`

- **Mơ hồ ở chỗ nào:** Người quay đầu nghiêng góc 3/4, tóc mái dài và bóng đổ che phủ hoàn toàn tai phải, không thấy vành tai.
- **Bạn quyết thế nào:** Gán `v = 1` (Occluded), chấm tại điểm ước lượng nằm ngang hàng với sống mũi - đuôi mắt phải và đối xứng với tai trái qua trục đầu.
- **Vì sao:** Vị trí của mắt phải và sống mũi nhìn thấy rất rõ ràng (`v = 2`), kết hợp với góc nghiêng đầu cho phép định vị chính xác vị trí tai phải bị che.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Nếu chọn `v = 0`, mô hình sẽ dự đoán thiếu khớp tai khi người nghiêng đầu, làm mất khả năng ước lượng góc quay đầu (head pose).

### Ca 3 - ảnh `train_11.jpg`, người thứ `1`, khớp `right_wrist`

- **Mơ hồ ở chỗ nào:** Cánh tay phải để xuôi tự nhiên và bàn tay hơi khuất ra phía sau đùi/hông, phần cổ tay bị che lấp bởi nếp gấp quần áo.
- **Bạn quyết thế nào:** Gán `v = 1`, đặt chấm tại vị trí đầu mút cẳng tay phải (nơi tiếp giáp với bàn tay ước lượng).
- **Vì sao:** Xương cẳng tay từ khuỷu tay phải (`v = 2`) đi thẳng xuống cho một vector chuyển động rất rõ ràng, điểm kết thúc của cẳng tay chính là khớp cổ tay.
- **Nếu người khác quyết ngược lại thì model học sai cái gì:** Mô hình sẽ ngắt cụt xương tay ở khuỷu tay hoặc đoán cổ tay trôi dạt sang bàn tay của người bên cạnh.

---

## 4. Sau khi so visibility report với bạn cùng nhóm

- **Khớp lệch `%v=1` nhiều nhất:** `left_hip` (bạn `28%` / họ `14%`, lệch `14%`) và `left_ear` (bạn `45%` / họ `27%`, lệch `18%`).
- **Nguyên nhân là guideline chưa rõ hay một trong hai bên gán sai:** Cả hai nguyên nhân. Về guideline, ban đầu nhóm chưa định nghĩa rõ ràng thế nào là "khớp còn trong khung nhưng bị che", dẫn đến bạn cùng nhóm bấm phím `o` (Outside - `v = 0`) khi không thấy trực quan. Về thao tác, bạn cùng nhóm có thói quen xoá điểm hoặc né tránh ước lượng các điểm khó.
- **Luật mới bổ sung vào mục 2 sau khi thống nhất:**
  * **Luật bao quát về cờ Visibility:** Bất cứ điểm giải phẫu nào mà phần thân liên đới còn nằm trong biên ảnh, bắt buộc phải dùng `v = 1` và đặt chấm ước lượng dựa trên trục xương liền kề. Cờ `v = 0` chỉ áp dụng khi và chỉ khi tâm khớp nằm ngoài 4 cạnh của khung hình ảnh.
