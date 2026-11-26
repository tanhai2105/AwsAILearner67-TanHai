---
title: "Giới thiệu"
date: 2026-11-18
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Cost Guard

- Máy Windows EC2 tính phí theo giờ. Một máy t3.small chạy 24/7 tốn khoảng $31.04 mỗi tháng, vượt quá ngân sách credit $20 của dự án.
- AWS Budgets có cảnh báo chi phí, nhưng dữ liệu billing chỉ cập nhật vài lần mỗi ngày, nên tự nó không kịp dừng một máy bị quên. Cost Guard tự động dừng máy bằng ba lớp serverless.

#### Tổng quan về workshop

Trong workshop này, bạn sẽ triển khai Cost Guard bằng một template CloudFormation. Ba lớp hoạt động độc lập, mỗi lớp bù đắp điểm yếu của các lớp còn lại.

- **Lớp 1 – Schedule Guard:** EventBridge Scheduler và một hàm Lambda dừng instance mỗi ngày lúc 22:00 (Asia/Ho_Chi_Minh), dù có ai nhớ hay không.
- **Lớp 2 – Cost Meter:** một hàm Lambda chạy mỗi 5 phút, ước tính chi phí tháng từ thời gian chạy và đơn giá theo giờ, rồi dừng instance khi ước tính chạm khoảng $14.
- **Lớp 3 – Budget Backstop:** một AWS Budget theo dõi hóa đơn thực, và ở mức 80% và 100% của $20 sẽ kích hoạt một hàm Lambda dừng instance.

![overview](../../../images/5-Workshop/5.1-Workshop-overview/diagram1.png)
