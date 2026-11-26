---
title: "Dọn dẹp tài nguyên"
date: 2026-11-18
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

## Dọn dẹp tài nguyên

Xóa mọi thứ bạn đã tạo để không bị tính phí sau workshop. Một stack chứa toàn bộ tài nguyên, nên một lần xóa là đủ.

### Bước 1 – Xóa stack

1. Mở **CloudFormation → Stacks → CostGuard**.
2. Chọn **Delete** và xác nhận.
3. Chờ đến khi stack biến mất khỏi danh sách hoặc hiện **DELETE_COMPLETE**.

Với CLI:

```bash
aws cloudformation delete-stack --region us-east-1 --stack-name CostGuard
```

Khi xóa stack, CloudFormation sẽ xóa instance cùng ổ đĩa của nó, bảng DynamoDB, các tham số Parameter Store, các hàm Lambda, log group, các schedule, Budget, các SNS topic và các alarm.

Nếu lúc deploy có truyền `ExistingInstanceId` thì instance đó **không** do stack tạo, nên stack cũng không xóa. Hãy tự terminate instance này.

### Bước 2 – Kiểm tra không còn gì sót

Kiểm tra từng console ở `us-east-1`:

- [ ] **EC2 → Instances:** không còn instance của lab, dù đang chạy hay đã dừng.
- [ ] **EC2 → Volumes:** không còn ổ đĩa nào sót lại.
- [ ] **EC2 → Elastic IPs:** không có địa chỉ nào được cấp phát.
- [ ] **Lambda:** không còn hàm `CostGuard-*`.
- [ ] **DynamoDB:** không còn bảng `CostGuardSessions`.
- [ ] **EventBridge → Scheduler:** không còn lịch `costguard-*`.
- [ ] **SNS:** không còn topic `costguard-*`.
- [ ] **CloudWatch:** không còn alarm `CostGuard-*` và log group `/aws/lambda/CostGuard-*`.
- [ ] **Budgets:** không còn `CostGuard-Budget`.
- [ ] **Systems Manager → Parameter Store:** không còn tham số `/costguard/`.

### Bước 3 – So sánh chi phí

Dữ liệu billing đến chậm, nên hãy chờ một hoặc hai ngày.

1. Mở **Billing and Cost Management → Cost Explorer**.
2. Chọn các ngày làm workshop, nhóm theo **Service**, và so chi phí thật của EC2, EBS và các dịch vụ khác với dự toán ở 5.7.
3. Kiểm tra tổng nằm dưới ngân sách $20.

### Nếu việc xóa bị lỗi

- **Stack kẹt ở `DELETE_FAILED`:** mở tab **Events**, tìm tài nguyên bị lỗi, xóa tay trong console của nó, rồi xóa stack lại.
- **Còn tài nguyên sau khi stack đã mất:** xóa tay. Tài nguyên của workshop có tag `Project=CostGuard` ở những loại hỗ trợ tag.
