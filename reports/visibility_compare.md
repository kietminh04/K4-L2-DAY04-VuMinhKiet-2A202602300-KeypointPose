# Báo cáo So sánh Phân bổ Visibility giữa Hai Bài Làm

- **Người thực hiện 1**: Vũ Minh Kiệt (`dataset/labels/train`)
- **Người thực hiện 2**: Nguyễn Hà My (`partner/dataset/labels/train`)
- **Ngày đối chiếu**: 16/09/2026

---

## Bảng so sánh tỷ lệ che khuất (%v=1)

| # | Khớp (Keypoint) | Kiệt (%v=1) | Hà My (%v=1) | Độ lệch | Nhận xét & Thống nhất |
| ---: | :--- | :---: | :---: | :---: | :--- |
| 0 | `nose` | 13% | 13% | 0% | Hoàn toàn thống nhất |
| 1 | `left_eye` | 16% | 16% | 0% | Hoàn toàn thống nhất |
| 2 | `right_eye` | 6% | 10% | 4% | Lệch nhẹ ở góc nghiêng 3/4 |
| 3 | `left_ear` | 23% | 16% | 7% | Lệch do tóc che tai; thống nhất dùng v=1 |
| 4 | `right_ear` | 19% | 16% | 3% | Sai số nhỏ chấp nhận được |
| 5 | `left_shoulder` | 0% | 0% | 0% | 100% nhìn thấy rõ (v=2) |
| 6 | `right_shoulder` | 0% | 0% | 0% | 100% nhìn thấy rõ (v=2) |
| 7 | `left_elbow` | 19% | 13% | 6% | Lệch do bệ tỳ tay che; thống nhất dùng v=1 |
| 8 | `right_elbow` | 16% | 16% | 0% | Hoàn toàn thống nhất |
| 9 | `left_wrist` | 16% | 19% | 3% | Thống nhất gióng theo cẳng tay |
| 10 | `right_wrist` | 6% | 6% | 0% | Hoàn toàn thống nhất |
| 11 | `left_hip` | 6% | 6% | 0% | Hoàn toàn thống nhất |
| 12 | `right_hip` | 3% | 3% | 0% | Hoàn toàn thống nhất |
| 13 | `left_knee` | 16% | 16% | 0% | Hoàn toàn thống nhất |
| 14 | `right_knee` | 16% | 16% | 0% | Hoàn toàn thống nhất |
| 15 | `left_ankle` | 16% | 16% | 0% | Hoàn toàn thống nhất |
| 16 | `right_ankle` | 13% | 13% | 0% | Hoàn toàn thống nhất |

---

## Kết luận
Độ tương đồng trong phân bổ trạng thái hiển thị giữa 2 người gán đạt trên **95%**. Những điểm lệch chủ yếu tập trung ở tai ngoài và khuỷu tay do bị che khuất bởi phụ kiện/nội thất xe, đã được đồng bộ chuẩn hóa vào `GUIDELINE_MINI.md`.
