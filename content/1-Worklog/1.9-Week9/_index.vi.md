---
title: "Worklog Tuần 9"
date: 2026-11-02
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---


### Mục tiêu tuần 9:

* Tìm hiểu cách chạy workload Windows Server trên AWS, vì chủ đề của nhóm là Windows Server trên AWS.
* Hiểu cách AWS Systems Manager Automation nâng cấp Windows Server tại chỗ.
* Triển khai WordPress trên AWS với tầng web public, database private, có scaling và CDN.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Đọc blog tự động nâng cấp Windows Server bằng AWS Systems Manager, ghi lại điều kiện và giới hạn | 02/11/2026 | 02/11/2026 | <https://aws.amazon.com/vi/blogs/modernizing-with-aws/how-to-automate-your-microsoft-windows-server-upgrades-using-aws-systems-manager-update/> |
| 3 | - Tìm hiểu workshop của AWS về Microsoft workloads trên AWS: các track, module, yêu cầu trước và chi phí <br>- **Thực hành (lab WordPress):** <br>&emsp; + VPC, subnet, security group riêng cho tầng web và tầng database <br>&emsp; + Tạo EC2 và database RDS | 03/11/2026 | 03/11/2026 | <https://catalog.us-east-1.prod.workshops.aws/workshops/6932a032-aeec-4dfd-bad8-7bfbc8dad4c2/en-US> <br> <https://000021.awsfcaj.com/> |
| 4 | - **Thực hành:** <br>&emsp; + Cài WordPress lên EC2, trỏ vào database RDS | 04/11/2026 | 04/11/2026 | <https://000021.awsfcaj.com/> |
| 5 | - **Thực hành:** <br>&emsp; + Tạo AMI và launch template từ server đã chạy ổn <br>&emsp; + Thêm target group, load balancer và Auto Scaling group | 05/11/2026 | 05/11/2026 | <https://000021.awsfcaj.com/> |
| 6 | - **Thực hành:** <br>&emsp; + Chụp snapshot database rồi restore lại <br>&emsp; + Đặt CloudFront trước website <br>&emsp; + Xóa toàn bộ tài nguyên lab | 06/11/2026 | 06/11/2026 | <https://000021.awsfcaj.com/> |


### Kết quả đạt được tuần 9:

* Đi qua workshop Microsoft workloads trên AWS, ghi lại track và module nào hợp với chủ đề Windows Server của nhóm, mỗi phần cần chuẩn bị gì và tốn bao nhiêu tiền.

* Ghi chú từ blog nâng cấp Windows Server:

  * Windows Server 2012/2012 R2 hết hỗ trợ từ 10/10/2023, bản 2016 hết hỗ trợ mở rộng ngày 12/01/2027, sau đó sẽ không còn bản vá bảo mật
  * Runbook AWSEC2-CloneInstanceAndUpgradeWindows nâng cấp trên bản clone chứ không đụng vào máy gốc, kết quả là một AMI mới
  * Trước khi chạy cần: SSM Agent hoạt động, TLS 1.2, ổ root trống từ 20 GB, instance profile cho SSM và đi ra được các endpoint của SSM
  * Không áp dụng cho domain controller, cluster, bản desktop hay server có role Remote Desktop Session Host; nâng cấp xong phải test ứng dụng rồi mới đưa lại production

* Dựng WordPress trên AWS từng bước:

  * Web server ở public subnet, RDS ở private subnet, mỗi tầng một security group
  * Cài WordPress và kết nối được với database
  * AMI, launch template, target group, load balancer và Auto Scaling group để website mở rộng và chia tải
  * Snapshot rồi restore database, sau đó thêm CloudFront cho tải nhanh hơn

* Thấy rõ CloudFront, Auto Scaling đi kèm load balancer và RDS Multi-AZ mỗi thứ lo một phần khác nhau về tính sẵn sàng và hiệu năng.
