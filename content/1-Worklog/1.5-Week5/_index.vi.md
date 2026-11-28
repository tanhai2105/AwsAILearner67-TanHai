---
title: "Worklog Tuần 5"
date: 2026-10-12
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---


### Mục tiêu tuần 5:

* Triển khai hai ứng dụng thật trên Amazon Lightsail, giữ an toàn, có backup và đúng cỡ.
* Đặt ứng dụng EC2 sau Elastic Load Balancing và Auto Scaling group.
* Thử bốn cách scaling và xem lúc nào nên dùng cách nào.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Xem tổng quan EC2 Auto Scaling Group và Elastic Load Balancing <br> - **Thực hành:** <br>&emsp; + Dựng phần mạng <br>&emsp; + Tạo EC2 và database instance, nhập dữ liệu mẫu <br>&emsp; + Cài và chạy web server | 12/10/2026 | 12/10/2026 | <https://000006.awsfcaj.com/> |
| 3 | - **Thực hành:** <br>&emsp; + Tạo launch template, target group và Application Load Balancer <br>&emsp; + Truy cập app qua load balancer để kiểm tra <br>&emsp; + Tạo Auto Scaling group và chuẩn bị metric cho predictive scaling | 13/10/2026 | 13/10/2026 | <https://000006.awsfcaj.com/> |
| 4 | - **Thực hành:** <br>&emsp; + Thử lần lượt bốn kiểu scaling: manual, scheduled, dynamic, predictive <br>&emsp; + Dọn lab | 14/10/2026 | 14/10/2026 | <https://000006.awsfcaj.com/> |
| 5 | - Đọc tổng quan Amazon Lightsail <br> - **Thực hành:** <br>&emsp; + Tạo database MySQL do Lightsail quản lý <br>&emsp; + Dựng blog WordPress trên blueprint Ubuntu, gắn static IP rồi cài WordPress <br>&emsp; + Dựng shop PrestaShop từ application blueprint, gắn static IP và tên miền | 15/10/2026 | 15/10/2026 | <https://000045.awsfcaj.com/> |
| 6 | - **Thực hành:** <br>&emsp; + Giới hạn lại rule SSH của firewall <br>&emsp; + Chụp snapshot, rồi tạo lại WordPress từ snapshot trên gói lớn hơn <br>&emsp; + Thêm alarm CPU <br>&emsp; + Xóa tài nguyên lab | 16/10/2026 | 16/10/2026 | <https://000045.awsfcaj.com/> |


### Kết quả đạt được tuần 5:

* Hiểu lý do dùng Auto Scaling: instance hỏng được thay tự động, số lượng máy co giãn theo lưu lượng, load balancer phân phối request, và lúc ít người dùng thì chi phí thấp hơn.

* Dựng hệ thống scaling cho app FCAJ Management (web server chạy trên EC2 kèm database):

  * Launch template khai báo AMI, instance type, security group và user data
  * Application Load Balancer và target group đặt phía trước các instance
  * Auto Scaling group trải trên 3 public subnet với desired 1, min 1, max 3, health check theo load balancer, group metrics của CloudWatch và thông báo qua SNS

* Điều mình rút ra về bốn kiểu scaling:

  * Manual: tự đổi desired capacity khi có một sự kiện chỉ xảy ra một lần
  * Scheduled: đổi số lượng máy vào giờ định sẵn khi lưu lượng lặp theo quy luật
  * Dynamic: phản ứng ngay theo metric như CPU hoặc số request
  * Predictive: dự đoán 48–72 giờ tiếp theo từ dữ liệu lịch sử, vì vậy cần thu thập metric trước đó

* Lightsail là cách đơn giản: một mức giá mỗi tháng đã gồm máy chủ, SSD, băng thông và static IP, nên không phải tính trước VPC, security group hay EBS.

* Chạy hai ứng dụng trên Lightsail ở Region Singapore, mỗi máy một static IP để địa chỉ cố định:

  * WordPress trên blueprint Ubuntu 24.04 với MySQL do Lightsail quản lý
  * PrestaShop dựng từ application blueprint sẵn có

* Vận hành giống một hệ thống nhỏ ngoài thực tế:

  * Rule SSH chỉ còn cho Lightsail browser SSH client truy cập thay vì cả internet
  * Instance Lightsail không đổi gói trực tiếp được, nên mình tạo snapshot, dựng máy lớn hơn từ snapshot rồi trỏ static IP sang máy mới
  * Blog vẫn còn nguyên bài sau khi chuyển, vì dữ liệu nằm ở managed database chứ không ở trên instance
  * Đã thêm alarm CPU
