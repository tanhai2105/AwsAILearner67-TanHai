---
title: "Worklog Tuần 3"
date: 2026-09-21
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---


### Mục tiêu tuần 3:

* Hiểu cách dựng mạng trên AWS bằng Amazon VPC.
* Thực hành Amazon EC2: tạo, kết nối và thay đổi instance.
* Chạy một ứng dụng mẫu trên EC2 nằm trong VPC tự tạo.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Đọc phần VPC trong lab: <br>&emsp; + Subnet, Route Table <br>&emsp; + Internet Gateway, NAT Gateway <br> - Đọc về hai lớp firewall: <br>&emsp; + Security Group <br>&emsp; + Network ACL | 21/09/2026 | 21/09/2026 | <https://000003.awsfcaj.com/> |
| 3 | - **Thực hành:** <br>&emsp; + Dựng VPC với subnet, Internet Gateway và Route Table <br>&emsp; + Thêm Security Group <br>&emsp; + Tạo EC2 trong VPC rồi thử kết nối <br>&emsp; + Thêm NAT Gateway cho phía private | 22/09/2026 | 22/09/2026 | <https://000003.awsfcaj.com/> |
| 4 | - Kiến thức nền EC2: <br>&emsp; + Instance type <br>&emsp; + AMI <br>&emsp; + EBS <br>&emsp; + Key pair <br> - Đọc các bước chuẩn bị của lab EC2 | 23/09/2026 | 23/09/2026 | <https://000004.awsfcaj.com/> |
| 5 | - Học thêm về EC2: <br>&emsp; + Đổi cỡ instance <br>&emsp; + EBS snapshot <br>&emsp; + Tự tạo AMI <br> - **Thực hành:** <br>&emsp; + Tạo VPC và Security Group cho lab <br>&emsp; + Tạo một máy Linux, một máy Windows và đăng nhập vào cả hai | 24/09/2026 | 24/09/2026 | <https://000004.awsfcaj.com/> |
| 6 | - Tìm hiểu cách dùng IAM để giới hạn việc tạo EC2 và chi phí <br> - **Thực hành:** <br>&emsp; + Triển khai app AWS User Management trên EC2 <br>&emsp; + Xóa hết tài nguyên của lab | 25/09/2026 | 25/09/2026 | <https://000004.awsfcaj.com/> |


### Kết quả đạt được tuần 3:

* Giải thích được các thành phần của VPC: subnet public và private, route table đẩy traffic ra internet gateway hay NAT gateway, và security group (stateful, gắn theo instance) khác network ACL (stateless, gắn theo subnet) chỗ nào.

* Tự dựng một VPC chạy được, đặt EC2 vào và kiểm tra thấy phía private chỉ ra internet qua NAT gateway.

* Biết khi tạo EC2 cần chọn gì: instance type, AMI, gắn ổ EBS và giữ kỹ key pair.

* Đăng nhập được cả máy Linux lẫn Windows, đổi cỡ một máy, chụp snapshot rồi tạo AMI riêng từ đó.

* Chạy được ứng dụng AWS User Management trên EC2, xong thì dọn sạch để không bị tính tiền tiếp.
