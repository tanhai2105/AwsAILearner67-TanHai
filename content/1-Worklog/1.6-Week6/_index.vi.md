---
title: "Worklog Tuần 6"
date: 2026-10-12
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---


### Mục tiêu tuần 6:

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


### Kết quả đạt được tuần 6:

* Hiểu vì sao cần Auto Scaling: máy lỗi thì tự thay, số máy tăng giảm theo lượng truy cập, load balancer chia request, giờ vắng thì tốn ít tiền hơn.

* Dựng phần scaling cho app FCAJ Management (web server trên EC2 cộng database):

  * Launch template chứa AMI, instance type, security group và user data
  * Application Load Balancer cùng target group đứng trước các máy
  * Auto Scaling group trên 3 public subnet, desired 1, min 1, max 3, dùng health check của load balancer, CloudWatch group metrics và thông báo SNS

* Rút ra về bốn kiểu scaling:

  * Manual: tự chỉnh desired capacity khi có sự kiện nhất thời
  * Scheduled: đổi số máy theo giờ cố định khi lưu lượng có quy luật
  * Dynamic: phản ứng theo metric trực tiếp như CPU hay số request
  * Predictive: dự báo 48 đến 72 giờ tới dựa trên lịch sử, nên phải có sẵn dữ liệu metric từ trước

* Lightsail là lựa chọn đơn giản: trả một giá mỗi tháng là có máy chủ, ổ SSD, băng thông và static IP, không phải tự lo VPC, security group hay EBS.

* Dựng hai app trên Lightsail ở Region Singapore, mỗi máy gắn static IP riêng để địa chỉ không bị đổi:

  * WordPress trên blueprint Ubuntu 24.04, dùng database MySQL do Lightsail quản lý
  * PrestaShop từ application blueprint có sẵn

* Vận hành như một hệ thống nhỏ thật sự:

  * Rule SSH giờ chỉ cho Lightsail browser SSH client vào, không mở cho cả internet nữa
  * Instance Lightsail không đổi gói tại chỗ được, nên mình chụp snapshot, tạo máy lớn hơn từ đó rồi chuyển static IP sang máy mới
  * Blog vẫn đủ bài viết sau khi chuyển vì dữ liệu nằm ở managed database chứ không nằm trên máy
  * Thêm alarm CPU
