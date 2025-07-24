**Họ tên:** Trương Thịnh  
**MSSV:** 2174802010070  
**Lớp:** 71ITAI40803  
**GVHD:** Nguyễn Thái Anh  

---

## THƯ MỤC  
- `exercise/`: chứa ảnh đầu vào `a.jpg`  
- `output_bai20/`: chứa ảnh kết quả:
  - `a_mean_filtered.jpg`
  - `a_edge_detected.jpg`
  - `a_random_color.jpg`
  - `a_hue.jpg`, `a_saturation.jpg`, `a_value.jpg`

---

## Câu 1: XỬ LÝ ẢNH VỚI OPENCV - BỘ LỌC, BIÊN, MÀU NGẪU NHIÊN, KÊNH HSV

### Mục tiêu:
- Áp dụng 4 kỹ thuật xử lý ảnh phổ biến bằng thư viện OpenCV:
  1. **Lọc trung bình (Mean Filter)**
  2. **Phát hiện biên (Sobel)**
  3. **Điều chỉnh màu ngẫu nhiên**
  4. **Tách kênh HSV: Hue, Saturation, Value**

---

## MÔ TẢ CÁC BƯỚC

### 1. Lọc trung bình (Mean Filter)
- Dùng hàm `cv2.blur()` với kernel `5x5` để làm mờ ảnh, giảm nhiễu
```python
mean_filtered = cv2.blur(img, (5, 5))
```

---

### 2. Phát hiện biên (Sobel)
- Chuyển ảnh sang **grayscale** bằng `cv2.cvtColor`
- Áp dụng **Sobel theo X và Y**, sau đó tính độ lớn vector biên:
```python
sobelx = cv2.Sobel(gray, cv2.CV_64F, 1, 0)
sobely = cv2.Sobel(gray, cv2.CV_64F, 0, 1)
sobel = cv2.magnitude(sobelx, sobely)
```

---

### 3. Ngẫu nhiên hóa màu ảnh
- Nhân từng kênh **B, G, R** với một hệ số ngẫu nhiên `[0.5, 1.5]`
- Dùng `np.clip()` để giới hạn giá trị pixel từ 0 đến 255
```python
factor = random.uniform(0.5, 1.5)
img[:, :, c] = np.clip(img[:, :, c] * factor, 0, 255)
```

---

### 4. Tách kênh HSV
- Chuyển ảnh sang hệ màu HSV
- Dùng `cv2.split()` để tách thành:
  - **Hue**: màu sắc
  - **Saturation**: độ bão hòa
  - **Value**: độ sáng
```python
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
h, s, v = cv2.split(hsv)
```

---

## KẾT QUẢ

| Phép xử lý       | Ảnh kết quả                 |
|------------------|-----------------------------|
| Lọc trung bình   | `a_mean_filtered.jpg`       |
| Phát hiện biên   | `a_edge_detected.jpg`       |
| Màu ngẫu nhiên   | `a_random_color.jpg`        |
| Tách kênh HSV    | `a_hue.jpg`, `a_saturation.jpg`, `a_value.jpg` |

---




## Câu 2: XỬ LÝ ẢNH MỨC XÁM NÂNG CAO

## THƯ MỤC  
- `image1.jpg`, `image2.jpg`, `image3.jpg`: ảnh đầu vào mức xám  
- `outputs/`: chứa các ảnh kết quả đã xử lý theo từng phương pháp

---

### Mục tiêu:
Thực hiện các phép biến đổi điểm ảnh trên ảnh grayscale:
1. Biến đổi âm ảnh (Inverse)
2. Chỉnh Gamma
3. Biến đổi Logarit
4. Cân bằng Histogram toàn cục
5. Kéo giãn tương phản
6. Cân bằng Histogram cục bộ (CLAHE)

---

## MÔ TẢ CHI TIẾT CÁC PHÉP BIẾN ĐỔI

### 1. Biến đổi âm ảnh (Inverse)
- Công thức: `output = 255 - input`
- Đảo ngược mức xám, biến vùng sáng thành tối và ngược lại

---

### 2. Chỉnh Gamma
- Gamma ∈ [0.5, 2.0] chọn ngẫu nhiên mỗi lần chạy
- Công thức: `output = 255 * (input / 255)^(1/gamma)`
- Điều khiển độ sáng ảnh

---

### 3. Biến đổi Logarit
- Hệ số C ∈ [1.0, 5.0] được sinh ngẫu nhiên
- Công thức: `output = C * log(1 + input)`
- Làm nổi bật chi tiết vùng tối

---

### 4. Cân bằng Histogram
- Dùng `cv2.equalizeHist()` để phân bố lại histogram ảnh
- Tăng độ tương phản cho ảnh mờ

---

### 5. Kéo giãn tương phản
- Tự động chọn ngẫu nhiên `min` ∈ [0, 100], `max` ∈ [155, 255]
- Công thức:  
  ```python
  output = (input - min) * 255 / (max - min)
  ```
- Tăng cường độ tương phản giữa vùng sáng/tối

---

### 6. Cân bằng Histogram cục bộ (CLAHE)
- Sử dụng OpenCV `CLAHE` với:
  - `clipLimit=2.0`
  - `tileGridSize=(8,8)`
- Cải thiện độ tương phản cục bộ, giữ lại chi tiết ảnh

---

## CÁCH HOẠT ĐỘNG CHƯƠNG TRÌNH

- Khi chạy, chương trình cho phép chọn phím tương ứng với phép biến đổi:

| Phím | Phép biến đổi                          |
|------|----------------------------------------|
| I    | Biến đổi âm ảnh                        |
| G    | Chỉnh Gamma                            |
| L    | Biến đổi Logarit                       |
| H    | Cân bằng Histogram                     |
| C    | Kéo giãn tương phản                    |
| A    | CLAHE (Cân bằng Histogram cục bộ)      |
| Q    | Thoát chương trình                     |

- Mỗi lần thực hiện:
  - 3 ảnh đầu vào được xử lý bằng phương pháp đã chọn
  - Kết quả được lưu vào thư mục `outputs/`
  - Đồng thời hiển thị ảnh dùng `cv2.imshow(...)`

Ví dụ:
```bash
output_gamma_1.jpg
output_gamma_2.jpg
output_gamma_3.jpg
```

---
## Câu 3: BIẾN ĐỔI HÌNH HỌC & TĂNG CƯỜNG CHẤT LƯỢNG ẢNH

### Mục tiêu:
Thực hiện các phép biến đổi hình học và tăng cường chất lượng ảnh trên 3 ảnh đầu vào:
- Thay đổi kích thước (resize)
- Xoay ảnh, lật ảnh
- Làm mờ ảnh
- Điều chỉnh độ sáng & tương phản

---

## THƯ MỤC DỮ LIỆU
- `colorful-ripe-tropical-fruits.jpg`
- `quang_ninh.jpg`
- `pagoda.jpg`

---

## MÔ TẢ CÁC PHÉP BIẾN ĐỔI

### 1. Ảnh 1: **Fruits**
- **Tác vụ:** Phóng to ảnh gốc thêm `+30 pixel` theo cả chiều rộng và chiều cao.
- **Công cụ:** `cv2.resize()`
- **Ảnh kết quả:** `resized_fruits.jpg`

---

### 2. Ảnh 2: **Quảng Ninh**
- **Tác vụ:**
  - **Xoay** ảnh 45 độ ngược chiều kim đồng hồ
  - **Lật ngang** ảnh sau khi xoay
- **Công cụ:**
  - `cv2.getRotationMatrix2D()`
  - `cv2.warpAffine()`
  - `cv2.flip(img, 1)`
- **Ảnh kết quả:** `rotated_flipped_quangninh.jpg`

---

### 3. Ảnh 3: **Chùa (Pagoda)**
#### a. Resize & Gaussian Blur
- **Tác vụ:**
  - Phóng to ảnh gấp 5 lần
  - Làm mờ Gaussian (`kernel = 7x7`)
- **Công cụ:** `cv2.resize()`, `cv2.GaussianBlur()`
- **Ảnh kết quả:** `pagoda_blurred.jpg`

#### b. Điều chỉnh độ sáng & tương phản
- **Tác vụ:**
  - Thay đổi độ tương phản bằng hệ số `alpha ∈ [0.5, 2.0]`
  - Thay đổi độ sáng bằng hệ số `beta ∈ [-50, 50]`
- **Công thức:**
  ```
  new_img = alpha * img + beta
  ```
- **Ảnh kết quả:** `pagoda_brightness_contrast.jpg`

- **Tham số được chọn ngẫu nhiên**, ví dụ:
  ```bash
  Áp dụng alpha = 1.72, beta = 36
  ```

---

## KẾT QUẢ TẠO RA

| Tên ảnh đầu ra                      | Mô tả kết quả                              |
|------------------------------------|--------------------------------------------|
| `resized_fruits.jpg`              | Ảnh hoa quả được phóng to                 |
| `rotated_flipped_quangninh.jpg`   | Ảnh tỉnh Quảng Ninh sau xoay và lật ngang |
| `pagoda_blurred.jpg`              | Ảnh chùa sau phóng to và làm mờ Gaussian  |
| `pagoda_brightness_contrast.jpg`  | Ảnh chùa sau điều chỉnh sáng & tương phản |

---
