# Mini guideline - nhóm: Cabin Pose  |  người gán: Vũ Minh Kiệt  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | Ước lượng dựa vào nếp gấp đũng quần và đường thắt lưng, gióng ngang sang mấu chuyển lớn xương đùi, tick v=1 (hoặc v=2 nếu thấy rõ form cơ thể) | Quần áo dài che mất mốc xương; nếu không ước lượng giải phẫu thì cả bài sẽ mất 100% khớp hông |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu mốc lỗ tai ngoài ước lượng được từ góc hàm và gò má -> tick `v = 1` và chấm điểm | Tai vẫn nằm trong khung hình, chỉ bị tóc/mũ che mờ; model cần học liên kết đầu-tai |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Toàn bộ các khớp chân nằm ngoài mép ảnh tick `v = 0` (Outside) và không chấm điểm | Khớp không còn trong khung hình; chấm bừa ngoài ảnh làm hỏng tỉ lệ bounding box |
| Cổ tay nằm sau tay lái / sau thân mình | Gióng theo trục cẳng tay, chấm điểm ngay sau vật cản và tick `v = 1` | Bàn tay và cẳng tay liên tục; vị trí cổ tay hoàn toàn xác định được bằng giải phẫu |
| Hai người chồng lên nhau | Gán từng người độc lập; khớp của người sau bị người trước đè lên tick `v = 1` | Đảm bảo nguyên tắc mỗi skeleton đủ 17 điểm, không nối nhầm xương giữa 2 người |
| Người nhỏ đến mức nào thì không gán nữa | Gán tất cả mọi người có mặt trong khoang xe; người đi bộ ngoài xe chỉ gán khi lộ rõ thân mình | Tập dữ liệu cabin tập trung vào hành khách và người lái xe |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_11.jpg`, người thứ `1`, khớp `left_wrist`

- Mơ hồ ở chỗ nào: Cổ tay trái của người lái với sang bệ điều khiển trung tâm, bị khuất sau cần số và màn hình.
- Bạn quyết thế nào: Ước lượng theo phương của cẳng tay trái và chấm điểm với cờ `v = 1`.
- Vì sao: Cẳng tay trái nhìn thấy rõ hướng di chuyển; cổ tay chắc chắn nằm ngay sau cụm cần số.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu để `v = 0`, model sẽ học sai rằng cổ tay đã ra ngoài mép ảnh và không học được tư thế với tay lái xe.

### Ca 2 - ảnh `train_14.jpg`, người thứ `1`, khớp `left_hip`

- Mơ hồ ở chỗ nào: Người lái mặc áo khoác mùa đông dày trùm kín eo và đùi, ngồi lọt thỏm trong ghế thể thao ôm sát.
- Bạn quyết thế nào: Xác định ranh giới gấp đùi khi ngồi, chấm mốc hông trái với cờ `v = 1`.
- Vì sao: Dù áo khoác che khuất bề mặt, điểm quay của khớp háng luôn nằm tại vị trí gập giữa thân trên và xương đùi.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu bỏ qua hoặc xóa điểm, khung xương thân dưới bị đứt đoạn, model mất khả năng kết nối cột sống - hông - đầu gối.

### Ca 3 - ảnh `train_16.jpg`, người thứ `2`, khớp `right_eye`

- Mơ hồ ở chỗ nào: Người ngồi ghế phụ quay góc 3/4, sống mũi che khuất một phần vùng mắt phải.
- Bạn quyết thế nào: Chấm điểm mắt phải với cờ `v = 2` ngay tại vị trí hốc mắt nhìn thấy được cạnh sống mũi.
- Vì sao: Khi zoom 200%, vùng đồng tử và khóe mắt phải vẫn hiển thị rõ ràng trên nền kính cửa sổ.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu để `v = 0`, model sẽ coi như mặt người chỉ có một mắt ở mọi góc nghiêng 3/4.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `23%` / họ `16%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline ban đầu của nhóm chưa quy định cụ thể trường hợp tóc dài trùm tai, bên bạn để `v = 1` còn đối tác nhầm lẫn để `v = 0`.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Trường hợp tóc trùm tai nhưng đầu vẫn trong khung hình thì 100% chọn `v = 1` có chấm điểm, cấm dùng `v = 0`.
