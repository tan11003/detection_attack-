# 🚀 XÂY DỰNG HỆ THỐNG PHÁT HIỆN TẤN CÔNG MẠNG BẰNG MÔ HÌNH HỌC MÁY LSTM

**Sinh viên:** Huỳnh Trần Nhật Tân  
**MSSV:** N21DCAT045  
**Lớp:** D21CQAT01-N  
**Giảng viên hướng dẫn:** ThS. Phan Thanh Hy  

---

## 📘 Giới thiệu
Dự án này là **Báo cáo Thực tập Tốt nghiệp Đại học**, tập trung vào việc **ứng dụng Học sâu (Deep Learning)** trong lĩnh vực **An ninh mạng (Cybersecurity)**.  
Mục tiêu là xây dựng **hệ thống phát hiện tấn công mạng thông minh (Network Intrusion Detection System)** sử dụng mô hình **LSTM** – một biến thể mạnh mẽ của mạng nơ-ron hồi tiếp (RNN) có khả năng ghi nhớ thông tin theo chuỗi thời gian.

---

## 🎯 Mục tiêu và Phạm vi

### 1️⃣ Mục tiêu
- Xây dựng hệ thống có thể **phân tích lưu lượng mạng** và **nhận diện hành vi bất thường**.  
- Ứng dụng mô hình **LSTM** để phát hiện cả các **tấn công mới (zero-day attacks)**.  
- Đánh giá hiệu suất qua các chỉ số: `Accuracy`, `Precision`, `Recall`, `F1-Score`.  
- Đề xuất cải tiến và khả năng triển khai thời gian thực.

### 2️⃣ Phạm vi
- Phát hiện các tấn công mạng phổ biến trong môi trường **IoT và mạng máy tính**, như:
  - DoS/DDoS
  - Brute Force
  - Port Scanning
  - Web Attacks
  - Botnet  
- Triển khai thử nghiệm ở mức **NIDS (Network Intrusion Detection System)** trong môi trường mô phỏng.

---

## 🧠 Cơ sở Lý thuyết

### ⚙️ Mô hình LSTM
LSTM là một biến thể của **Recurrent Neural Network (RNN)**, được thiết kế để khắc phục hiện tượng **mất thông tin theo thời gian (vanishing gradient)**.  
Cấu trúc gồm 3 cổng chính:
- **Forget Gate:** Quyết định thông tin nào bị loại bỏ.  
- **Input Gate:** Quyết định thông tin nào được thêm vào.  
- **Output Gate:** Xác định thông tin nào được đưa ra ngoài.  

### 🔐 IDS/IPS
- **IDS (Intrusion Detection System):** Giám sát, phát hiện hành vi tấn công.  
- **IPS (Intrusion Prevention System):** Phát hiện và **chặn** các tấn công trực tiếp.  

Cơ chế hoạt động gồm:
- Phát hiện bất thường (Anomaly-based)
- Phân tích trạng thái giao thức (Stateful Protocol Analysis)
- Phản ứng chủ động (Active Response)

---

## 🧩 Huấn luyện Mô hình

### 📊 Bộ dữ liệu
- **Tên:** CIC-IoT2023 (Canadian Institute for Cybersecurity)  
- **Quy mô:** ~3 triệu bản ghi (10% tập dữ liệu gốc)  
- **Đặc trưng:** ~47 thuộc tính, đa dạng và cập nhật thực tế.  

### 🔧 Tiền xử lý dữ liệu
1. **Làm sạch:** Loại bỏ giá trị null, chuyển Boolean → float.  
2. **Chọn đặc trưng:** Dùng ma trận tương quan và Random Forest để chọn **Top 15 feature**.  
3. **Giảm nhiễu:** Loại bỏ đặc trưng dư thừa, tránh overfitting.  
4. **Chuẩn hóa:** Đưa dữ liệu về cùng thang đo.  
5. **Tạo chuỗi:** Biến dữ liệu thành các **sequence độ dài 10** cho đầu vào LSTM (dạng 3D).  

### 🧮 Kiến trúc mô hình
- `LSTM(64 units)`  
- `Dropout(0.3)`  
- `Dense(1, activation='sigmoid')`  
- `Optimizer: Adam`, `Loss: Binary Crossentropy`  

**Kết quả:**
| Metric | Giá trị |
|:-------|:--------|
| Test Accuracy | 97.65% |
| Test Loss | 0.1115 |

➡️ **Kết luận:** Mô hình có độ chính xác cao, sai số thấp, tổng quát hóa tốt, không bị overfitting.

---

## 🧪 Hệ thống Demo IDS/IPS

### 💻 Môi trường thử nghiệm
<img width="963" height="522" alt="image" src="https://github.com/user-attachments/assets/33450a2f-0f8e-44a2-8d6a-2a8f1774a35b" />

| Vai trò | Hệ điều hành | IP |
|:---------|:--------------|:--|
| Máy tấn công | Kali Linux | 192.168.2.52 |
| Máy phòng thủ/Web server | Windows 10 | 192.168.2.51 |

### 🔄 Quy trình hoạt động
1. Flask Web Server khởi chạy cùng luồng bắt gói tin TCP (`sniff_thread_func`).  
2. Mỗi gói tin được xử lý (`process_packet`) → trích xuất đặc trưng → đưa vào mô hình LSTM.  <img width="940" height="319" alt="image" src="https://github.com/user-attachments/assets/1aeb9741-356b-42c8-909b-2ecaafd53b94" />

3. Nếu bị phát hiện tấn công:
   - Ghi log cảnh báo thời gian thực.  
   - Chặn IP bằng lệnh hệ thống (`netsh`).  
   - Lưu danh sách IP bị chặn vào `blocked.txt`.  
4. Giao diện Web cung cấp:
   - `/alerts` – Danh sách cảnh báo.  
   - `/blocked` – IP bị chặn.  
   - `/unblock` – Mở chặn IP.

---

## 🏁 Kết luận & Hướng phát triển

### ✅ Kết luận
- Mô hình **LSTM đạt Accuracy 97.65%**.  
- Phát hiện và phản ứng hiệu quả với các hành vi tấn công mạng.  
- Hỗ trợ cảnh báo thời gian thực và tự động chặn IP độc hại.

### 🔮 Hướng phát triển
- Giảm tỷ lệ **False Positive**.  
- Phân loại chi tiết hơn **8 lớp tấn công** trong CIC-IoT2023.  
- Kết hợp **CNN-LSTM** hoặc **Attention Mechanism** để tối ưu.  
- Cân bằng dữ liệu (class imbalance) và thử nghiệm **real-time deployment**.

---

## 🧑‍💻 Liên hệ **Tác giả:** Huỳnh Trần Nhật Tân
📧 Email: *huynhtrannhattann@gmail.com* 
📍 Học viện Công nghệ Bưu chính Viễn thông (PTIT) 

