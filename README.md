<h1 align="center">🗑️ Phân Loại Rác với EfficientNetB0</h1>  

---

## Giới thiệu  
Phân loại rác là một bước quan trọng để xây dựng **hệ thống tái chế thông minh** và giảm thiểu tác động xấu đến môi trường.  
Trong dự án này, chúng tôi xây dựng mô hình học sâu sử dụng **EfficientNetB0** kết hợp với **transfer learning** để phân loại rác thành 4 loại chính: Hữu cơ, Nhựa, Kim loại và Giấy.  

---

## Giới thiệu về EfficientNetB0  
**EfficientNet** là một họ mô hình mạng nơ-ron tích chập (CNN) được Google AI đề xuất năm 2019, nổi bật với khả năng cân bằng giữa **độ chính xác** và **hiệu quả tính toán**.  

- **EfficientNetB0** là phiên bản cơ sở, được thiết kế bằng **Neural Architecture Search**.  
- Ý tưởng chính: **Compound Scaling** – thay vì chỉ tăng một yếu tố (sâu hơn hoặc rộng hơn), EfficientNet tăng **đồng thời** chiều sâu (depth), chiều rộng (width), và độ phân giải (resolution) theo một tỷ lệ cân bằng.  
- So với các mô hình CNN truyền thống (ResNet, VGG, Inception), EfficientNetB0 đạt:  
  - **Độ chính xác cao hơn**  
  - **Ít tham số hơn** → huấn luyện nhanh và tiết kiệm tài nguyên hơn  
- Đây là lý do EfficientNetB0 thường được dùng làm **baseline mạnh** cho các bài toán phân loại ảnh có dữ liệu vừa và nhỏ.  

---

## Dữ liệu  
- Bộ dữ liệu bao gồm hình ảnh rác chia thành **4 lớp**:  
  - Hữu cơ (Organic)  
  - Nhựa (Plastic)  
  - Kim loại (Metal)  
  - Giấy (Paper)  
- Tỷ lệ chia tập: **70% / 20% / 10%** (Train / Validation / Test)  
- Các kỹ thuật tăng cường dữ liệu:  
  - Xoay ngẫu nhiên  
  - Lật ngang/dọc  
  - Phóng to/thu nhỏ  
  - Dịch chuyển theo chiều rộng/chiều cao  

---

## Phương pháp  
### 1. Tiền xử lý  
- Chỉnh kích thước toàn bộ ảnh về `(224, 224)`  
- Chuẩn hóa giá trị pixel về `[0, 1]`  
- Áp dụng augmentation cho tập huấn luyện  

### 2. Kiến trúc mô hình  
- Backbone: **EfficientNetB0 (ImageNet pretrained)**  
- Các lớp tùy chỉnh:  
  - `GlobalAveragePooling2D`  
  - `Dropout(0.4)`  
  - `Dense(128, activation='relu')`  
  - `Dropout(0.3)`  
  - `Dense(4, activation='softmax')`  

### 3. Chiến lược huấn luyện  
- **Giai đoạn 1:** Freeze EfficientNetB0 backbone → train head layers  
- **Giai đoạn 2:** Fine-tune toàn bộ mô hình với learning rate nhỏ  
- Optimizer: Adam  
- Loss: Categorical Crossentropy  
- Callbacks: `EarlyStopping`, `ReduceLROnPlateau`, `ModelCheckpoint`  

---

## Kết quả  
- **Validation Accuracy:** ~ **91–92%**  
- **Validation Loss (min):** ~ **0.258**  
- Confusion Matrix cho thấy lỗi chủ yếu giữa **Nhựa ↔ Kim loại**  

---

## Trực quan hóa  
- Đường cong huấn luyện  
- Ma trận nhầm lẫn  

pred = model.predict(x)
print("Dự đoán:", classes[np.argmax(pred)])
