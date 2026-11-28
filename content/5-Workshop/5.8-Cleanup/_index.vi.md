---
title: "Dọn dẹp tài nguyên"
date: 2026-11-18
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

## Dọn dẹp tài nguyên

Bạn đã hoàn thành toàn bộ workshop Cost Guard: máy lab được bảo vệ bởi ba lớp và cả ba lớp đều đã được kiểm thử. Giờ chỉ còn bước cuối cùng là dọn dẹp, xóa mọi thứ đã tạo để không bị tính phí về sau. Ảnh bên dưới chụp từ lần dọn thật của nhóm ngày 26/11/2026 ở `us-east-1`, làm bằng IAM user, không dùng root user.

Thứ tự quan trọng: xóa stack `CostGuard` trước, sau đó tới máy lab, cuối cùng là vài tài nguyên được tạo ngoài stack.

### Bước 1 – Xem mình đang có gì

1. Mở **CloudFormation → Stacks**. Tài khoản của nhóm có hai stack: `CostGuard` (ba lớp bảo vệ) và `costguard-instance` (máy Windows lab, truyền vào `CostGuard` qua `ExistingInstanceId`).

![Hai stack trước khi dọn](../../../images/5-Workshop/5.8-cleanup/69-cleanup-stacks-before.png)

2. Mở tab **Resources** của từng stack để biết stack sẽ xóa những gì. `CostGuard` có 29 tài nguyên: các hàm Lambda, role, log group, schedule, bảng DynamoDB, SNS topic, Budget và alarm.

![Tài nguyên của stack CostGuard](../../../images/5-Workshop/5.8-cleanup/70-cleanup-costguard-resources.png)

![Tài nguyên của stack costguard-instance: instance, launch template, security group](../../../images/5-Workshop/5.8-cleanup/71-cleanup-instance-stack-resources.png)

3. Ghi lại instance ID trong **EC2 → Instances** để lát nữa kiểm tra máy đã bị terminate.

![Máy lab trước khi dọn](../../../images/5-Workshop/5.8-cleanup/72-cleanup-ec2-before.png)

### Bước 2 – Xóa stack CostGuard

1. Chọn `CostGuard`, bấm **Delete**, gõ tên stack để xác nhận.

![Hộp xác nhận xóa CostGuard](../../../images/5-Workshop/5.8-cleanup/73-cleanup-delete-costguard-confirm.png)

![Đã gõ tên stack vào ô xác nhận](../../../images/5-Workshop/5.8-cleanup/74-cleanup-delete-costguard-typed.png)

2. Stack chuyển sang **DELETE_IN_PROGRESS**. Tab **Events** hiện từng tài nguyên đang bị xóa.

![CostGuard ở trạng thái DELETE_IN_PROGRESS](../../../images/5-Workshop/5.8-cleanup/75-cleanup-costguard-delete-in-progress.png)

![Events trong lúc xóa CostGuard](../../../images/5-Workshop/5.8-cleanup/76-cleanup-costguard-events-deleting.png)

3. Chờ tới **DELETE_COMPLETE**. Stack biến mất khỏi danh sách đang chạy; muốn xem lại thì đổi bộ lọc sang **Deleted**.

![Danh sách stack sau khi CostGuard đã xóa](../../../images/5-Workshop/5.8-cleanup/77-cleanup-stacks-after-costguard.png)

![CostGuard ở trạng thái DELETE_COMPLETE](../../../images/5-Workshop/5.8-cleanup/78-cleanup-costguard-delete-complete.png)

![Các event cuối của CostGuard](../../../images/5-Workshop/5.8-cleanup/79-cleanup-costguard-events-complete.png)

Với CLI:

```bash
aws cloudformation delete-stack --region us-east-1 --stack-name CostGuard
aws cloudformation wait stack-delete-complete --region us-east-1 --stack-name CostGuard
```

### Bước 3 – Xóa máy lab

Nếu máy do chính `CostGuard` tạo (để trống `ExistingInstanceId`) thì Bước 2 đã terminate máy rồi, chuyển sang Bước 4.

Nếu có truyền `ExistingInstanceId` thì máy đó **không** do stack tạo, nên stack không xóa. Nhóm tạo máy bằng một stack riêng, nên xóa stack đó:

1. Chọn `costguard-instance`, bấm **Delete** và gõ tên stack.

![Hộp xác nhận xóa costguard-instance](../../../images/5-Workshop/5.8-cleanup/80-cleanup-delete-instance-confirm.png)

![Đã gõ tên stack vào ô xác nhận](../../../images/5-Workshop/5.8-cleanup/81-cleanup-delete-instance-typed.png)

![costguard-instance ở trạng thái DELETE_IN_PROGRESS](../../../images/5-Workshop/5.8-cleanup/82-cleanup-instance-delete-in-progress.png)

2. Máy chuyển sang **Terminated**. Khoảng một giờ sau AWS tự ẩn máy đã terminate; máy này không tốn phí.

![Máy lab đã terminate](../../../images/5-Workshop/5.8-cleanup/83-cleanup-ec2-terminated.png)

3. Danh sách stack đang chạy giờ trống, bộ lọc **Deleted** hiện cả hai stack.

![Không còn stack nào đang chạy](../../../images/5-Workshop/5.8-cleanup/84-cleanup-stacks-active-empty.png)

![Hai stack trong bộ lọc Deleted](../../../images/5-Workshop/5.8-cleanup/85-cleanup-stacks-deleted-list.png)

![Events của costguard-instance: đã xóa instance, launch template và security group](../../../images/5-Workshop/5.8-cleanup/86-cleanup-instance-events-complete.png)

Nếu bạn tạo máy bằng tay thì terminate trong **EC2 → Instances → Instance state → Terminate instance**.

### Bước 4 – Xóa những thứ stack không tạo

Trong lúc làm dự án có hai thứ được tạo ngoài stack. Dù làm đúng hướng dẫn, bạn vẫn nên kiểm tra.

**IAM role do console Scheduler tạo.** Khi tạo schedule trong console EventBridge Scheduler và chọn *Create new role*, AWS tạo role `Amazon_EventBridge_Scheduler_LAMBDA_*` và policy `Amazon-EventBridge-Scheduler-Execution-Policy-*`. Xóa stack không xóa hai thứ này.

1. Mở **IAM → Roles**, tìm `Amazon_EventBridge_Scheduler`. Mở role, kiểm tra policy chỉ cho gọi hàm `CostGuard-*`.

![Role Scheduler còn sót](../../../images/5-Workshop/5.8-cleanup/104-cleanup-leftover-scheduler-role.png)

2. Xóa role, rồi vào **IAM → Policies** xóa policy (lúc này policy không còn gắn vào đâu).

![Xóa role](../../../images/5-Workshop/5.8-cleanup/105-cleanup-delete-scheduler-role-confirm.png)

![Đã xóa role](../../../images/5-Workshop/5.8-cleanup/106-cleanup-scheduler-role-deleted.png)

![Xóa policy](../../../images/5-Workshop/5.8-cleanup/107-cleanup-delete-scheduler-policy-confirm.png)

![Đã xóa policy](../../../images/5-Workshop/5.8-cleanup/108-cleanup-scheduler-policy-deleted.png)

**Bucket S3 `cf-templates-*`.** Khi tải file template lên trong console CloudFormation, AWS lưu file vào bucket tên `cf-templates-<chuỗi ngẫu nhiên>-us-east-1`. Chỉ xóa nếu bucket chỉ chứa template của workshop này.

1. Mở bucket, xem các object. Bucket của nhóm chỉ có template `costguard` và `cost-guard`.

![Các object trong bucket cf-templates](../../../images/5-Workshop/5.8-cleanup/109-cleanup-s3-cf-templates-objects.png)

2. Bấm **Empty**, gõ `permanently delete` rồi xác nhận.

![Làm rỗng bucket](../../../images/5-Workshop/5.8-cleanup/110-cleanup-s3-empty-confirm.png)

![Bucket đã rỗng](../../../images/5-Workshop/5.8-cleanup/111-cleanup-s3-empty-done.png)

3. Bấm **Delete**, gõ tên bucket rồi xác nhận.

![Xóa bucket](../../../images/5-Workshop/5.8-cleanup/112-cleanup-s3-delete-confirm.png)

![Đã xóa bucket](../../../images/5-Workshop/5.8-cleanup/113-cleanup-s3-bucket-deleted.png)

### Bước 5 – Kiểm tra không còn gì sót

Kiểm tra từng console ở `us-east-1`:

- [ ] **EC2 → Instances:** không còn máy lab đang chạy hay đã dừng (dòng *Terminated* thì được).

![EC2 instances](../../../images/5-Workshop/5.8-cleanup/87-check-ec2-instances.png)

- [ ] **EC2 → Volumes:** không còn ổ đĩa nào sót lại.

![EBS volumes](../../../images/5-Workshop/5.8-cleanup/88-check-ebs-volumes.png)

- [ ] **EC2 → Elastic IPs:** không có địa chỉ nào được cấp phát.

![Elastic IPs](../../../images/5-Workshop/5.8-cleanup/89-check-elastic-ips.png)

- [ ] **EC2 → Security Groups** và **Launch Templates:** chỉ còn `default`, không còn launch template của lab.

![Security groups](../../../images/5-Workshop/5.8-cleanup/90-check-security-groups.png)

![Launch templates](../../../images/5-Workshop/5.8-cleanup/91-check-launch-templates.png)

- [ ] **Lambda:** không còn hàm `CostGuard-*`.

![Các hàm Lambda](../../../images/5-Workshop/5.8-cleanup/92-check-lambda-functions.png)

- [ ] **DynamoDB:** không còn bảng `CostGuardSessions`.

![Các bảng DynamoDB](../../../images/5-Workshop/5.8-cleanup/93-check-dynamodb-tables.png)

- [ ] **EventBridge → Scheduler** và **Rules:** không còn lịch `costguard-*` và rule của CostGuard.

![Các lịch Scheduler](../../../images/5-Workshop/5.8-cleanup/94-check-scheduler-schedules.png)

![Các rule EventBridge trên default bus](../../../images/5-Workshop/5.8-cleanup/114-check-eventbridge-rules-default-bus.png)

- [ ] **SNS:** không còn topic `costguard-*` và subscription của nó.

![SNS topics](../../../images/5-Workshop/5.8-cleanup/96-check-sns-topics.png)

![SNS subscriptions](../../../images/5-Workshop/5.8-cleanup/97-check-sns-subscriptions.png)

- [ ] **CloudWatch:** không còn alarm `CostGuard-*` và log group `/aws/lambda/CostGuard-*`.

![CloudWatch alarms](../../../images/5-Workshop/5.8-cleanup/98-check-cloudwatch-alarms.png)

![CloudWatch log groups](../../../images/5-Workshop/5.8-cleanup/99-check-cloudwatch-log-groups.png)

- [ ] **Budgets:** không còn `CostGuard-Budget`. Budget do người khác tạo trong tài khoản dùng chung thì để nguyên.

![Budgets](../../../images/5-Workshop/5.8-cleanup/100-check-budgets.png)

- [ ] **Systems Manager → Parameter Store:** không còn tham số `/costguard/`.

![Parameter Store](../../../images/5-Workshop/5.8-cleanup/101-check-parameter-store.png)

- [ ] **IAM → Roles:** không còn role `CostGuard-*`.

![IAM roles](../../../images/5-Workshop/5.8-cleanup/115-check-iam-roles-costguard.png)

- [ ] **S3:** không còn bucket `cf-templates-*` của workshop.

![S3 buckets](../../../images/5-Workshop/5.8-cleanup/103-check-s3-buckets.png)

Kiểm tra tương tự bằng **CloudShell**. Mỗi dòng đếm số tài nguyên có chữ `costguard` trong tên; mọi dòng phải ra `0`, hai dòng cuối (ổ EBS, Elastic IP) cũng phải ra `0`:

```bash
c(){ printf "%-28s %s\n" "$1" "$(eval "$2" | grep -ci costguard)"; }
c "Stacks (active)" "aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE --query 'StackSummaries[].StackName' --output text"
c "EC2 running/stopped" "aws ec2 describe-instances --filters Name=instance-state-name,Values=running,stopped --query 'Reservations[].Instances[].Tags' --output text"
c "Lambda functions" "aws lambda list-functions --query 'Functions[].FunctionName' --output text"
c "Scheduler schedules" "aws scheduler list-schedules --query 'Schedules[].Name' --output text"
c "DynamoDB tables" "aws dynamodb list-tables --output text"
c "SNS topics" "aws sns list-topics --output text"
c "CloudWatch alarms" "aws cloudwatch describe-alarms --query 'MetricAlarms[].AlarmName' --output text"
c "Log groups" "aws logs describe-log-groups --query 'logGroups[].logGroupName' --output text"
c "SSM parameters" "aws ssm describe-parameters --query 'Parameters[].Name' --output text"
c "IAM roles" "aws iam list-roles --query 'Roles[].RoleName' --output text"
c "S3 buckets" "aws s3 ls | grep -i -e costguard -e cf-templates"
c "Budgets" "aws budgets describe-budgets --account-id $(aws sts get-caller-identity --query Account --output text) --query 'Budgets[].BudgetName' --output text"
aws ec2 describe-volumes --query "length(Volumes)"
aws ec2 describe-addresses --query "length(Addresses)"
```

![CloudShell: mọi dòng đều bằng 0](../../../images/5-Workshop/5.8-cleanup/116-check-cloudshell-final-sweep.png)

### Video demo workshop

Link clip demo bài workshop: <https://drive.google.com/file/d/1NrNFPlScetGuT9zNIzrrsFm-SImVmhyQ/view?usp=sharing>
