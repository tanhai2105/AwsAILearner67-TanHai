---
title: "Worklog Tuần 7"
date: 2026-10-19
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---


### Mục tiêu tuần 7:

* Làm quen với việc giám sát trên AWS qua Amazon CloudWatch (metric, log, alarm, dashboard).
* Hiểu hybrid DNS với Route 53 Resolver bằng cách tự dựng lab từ đầu đến cuối.
* Chọn chủ đề workshop bằng cách so sánh vài ý tưởng theo tiêu chí chấm điểm của FCAJ.
* Ôn lại Amazon S3 và tự ghi chú.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Xem phần giới thiệu CloudWatch <br>- **Thực hành:** <br>&emsp; + Dựng stack lab (ba EC2 chạy CloudWatch agent) bằng CloudFormation <br>&emsp; + Xem metric, thử search expression, math expression và dynamic label | 19/10/2026 | 19/10/2026 | <https://000008.awsfcaj.com/> |
| 3 | - **Thực hành:** <br>&emsp; + Lọc log ứng dụng bằng CloudWatch Logs Insights <br>&emsp; + Thêm metric filter cho các dòng lỗi <br>&emsp; + Đặt alarm gửi mail cho mình qua Amazon SNS | 20/10/2026 | 20/10/2026 | <https://000008.awsfcaj.com/> |
| 4 | - **Thực hành:** <br>&emsp; + Làm CloudWatch dashboard rồi xóa tài nguyên lab <br>- So sánh bốn chủ đề workshop theo tiêu chí chấm điểm FCAJ: kiểm duyệt media bằng Rekognition, gửi cảnh báo bảo mật về Telegram, REST API serverless, backup và DR tự động | 21/10/2026 | 21/10/2026 | <https://000008.awsfcaj.com/> |
| 5 | - Đọc về hybrid DNS và Route 53 Resolver <br>- **Thực hành:** <br>&emsp; + Tạo key pair, dựng stack mạng <br>&emsp; + Mở security group, vào Remote Desktop Gateway <br>&emsp; + Cài AWS Managed Microsoft AD đóng vai DNS "on-premises" | 22/10/2026 | 22/10/2026 | <https://000010.awsfcaj.com/> |
| 6 | - **Thực hành:** <br>&emsp; + Thêm outbound endpoint, resolver rule và inbound endpoint <br>&emsp; + Kiểm tra phân giải bằng nslookup và Resolve-DnsName, xong thì dọn tài nguyên <br>- Học lab "Getting started with Amazon S3", ghi chú phân biệt bucket và object | 23/10/2026 | 23/10/2026 | <https://000010.awsfcaj.com/> <br> <https://000057.awsstudygroup.com/> |


### Kết quả đạt được tuần 7:

* Giờ mình hiểu CloudWatch là chỗ gom metric, log và alarm:

  * Stack lab cấp cho agent một IAM role chỉ đủ quyền cần dùng, chứ không phải AdministratorAccess
  * Ghép metric bằng search/math expression, dùng dynamic label cho biểu đồ dễ đọc hơn
  * Logs Insights lọc log rất nhanh; metric filter biến các dòng ERROR thành metric, và mình để ý là filter chỉ đếm sự kiện đến sau lúc tạo nó
  * Alarm trên metric đó gửi mail về qua SNS, còn dashboard gom hết lên một màn hình

* Dựng xong lab hybrid DNS:

  * Mạng (VPC, public/private subnet, Remote Desktop Gateway) dựng từ CloudFormation template
  * AWS Managed Microsoft AD với domain onprem.example.com đóng vai DNS on-premises
  * Outbound endpoint cho truy vấn đi từ AWS ra on-premises, inbound endpoint cho chiều ngược lại, resolver rule để chọn domain nào được chuyển tiếp
  * Từ gateway kiểm tra phân giải tên được bằng cả nslookup lẫn Resolve-DnsName

* So bốn ý tưởng workshop (kiểm duyệt media bằng Rekognition, cảnh báo bảo mật qua Telegram, REST API serverless, backup và DR tự động) với tiêu chí chấm của FCAJ, nhờ vậy mình rõ hơn một chủ đề workshop tốt cần gì.

* Tự ghi chú về S3: bucket là "thùng chứa" có tên duy nhất toàn cầu, nằm trong một Region; object là file kèm metadata, được truy cập qua key.
