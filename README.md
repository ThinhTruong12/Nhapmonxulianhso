# Nhapmonxulianhso
Bài 1
Đọc ảnh
1. Sử dụng imageio.v2.imread để đọc ảnh scence.png vào biến data.
Ảnh được lưu dưới dạng mảng NumPy 3 chiều với kích thước (chiều cao, chiều rộng, 3 kênh màu RGB).

2. Sau đó tách từng kênh màu

"rdata = data[:, :, 0]: Lấy kênh màu đỏ (Red)."

"gdata = data[:, :, 1]: Lấy kênh màu lục (Green)."

"bdata = data[:, :, 2]: Lấy kênh màu xanh dương (Blue)."

4. Hiển thị từng kênh màu dưới dạng ảnh đơn kênh

5. Dùng matplotlib.pyplot.imshow để hiển thị từng kênh màu riêng biệt:

rdata được hiển thị với bảng màu (cmap) là 'Reds' để làm nổi bật màu đỏ.

gdata được hiển thị với bảng màu 'Greens' làm nổi bật màu xanh lá.

bdata được hiển thị với bảng màu 'Blues' làm nổi bật màu xanh dương.

Kết quả sẽ cho ra được :

Ảnh kênh đỏ sẽ hiển thị ảnh màu đỏ.

Ảnh kênh lục sẽ hiển thị ảnh màu lục.

Ảnh kênh xanh sẽ hiển thị ảnh màu xanh dương.

Bài 2
1. Hoán đổi thứ tự các kênh màu:

rgb_to_grb = img[:, :, [1, 0, 2]]

Đổi thứ tự từ RGB → GRB: hoán đổi Red và Green.

rgb_to_bgr = img[:, :, [2, 1, 0]]

Đổi thứ tự từ RGB → BGR: hoán đổi Red và Blue.

rgb_to_brg = img[:, :, [2, 0, 1]]

Đổi thứ tự từ RGB → BRG: Blue đứng đầu, Green đứng cuối.
2. Sau đó lưu 3 ảnh đã hoán đổi thành các file ảnh mới với tên tương ứng.

3. Cuối cùng hiển thị ảnh scence_bgr.png để kiểm tra hiệu ứng trực quan sau khi hoán đổi màu.

 Kết quả:

scence_grb.png: Ảnh với màu đỏ và xanh lá bị hoán đổi.

scence_bgr.png: Ảnh với màu đỏ và xanh dương bị hoán đổi.

scence_brg.png: Hoán đổi kiểu Blue–Red–Green.

Bài 3

1. Đọc ảnh và chuẩn hóa giá trị

img = iio.imread('scence.png') / 255.0

Đọc ảnh gốc scence.png.

Chia giá trị pixel cho 255 để chuẩn hóa về khoảng [0, 1] (vì matplotlib.colors.rgb_to_hsv yêu cầu như vậy).

if img.shape[-1] == 4:
    img = img[..., :3]

Nếu ảnh có 4 kênh (RGBA), loại bỏ kênh Alpha để chỉ giữ lại RGB.

2. Chuyển sang không gian màu HSV

hsv = mcolors.rgb_to_hsv(img)

Sử dụng matplotlib.colors.rgb_to_hsv để chuyển từ RGB sang HSV.

3. Tách các kênh

H = hsv[..., 0]

S = hsv[..., 1]

V = hsv[..., 2]

Tách từng kênh:

Hue (H) nằm trong khoảng [0, 1], biểu thị màu sắc (vòng tròn màu).

Saturation (S) thể hiện độ đậm màu.

Value (V) là độ sáng.

4. Lưu ảnh từng kênh dưới dạng ảnh xám

Chuyển từng kênh từ [0, 1] về [0, 255] rồi ép kiểu uint8 để lưu thành ảnh .png.

5. Hiển thị ảnh từng kênh

for channel, name in zip([H, S, V], ['Hue', 'Saturation', 'Value']):
    plt.imshow(channel, cmap='gray')
    plt.title(name)
    plt.show()

Kết quả

Ảnh hue.png: thể hiện thông tin về màu.

Ảnh saturation.png: độ bão hòa màu.

Ảnh value.png: độ sáng.


Bài 4

1. Đọc ảnh và chuẩn hóa giá trị

Đọc ảnh gốc và chuẩn hóa pixel về khoảng [0, 1].

2. Chuyển từ RGB sang HSV

rgb2hsv = np.vectorize(colorsys.rgb_to_hsv)

h, s, v = rgb2hsv(rgb[:,:,0], rgb[:,:,1], rgb[:,:,2])

Sử dụng colorsys.rgb_to_hsv để chuyển từng pixel sang hệ HSV.

Dùng np.vectorize để áp dụng cho toàn bộ mảng ảnh.

3. Thay đổi kênh H và V theo yêu cầu

h = (1/3) * h

v = (3/4) * v

Hue được giảm còn 1/3 → thay đổi tông màu của ảnh.

Value giảm còn 3/4 → ảnh trở nên tối hơn.

h = np.clip(h, 0, 1)

v = np.clip(v, 0, 1)

Đảm bảo các giá trị vẫn nằm trong khoảng [0, 1].

4. Chuyển từ HSV trở lại RGB

hsv2rgb = np.vectorize(colorsys.hsv_to_rgb)

r, g, b = hsv2rgb(h, s, v)

Sử dụng colorsys.hsv_to_rgb để chuyển từng pixel ngược lại từ HSV về RGB.

rgb_new = np.stack([r, g, b], axis=2)

rgb_new_uint8 = (rgb_new * 255).astype(np.uint8)

Gộp lại thành ảnh mới và đưa về định dạng pixel 8-bit.

5. Lưu ảnh và hiển thị kết quả

Hiển thị ảnh sau khi xử lý để quan sát trực quan sự thay đổi.

Kết quả

hsv_modified.png là ảnh đã được thay đổi màu:

Tông màu khác đi (do đổi Hue).

Tối hơn (do giảm Value).

Bài 5
1. Đọc các ảnh màu

a_rgb = iio.imread('baby.jpeg')

x_rgb = iio.imread('balloons_noisy.png')

z_rgb = iio.imread('flower.jpeg')

Đọc 3 ảnh màu đầu vào: baby, balloons_noisy, flower.

2. Chuyển sang ảnh xám (grayscale)

from skimage.color import rgb2gray

a = (rgb2gray(a_rgb) * 255).astype(np.uint8)

x = (rgb2gray(x_rgb) * 255).astype(np.uint8)

z = (rgb2gray(z_rgb) * 255).astype(np.uint8)

Sử dụng rgb2gray() để chuyển ảnh từ RGB sang grayscale.

Nhân với 255 để đưa dữ liệu từ khoảng [0,1] về [0,255].

Ép kiểu sang uint8 để lưu ảnh dưới dạng chuẩn 8-bit.

3. Tạo và áp dụng bộ lọc trung bình

k = np.ones((5, 5)) / 25

Tạo kernel 5x5 (bộ lọc trung bình) với tất cả các giá trị bằng nhau.

Tác dụng: làm mờ ảnh bằng cách lấy trung bình lân cận 5x5 pixel.

from scipy import ndimage as sn

b = sn.convolve(a, k).astype(np.uint8)

c = sn.convolve(x, k).astype(np.uint8)

d = sn.convolve(z, k).astype(np.uint8)

Sử dụng hàm convolve() để áp dụng bộ lọc cho ảnh xám.

Tạo ảnh đầu ra đã làm mờ: b, c, d.

4. Lưu và hiển thị kết quả

Lưu ảnh mới đã được làm mờ bằng mean filter.

Hiển thị ảnh dưới dạng ảnh xám (dùng cmap='gray').

Tương tự với ảnh balloons_noisy và flower.

Kết quả
Ảnh đầu ra là các ảnh xám đã được làm mịn

Bài 6 
1. Đọc ảnh và chuyển sang ảnh xám:

img_rgb = iio.imread(filename)

img = rgb2gray(img_rgb)

Ảnh màu được chuyển thành ảnh xám để xử lý dễ dàng hơn.

2. Định nghĩa bộ lọc trung bình (Mean filter):

mean_kernel = np.ones((5, 5)) / 25

mean_filtered = sn.convolve(img, mean_kernel)

Bộ lọc này lấy trung bình giá trị pixel trong cửa sổ 5x5 để làm mượt ảnh.

3. Áp dụng các bộ lọc khác:

median_filtered = sn.median_filter(img, size=5)

max_filtered = sn.maximum_filter(img, size=5)

min_filtered = sn.minimum_filter(img, size=5)

median_filter: giảm nhiễu muối tiêu.

maximum_filter: tăng cường vùng sáng.

minimum_filter: tăng cường vùng tối.

4. Hiển thị ảnh gốc và các ảnh đã lọc để so sánh:

plt.imshow(images[i], cmap='gray')

plt.title(titles[i])

Kết quả thu được:

Ảnh sau lọc trung bình mượt mà hơn, giảm nhiễu nhẹ.

Lọc trung vị rất hiệu quả trong việc loại bỏ nhiễu đốm (salt-and-pepper noise).

Lọc giá trị lớn nhất làm nổi bật các vùng sáng.

Lọc giá trị nhỏ nhất làm nổi bật các vùng tối.

Bài 7

Chương trình thực hiện các bước chính sau:

1. Đọc ảnh màu và chuyển sang ảnh xám bằng cách sử dụng lệnh:

img_rgb = iio.imread(filename)

img_gray = rgb2gray(img_rgb)

2. Khử nhiễu ảnh bằng bộ lọc trung vị với kích thước cửa sổ 6x6:

denoised = sn.median_filter(img_gray, size=6)

Phát hiện biên cạnh bằng bộ lọc Sobel theo cả hai chiều x và y:

sobel_x = sn.sobel(denoised, axis=0)

sobel_y = sn.sobel(denoised, axis=1)

3. Sau đó tính độ lớn biên cạnh bằng np.hypot(sobel_x, sobel_y) và chuẩn hóa về khoảng [0,1].

Tương tự, phát hiện biên cạnh bằng bộ lọc Prewitt:

prewitt_x = sn.prewitt(denoised, axis=0)

prewitt_y = sn.prewitt(denoised, axis=1)

Cũng tính độ lớn biên cạnh và chuẩn hóa.

Phát hiện biên cạnh bằng bộ lọc Canny với tham số sigma bằng 1.0:

canny_edge = canny(denoised, sigma=1.0)

4. Lưu ảnh kết quả các bước trên bằng cách nhân ảnh với 255 rồi chuyển sang kiểu uint8.

5.  Hiển thị đồng thời ảnh đã khử nhiễu, ảnh biên cạnh từ Sobel, Prewitt và Canny để so sánh.

Kết quả:

Ảnh sau khử nhiễu có độ sạch cao, giúp phát hiện biên chính xác hơn.

Bộ lọc Sobel và Prewitt cho các đường biên lớn và rõ nét.

Bộ lọc Canny cho biên cạnh sắc nét, loại bỏ nhiễu hiệu quả hơn và phát hiện biên chính xác.

Việc hiển thị đồng thời giúp ta dễ dàng so sánh hiệu quả của từng bộ lọc trên các ảnh khác nhau.

Bài 8
Chương trình đọc ảnh màu, chuẩn hóa giá trị về khoảng [0, 1] nếu cần.

Áp dụng bộ lọc trung vị 3x3 để khử nhiễu trên từng kênh màu R, G, B riêng biệt.

Đổi màu RGB bằng cách nhân mỗi kênh với hệ số ngẫu nhiên trong khoảng 0.5 đến 1.5, tạo hiệu ứng màu sắc đa dạng và mới lạ.

Hiển thị ảnh đã xử lý và lưu ảnh với hậu tố _processed.png.

Xử lý từng ảnh trong danh sách và thông báo lỗi nếu gặp vấn đề.

Kết quả:

Ảnh sau khi khử nhiễu sạch hơn, giảm nhiễu muối tiêu.

Màu sắc ảnh được thay đổi ngẫu nhiên, tạo hiệu ứng độc đáo.

Quá trình xử lý giữ nguyên cấu trúc ảnh gốc nhưng mang lại sự đa dạng màu sắc.

Bài 9
 1 . Đọc và chuẩn hóa ảnh:

Dùng img = mpimg.imread(filename) để đọc ảnh. Nếu ảnh đang ở dạng uint8, chuẩn hóa về [0, 1] bằng img = img / 255.0.

Nếu ảnh có 4 kênh (RGBA), chỉ lấy 3 kênh đầu bằng img = img[..., :3].

 2 . Khử nhiễu bằng median filter:

Tạo một ảnh mới có cùng kích thước denoised = np.zeros_like(img) rồi lọc riêng từng kênh màu bằng vòng lặp:

denoised[..., i] = median_filter(img[..., i], size=3) với i = 0, 1, 2.

3 . Biến đổi ngẫu nhiên HSV:

Chuyển ảnh sang không gian HSV bằng img_hsv = colors.rgb_to_hsv(img_rgb).

Sau đó, tạo các biến đổi ngẫu nhiên:

Hue: img_hsv[..., 0] = (img_hsv[..., 0] + delta_h) % 1.0

Saturation: img_hsv[..., 1] = np.clip(img_hsv[..., 1] + delta_s, 0, 1)

Value: img_hsv[..., 2] = np.clip(img_hsv[..., 2] + delta_v, 0, 1)

Chuyển ngược lại sang RGB bằng colors.hsv_to_rgb(img_hsv).

 4 . Tránh tạo ảnh trùng:

Sau mỗi lần biến đổi, tính băm ảnh bằng cách chuyển về uint8 rồi hashlib.md5(img_uint8.tobytes()).hexdigest().

Nếu băm chưa có trong processed_hashes, ảnh được chấp nhận; nếu trùng, thực hiện lại bước biến đổi HSV.

5 . Hiển thị và lưu ảnh:

Ảnh được hiển thị bằng plt.imshow(img_mod) và lưu bằng plt.imsave(save_path, img_mod).

Kết quả
Chương trình tạo ra các ảnh mới có màu sắc ngẫu nhiên dựa trên ảnh gốc, đã được khử nhiễu. Mỗi ảnh được lưu với tên _modifiled.png, ví dụ: baby_modifiled.png. Các ảnh sau xử lý có màu khác biệt nhờ biến đổi HSV, không bị trùng lặp nhờ kiểm tra mã băm.
