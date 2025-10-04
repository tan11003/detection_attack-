Xây dựng Hệ thống Phát hiện Tấn công Mạng bằng Mô hình Học máy LSTM
Báo cáo Thực tập Tốt nghiệp Đại học.
Sinh viên thực hiện: Huỳnh Trần Nhật Tân Mã Sinh Viên: N21DCAT045 Lớp: D21CQAT01-N Người hướng dẫn: ThS. Phan Thanh Hy

--------------------------------------------------------------------------------
1. Giới thiệu Đề tài
Trong bối cảnh các cuộc tấn công mạng ngày càng tinh vi và khó phát hiện, hệ thống bảo mật truyền thống như tường lửa hay IDS dựa trên chữ ký (signature-based) thường không đủ khả năng phát hiện các kiểu tấn công mới (zero-day). Đề tài này khai thác tiềm năng của học sâu (Deep Learning) và mô hình Long Short-Term Memory (LSTM), một nhánh của mạng nơ-ron hồi tiếp (RNN), để xây dựng một hệ thống phát hiện xâm nhập mạng thông minh.
Mục tiêu chính là thiết kế hệ thống có khả năng phân tích lưu lượng mạng hoặc nhật ký hệ thống theo chuỗi thời gian, từ đó nhận diện nhanh chóng và chính xác các hành vi tấn công, đặc biệt là các hành vi phức tạp diễn ra theo trình tự.
2. Mục tiêu và Phạm vi
Mục tiêu chính
1. Xây dựng và triển khai hệ thống phát hiện tấn công mạng thông minh có khả năng phân tích lưu lượng mạng và nhận diện các hành vi bất thường, bao gồm cả các cuộc tấn công mới (zero-day).
2. Ứng dụng mô hình học máy LSTM để xử lý dữ liệu mạng theo chuỗi thời gian, khai thác khả năng ghi nhớ dài hạn của LSTM nhằm tăng độ chính xác và giảm tỷ lệ báo động giả.
3. Đánh giá hiệu quả của mô hình bằng các chỉ số như Accuracy, Precision, Recall, F1-score.
4. Hỗ trợ chức năng phản ứng như chặn các địa chỉ IP tấn công.
Phạm vi Đề tài
• Tập trung vào việc phát hiện các tấn công mạng ở mức NIDS (Network Intrusion Detection System).
• Chỉ xét đến các loại tấn công phổ biến trong môi trường mạng máy tính và IoT, như: DoS/DDoS, Brute Force, Port Scanning, Web Attacks, Botnet, v.v..
• Mô hình được xây dựng và đánh giá trên môi trường mô phỏng/lab, chưa triển khai trực tiếp trong môi trường mạng thực tế thời gian thực.
• Hệ thống tập trung vào phát hiện (detection), không bao gồm chức năng khắc phục hậu quả (mitigation).
3. Cơ sở Lý thuyết và Công nghệ
3.1. Mô hình LSTM
LSTM (Long Short-Term Memory) là một loại mạng nơ-ron hồi tiếp (RNN) được thiết kế đặc biệt để xử lý dữ liệu chuỗi và khắc phục vấn đề mất/nhạt dần thông tin (vanishing gradient problem) mà RNN truyền thống hay gặp. LSTM sử dụng cơ chế bộ nhớ thông minh, cho phép lưu giữ các trạng thái quan trọng trong thời gian dài.
Một khối LSTM bao gồm ba cổng chính:
• Cổng quên (Forget Gate): Xác định thông tin nào trong trạng thái cũ nên bị loại bỏ.
• Cổng đầu vào (Input Gate): Xác định thông tin mới nào sẽ được lưu vào bộ nhớ.
• Cổng đầu ra (Output Gate): Xác định phần nào của bộ nhớ sẽ được đưa ra ngoài như đầu ra tại bước thời gian hiện tại.
3.2. Bộ Dữ liệu
• Tên bộ dữ liệu: CIC-IoT2023 (Canadian Institute for Cybersecurity – Internet of Things 2023).
• Ưu điểm: Có tính thực tế cao, đa dạng và hiện đại, bao gồm nhiều loại tấn công thực tế chưa được ghi nhận trong các bộ dữ liệu IoT trước đó.
• Quy mô: Dùng 10% tập dữ liệu, khoảng hơn 3 triệu bản ghi, với khoảng 46–47 tính năng (features).
3.3. Tiền xử lý Dữ liệu và Huấn luyện
1. Tiền xử lý: Dữ liệu được làm sạch (không có giá trị null), chuyển đổi dữ liệu kiểu boolean sang số thực (float).
2. Trích xuất Đặc trưng: Sử dụng thuật toán Random Forest để trích xuất ra các đặc trưng quan trọng nhất.
3. Giảm nhiễu: Loại bỏ những cột có dữ liệu không cân bằng hoặc không rõ ý nghĩa để tránh hiện tượng overfitting.
4. Chuẩn hóa dữ liệu: Sử dụng các kỹ thuật chuẩn hóa (Scaler) để đưa các đặc trưng về cùng thang đo, giúp mô hình học tốt hơn và ổn định hơn.
5. Tạo Chuỗi: Dữ liệu được tạo thành các chuỗi (sequences) có độ dài seq_len (mặc định 10 bước) để phù hợp với yêu cầu đầu vào 3D của LSTM.
6. Huấn luyện: Mô hình LSTM được huấn luyện với Optimizer Adam và Loss Binary Crossentropy. Sử dụng EarlyStopping để tránh overfitting.
4. Cấu hình Demo Hệ thống
Hệ thống được thử nghiệm trong môi trường mô phỏng gồm hai máy ảo tương tác trong mạng nội bộ:
Thành phần
Hệ điều hành
Địa chỉ IP
Vai trò
Máy Tấn công (Attack Machine)
Kali Linux
192.168.2.52
Khởi tạo các cuộc tấn công (DDoS, v.v.).
Máy Phòng thủ (Web Server / IDS)
Windows 10
192.168.2.51
Chạy Web Server và mô hình LSTM phát hiện tấn công.
Cài đặt trên Máy Tấn công (Kali Linux)
# Thiết lập IP tĩnh
iface eth0 inet static
address 192.168.2.52
netmask 255.255.255.0
gateway 192.168.2.1
sudo systemctl restart networking

# Cài đặt các công cụ tấn công
sudo apt install hping3 -y
sudo apt install slowloris -y
sudo pip3 install uf0net
5. Kết quả Đánh giá Mô hình
Sau khi huấn luyện mô hình với bộ dữ liệu CIC-IoT2023, kết quả đánh giá cho thấy mô hình hoạt động hiệu quả.
• Test Accuracy: 0.9765 (~97.65%).
• Test Loss: 0.1115.
Mô hình hội tụ nhanh (trước epoch 5), hoạt động ổn định và đáng tin cậy để triển khai cho bài toán phát hiện tấn công.
6. Hướng Dẫn Vận hành Hệ thống Demo
Hệ thống IDS/IPS demo chạy trên máy phòng thủ, sử dụng Flask để quản lý giao diện web và một luồng nền để bắt gói tin.
Luồng Hoạt động (Tóm tắt)
1. Hệ thống chạy Web Server và một luồng bắt gói tin (sniff_thread_func).
2. Mỗi gói tin TCP được bắt và xử lý (process_packet): lọc IP, cập nhật thống kê, trích xuất đặc trưng và đưa vào mô hình LSTM để dự đoán.
3. Nếu mô hình dự đoán là tấn công (ATTACK_LABEL):
    ◦ Hệ thống ghi log cảnh báo.
    ◦ Hệ thống tự động kích hoạt chức năng chặn IP (IP Blocking) bằng cách gọi lệnh hệ thống (ví dụ: netsh trên Windows).
4. Giao diện web (Flask) hiển thị các thông tin thống kê, danh sách cảnh báo, và danh sách các IP đã bị chặn. Chức năng mở chặn IP cũng được hỗ trợ qua giao diện.
Các Chức năng Quản lý
• http://192.168.2.51:5000/: Trang tổng quan (Overview), hiển thị danh sách IP kèm thống kê.
• http://192.168.2.51:5000/alerts: Trang cảnh báo, hiển thị log các sự kiện tấn công theo thời gian thực.
• http://192.168.2.51:5000/blocked: Danh sách các IP đã bị chặn.
• /block/<ip>: Chặn một IP trực tiếp (qua lệnh firewall).
• /unblock/<ip>: Gỡ chặn IP.
• /shutdown: Dừng Server.
