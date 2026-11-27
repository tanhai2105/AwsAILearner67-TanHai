---
title: "Worklog Tuần 8"
date: 2026-10-26
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---


### Mục tiêu tuần 8:

* Thao tác với AWS từ terminal bằng AWS CLI thay vì console.
* Tạo tài nguyên S3, SNS, IAM, mạng và EC2 hoàn toàn bằng lệnh.
* Tiếp tục làm workshop và soát lại kiến trúc, thông tin trong proposal.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Làm tiếp workshop <br>- Đọc lại proposal, đối chiếu với kiến trúc | 26/10/2026 | 26/10/2026 | |
| 3 | - Đọc cách AWS CLI hoạt động: profile, file cấu hình, định dạng output <br>- **Thực hành:** <br>&emsp; + Cài CLI, chạy aws configure cho profile của mình <br>&emsp; + Liệt kê tài nguyên đang có từ terminal | 27/10/2026 | 27/10/2026 | <https://000011.awsfcaj.com/> |
| 4 | - **Thực hành:** <br>&emsp; + Lệnh S3: tạo bucket, upload và liệt kê object <br>&emsp; + Lệnh SNS: topic và subscription | 28/10/2026 | 28/10/2026 | <https://000011.awsfcaj.com/> |
| 5 | - **Thực hành:** <br>&emsp; + IAM bằng lệnh: group, user, access key <br>&emsp; + Mạng bằng lệnh: VPC, subnet, internet gateway, route table | 29/10/2026 | 29/10/2026 | <https://000011.awsfcaj.com/> |
| 6 | - **Thực hành:** <br>&emsp; + Tạo key pair, security group, chạy EC2 từ CLI rồi SSH vào <br>&emsp; + Terminate và xóa sạch tài nguyên còn sót | 30/10/2026 | 30/10/2026 | <https://000011.awsfcaj.com/> |


### Kết quả đạt được tuần 8:

* Workshop tiến thêm một bước, đồng thời mình rà lại proposal để chắc kiến trúc và thông tin vẫn khớp nhau.

* Hiểu AWS CLI là công cụ mã nguồn mở gọi cùng bộ API public như console, nên cái gì bấm được trên console thì cũng viết script được.

* Cài đặt CLI cho đúng:

  * aws configure lưu access key, secret key, Region mặc định và định dạng output; muốn dùng profile khác thì thêm --profile
  * json với yaml hợp cho script, còn table và text thì mình đọc bằng mắt dễ hơn
  * Tính năng auto-prompt giúp nhiều lúc mình chưa thuộc cú pháp

* Tạo tài nguyên chỉ bằng terminal:

  * Bucket S3 kèm object, topic SNS kèm subscription
  * IAM group, user và access key
  * Một mạng nhỏ đầy đủ: VPC, subnet, internet gateway, route table
  * Một EC2 trong mạng đó với key pair và security group riêng, SSH vào được rồi terminate

* Rút ra bài học về credential: không bao giờ chia sẻ access key, ưu tiên IAM role, key và key pair không dùng nữa thì xóa.
