---
title: "Worklog Tuần 4"
date: 2026-09-28
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---


### Mục tiêu tuần 4:

* Làm việc trên terminal với AWS CloudShell và AWS CLI.
* Hiểu Amazon S3 lưu dữ liệu ra sao và Amazon CloudFront phân phối thế nào.
* Host trang tĩnh trên S3, đặt CloudFront phía trước và tập quản lý object.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học cơ bản về S3: bucket với object, các storage class, versioning và replication <br> - **Thực hành:** <br>&emsp; + Tạo một bucket <br>&emsp; + Tải file website mẫu lên | 28/09/2026 | 28/09/2026 | <https://000057.awsfcaj.com/> |
| 3 | - **Thực hành:** <br>&emsp; + Bật static website hosting với index.html làm trang chủ <br>&emsp; + Tắt Block public access và đặt object ở chế độ public bằng ACL <br>&emsp; + Truy cập site qua website endpoint, ghi lại thời gian tải | 29/09/2026 | 29/09/2026 | <https://000057.awsfcaj.com/> |
| 4 | - Đọc về Amazon CloudFront <br> - **Thực hành:** <br>&emsp; + Bật lại Block all public access cho bucket <br>&emsp; + Tạo CloudFront distribution với bucket là origin và cập nhật bucket policy <br>&emsp; + Mở site qua CloudFront, so thời gian tải với website endpoint của S3 | 30/09/2026 | 30/09/2026 | <https://000057.awsfcaj.com/> |
| 5 | - Xem tổng quan AWS CloudShell <br> - **Thực hành:** <br>&emsp; + Mở CloudShell và chạy thử vài lệnh Linux (pwd, ls -la, mkdir, touch, vi, cat) <br>&emsp; + Dùng AWS CLI: xem version, liệt kê bucket S3 và EC2 instance <br>&emsp; + Upload file lên CloudShell rồi download về máy <br>&emsp; + Xóa môi trường CloudShell | 01/10/2026 | 01/10/2026 | <https://000049.awsfcaj.com/> |
| 6 | - Quản lý object trên S3: versioning, di chuyển object, replication sang Region khác <br> - **Thực hành:** <br>&emsp; + Bật versioning, sửa index.html và upload lại, xem các phiên bản <br>&emsp; + Đưa object sang bucket thứ hai <br>&emsp; + Tạo replication rule sang bucket ở N. Virginia <br>&emsp; + Dọn tài nguyên lab | 02/10/2026 | 02/10/2026 | <https://000057.awsfcaj.com/> |


### Kết quả đạt được tuần 4:

* Hiểu S3 lưu object trong bucket, còn storage class quyết định giá và tốc độ truy cập. Đã tạo bucket và đưa file website lên.

* Đưa được website tĩnh lên chạy từ S3. Muốn truy cập được thì phải tắt Block public access và mở object bằng ACL, mà cách này chỉ chạy HTTP qua website endpoint.

* Sau đó sửa lại cho an toàn: bật lại chặn public và cho CloudFront đọc bucket bằng bucket policy. Qua CloudFront trang tải nhanh hơn, xem được edge location (PoP) nào trả kết quả.

* Thấy CloudShell khá tiện: mở trình duyệt là có terminal, AWS CLI đăng nhập sẵn. Mình tập lệnh Linux cơ bản trên đó, dùng CLI liệt kê bucket với instance, và chuyển file qua lại.

* Tập quản lý object:

  * Bật versioning rồi thì index.html bị ghi đè vẫn lấy lại được bản cũ
  * Chuyển object giữa các bucket
  * Replication rule tự chép object mới sang bucket ở Region khác, coi như có bản dự phòng
