---
title: "Worklog Tuần 11"
date: 2026-11-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---


### Mục tiêu tuần 11:

* Hoàn thiện workshop Cost Guard: ảnh chụp, dòng kiểm tra, câu chữ tiếng Việt, email cảnh báo và sơ đồ kiến trúc.
* Test lại cả 3 lớp từ đầu đến cuối trên AWS và review thay đổi code mới nhất.
* Chuẩn bị file của nhóm cho FCAJ Submission.
* Viết các phần cá nhân và kiểm tra lần cuối website trước khi nộp.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thêm ảnh AWS Budgets vào 5.6, bổ sung ảnh còn thiếu và dòng "Kiểm tra" dưới mọi ảnh của 5.3–5.7 <br>- Viết lại câu chữ tiếng Việt của workshop <br>- Đổi email cảnh báo sang email nhóm (phải update stack hai lần vì budget có tên cố định) <br>- Vẽ lại sơ đồ kiến trúc trên draw.io bằng bộ icon chính thức của AWS (07/2026) | 23/11/2026 | 23/11/2026 | |
| 3 | - Review thay đổi của bạn cùng nhóm ở Lambda Cost Meter (tính chi phí theo LaunchTime của instance) <br>- Bắt đầu test lại cả 3 lớp trên AWS | 24/11/2026 | 25/11/2026 | |
| 4 | - Test xong end to end, so code đang deploy với template và đăng review <br>- Soạn file tổng hợp .md của nhóm và file kiến trúc .xml cho FCAJ Submission | 25/11/2026 | 25/11/2026 | |
| 5 | - Viết các phần cá nhân: sự kiện, tự đánh giá, nhận xét góp ý | 26/11/2026 | 26/11/2026 | |
| 6 | - Kiểm tra lần cuối toàn bộ website <br>- Viết lại worklog theo góp ý của trưởng nhóm để phần cá nhân không trùng với các bạn | 27/11/2026 | 27/11/2026 | |


### Kết quả đạt được tuần 11:

* Các mục 5.3–5.7 của workshop đã đủ ảnh, dưới mỗi ảnh có dòng "Kiểm tra", mục 5.6 có thêm ảnh AWS Budgets; phần tiếng Việt đọc tự nhiên hơn.

* Cảnh báo giờ gửi về email nhóm. Mình phải update stack hai lần vì budget dùng tên cố định, sau này viết template cần để ý chỗ này.

* Sơ đồ kiến trúc đã vẽ lại trên draw.io bằng bộ icon chính thức của AWS (07/2026).

* Review thay đổi Lambda Cost Meter tính chi phí theo LaunchTime, rồi test lại cả 3 lớp trên AWS:

  * Layer 2 tắt instance sau 3 phút 54 giây kể từ lúc mình hạ ngưỡng
  * Layer 3 tắt trong 7 giây
  * Code đang deploy không giống template, mình đã ghi rõ trong review

* Soạn xong bản nháp file tổng hợp .md của nhóm và file kiến trúc .xml cho FCAJ Submission.

* Đã viết xong phần cá nhân và kiểm tra lại website. Việc nộp link workshop trên FCAJ Portal để sang tuần 12.
