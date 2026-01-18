# DATN_StarGAN
**Đồ án tốt nghiệp – Xây dựng ứng dụng thay đổi thuộc tính ảnh khuôn mặt dựa trên mô hình StarGAN**

---

## 1. Giới thiệu

Repository này chứa toàn bộ mã nguồn phục vụ đồ án tốt nghiệp với mục tiêu **nghiên cứu, huấn luyện, đánh giá và triển khai mô hình StarGAN cho bài toán thay đổi thuộc tính ảnh khuôn mặt người** (Facial Attribute Manipulation) trên tập dữ liệu CelebA.

Nội dung của repository được tổ chức theo đúng quy trình nghiên cứu thực nghiệm, bao gồm:
- Huấn luyện mô hình StarGAN theo nhiều giai đoạn dựa trên checkpoint,
- Đánh giá định lượng chất lượng ảnh sinh bằng các chỉ số SSIM và FID,
- Xây dựng các ứng dụng minh họa khả năng ứng dụng thực tế của mô hình (web app và chatbot).

---

## 2. Cấu trúc thư mục

DATN_StarGAN/
│
├── DATN_StarGAN_Train/
│ ├── stargan_face.py
│ ├── stargan_face_ep2.py
│ ├── stargan_face_ep3.py
│ ├── stargan_face_ep4.py
│ └── ...
│
├── DATN_StarGAN_Evaluate/
│ ├── evaluate-stargan-graph.py
│ ├── evaluate-stargan-graph-2.py
│ ├── evaluate-stargan-graph-3.py
│ ├── evaluate-stargan-graph-4.py
│ ├── evaluate-stargan-graph-5.py
│ └── evaluate-stargan-graph-draw.py
│
└── DATN_StarGAN_App/
├── stargan-web.py
└── stargan-chatbot.py


---

## 3. Huấn luyện mô hình

Thư mục `DATN_StarGAN_Train` chứa các file phục vụ **quá trình huấn luyện mô hình StarGAN theo từng giai đoạn**.

### 3.1 Huấn luyện theo checkpoint

Do hạn chế về tài nguyên GPU và thời gian huấn luyện (đặc biệt trong môi trường Kaggle), quá trình huấn luyện không được thực hiện trong một lần duy nhất. Thay vào đó:

- Mỗi file `stargan_face_ep*.py` tương ứng với một giai đoạn huấn luyện,
- Các giai đoạn sau sử dụng lại checkpoint đã lưu từ giai đoạn trước (`resume_iters`) để tiếp tục huấn luyện,
- Cách tiếp cận này giúp đảm bảo tính liên tục của quá trình học và tối ưu việc sử dụng tài nguyên miễn phí.

### 3.2 Các file huấn luyện

- `stargan_face.py`  
  Huấn luyện mô hình StarGAN từ đầu trên tập dữ liệu CelebA.

- `stargan_face_ep2.py`, `stargan_face_ep3.py`, `stargan_face_ep4.py`, ...  
  Các giai đoạn huấn luyện tiếp theo, sử dụng checkpoint đã lưu để:
  - tăng số vòng lặp huấn luyện,
  - cải thiện chất lượng ảnh sinh,
  - ổn định hơn quá trình học của mô hình.

---

## 4. Đánh giá mô hình

Thư mục `DATN_StarGAN_Evaluate` chứa các file phục vụ **đánh giá định lượng chất lượng ảnh sinh** tại các checkpoint khác nhau.

### 4.1 Đánh giá theo checkpoint

Các file:
- `evaluate-stargan-graph.py`
- `evaluate-stargan-graph-2.py`
- `evaluate-stargan-graph-3.py`
- `evaluate-stargan-graph-4.py`
- `evaluate-stargan-graph-5.py`

được sử dụng để:
- Tải các checkpoint tương ứng,
- Sinh ảnh theo từng thuộc tính khuôn mặt,
- Tính toán các chỉ số đánh giá:
  - **SSIM (Structural Similarity Index)**,
  - **FID (Fréchet Inception Distance)**.

### 4.2 Tổng hợp và trực quan hóa

- `evaluate-stargan-graph-draw.py`  
  File này tổng hợp toàn bộ kết quả đánh giá từ các checkpoint và vẽ các biểu đồ so sánh, phục vụ phân tích và lựa chọn mô hình tối ưu.

Checkpoint có **FID nhỏ nhất (500,000 iterations)** được lựa chọn để sử dụng cho các ứng dụng minh họa.

---

## 5. Ứng dụng mô hình

Thư mục `DATN_StarGAN_App` chứa các file triển khai mô hình StarGAN đã huấn luyện vào các ứng dụng thực tế.

### 5.1 Ứng dụng Web

- `stargan-web.py`  
- Cho phép người dùng:
  - tải ảnh khuôn mặt,
  - lựa chọn một trong 17 thuộc tính,
  - nhận ảnh kết quả sau khi mô hình xử lý.
- Giao diện được xây dựng bằng **Gradio**, kết hợp pipeline xử lý ảnh hoàn chỉnh.

### 5.2 Chatbot Telegram

- `stargan-chatbot.py`  
- Tích hợp mô hình StarGAN vào chatbot Telegram.
- Người dùng có thể gửi ảnh trực tiếp, chọn thuộc tính và nhận lại ảnh kết quả ngay trong khung chat.

Cả hai ứng dụng đều sử dụng **chung một pipeline xử lý cốt lõi**, chỉ khác nhau ở lớp giao diện và cơ chế tương tác với người dùng.

---

## 6. Dữ liệu sử dụng

- **Dataset**: CelebA  
- **Nguồn**: https://www.kaggle.com/datasets/jessicali9530/celeba-dataset  
- Bao gồm hơn 200,000 ảnh khuôn mặt người nổi tiếng với 40 thuộc tính nhị phân.
- Trong đồ án này, 17 thuộc tính phổ biến được lựa chọn để huấn luyện và đánh giá mô hình.

---

## 7. Ghi chú

- Mã nguồn được xây dựng và thử nghiệm chủ yếu trong môi trường Kaggle.
- Các tham số huấn luyện và checkpoint được lựa chọn dựa trên nhiều lần thử nghiệm thực tế.
- Repository phục vụ mục đích nghiên cứu và học thuật.

---

## 8. Tác giả

**Lý Văn Hiếu**  
Đồ án tốt nghiệp – Chuyên ngành: Kỹ sư chuyên sâu Trí tuệ nhân tạo tạo sinh, Trường Công nghệ thông tin và Truyền thông, Đại học Bách Khoa Hà Nội.
