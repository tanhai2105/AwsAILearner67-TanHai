---
title: "Worklog Tuần 10"
date: 2026-11-09
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---


### Mục tiêu tuần 10:

* Bảo vệ ứng dụng web bằng AWS WAF.
* Dùng Amazon GuardDuty để phát hiện và xử lý mối đe dọa, kể cả xử lý tự động bằng Amazon EventBridge và AWS Lambda.
* Lên kế hoạch cho phần việc của mình trong dự án Windows Server trên AWS của nhóm.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Đọc tin nhắn và tài liệu của nhóm, tóm lại các việc của mình <br>- Lập kế hoạch nhóm cho dự án Windows Server trên AWS, chia việc thành 3 phần chạy song song <br>- Kiểm tra điểm danh trên FCAJ Portal <br>- **Thực hành:** <br>&emsp; + Kịch bản 1 của GuardDuty: EC2 bị xâm nhập, xử lý tự động bằng EventBridge và Lambda | 09/11/2026 | 09/11/2026 | <https://000098.awsfcaj.com/> |
| 3 | - Đọc cách Amazon GuardDuty hoạt động: nguồn dữ liệu và finding <br>- **Thực hành:** <br>&emsp; + Chuẩn bị môi trường lab bằng CloudFormation template | 10/11/2026 | 10/11/2026 | <https://000098.awsfcaj.com/> |
| 4 | - **Thực hành:** <br>&emsp; + Kịch bản 2: lộ IAM credential, xử lý thủ công | 11/11/2026 | 11/11/2026 | <https://000098.awsfcaj.com/> |
| 5 | - **Thực hành:** <br>&emsp; + Kịch bản 3: credential của IAM role bị đánh cắp, xử lý bằng Lambda <br>&emsp; + Xem lại các finding và xóa tài nguyên lab | 12/11/2026 | 12/11/2026 | <https://000098.awsfcaj.com/> |
| 6 | - Đọc giới thiệu AWS WAF <br>- **Thực hành:** <br>&emsp; + Host web app mẫu trên S3 bucket <br>&emsp; + Tạo web ACL với managed rule, thêm custom rule và advanced custom rule <br>&emsp; + Gửi request thử, bật logging, dọn tài nguyên | 13/11/2026 | 13/11/2026 | <https://000026.awsfcaj.com/> |


### Kết quả đạt được tuần 10:

* Hiểu AWS WAF lọc request HTTP trước khi tới ứng dụng hay API, chặn các kiểu tấn công phổ biến như SQL injection, cross-site scripting, và mình có thể tự viết rule cho phép/chặn.

* Trong lab WAF, mình tạo web ACL với managed rule, thêm rule chặn request có một header nhất định và một advanced custom rule, thấy các request thử bị chặn hiện trên dashboard và sampled requests, rồi bật logging.

* Hiểu GuardDuty là dịch vụ phát hiện mối đe dọa, quản lý hoàn toàn, chạy theo Region:

  * Đọc VPC Flow Logs, CloudTrail và DNS log mà không ảnh hưởng tài nguyên đang giám sát
  * Mỗi finding có tên loại dễ đọc và có thông báo khi finding thay đổi
  * Chi phí tính theo số sự kiện CloudTrail và lượng flow log/DNS log, mỗi Region có 30 ngày dùng thử

* Làm qua ba kịch bản tấn công:

  * EC2 bị xâm nhập (UnauthorizedAccess:EC2/MaliciousIPCaller.Custom) được EventBridge và Lambda xử lý tự động
  * Lộ IAM credential (Recon:IAMUser/MaliciousIPCaller.Custom, UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom), lần ra ai đang gọi API rồi sửa thủ công
  * Credential của role bị đánh cắp (UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration) xử lý bằng Lambda

* Với dự án nhóm, mình gom tin nhắn và tài liệu rời rạc thành danh sách việc của bản thân và một kế hoạch 3 phần làm song song; điểm danh trên FCAJ Portal đã kiểm tra xong.
