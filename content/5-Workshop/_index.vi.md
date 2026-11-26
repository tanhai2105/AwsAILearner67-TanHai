---
title: "Workshop"
date: 2026-11-18
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Cost Guard: Bảo vệ credit máy lab Windows trên AWS bằng ba lớp serverless

#### Tổng quan

Máy Windows EC2 tính tiền theo giờ. Chỉ cần quên tắt một máy là ngân sách credit nhỏ có thể cạn sau vài ngày. Ví dụ, máy t3.small chạy liên tục 24/7 tốn khoảng $31.04 mỗi tháng, vượt ngân sách credit $20 của dự án. AWS Budgets cảnh báo được chi phí, nhưng dữ liệu hóa đơn cập nhật trễ nên tự nó không dừng máy kịp.

Trong workshop này, bạn xây dựng **Cost Guard**, hệ thống serverless tự động dừng máy lab. Cost Guard gồm ba lớp độc lập, lớp này bù cho điểm yếu của lớp kia:

- **Lớp 1 – Schedule Guard:** dừng máy lúc 22:00 mỗi ngày (Asia/Ho_Chi_Minh), dù có ai nhớ tắt máy hay không.
- **Lớp 2 – Cost Meter:** chạy 5 phút một lần, ước tính chi phí trong tháng dựa trên thời gian máy chạy và đơn giá theo giờ, rồi dừng máy khi ước tính chạm ngưỡng khoảng $14.
- **Lớp 3 – Budget Backstop:** một AWS Budget theo dõi hóa đơn thật. Khi chi phí chạm 80% và 100% của $20, Budget báo qua SNS để một hàm Lambda dừng máy.

Toàn bộ hệ thống deploy bằng một template CloudFormation. Sau đó bạn kiểm thử từng lớp, rà soát bảo mật và chi phí, cuối cùng xóa hết tài nguyên để không phát sinh thêm phí.

#### Các dịch vụ AWS sử dụng

- **Amazon EC2 và Amazon EBS:** máy lab Windows mà Cost Guard bảo vệ.
- **Amazon EventBridge Scheduler:** lệnh dừng hằng ngày và lần chạy 5 phút của bộ đo chi phí.
- **AWS Lambda:** logic của ba lớp.
- **Amazon DynamoDB:** bản ghi mọi hành động và ước tính chi phí.
- **Amazon SNS:** email cảnh báo và đường nối giữa Budgets với Lambda.
- **AWS Budgets:** ngân sách hằng tháng và các cảnh báo 80%, 100%.
- **AWS Systems Manager Parameter Store:** đơn giá, ngưỡng và hạn mức ngân sách.
- **Amazon CloudWatch:** log và alarm cho lỗi của hàm.
- **AWS IAM:** một role quyền tối thiểu cho từng hàm.
- **AWS CloudFormation:** toàn bộ hệ thống trong một stack.
- **AWS Cost Explorer:** so sánh chi phí thật với dự toán sau khi dọn dẹp.

#### Nội dung

1. [Giới thiệu](5.1-Workshop-overview/)
2. [Các bước chuẩn bị](5.2-Prerequisite/)
3. [Triển khai nền tảng](5.3-Foundation/)
4. [Lớp 1 – Schedule Guard](5.4-Layer1-Schedule/)
5. [Lớp 2 – Cost Meter](5.5-Layer2-CostMeter/)
6. [Lớp 3 – Budget Backstop và giám sát](5.6-Layer3-Backstop/)
7. [Rà soát bảo mật và chi phí](5.7-Security/)
8. [Dọn dẹp tài nguyên](5.8-Cleanup/)
