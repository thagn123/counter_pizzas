# counter_pizza
Lấy video từ link gg drive: https://drive.google.com/drive/folders/19QSILvBetBvcXyHjR85DahatiHOQSp_A
- Chạy file
```
python extract_frame.py
```
- Để lấy các khung hình huấn luyện.
các khung hình sẽ được ghi đè trong file frames
tiếp tục lấy các frame, sử dụng Label studio để gán nhãn và các bounding_boxs cho các đối tượng
- Sử dụng yoloV9 huấn luyện trên tập dữ liệu cá nhân nhằm nhận diện cụ thể chính xác với bối cảnh

## file mydata.yaml:
- phần "path" dán đường dẫn thích hợp của folder datasets, tùy theo vị trí tệp của bạn

## Logic đếm số lượng pizza
- sẽ là: đếm số lượng pizza đã được cho vào hộp và đưa đến tay của khách hàng
tuy nhiên, trong video sẽ có nhiều bối cảnh khác nhau và việc train mô hình theo dõi đối tượng yêu cầu nhiều tài nghuyên hơn nên chúng ta sẽ chỉ đếm số lượng pizza đã được cho vào hộp ( với thực phẩm như pizza việc cho vào hộp đồng nghĩa đã có hoàn thiện đơn hàng, gần như chắc chắn đơn hàng sẽ được giao đến tay khác hàng)

* Logic đếm pizza
### Nguyên tắc:
- Pizza được tính là hoàn tất khi được cho vào hộp.
- Tâm của bounding box pizza và box gần nhau → được xem là một lần "cho vào hộp".
### Cụ thể:
- Xác định tâm của mỗi bounding box.
- Nếu khoảng cách giữa tâm pizza và box < 80 px → đếm +1.
- Tránh đếm trùng bằng cách kiểm tra khoảng cách giữa các pizza đã đếm trước đó.

### 📌 Lưu ý
- Việc đếm này dựa trên giả định rằng: khi pizza được đóng hộp, đơn hàng đã hoàn thiện.
- Không sử dụng tracking để đơn giản hóa quá trình xử lý video.


# 🍕 Counter Pizza – YOLOv8 Video Counting System with Docker

Hệ thống đếm bánh pizza trong video bằng YOLOv8, được đóng gói hoàn toàn bằng Docker. Bao gồm quy trình từ trích xuất dữ liệu, huấn luyện mô hình đến hiển thị kết quả trực quan.



## 📁 Cấu trúc dự án

```
counter_pizza/
├── videos/              # Video đầu vào (.mp4)
├── data/                # Tập ảnh và nhãn gắn
├── weights/             # Mô hình đã huấn luyện (.pt)
├── extract_data.py      # Tách khung hình từ video để gắn nhãn
├── train_model.py       # Huấn luyện mô hình YOLOv8
├── main.py              # Đếm pizza từ video và hiển thị kết quả
├── requirements.txt     # Thư viện cần cài
├── Dockerfile           # Đóng gói ứng dụng
├── docker-compose.yml   # Quản lý các dịch vụ
└── README.md
```

##  🚀 Hướng dẫn sử dụng

### 1. Clone repo

```bash
git clone https://github.com/thagn123/counter_pizza.git
cd counter_pizza
```

### 2. Build Docker image

```bash
docker-compose build
```

### 3. [Tùy chọn] Trích xuất khung hình từ video

```bash
docker-compose run --rm extract
```

> Ảnh sẽ được lưu trong `data/` – dùng để gắn nhãn thủ công hoặc qua Roboflow, LabelImg...

### 4. Huấn luyện mô hình với dữ liệu cá nhân

```bash
docker-compose run --rm train
```

> Mô hình được lưu vào thư mục `weights/` dưới dạng `.pt`

### 5. Chạy đếm pizza từ video

```bash
docker-compose up detect
```

> Kết quả sẽ hiển thị trực tiếp trên khung hình video.

---

## 📌 Tùy chỉnh

- Đường dẫn video trong `main.py` có thể chỉnh tại:

```python
video_path = "videos/train_pizza.mp4"
```

- Nếu dùng Streamlit/Gradio giao diện, có thể mở tại `http://localhost:8501`

---

## 🛠️ Yêu cầu

- Docker & Docker Compose
- Python ≥ 3.8 (nếu không dùng Docker)
- RAM ≥ 4GB (khuyên dùng 8GB trở lên khi train mô hình)
```

---

Nếu bạn muốn, mình có thể thêm ảnh minh họa, GIF demo hoặc badge đẹp mắt (Build | Docker | YOLOv8) để README trông “xịn sò” hơn nữa. Up cho dân dev nhìn vào là yêu liền 😎

Muốn mình làm giúp phần đó không?
# counter_pizzas
