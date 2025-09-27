<h1 align="center">🗑️ Phân Loại Rác với EfficientNetB0</h1>

# Bài toán

Phân loại rác là một bước quan trọng để xây dựng hệ thống tái chế thông minh và giảm tác động xấu đến môi trường.
Trong dự án này, chúng tôi phát triển một mô hình học sâu để phân loại hình ảnh rác thành 4 loại bằng cách sử dụng EfficientNetB0 kết hợp với transfer learning.

# Dữ liệu

Bộ dữ liệu bao gồm hình ảnh rác chia thành 4 lớp:

Rác hữu cơ (Organic)

Nhựa (Plastic)

Kim loại (Metal)

Giấy (Paper)

Tỷ lệ chia tập: 70% / 20% / 10% (Train / Validation / Test)

Các kỹ thuật tăng cường dữ liệu (Data Augmentation):

Xoay ngẫu nhiên

Lật ngang/dọc

Phóng to/thu nhỏ

Dịch chuyển theo chiều rộng/chiều cao

#Phân tích dữ liệu (EDA)

Phân bố các lớp không hoàn toàn cân bằng → một số lớp (ví dụ: Kim loại) có ít mẫu hơn.

Các ảnh mẫu cho thấy có sự đa dạng lớn trong cùng một lớp (góc chụp, ánh sáng, nền khác nhau).

Do đó, áp dụng data augmentation để tăng khả năng khái quát hóa cho mô hình.

# Phương pháp
1. Tiền xử lý

Chỉnh kích thước toàn bộ ảnh về (224, 224)

Chuẩn hóa giá trị pixel về [0, 1]

Chỉ áp dụng tăng cường dữ liệu cho tập huấn luyện

2. Kiến trúc mô hình

Mô hình nền: EfficientNetB0 (huấn luyện trước trên ImageNet)

Các lớp tùy chỉnh (custom head):

GlobalAveragePooling2D

Dropout(0.4)

Dense(128, activation='relu')

Dropout(0.3)

Dense(4, activation='softmax')

3. Chiến lược huấn luyện

Giai đoạn 1: Đóng băng EfficientNetB0 backbone, chỉ huấn luyện head layers

Giai đoạn 2: Mở tất cả các lớp, fine-tune với learning rate nhỏ

Bộ tối ưu (Optimizer): Adam

Hàm mất mát (Loss): Categorical Crossentropy

Callbacks: EarlyStopping, ReduceLROnPlateau, ModelCheckpoint

# Kết quả
Độ chính xác Validation: ~ 91–92%

Validation Loss (min): ~ 0.258

Ma trận nhầm lẫn (Confusion Matrix) cho thấy lỗi thường gặp giữa Nhựa ↔ Kim loại

Độ chính xác Validation: ~ 91–92%

Validation Loss (min): ~ 0.258

Ma trận nhầm lẫn (Confusion Matrix) cho thấy lỗi thường gặp giữa Nhựa ↔ Kim loại
