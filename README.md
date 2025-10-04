TÓM TẮT DỰ ÁN: XÂY DỰNG HỆ THỐNG PHÁT HIỆN TẤN CÔNG MẠNG BẰNG MÔ HÌNH HỌC MÁY LSTM
Đây là Báo cáo Thực tập Tốt nghiệp Đại học của sinh viên Huỳnh Trần Nhật Tân (Mã Sinh Viên: N21DCAT045, Lớp: D21CQAT01-N), được hướng dẫn bởi ThS. Phan Thanh Hy. Đề tài tập trung vào việc ứng dụng Học sâu (Deep Learning) để giải quyết các thách thức trong An ninh mạng hiện đại.
1. Mục tiêu Đề tài và Bối cảnh
1.1. Lý do lựa chọn Đề tài
Các hệ thống mạng máy tính ngày càng đóng vai trò cốt lõi, nhưng các cuộc tấn công mạng đang gia tăng về số lượng và mức độ tinh vi. Các phương pháp bảo mật truyền thống như tường lửa hay IDS dựa trên chữ ký (signature-based) thường không đủ khả năng phát hiện các kiểu tấn công mới hoặc biến thể chưa từng xuất hiện (zero-day). Mô hình LSTM (Long Short-Term Memory) nổi bật nhờ khả năng ghi nhớ và khai thác thông tin theo chuỗi thời gian, phù hợp với đặc thù dữ liệu lưu lượng mạng.
1.2. Mục tiêu Chính
1. Xây dựng và triển khai hệ thống phát hiện tấn công mạng thông minh có khả năng phân tích lưu lượng mạng và nhận diện các hành vi bất thường, bao gồm cả các cuộc tấn công mới (zero-day).
2. Ứng dụng mô hình học máy LSTM để xử lý dữ liệu mạng theo chuỗi thời gian, nhằm tăng độ chính xác và giảm tỷ lệ báo động giả.
3. Đánh giá hiệu quả của mô hình bằng các chỉ số như Accuracy, Precision, Recall, F1-score.
4. Đề xuất cải tiến và hướng phát triển cho khả năng triển khai thời gian thực.
1.3. Phạm vi Đề tài
Đề tài tập trung vào phát hiện các tấn công mạng ở mức NIDS (Network Intrusion Detection System). Chỉ xét đến các loại tấn công phổ biến trong môi trường mạng máy tính và IoT, như: DoS/DDoS, Brute Force, Port Scanning, Web Attacks, Botnet, v.v.. Mô hình được xây dựng và đánh giá trên môi trường mô phỏng/lab, chưa triển khai trực tiếp trong môi trường mạng thực tế thời gian thực.
2. Cơ sở Lý thuyết (Mô hình và Phòng thủ)
2.1. Mô hình LSTM
LSTM là một loại mạng nơ-ron hồi tiếp (RNN) được thiết kế để xử lý dữ liệu chuỗi và khắc phục vấn đề mất/nhạt dần thông tin (vanishing gradient problem) của RNN truyền thống.
• Cấu trúc: Một khối LSTM bao gồm ba cổng chính: Cổng quên (Forget Gate), Cổng đầu vào (Input Gate), và Cổng đầu ra (Output Gate). Cơ chế này giúp LSTM duy trì trạng thái ẩn và ghi nhớ các mối quan hệ theo thời gian.
2.2. Cơ chế Phòng thủ IDS/IPS
• IDS (Intrusion Detection System): Giám sát mạng để phát hiện các hành vi bất thường hoặc dấu hiệu tấn công. Gồm hai loại chính là dựa trên chữ ký (Signature-Based) và dựa trên hành vi bất thường (Anomaly-Based).
• IPS (Intrusion Prevention System): Phiên bản nâng cao của IDS, hoạt động trực tiếp trên đường mạng (inline) để chủ động ngăn chặn các cuộc tấn công độc hại diễn ra trong thời gian thực.
Các cơ chế được sử dụng bao gồm: Phòng thủ dựa trên bất thường (Anomaly-based Detection), Phân tích trạng thái giao thức (Stateful Protocol Analysis), Phản ứng chủ động (Active Response, như chặn IP), và Ghi log/cảnh báo.
3. Huấn luyện Mô hình
3.1. Bộ dữ liệu
• Tên: CIC-IoT2023 (Canadian Institute for Cybersecurity – Internet of Things 2023).
• Ưu điểm: Có tính thực tế cao, đa dạng và hiện đại, khắc phục nhược điểm của các bộ dữ liệu giả lập trước đây.
• Quy mô: Trích xuất hơn 3 triệu bản ghi từ 10% tập dữ liệu gốc để dễ dàng phân tích, chứa khoảng 46–47 tính năng.
3.2. Quy trình Tiền xử lý Dữ liệu
1. Làm sạch: Dữ liệu không có giá trị null. Các giá trị boolean (True/False) được chuyển sang kiểu số thực (float).
2. Lựa chọn Đặc trưng: Sử dụng ma trận tương quan và thuật toán Random Forest để trích xuất Top 15 đặc trưng quan trọng nhất.
3. Giảm nhiễu: Loại bỏ các cột có dữ liệu không cân bằng hoặc không rõ ý nghĩa để tránh hiện tượng overfitting và giảm số chiều dữ liệu.
4. Chuẩn hóa: Dữ liệu được chuẩn hóa (scaling) để đưa các đặc trưng về cùng thang đo, giúp mô hình học tốt hơn và ổn định hơn.
5. Tạo Chuỗi: Dữ liệu được định dạng thành các chuỗi liên tiếp (sequence) có độ dài seq_len (mặc định 10 bước) để phù hợp với yêu cầu đầu vào 3D của LSTM.
3.3. Kết quả Đánh giá Mô hình
• Kiến trúc: Mô hình sử dụng lớp LSTM (64 units), Dropout (0.3), lớp Dense (1, sigmoid), Optimizer Adam, và Loss Binary Crossentropy.
• Hiệu suất:
    ◦ Test Accuracy: 0.9765 (~97.65%).
    ◦ Test Loss: 0.1115.
• Nhận xét: Độ chính xác cao và Test Loss thấp, gần với Val Loss, cho thấy mô hình tổng quát hóa tốt và tránh được overfitting.
4. Thực nghiệm Hệ thống Demo (IDS/IPS)
4.1. Cấu hình Thực nghiệm
Hệ thống được thiết lập trong môi trường mô phỏng:
• Máy Tấn công: Kali Linux (IP: 192.168.2.52), vai trò khởi tạo tấn công.
• Máy Phòng thủ/Web Server: Windows 10 (IP: 192.168.2.51), chạy Web Server và mô hình LSTM phát hiện tấn công.
4.2. Cách thức Hoạt động
1. Hệ thống trên máy phòng thủ khởi chạy Flask Web Server và một luồng nền để bắt gói tin TCP (sniff_thread_func).
2. Mỗi gói tin được xử lý (process_packet) để lọc IP, cập nhật thống kê, trích xuất đặc trưng, và đưa vào mô hình LSTM để dự đoán.
3. Nếu mô hình dự đoán là tấn công (ATTACK_LABEL), hệ thống sẽ:
    ◦ Ghi log cảnh báo theo thời gian thực.
    ◦ Tự động kích hoạt chức năng chặn địa chỉ IP (IP Blocking) bằng cách gọi lệnh hệ thống (ví dụ: netsh trên Windows).
    ◦ Cập nhật trạng thái IP bị chặn vào file blocked.txt để giữ trạng thái.
4. Giao diện web cung cấp chức năng: Tổng quan thống kê IP, danh sách cảnh báo (/alerts), danh sách IP đã bị chặn (/blocked), và chức năng mở chặn IP.
5. Kết luận và Kiến nghị Hướng Phát triển
5.1. Kết luận
Hệ thống LSTM đã được xây dựng thành công, đạt độ chính xác cao (97.65%), có khả năng phát hiện chính xác các hành vi tấn công mạng và đưa ra cảnh báo kịp thời. Hệ thống demo còn hỗ trợ chức năng phản ứng chủ động như chặn địa chỉ IP tấn công.
5.2. Hạn chế và Hướng Phát triển
• Hạn chế: Hệ thống còn tồn tại nguy cơ cảnh báo giả (false positive) và hiệu năng có thể giảm khi triển khai trên lưu lượng mạng lớn.
• Kiến nghị Phát triển:
    1. Cải tiến hệ thống để có khả năng phân loại và phát hiện đầy đủ 8 lớp tấn công có trong bộ dữ liệu CIC-IoT-2023.
    2. Tối ưu kiến trúc bằng cách kết hợp với các mô hình tiên tiến hơn như CNN-LSTM hoặc Attention Mechanism.
    3. Bổ sung các kỹ thuật xử lý mất cân bằng dữ liệu (class imbalance).
    4. Thử nghiệm trong môi trường thời gian thực (real-time deployment) để đánh giá hiệu quả phản ứng
