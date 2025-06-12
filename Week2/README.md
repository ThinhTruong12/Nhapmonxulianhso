Họ tên: Trương Thịnh MSSV: 2174802010070 Lớp: 71ITAI40803 GVHD: Nguyễn Thái Anh


## THƯ MỤC 
- `exercise/` : chứa các ảnh gốc cần xử lý
- `output_bai1/`, `output_bai2/`, `output_bai3/`, `output_bai4/` : chứa ảnh sau khi biến đổi




## BÀI 1: BIẾN ĐỔI ẢNH CƠ BẢN

Các phương pháp hỗ trợ:
- `I`: Inverse → Đảo ngược ảnh: `255 - pixel`
- `G`: Gamma Correction → Làm sáng hoặc tối ảnh theo công thức: `c * pixel^γ`
- `L`: Log Transformation → Nén độ tương phản ảnh sáng: `c * log(1 + pixel)`
- `H`: Histogram Equalization → Cân bằng độ sáng toàn ảnh
- `C`: Contrast Stretching → Kéo dãn độ tương phản bằng nội suy tuyến tính

 Cấu trúc code:
- Đọc ảnh bằng PIL, chuyển về grayscale
- Áp dụng phép biến đổi tương ứng dựa vào phím người dùng nhập
- Hiển thị và lưu ảnh kết quả



## BÀI 2: BIẾN ĐỔI MIỀN TẦN SỐ

 Các lựa chọn:
- `F`: Fast Fourier Transform → Biểu diễn phổ tần số bằng log-magnitude
- `L`: Butterworth Lowpass → Làm mờ ảnh, giữ chi tiết lớn, loại bỏ nhiễu
- `H`: Butterworth Highpass → Giữ chi tiết sắc nét, loại bỏ vùng đồng đều

 Cấu trúc code:
- Dùng `numpy.fft.fft2` và `fftshift` để đưa ảnh về miền tần số
- Áp dụng mặt nạ lọc Butterworth (tùy loại)
- Biến đổi ngược bằng `ifft2` → lấy phần thực để tạo ảnh kết quả
- Hiển thị và lưu ảnh



## BÀI 3: HOÁN ĐỔI RGB + PHÉP BIẾN ĐỔI NGẪU NHIÊN (TỪ BÀI 1)

 Quy trình:
1. Đọc ảnh RGB
2. Hoán đổi thứ tự màu (ví dụ: RGB → BGR, BRG, GRB,...)
3. Ngẫu nhiên chọn 1 phép biến đổi từ Bài 1 để áp dụng
4. Hiển thị và lưu kết quả

 Ý nghĩa:
- Thể hiện khả năng xử lý ảnh màu và kết hợp nhiều thao tác


 ## BÀI 4: HOÁN ĐỔI RGB + BIẾN ĐỔI TẦN SỐ NGẪU NHIÊN + BỘ LỌC CỰC TRỊ

 Quy trình:
1. Đọc ảnh RGB và hoán đổi màu
2. Ngẫu nhiên chọn 1 phép biến đổi từ Bài 2
3. Nếu:
   - Là **Lowpass** → áp dụng thêm Min Filter
   - Là **Highpass** → áp dụng thêm Max Filter
4. Biến đổi ảnh, hiển thị và lưu kết quả

 Giải thích các bộ lọc:
- **Min Filter**: làm mượt ảnh, giảm nhiễu
- **Max Filter**: giữ lại biên và chi tiết mạnh


