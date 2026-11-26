---
title: "Blog 3"
date: 2026-11-18
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

### AMAZON CLOUDWATCH: "ĐÔI MẮT" GIÁM SÁT TOÀN BỘ HỆ THỐNG TRÊN AWS

**Đăng tại:** AWS Study Group (Facebook)

Triển khai ứng dụng lên Cloud mới chỉ là bước đầu. Sau đó bạn cần biết hệ thống có đang chạy khỏe không, tốt nhất là trước khi người dùng phàn nàn. Mình đang thực tập tại AWS và dành thời gian đọc tài liệu về Amazon CloudWatch. Bài này là bản tóm tắt để bạn nào mới học AWS nắm nhanh bức tranh tổng thể.

![Amazon CloudWatch: nguồn dữ liệu (EC2, ECS, EKS, Lambda, RDS, ứng dụng OpenTelemetry) đi vào CloudWatch, và đầu ra là dashboard, alarm, SNS, Auto Scaling, Investigations, SLO](../../../images/3-BlogsPosted/3.3-Blog3/cloudwatch-overview.png)

**Các điểm chính cần nắm:**

- **CloudWatch là gì:** dịch vụ giám sát và observability của AWS. CloudWatch gom metrics, logs và traces từ tài nguyên AWS lẫn ứng dụng của bạn, cho phép quan sát gần thời gian thực, đặt cảnh báo và tự động phản ứng khi có bất thường. Nhiều dịch vụ AWS tự gửi metric cơ bản về CloudWatch mà không cần cấu hình thêm.
- **Metrics và Dashboards:** metrics là dữ liệu hiệu năng theo thời gian (CPU, độ trễ, số request), dùng metric có sẵn hoặc tự đẩy custom metric qua OpenTelemetry (OTLP) hay API PutMetricData. Dashboards gom biểu đồ metrics và logs vào một màn hình, hỗ trợ nhiều Region và nhiều account.
- **Alarms:** theo dõi metric so với ngưỡng, rồi gửi thông báo (ví dụ qua SNS) hoặc kích hoạt hành động như Auto Scaling. Composite alarm gộp nhiều alarm thành một để giảm "nhiễu" cảnh báo.
- **Logs:** lưu theo log group và log stream, truy vấn bằng Logs Insights, có phát hiện bất thường trong log, metric filter (biến log thành metric) và subscription filter (đẩy log sang S3, Firehose).
- **Giám sát ứng dụng và hạ tầng:** Application Signals tự thu thập số request, độ trễ, lỗi mà không cần sửa code, kèm application map và SLO. Synthetics (canary) và RUM phát hiện sự cố trước khi người dùng gặp và đo trải nghiệm thật. Có thêm Database Insights, Lambda Insights, Container Insights. CloudWatch agent lấy metrics hệ thống, logs và traces từ EC2 và cả server on-premises.
- **Điểm mới đáng chú ý:** hỗ trợ OpenTelemetry và PromQL (dùng chung bộ instrumentation, không bị khóa vào định dạng riêng); cross-account observability (một account trung tâm xem dữ liệu của nhiều account, liên kết qua AWS Organizations); CloudWatch Omni, trải nghiệm observability có AI hỗ trợ, cho phép hỏi bằng ngôn ngữ tự nhiên để tạo truy vấn và theo dõi chất lượng phản hồi của AI agent; theo dõi được cả workload ngoài AWS.
- **Lưu ý khi đưa vào thực tế:** bắt đầu từ metric và alarm quan trọng nhất (CPU, lỗi, độ trễ) rồi mở rộng dần; dùng composite alarm để tránh alarm fatigue; chi phí phụ thuộc vào lượng metrics, logs và tính năng bạn dùng, nên cần xem trang giá và hướng dẫn tối ưu chi phí của AWS.

**Kết luận và cảm nghĩ:**

CloudWatch hữu ích vì nối được ba loại dữ liệu: metrics cho biết hệ thống hoạt động ra sao theo thời gian, logs cho biết vì sao, còn alarm báo khi nào cần xem. Chi phí tăng theo lượng metrics và logs, nên mình sẽ bắt đầu từ vài chỉ số như CPU, lỗi và độ trễ, rồi chỉ thêm khi cần.

**Tài liệu tham khảo:**

1. [What is Amazon CloudWatch (User Guide)](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
2. [Trang sản phẩm Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)
3. [Tính năng CloudWatch](https://aws.amazon.com/cloudwatch/features/)
4. [Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)
5. [Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html)
6. [Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html)
7. [Application Signals](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Application-Monitoring-Sections.html)
8. [Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)
9. [Cross-account observability](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Cross-Account-Methods.html)
10. [OpenTelemetry](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-OpenTelemetry-Sections.html)
11. [CloudWatch Omni](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-omni.html)
12. [Bảng giá](https://aws.amazon.com/cloudwatch/pricing/)

**Link:** [Bài viết trên AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2341916996573259)
