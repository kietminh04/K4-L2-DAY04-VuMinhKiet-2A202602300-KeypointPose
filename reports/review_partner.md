# Biên bản Đánh giá Chéo Bài Làm của Bạn Cùng Nhóm

- **Người thực hiện gán nhãn**: Nguyễn Hà My (`hami210`)
- **Người kiểm tra & đánh giá**: Vũ Minh Kiệt (`2A202602300`)
- **Ngày đánh giá**: 16/09/2026

---

## 1. Tổng quan kết quả kiểm tra
- Toàn bộ 20 ảnh trong tập train đều có file nhãn tương ứng.
- Đủ 31 skeletons, mỗi người có đầy đủ 17 keypoints theo đúng quy chuẩn COCO.
- Không có lỗi nghiêm trọng về đảo Trái/Phải (`left/right swap`) hay nối nhầm người (`person crossing`).
- Cấu trúc file nhãn đạt 56 giá trị chuẩn hóa trên mỗi dòng.

## 2. Chi tiết các điểm cần hiệu chỉnh (Feedback)

| STT | Tệp ảnh | Người thứ | Khớp (Keypoint) | Hiện trạng | Khuyến nghị sửa đổi |
| :---: | :--- | :---: | :--- | :--- | :--- |
| 1 | `train_04.jpg` | 1 | `left_ear` | Đang để `v = 0` do tóc che | Chuyển sang `v = 1`, chấm điểm ước lượng tại vị trí dái tai sau lọn tóc |
| 2 | `train_12.jpg` | 1 | `left_wrist` | Điểm rơi hơi lệch ra khoảng không ngoài vô-lăng | Kéo tâm điểm bám sát khớp cổ tay ngay dưới lòng bàn tay cầm lái |
| 3 | `train_16.jpg` | 2 | `right_eye` | Đang tick `v = 0` | Chuyển sang `v = 2`, vì khi zoom 200% vẫn quan sát rõ khóe mắt phải |

## 3. Đánh giá chung
Bài làm của bạn Hà My có chất lượng gán nhãn rất tốt, đường nối khung xương giải phẫu tự nhiên và bám sát thực tế ngồi trong cabin. Sau khi thống nhất lại quy ước về cờ `v = 1` cho các trường hợp che khuất một phần, bộ dữ liệu đã đạt độ tin cậy rất cao.
