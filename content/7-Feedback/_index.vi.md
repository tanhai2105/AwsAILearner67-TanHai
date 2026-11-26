---
title: "Chia sẻ và Đóng góp ý kiến"
date: 2026-11-26
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

> Đây là cảm nhận của mình sau gần ba tháng tham gia First Cloud AI Journey, kèm vài góp ý mong giúp chương trình tốt hơn cho các khoá sau.

### Đánh giá tổng thể

**1. Môi trường làm việc**

Các buổi sự kiện ở văn phòng AWS tầng 26 Bitexco cho mình cảm giác được làm trong môi trường thật, gặp diễn giả từ doanh nghiệp và các bạn thực tập sinh khác. Phần lớn thời gian còn lại nhóm mình làm online qua WhatsApp và GitHub, tự chia việc và hẹn mốc với nhau.

**2. Sự hỗ trợ từ Mentor / Admin**

Tài liệu, template workshop và luật chấm điểm được đưa ra rõ ràng, nên nhóm biết phải nộp gì và chấm theo tiêu chí nào. Khi chọn đề tài, mình dùng chính luật chấm điểm đó để so sánh bốn ý tưởng trước khi chốt.

**3. Mức độ liên quan đến ngành học**

Mình học Kỹ thuật phần mềm. Ở trường mình viết code chạy trên máy mình; ở đây code Lambda chạy trên AWS, phải nghĩ tới quyền IAM, chi phí và việc dọn tài nguyên sau khi dùng. Phần này trường chưa dạy nhiều nên mình học được rất nhiều.

**4. Cơ hội học hỏi & phát triển kỹ năng**

- Viết hạ tầng bằng CloudFormation và hiểu vì sao "deploy xong" chưa phải là "chạy đúng": lần test lại cuối kỳ mình phát hiện code đang chạy trên AWS khác với template.
- Dự toán chi phí bằng AWS Pricing Calculator: cùng một máy Windows `t3.small`, chạy 120 giờ lab mỗi tháng khoảng $7.12, còn để chạy 24/7 khoảng $31.04. Đây chính là lý do dự án Cost Guard tồn tại.
- Làm việc nhóm qua Git: branch, review, không merge khi trưởng nhóm chưa đồng ý.

**5. Văn hoá & tinh thần đồng đội**

Nhóm ba người nhưng ai cũng có phần rõ ràng và giúp nhau khi cần: một bạn quay demo, một bạn ghép site, mình test lại và dọn tài nguyên. Khi phát hiện lỗi ở phần của bạn khác, mình nhắn thẳng vào nhóm kèm bằng chứng, và mọi người sửa trên tinh thần xây dựng.

**6. Chính sách / quyền lợi**

Thời gian linh hoạt, được tham gia các sự kiện của AWS và cộng đồng (Cloud & AI Day, Student Community Day, Buildrathon) miễn phí. Đây là điểm cộng lớn với sinh viên.

---

### Câu hỏi thêm

- **Điều bạn hài lòng nhất trong quá trình thực tập?**

  Lúc test lại cả ba lớp của Cost Guard trên AWS và thấy hệ thống tự tắt máy đúng như thiết kế: Lớp 2 tắt máy 3 phút 54 giây sau khi hạ ngưỡng, Lớp 3 tắt trong 7 giây. Lần đầu mình thấy thứ mình viết chạy thật trên cloud và đo được kết quả.

- **FCAJ/AWS nên cải thiện điều gì cho các thực tập sinh sau?**

  Nên cấp cho mỗi nhóm một tài khoản lab riêng có sẵn budget. Nhóm mình phải dùng chung tài khoản của một bạn, chung một IAM user. Việc gửi mật khẩu qua chat và tự quản quyền như vậy không an toàn, và nhiều dịch vụ (ví dụ Amazon Inspector) bị khoá trên Free plan.

- **Bạn có giới thiệu chương trình cho bạn bè không? Vì sao?**

  Có. Chương trình bắt mình làm một dự án thật từ đề xuất, triển khai, test tới dọn dẹp, chứ không chỉ học lý thuyết. Các sự kiện cũng giúp mình gặp người đang làm cloud thật.

---

### Góp ý & kỳ vọng

- Cấp tài khoản lab riêng cho từng nhóm như trên, hoặc ít nhất hướng dẫn cách chia quyền IAM cho từng thành viên ngay từ tuần đầu.
- Gom thông báo quan trọng (hạn nộp, template, phần nào làm theo nhóm hay cá nhân) vào một trang cố định, vì tin nhắn trong nhóm chat rất dễ bị trôi.
- Mình mong chương trình tiếp tục có các buổi gặp mặt trực tiếp như các sự kiện ở Bitexco, vì đó là lúc mình học được nhiều nhất từ người khác.
