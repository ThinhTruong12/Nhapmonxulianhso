Họ tên: Trương Thịnh  
MSSV: 2174802010070  
Lớp: 71ITAI40803  
GVHD: Nguyễn Thái Anh  

## THƯ MỤC 
- `exercise/` : chứa ảnh gốc `dalat.jpg` để xử lý  
- `lang_biang.jpg` : ảnh kết quả sau xử lý  

## BÀI 1: PHÂN NGƯỠNG OTSU × 0.3 & DỊCH ẢNH SANG PHẢI 100 PIXELS

### Mục tiêu:
- Cắt một vùng ảnh LangBiang từ ảnh gốc.
- Chuyển sang ảnh xám (grayscale).
- Phân ngưỡng bằng Otsu, sau đó **nhân với 0.3** để tạo ngưỡng tùy chỉnh.
- Tạo ảnh nhị phân trắng đen.
- **Dịch ảnh sang phải 100 pixels**.
- Hiển thị và lưu ảnh kết quả.

### Quy trình thực hiện:

1. **Đọc ảnh**:  
   - Dùng `PIL.Image` để đọc ảnh RGB từ thư mục `exercise/`.

2. **Cắt vùng ảnh LangBiang**:  
   - Vị trí: góc trên bên trái.  
   - Kích thước: `H / 2`, `W / 3`.

3. **Chuyển sang grayscale**:  
   - Dùng `.convert('L')` để chuyển vùng ảnh sang ảnh xám.

4. **Tính ngưỡng Otsu và nhân 0.3**:  
   - Dùng `threshold_otsu()` từ `skimage.filters`.  
   - Nhân kết quả với 0.3 để tạo ngưỡng tùy chỉnh.

5. **Tạo ảnh nhị phân**:  
   - Dựa trên ngưỡng tùy chỉnh, tạo ảnh trắng đen (`True/False`).

6. **Dịch ảnh sang phải 100 pixels**:  
   - Tạo ảnh mới có chiều rộng lớn hơn 100 pixels.  
   - Gán vùng ảnh nhị phân vào vị trí mới (bắt đầu từ cột 100).

7. **Hiển thị và lưu ảnh kết quả**:  
   - Hiển thị bằng `matplotlib.pyplot`.  
   - Lưu kết quả với tên `lang_biang.jpg`.

### Cấu trúc code:

- `PIL` : đọc và xử lý ảnh gốc.
- `numpy` : thao tác cắt, gán và xử lý mảng ảnh.
- `skimage.filters` : tính ngưỡng Otsu.
- `matplotlib.pyplot` : hiển thị ảnh sau xử lý.

### Kết quả:

- Ảnh sau xử lý là ảnh **nhị phân đen trắng** của vùng LangBiang.  
- Dịch ảnh sang phải 100 pixels, phần nền phía trái là màu đen.  
- Ảnh được lưu thành `lang_biang.jpg` với tiêu đề hiển thị:  
  **"LangBiang (Otsu × 0.3 & Shift 100px)"**.

## BÀI 2: NHỊ PHÂN ẢNH HỒ XUÂN HƯƠNG & XOAY 45 ĐỘ

### Mục tiêu:
- Cắt vùng ảnh Hồ Xuân Hương từ ảnh gốc.
- Chuyển sang ảnh xám (grayscale).
- Phân ngưỡng bằng cách so sánh với **giá trị xám trung bình**.
- Tạo ảnh nhị phân trắng đen.
- Chuyển ảnh nhị phân sang RGB.
- **Xoay ảnh 45 độ**, tô phần trống bằng màu đen.
- Hiển thị và lưu ảnh kết quả.

### Quy trình thực hiện:

1. **Đọc ảnh**:  
   - Dùng `PIL.Image` để đọc ảnh RGB từ thư mục `exercise/`.

2. **Cắt vùng ảnh Hồ Xuân Hương**:  
   - Nằm giữa hàng trên (giữa ảnh gốc).  
   - Tọa độ: `y = 0 → h`, `x = w → 2w`.

3. **Chuyển sang grayscale**:  
   - Dùng `rgb2gray()` từ `skimage.color`.

4. **Tính ngưỡng và phân nhị phân**:  
   - Tính trung bình toàn ảnh xám → chọn những pixel có giá trị **nhỏ hơn trung bình** để tạo ảnh nhị phân (`True` nếu tối hơn).  
   - (Có thể dùng `threshold_local()` nếu ảnh sáng/tối không đều.)

5. **Tạo ảnh RGB từ ảnh nhị phân**:  
   - Dùng `np.stack()` để nhân ảnh nhị phân thành 3 kênh.

6. **Xoay ảnh 45 độ**:  
   - Dùng `rotate(45, expand=True, fillcolor=(0, 0, 0))`.

7. **Hiển thị và lưu ảnh kết quả**:  
   - Hiển thị bằng `matplotlib.pyplot`.  
   - Lưu kết quả với tên `ho_xuan_huong.jpg`.

### Cấu trúc code:

- `PIL` : đọc và xoay ảnh, lưu kết quả.
- `numpy` : xử lý ảnh nhị phân và RGB.
- `skimage.color` : chuyển ảnh sang grayscale.
- `skimage.filters` : ngưỡng cục bộ `threshold_local`.
- `matplotlib.pyplot` : hiển thị ảnh sau xử lý.

### Kết quả:

- Ảnh sau xử lý hiển thị Hồ Xuân Hương ở giữa ảnh gốc,  
  được chuyển sang ảnh nhị phân trắng đen dựa trên **mức xám trung bình**.  
- Ảnh được chuyển sang RGB và **xoay 45 độ**, phần trống được tô đen.  
- Ảnh kết quả được lưu với tên `ho_xuan_huong.jpg` và hiển thị như hình bên dưới.

## BÀI 3: BIẾN DẠNG ẢNH & LÀM MỊN (CLOSING) – QUẢNG TRƯỜNG LÂM VIÊN

### Mục tiêu:
- Cắt vùng ảnh Quảng trường Lâm Viên.
- Chuyển sang ảnh xám (grayscale).
- Tạo ảnh nhị phân với ngưỡng cố định (ảnh tối).
- Biến dạng không gian bằng hàm lượng giác.
- Làm mịn ảnh bằng phép đóng (closing).
- Hiển thị và lưu ảnh kết quả.

### Quy trình thực hiện:

1. **Đọc ảnh**:  
   - Dùng `PIL.Image` để đọc ảnh từ thư mục `exercise/`.

2. **Cắt vùng ảnh Quảng trường Lâm Viên**:  
   - Vị trí: hàng trên, cột phải (1 trong 6 vùng).  
   - Tọa độ: `y = 0 → h`, `x = 2w → 3w`.

3. **Chuyển sang grayscale**:  
   - Dùng `rgb2gray()` từ `skimage.color`.

4. **Tạo ảnh nhị phân**:  
   - Dùng ngưỡng cố định `0.6` để chọn vùng tối hơn.

5. **Biến dạng ảnh (geometric distortion)**:  
   - Dùng hàm `geometric_transform()` với công thức biến đổi tọa độ:  
     ```python
     a = 10 * cos(y / 10) + y  
     b = 10 * sin(x / 10) + x
     ```

6. **Làm mịn bằng closing**:  
   - Áp dụng `binary_closing()` để kết nối các điểm gần nhau.

7. **Hiển thị và lưu ảnh kết quả**:  
   - Hiển thị bằng `matplotlib.pyplot`.  
   - Lưu với tên `quan_truong_lam_vien.jpg`.

### Cấu trúc code:

- `PIL` : đọc và lưu ảnh.
- `numpy` : xử lý mảng ảnh.
- `skimage.color` : chuyển ảnh sang grayscale.
- `scipy.ndimage` : biến dạng ảnh và phép closing.
- `matplotlib.pyplot` : hiển thị ảnh sau xử lý.

### Kết quả:

- Ảnh nhị phân vùng Quảng trường Lâm Viên được tạo dựa trên ngưỡng tối `0.6`.  
- Áp dụng biến dạng lượng giác làm méo ảnh một cách mềm mại.  
- Phép **closing** giúp làm mịn ảnh, lấp kín các lỗ và tách các chi tiết nhỏ.  
- Ảnh kết quả được lưu với tên `quan_truong_lam_vien.jpg`.

## BÀI 4: MENU XỬ LÝ ẢNH – CHỌN VÀ KẾT HỢP HÀM BIẾN ĐỔI

### Mục tiêu:
- Thiết kế **menu tương tác** cho phép người dùng chọn các thao tác xử lý ảnh.
- Áp dụng các phép biến đổi hình học (geometric) và phân đoạn ảnh (segmentation).
- Cho phép thực hiện 1 hoặc **kết hợp 2 thao tác** liên tiếp.
- Hiển thị ảnh kết quả và lưu thành `output.jpg`.

### Quy trình thực hiện:

1. **Hiển thị menu**:  
   - Người dùng được chọn:
     - **1 thao tác** (biến đổi hình học hoặc phân đoạn).
     - **2 thao tác liên tiếp** (1 hình học → 1 phân đoạn).

2. **Các lựa chọn xử lý ảnh**:  
   - **Geometric transformation**:  
     - a. `coordinate_mapping`: Lật ảnh theo chiều ngang.  
     - b. `rotate`: Xoay ảnh 45 độ.  
     - c. `scale`: Phóng to ảnh theo tỉ lệ 1.5.  
     - d. `shift`: Dịch ảnh sang phải 100 pixels.

   - **Segmentation**:  
     - e. `adaptive_thresholding`: Dùng ngưỡng cố định để tạo ảnh nhị phân.  
     - f. `binary_dilation`: Giãn nở vùng trắng trong ảnh nhị phân.  
     - g. `binary_erosion`: Co lại vùng trắng trong ảnh nhị phân.  
     - h. `otsu`: Phân ngưỡng tự động bằng thuật toán Otsu.

3. **Áp dụng lựa chọn**:  
   - Hàm `apply_option()` sẽ nhận mã lựa chọn (`a` → `h`) và gọi hàm tương ứng.
   - Có thể kết hợp 2 thao tác liên tiếp: ví dụ `rotate` → `otsu`.

4. **Hiển thị và lưu kết quả**:  
   - Dùng `matplotlib.pyplot` để hiển thị ảnh.
   - Lưu ảnh kết quả thành `output.jpg`.

### Cấu trúc code:

- `PIL` : đọc ảnh, chuyển đổi, xoay, lật, resize.
- `numpy` : xử lý ảnh nhị phân và ma trận.
- `matplotlib.pyplot` : hiển thị ảnh.
- Tự viết các hàm xử lý nhị phân: erosion, dilation, threshold, Otsu.

### Kết quả:

- Người dùng có thể chọn **1 hoặc 2 thao tác xử lý ảnh** từ menu.
- Ảnh đầu ra được xử lý đúng theo yêu cầu và lưu thành `output.jpg`.
- Ví dụ: chọn `b → h` sẽ xoay ảnh 45 độ rồi phân ngưỡng bằng Otsu.