**Họ tên:** Trương Thịnh  
**MSSV:** 2174802010070  
**Lớp:** 71ITAI40803  
**GVHD:** Nguyễn Thái Anh  

---



## BÀI 1: CẮT VÙNG QUẢ TÁO TỪ ẢNH GỐC

### Mục tiêu:
- Cắt một vùng chứa **quả táo** trong ảnh `fruit.jpg`.
- Lưu ảnh quả táo vào file riêng.
- Hiển thị ảnh kết quả sau khi cắt.

---

### Quy trình thực hiện:

1. **Đọc ảnh**:
   - Dùng `imageio.v2.imread()` để đọc ảnh `fruit.jpg`.

2. **Cắt vùng quả táo**:
   - Sử dụng slicing của NumPy để lấy vùng ảnh chứa quả táo.
   - Tọa độ được xác định thủ công:
     - `y = 800 → 1220`
     - `x = 1550 → 1880`
   - Biến `bmg` chứa phần ảnh quả táo.

3. **Lưu ảnh kết quả**:
   - Dùng `imageio.imsave()` để lưu ảnh cắt được dưới tên `apple.jpg`.

4. **Hiển thị ảnh kết quả**:
   - Dùng `matplotlib.pyplot` để hiển thị quả táo.

---

### Cấu trúc code:

- `numpy`: cắt vùng ảnh.
- `imageio.v2`: đọc và lưu ảnh.
- `scipy.ndimage`: import sẵn nhưng không sử dụng trong bài này.
- `matplotlib.pyplot`: hiển thị ảnh kết quả.

---

### Kết quả:

- Ảnh `apple.jpg` chứa riêng vùng quả táo được trích xuất từ ảnh lớn.
- Hiển thị đúng vùng cần xử lý.

---

## BÀI 2: DỊCH ẢNH XÁM THEO HAI CHIỀU

### Mục tiêu:
- Đọc ảnh ở chế độ **grayscale (xám)**.
- Dịch ảnh **xuống dưới 50 pixels** và **sang trái 30 pixels**.
- Hiển thị ảnh kết quả bằng thang độ xám (`cmap='gray'`).

---

### Quy trình thực hiện:

1. **Đọc ảnh grayscale**:
   - Dùng `imageio.v2.imread()` với tham số `mode='L'` để đọc ảnh dưới dạng ảnh xám.

2. **Dịch ảnh**:
   - Sử dụng `scipy.ndimage.shift()` để dịch chuyển:
     - Trục y: +50 → xuống dưới
     - Trục x: -30 → sang trái
   - Giữ nguyên giá trị kênh sáng (ảnh đơn kênh).

3. **Hiển thị kết quả**:
   - Dùng `matplotlib.pyplot` để hiển thị ảnh xám (`cmap='gray'`).
   - Ẩn trục tọa độ với `plt.axis('off')`.

---

### Cấu trúc code:

- `imageio.v2`: đọc ảnh đầu vào.
- `scipy.ndimage.shift`: thực hiện thao tác dịch ảnh theo vector.
- `matplotlib.pyplot`: hiển thị ảnh sau xử lý.

---

### Kết quả:

- Ảnh `fruit.jpg` sau khi chuyển sang ảnh xám được **dịch xuống dưới 50 pixel và sang trái 30 pixel**.
- Không thay đổi kích thước ảnh, vùng bị khuyết sẽ được điền giá trị mặc định (`0` – màu đen).

---
## BÀI 3: PHÓNG TO VÀ THU NHỎ ẢNH RGB

### Mục tiêu:
- Đọc ảnh màu RGB từ file `fruit.jpg`.
- Phóng to ảnh lên **3 lần**.
- Thu nhỏ ảnh xuống còn **0.3 lần** kích thước gốc.
- Hiển thị ảnh kết quả bằng `matplotlib`.

---

### Quy trình thực hiện:

1. **Đọc ảnh**:
   - Dùng `imageio.v2.imread()` để đọc ảnh màu (`fruit.jpg`).
   - Ảnh đầu vào có shape `(H, W, 3)`.

2. **Phóng to ảnh (Zoom x3)**:
   - Dùng `scipy.ndimage.zoom()` với hệ số `(3, 3, 1)`:
     - Phóng 3 lần theo chiều cao và chiều rộng.
     - Kênh màu giữ nguyên.

3. **Thu nhỏ ảnh (Zoom x0.3)**:
   - Dùng `zoom()` với hệ số `(0.3, 0.3, 1)`:
     - Giảm kích thước xuống còn 30% ảnh gốc.
     - Kênh màu giữ nguyên.

4. **Hiển thị ảnh**:
   - Dùng `matplotlib.pyplot.imshow()` để hiển thị từng ảnh.
   - Gắn tiêu đề cho từng ảnh để phân biệt rõ ràng.

---

### Cấu trúc code:

- `numpy`: xử lý dữ liệu mảng (tùy chọn).
- `scipy.ndimage.zoom`: phóng to / thu nhỏ ảnh.
- `imageio.v2`: đọc ảnh.
- `matplotlib.pylab`: hiển thị ảnh (dùng như `plt`).

---

### Kết quả:

- Ảnh gốc được phóng to và thu nhỏ đúng tỷ lệ.
- Kích thước ảnh sau xử lý:
  - `Zoom x3`: tăng gấp 3 lần cả chiều rộng và cao.
  - `Zoom x0.3`: giảm còn 30% ảnh gốc.
- Ảnh hiển thị đúng màu và tỷ lệ không bị méo.

---
## BÀI 4: XOAY ẢNH VỚI `reshape=True` VÀ `reshape=False`

### Mục tiêu:
- Xoay ảnh RGB một góc **45 độ**.
- So sánh kết quả khi:
  - **`reshape=True`** → ảnh sẽ tự mở rộng khung chứa, không bị cắt.
  - **`reshape=False`** → giữ nguyên kích thước ảnh gốc, nhưng có thể bị **cắt mép**.

---

### Quy trình thực hiện:

1. **Đọc ảnh RGB**:
   - Dùng `imageio.v2.imread()` để đọc ảnh `fruit.jpg`.

2. **Xoay ảnh với `reshape=True`**:
   - Dùng `scipy.ndimage.rotate(...)`:
     - Góc xoay: `45 độ`
     - `reshape=True` → kích thước ảnh sau xoay **tự điều chỉnh lớn hơn** để không cắt ảnh.
   - Kết quả có kích thước lớn hơn ảnh gốc.

3. **Xoay ảnh với `reshape=False`**:
   - Dùng cùng hàm trên, nhưng với `reshape=False`.
   - Ảnh giữ kích thước ban đầu, **nội dung xoay có thể bị cắt** nếu vượt ra khỏi khung hình.

4. **Hiển thị ảnh**:
   - Dùng `matplotlib.pyplot` để hiển thị 2 ảnh đã xoay với tiêu đề rõ ràng.

---

### Cấu trúc code:

- `imageio.v2`: đọc ảnh từ file.
- `scipy.ndimage.rotate`: xoay ảnh với tuỳ chọn reshape.
- `matplotlib.pylab`: hiển thị ảnh.

---

### Kết quả:

| Kiểu xoay         | Kết quả                             |
|------------------|-------------------------------------|
| `reshape=True`   | Ảnh không bị cắt, nhưng kích thước lớn hơn |
| `reshape=False`  | Ảnh giữ nguyên size, nhưng bị cắt góc |

- In ra `shape` của từng ảnh giúp kiểm tra thay đổi kích thước.

---
## BÀI 5: DILATION – EROSION – BIẾN ĐỔI HÌNH HỌC

### Mục tiêu:
- Áp dụng ba kỹ thuật xử lý ảnh:
  1. **Dilation (Giãn ảnh)**
  2. **Erosion (Xói mòn ảnh)**
  3. **Biến đổi hình học (Geometric Transform)**

---

### A. **Dilation – Giãn ảnh**

1. **Dữ liệu đầu vào**: ảnh màu `world_cup.jpg`
2. **Kỹ thuật áp dụng**:
   - Dùng `scipy.ndimage.grey_dilation` với kernel size `(5, 5, 1)`
   - Áp dụng giãn ảnh theo kênh màu
3. **Hiển thị kết quả** bằng `matplotlib`

👉 **Tác dụng**: mở rộng các vùng sáng, làm đối tượng lớn hơn.

---

### B. **Erosion – Xói mòn ảnh**

1. **Dữ liệu đầu vào**: ảnh màu `world_cup.jpg`
2. **Kỹ thuật áp dụng**:
   - Dùng `scipy.ndimage.grey_erosion` với size `(5, 5, 1)`
3. **Hiển thị ảnh kết quả**

👉 **Tác dụng**: thu hẹp các vùng sáng, làm đối tượng nhỏ đi.

---

### C. **Geometric Transform – Biến đổi hình học**

1. **Dữ liệu đầu vào**: ảnh xám (`mode='L'`)
2. **Hàm biến đổi tọa độ**:
   - Sử dụng sóng hình sin để làm méo ảnh:
     ```python
     a = 5 * sin(y/5.0) + y
     b = 5 * sin(x/5.0) + x
     ```
3. **Áp dụng bằng `nd.geometric_transform()`**
4. **Hiển thị ảnh kết quả (dạng grayscale)**

👉 **Tác dụng**: tạo hiệu ứng gợn sóng / biến dạng hình học thú vị.

---

### Cấu trúc code:

- `imageio.v2`: đọc ảnh màu và ảnh xám
- `scipy.ndimage`:
  - `grey_dilation`, `grey_erosion`: toán tử hình thái
  - `geometric_transform`: biến đổi theo tọa độ
- `matplotlib.pyplot`: hiển thị ảnh kết quả

---

### Kết quả:

| Kỹ thuật           | Hiệu ứng                        |
|-------------------|---------------------------------|
| Dilation          | Đối tượng sáng bị giãn to ra    |
| Erosion           | Đối tượng sáng bị thu nhỏ lại   |
| Geometric (sin)   | Ảnh bị biến dạng kiểu gợn sóng |

---