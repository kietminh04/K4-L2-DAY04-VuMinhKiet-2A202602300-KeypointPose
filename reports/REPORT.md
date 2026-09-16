# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Vũ Minh Kiệt   Nhóm: Khóa 4 (VinAI / VinUni) - Nhóm Cabin Pose   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 31 |
| v=2 / v=1 / v=0 | 415 / 64 / 48 |
| Thời gian trung bình mỗi ảnh | ~4.0 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear`: 23% (v=2: 20, v=1: 7, v=0: 4)
2. `right_ear`: 19% (v=2: 23, v=1: 6, v=0: 2) & `left_elbow`: 19% (v=2: 25, v=1: 6, v=0: 0)
3. `left_wrist` / `left_knee` / `right_knee` / `left_ankle`: 16% (5 khớp v=1 mỗi loại)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Ba khớp trên có tỷ lệ bị che khuất (`%v=1`) cao nhất xuất phát trực tiếp từ góc máy hẹp trong khoang cabin xe ô tô. Người ngồi trong xe thường hướng mặt nghiêng hoặc quay người về phía kính lái/vô-lăng, khiến tai phía xa và cánh tay phía đối diện thường xuyên bị che khuất bởi đầu, tựa đầu hoặc bệ tỳ tay trung tâm. Tuy nhiên, các khớp này không phải là khớp khó xác định vị trí giải phẫu nhất; khớp khó gán nhất trên thực tế là hông (`left_hip`, `right_hip`). Hông người lái và hành khách luôn bị quần áo dài (quần tây/quần jeans) và thành ghế ép sát che hoàn toàn bề mặt xương, đòi hỏi người gán phải ước lượng gián tiếp từ nếp gấp đũng quần và đường thắt lưng thay vì nhìn thấy mốc xương trực tiếp.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9607 | 0.9632 |
| OKS@0.50 | 0.9355 | 0.9355 |
| OKS@0.75 | 0.9355 | 0.9355 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_16.jpg` + người thứ 2 + keypoint `right_eye` + Thao tác: Bổ sung tọa độ mốc mắt phải (x=0.5703, y=0.2529) với trạng thái v=2 (nhìn thấy rõ) thay vì để v=0 (outside), do đối chiếu nhãn Gold cho thấy ở góc nghiêng 3/4 của khuôn mặt người lái, một phần đồng tử mắt phải vẫn hiển thị rõ ràng trên ảnh.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh (0 lỗi ở cả trước và sau khi so khớp với nhãn Gold). Ngay từ khâu warm-up và thiết lập ban đầu, nguyên tắc giải phẫu học cơ thể (Anatomical Perspective: góc nhìn từ chính cơ thể người được gán, không nhìn theo màn hình) đã được áp dụng triệt để, kết hợp với công cụ kiểm tra màu xương (xanh = bên trái, cam = bên phải) qua `tools/visualize_pose.py`.

## 3. Kiểm chéo

Bạn cùng nhóm: N/A (Bài làm thực hiện độc lập)

Khớp lệch `%v=1` nhiều nhất cần rà soát theo guideline:

| Khớp | %v=1 quan sát | Tiêu chuẩn đề ra | Lệch | Nguyên nhân & Hướng xử lý |
| --- | ---: | ---: | ---: | --- |
| `left_ear` | 23% | 15% | 8% | Do góc quay nghiêng của người lái và tóc che một phần vành tai |
| `left_elbow` | 19% | 12% | 7% | Do khuỷu tay chạm mép bệ điều khiển trung tâm trong cabin |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi chuẩn hóa:

- Khi tóc, mũ bảo hiểm hoặc tựa ghế che khuất một phần vành tai nhưng mốc giải phẫu lỗ tai ngoài vẫn ước lượng được dựa trên cung gò má và góc hàm, bắt buộc tick `Occluded` (v=1) và chấm điểm ước lượng, tuyệt đối không tick `Outside` (v=0).

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   - `pose_mAP50-95` tăng +0.0055 (từ 0.6853 lên 0.6908) và `pose_precision` tăng +0.0058 (từ 0.9734 lên 0.9792).
   - 20 ảnh nhãn sạch chất lượng cao đã dạy cho mô hình thích nghi với phân phối tư thế người ngồi co chân và đặt tay trên vô-lăng trong cabin xe—một miền dữ liệu mà tập COCO gốc ngoài trời không bao quát hết. Việc `box_mAP` giảm nhẹ (-0.0078) là bình thường do tập train 20 ảnh có quy mô nhỏ làm mô hình co cụm bounding box quanh cabin, nhưng độ chuẩn xác điểm mốc (keypoints) được cải thiện rõ rệt.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?
   - `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) khoảng 0.1133 (11.33%).
   - Model tìm *người* (bounding box) dễ hơn tìm *khớp* (keypoints) rất nhiều. Bounding box bao trọn cơ thể có diện tích lớn, nhiều đặc trưng ngữ nghĩa tổng thể (khuôn mặt, thân áo, tương phản với nền ghế). Ngược lại, khớp giải phẫu là các điểm pixel đơn lẻ, dễ bị che khuất bởi vô-lăng, táp-lô hoặc ghế xe và có tính mơ hồ cao.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   - Ở ảnh `test_02.jpg`, mô hình gặp lỗi **lệch nhẹ** ở khớp cổ chân phải (`right_ankle`) của người ngồi ghế sau: chân bị vùng tối dưới gầm ghế trước che khuất khiến mô hình ước lượng lệch khoảng 15 pixel về phía chân ghế.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   - Ảnh có OKS thấp nhất là `train_11.jpg` (OKS = 0.724).
   - Nhãn của tôi đúng hơn. Trong ảnh này, cánh tay người lái đang với ra bệ trung tâm và bị khuất sau cần số; tôi đã ước lượng dựa trên trục cẳng tay và đặt cờ v=1, trong khi mô hình bị bóng phản chiếu trên kính bảng điều khiển đánh lừa và kéo khớp cổ tay lệch hẳn ra ngoài khoảng không.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?
   - Có, ảnh `train_03.jpg` là ảnh tôi có OKS so với gold thấp nhất (0.872) và cũng là ảnh model đoán lệch số người nhiều nhất (model đoán 4 người trong khi nhãn có 3 người).
   - Điều này phản ánh bức ảnh `train_03.jpg` có độ phức tạp thị giác rất cao: góc chụp chéo từ ngoài cửa kính, độ phản quang kính xe lớn và có một người đứng ngoài xe lọt một phần đầu vào khung hình, tạo ra sự mơ hồ giải phẫu cho cả người gán lẫn mô hình trí tuệ nhân tạo.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người, khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Trong ảnh `train_05.jpg`, người thứ 1 (người lái xe), khớp cổ tay phải (`right_wrist`):
1. **Căn cứ thị giác**: Nhìn thấy toàn bộ cánh tay và khuỷu tay phải hướng về phía 3 giờ của vô-lăng; bàn tay đang cầm lấy vô-lăng nhưng phần cổ tay bị vành vô-lăng kim loại và bệ táp-lô che khuất trực diện.
2. **Cơ sở giải phẫu**: Dựa vào trục giải phẫu thẳng hàng của xương quay và xương trụ cẳng tay, điểm khớp cổ tay chắc chắn nằm ngay phía sau vành vô-lăng và nằm sâu trong khung hình (cách mép ảnh hơn 200 pixel).
3. **Quyết định**: Điểm mốc còn trong khung hình nhưng bị vật thể che khuất nên tôi chọn trạng thái `Occluded` (v=1) kèm chấm tọa độ ước lượng giải phẫu, tuyệt đối không chọn `Outside` (v=0).
