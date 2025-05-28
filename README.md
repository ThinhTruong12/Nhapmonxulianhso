# Nhapmonxulianhso
Bài 1
Đọc ảnh
1. Sử dụng imageio.v2.imread để đọc ảnh scence.png vào biến data.
Ảnh được lưu dưới dạng mảng NumPy 3 chiều với kích thước (chiều cao, chiều rộng, 3 kênh màu RGB).

2. Sau đó tách từng kênh màu
rdata = data[:, :, 0]: Lấy kênh màu đỏ (Red).
gdata = data[:, :, 1]: Lấy kênh màu lục (Green).
bdata = data[:, :, 2]: Lấy kênh màu xanh dương (Blue).

3. Hiển thị từng kênh màu dưới dạng ảnh đơn kênh
4. Dùng matplotlib.pyplot.imshow để hiển thị từng kênh màu riêng biệt:
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

 Kết quả
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
