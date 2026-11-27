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
| 2 | - Đọc lý thuyết EC2: các loại instance, AMI, ổ EBS và cặp key pair <br> - Xem qua phần chuẩn bị của lab EC2 | 21/09/2026 | 21/09/2026 | <https://000004.awsfcaj.com/> |
| 3 | - Đọc phần mạng của lab VPC: subnet, route table, internet gateway và NAT gateway <br> - Phân biệt hai lớp tường lửa: security group và network ACL | 22/09/2026 | 22/09/2026 | <https://000003.awsfcaj.com/> |
| 4 | - **Thực hành:** <br>&emsp; + Dựng VPC gồm subnet, internet gateway và route table, rồi thêm security group <br>&emsp; + Đặt một EC2 vào VPC và thử kết nối <br>&emsp; + Gắn NAT gateway để máy ở subnet private ra được internet | 23/09/2026 | 23/09/2026 | <https://000003.awsfcaj.com/> |
| 5 | - Đọc tiếp về EC2: đổi cỡ instance, chụp EBS snapshot và tự tạo AMI <br> - **Thực hành:** <br>&emsp; + Chuẩn bị VPC và security group cho lab <br>&emsp; + Chạy một máy Linux và một máy Windows, đăng nhập thử cả hai | 24/09/2026 | 24/09/2026 | <https://000004.awsfcaj.com/> |
| 6 | - Tìm hiểu cách dùng IAM để hạn chế việc tạo EC2, qua đó kiểm soát chi phí <br> - **Thực hành:** <br>&emsp; + Chạy ứng dụng AWS User Management trên EC2 <br>&emsp; + Dọn toàn bộ tài nguyên của lab | 25/09/2026 | 25/09/2026 | <https://000004.awsfcaj.com/> |


### Kết quả đạt được tuần 3:

* Biết khi tạo EC2 cần chọn gì: instance type, AMI, gắn ổ EBS và giữ kỹ key pair.

* Giải thích được các thành phần của VPC: subnet public và private, route table đẩy traffic ra internet gateway hay NAT gateway, và security group (stateful, gắn theo instance) khác network ACL (stateless, gắn theo subnet) chỗ nào.

* Tự dựng một VPC chạy được, đặt EC2 vào và kiểm tra thấy phía private chỉ ra internet qua NAT gateway.

* Đăng nhập được cả máy Linux lẫn Windows, đổi cỡ một máy, chụp snapshot rồi tạo AMI riêng từ đó.

* Chạy được ứng dụng AWS User Management trên EC2, xong thì dọn sạch để không bị tính tiền tiếp.
