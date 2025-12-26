# Overview
![](image/zammad_logo.png)  
Dự án này trình bày việc triển khai và cấu hình Zammad, một hệ thống hỗ trợ/quản lý yêu cầu mã nguồn mở, trên Google Cloud Platform (GCP) với môi trường sẵn sàng cho sản xuất.  
Mục tiêu là xây dựng một hệ thống hỗ trợ đầy đủ chức năng bao gồm quản lý yêu cầu, tích hợp email, tiện ích trò chuyện trực tuyến, quy tắc tự động hóa và bảo mật SSL.  

Dự án này bao gồm:  
- Cung cấp một máy ảo (VM) trên GCP và triển khai Zammad với Nginx.
- Bảo mật hệ thống bằng HTTPS (Let’s Encrypt).
- Thiết lập các kênh email (IMAP/SMTP) để tạo và trả lời yêu cầu hỗ trợ.
- Cấu hình tiện ích trò chuyện web và nhúng nó vào trang web.
- Triển khai Nhóm, Vai trò, Kích hoạt và SLA để mô phỏng quy trình hỗ trợ thực tế.
  
Việc triển khai này mô phỏng cách thức hoạt động của các nhóm hỗ trợ CNTT hiện đại và làm nổi bật các kỹ năng quản trị Linux, cơ sở hạ tầng đám mây, mạng và tích hợp hệ thống.

# Cài đặt & cấu hình Zammad
## Thiết lập môi trường Zammad
Chọn VPC trên Google Cloud Platform
![](image/ChooseVPC.png)  

OS được chọn để cài đặt Ubuntu 22.04 LTS, 30 Gb ổ cứng.  
![](image/OS-VPC.png)  

Cho phép HTTP, HTTPS truy cập WebUI Zammad.  
![](image/Network-VPC.png)  

## Cài đặt Zammad
Tài liệu cài đặt Zammad ở Ubuntu: __https://docs.zammad.org/en/latest/install/package.html__  
Tài liệu cài đặt Elasticsearch: __https://docs.zammad.org/en/latest/install/elasticsearch.html__  
  
Cài đặt các gói cần thiết.  
![](image/instal-package.png)  

Cài đặt Elasticsearch
![](image/install-elastic1.png)  
![](image/install-elastic2.png)  

Cấu hình trong file /etc/elasticsearch/elasticsearch.yml  
![](image/edit-conf-elastic.png)  

Kiểm tra ngôn ngữ.  
![](image/setupLANG.png)  

Cập nhật và Cài đặt Zammad.  
![](image/install-zammad.png)  

Thiết lập rule firewall.  
![](image/setup-firewall-rule.png)  

Dashboard Zammad.  
![](image/Dashboard-Zammad.png)  
Tích hợp Elasticsearch vào Zammad  
Dùng command: __sudo zammad run rails r "Setting.set('es_url', 'http://localhost:9200')"__  
Sau đó rebuild Zammad  
![](image/setupElasticsearch-toZammad.png)  
  
Dashboard của Elasticsearch  
![](image/Elasticsearch_Dashboard.png)  
# Groups – Roles – Users
## Groups
Giải thích từng trường khi tạo group:  
- Name *: Tên group xử lý ticket.  
- Parent group: Group cha (nếu chia cấp).  
- Assignment Timeout: Thời gian giữ ticket cho agent.  
- Follow-up possible *: Cho phép khách reply email cũ.  
- Assign Follow-Ups *: Giữ ticket trong group này khi có reply.  
- Sending Email Address: Email dùng để gửi phản hồi.  
- Signature: Chữ ký email tự động.  
- Shared Drafts: Cho phép nhiều agent soạn chung 1 email.
  
![](image/Create_Group1.png)  
![](image/Create_Group2.png)  
  
## Users
Giải thích từng trường khi tạo user:  
- First name / Last name: Tên người dùng.  
- Email: Định danh chính của user.  
- Organization:  Công ty / phòng ban của user.  
- VIP: Đánh dấu user ưu tiên (Có thể dùng trigger nâng priority).  
- Roles: Vai trò người dùng (Quyết định quyền hạn: Customer, Agent, Admin,...).  
- Active *: Trạng thái user.  
  
![](image/Create_User1.png)  
![](image/Create_User2.png)  
  
# Email Channel
## Setup Email Accounts
- Kết nối mailbox thật (Gmail, M365, IMAP/SMTP).
- Nhận email → tạo ticket
- Gửi email → reply ticket  
![](image/Email_Inbound.png)  
![](image/Archive_Email.png)  
![](image/Email_Outbound.png)
  
## Setup Email Notification
- Dùng để GỬI thông báo hệ thống.  
- Thông báo: Ticket mới, Ticket được assign, Ticket được cập nhật.
- Gửi cho: Agent, Customer.  
![](image/Email_Notification.png)  
  
## Overview
![](image/Email_Channel.png)  
  
## Kiểm tra tạo ticket bằng email
dùng email cá nhân gửi đến email trên Zammad.  
![](image/gmail-to-emailZammad.png)  
![](image/Test-ticket-email.png)  
  
## Filter Email
### Filter System / No-reply (Chặn mail hệ thống)
![](image/Filter1.png)  

### Filter IT Support
![](image/Filter2.png)  

### Filter HR
![](image/Filter3.png)  

### Filter Default / Fallback (Email không match rule nào)
![](image/Filter4.png)  

### Filter VIP User
![](image/Filter5.png)  

## Signatures Email
### Signature Sales
![](image/Signature1.png)  

### Signature IT Support
![](image/Signature2.png)  

### Signature HR
![](image/Signature3.png)  

# SLA và Triggers
## SLAs
SLA là tập hợp các cam kết về thời gian phản hồi và xử lý ticket, nhằm đảm bảo chất lượng dịch vụ hỗ trợ CNTT.  
- Xác định phải phản hồi trong bao lâu  
- Xác định phải xử lý/xong trong bao lâu  
- Áp dụng khác nhau theo mức độ ưu tiên  
  
### General SLA
SLA mặc định cho phần lớn ticket thông thường từ nhân viên/khách hàng, không mang tính khẩn cấp.
Áp dụng cho:  
- Ticket IT nội bộ
- Yêu cầu hỗ trợ chung
- Lỗi không ảnh hưởng lớn đến vận hành  
![](image/SLA1.png)

### VIP SLA
SLA ưu tiên cao cho người dùng VIP / quản lý / khách hàng quan trọng.  
Áp dụng cho:  
- User có cờ VIP = yes
- Ban giám đốc, trưởng phòng
- Khách hàng chiến lược  
![](image/SLA2.png)

### Critical / Incident SLA
SLA cho sự cố nghiêm trọng, ảnh hưởng trực tiếp đến hoạt động kinh doanh hoặc toàn hệ thống.  
Áp dụng cho:  
- System down
- Mạng nội bộ / email / ERP / website ngưng hoạt động
- Sự cố an ninh, mất dữ liệu  
![](image/SLA3.png)

## Triggers
Trigger là cơ chế tự động thực thi hành động khi ticket thỏa mãn các điều kiện nhất định.  
- Kích hoạt khi ticket được tạo hoặc cập nhật  
- Dựa trên điều kiện (VIP, trạng thái, priority, group…)  
- Tự động gửi thông báo, đổi trạng thái, gán group/agent, đặt priority…  
  
### Notify agent when VIP ticket created
Thông báo ngay khi có ticket VIP.  
![](image/Trigger1.png)  
![](image/Trigger2.png)  
  
# Live Chat Channel
Zammad đã cho tích hợp đoạn script từ đó tích hợp vào website.  
![](image/Livechat1.png)  
  
Live chat thông thường sẽ được gắn vào mục Contact của các website.  
![](image/Livechat2.png)  
  
Demo Live chat channel   
![](image/demoLivechat1.png)  
![](image/demoLivechat2.png)  

