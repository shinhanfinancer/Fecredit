# FE Credit – Demo luồng đăng ký vay & eKYC (Frontend)

Dự án **FE Credit Frontend** mô phỏng luồng đăng ký vay tiêu dùng trực tuyến, bao gồm:
- Tính toán khoản vay
- Đăng ký thông tin khách hàng
- Xác thực OTP, eKYC khuôn mặt
- Tra cứu kết quả xét duyệt
- Mô phỏng giải ngân / thanh toán qua các kênh như MoMo

Dự án là **ứng dụng frontend tĩnh**, triển khai chủ yếu bằng **HTML/CSS/JavaScript** với các thư viện UI và tiện ích phổ biến.

---

## 1. Cấu trúc & Chức năng chính

Một số trang chính trong thư mục `pages/`:

- `loan_calculator.html`  
  - Trang **máy tính khoản vay**: nhập số tiền, kỳ hạn, lãi suất, xem lịch trả nợ minh họa.
  - Có biểu đồ, xuất PDF (sử dụng `jsPDF`), giao diện tailwind + custom CSS.

- `loan_registration.html` / `step1.html`  
  - Trang **đăng ký khoản vay**: thu thập thông tin cá nhân, thông tin khoản vay.
  - Giao diện responsive, sử dụng Bootstrap 5 & FECredit style.

- `step2.html`  
  - Bước **eKYC khuôn mặt** (demo): tích hợp `face-api.js` để phát hiện khuôn mặt, chụp ảnh, kiểm tra điều kiện.

- `otp.html`  
  - Trang **nhập OTP**: mock gửi/nhận OTP, có tích hợp `emailjs` để gửi email trong chế độ demo.

- `step5.html`  
  - Bước xem **chi tiết khoản vay & ký hợp đồng**: hiển thị thông tin khoản vay, điều khoản, xác nhận đồng ý.
  - Có tích hợp **Firebase config** ở chế độ demo (cần thay bằng cấu hình thực tế nếu dùng production).

- `check_result.html`  
  - Trang **xem kết quả xét duyệt**: nhập mã hồ sơ / dữ liệu mã hóa, giải mã và hiển thị kết quả.

- `momo.html`  
  - Mô phỏng **xác nhận giải ngân/thanh toán qua MoMo**.

- `step7.html`, `step7_fixed.html`, `step8.html`, `step9.html`  
  - Các bước mô phỏng **điều kiện giải ngân, phê duyệt khoản vay, thông báo kết quả**, giao diện theo brand FE Credit.

Ngoài ra còn có:
- `assets/css/` – CSS dùng chung, FECredit design system.
- `assets/js/` – các script chung, ví dụ: `face-api.min.js`, script xử lý dữ liệu, lưu localStorage, gọi API (nếu có cấu hình).

---

## 2. Công nghệ & Thư viện sử dụng

Dự án sử dụng chủ yếu:

### Frontend

- **HTML5**, **CSS3**, **JavaScript (ES6+)**
- **Bootstrap 5** – layout & component UI responsive
- **Tailwind CSS (CDN)** – một số trang sử dụng để build layout nhanh
- **Google Fonts** (Inter, Roboto)
- **Bootstrap Icons**, **Font Awesome** – icon

### Thư viện JavaScript

- **EmailJS** – gửi email OTP demo từ phía client
- **face-api.js** – nhận diện / phát hiện khuôn mặt cho bước eKYC
- **CryptoJS** – mã hóa/giải mã dữ liệu (ví dụ mã hồ sơ, payload kết quả)
- **jsPDF** – xuất file PDF (bảng tính khoản vay, điều khoản, v.v.)
- **Firebase (config demo)** – một số bước có sử dụng cấu hình Firebase (demo) cho lưu trữ/đồng bộ (cần thay bằng config thực tế nếu dùng thật)
- **jQuery** (một số trang cũ) – thao tác DOM, xử lý sự kiện

---

## 3. Yêu cầu môi trường

Vì đây là **ứng dụng tĩnh**, yêu cầu môi trường rất đơn giản:

- Trình duyệt hiện đại (Chrome, Edge, Firefox, Safari) hỗ trợ:
  - ES6
  - `localStorage`
  - `fetch API`
  - Camera (cho bước eKYC nếu test trên http/https đúng quyền)
- **Web server tĩnh** (tùy chọn, khuyến nghị) để load file qua `http://` thay vì `file://`:
  - Node.js + `http-server`, hoặc
  - Nginx/Apache, hoặc
  - Live Server trên VS Code.

---

## 4. Cách cài đặt & chạy dự án

### 4.1. Clone mã nguồn

```bash
git clone https://github.com/shinhanfinancer/fecredit.git
cd fecredit
```

Dự án không yêu cầu build phức tạp, chỉ cần phục vụ các file tĩnh.

### 4.2. Chạy bằng web server tĩnh (khuyến nghị)

#### Cách 1: Dùng `http-server` (Node.js)

1. Cài Node.js (>= 16) nếu chưa có.
2. Cài http-server:

```bash
npm install -g http-server
```

3. Chạy server tại thư mục gốc repo:

```bash
http-server .
```

4. Mở trình duyệt tại địa chỉ in ra, ví dụ:  
   `http://localhost:8080/pages/loan_calculator.html`  
   hoặc  
   `http://localhost:8080/pages/loan_registration.html`

#### Cách 2: Dùng VS Code Live Server

1. Mở thư mục `fecredit` trong VS Code.
2. Cài extension **Live Server**.
3. Nhấn chuột phải vào file HTML (ví dụ `pages/loan_calculator.html`) → **Open with Live Server**.

---

## 5. Cách sử dụng luồng chức năng

### 5.1. Demo tính toán khoản vay

1. Truy cập `loan_calculator.html`.
2. Nhập:
   - Số tiền vay
   - Kỳ hạn
   - Lãi suất/hoặc chọn gói sản phẩm
3. Xem:
   - Số tiền trả hàng tháng (minh họa)
   - Biểu đồ cấu trúc gốc/lãi
4. (Tùy chọn) Xuất PDF hoặc tải kết quả.

### 5.2. Demo đăng ký khoản vay & eKYC

1. Truy cập `loan_registration.html` hoặc `step1.html`.
2. Nhập thông tin cá nhân, thông tin khoản vay.
3. Chuyển qua `step2.html` để:
   - Bật camera (cho phép trình duyệt sử dụng camera).
   - Thực hiện chụp/kiểm tra khuôn mặt (demo).
4. Tiếp tục các bước: OTP (`otp.html`), xem chi tiết khoản vay (`step5.html`), điều kiện giải ngân (`step7*.html`), phê duyệt (`step8.html`, `step9.html`).

### 5.3. Tra cứu kết quả xét duyệt

1. Truy cập `check_result.html`.
2. Nhập thông tin/mã hồ sơ hoặc dữ liệu được mã hóa (tùy logic đã setup).
3. Hệ thống sẽ giải mã (CryptoJS) và hiển thị kết quả demo.

### 5.4. Mô phỏng giải ngân qua MoMo

1. Truy cập `momo.html`.
2. Thực hiện thao tác xác nhận theo giao diện mô phỏng.
3. Trang hiển thị trạng thái giải ngân / thanh toán (demo).

---

## 6. Cấu hình & Lưu ý bảo mật

> **Lưu ý:** Repo này có thể đang sử dụng **cấu hình DEMO** cho EmailJS, Firebase, v.v.  
> Khi triển khai thực tế, cần:

1. **Thay thế tất cả API key / config demo**:
   - EmailJS: `emailjs.init(...)` → dùng key riêng, không commit public.
   - Firebase: thay `firebaseConfig` bằng cấu hình riêng, sử dụng biến môi trường nếu có backend.

2. **Không commit thông tin nhạy cảm**:
   - API key thật
   - Thông tin khách hàng
   - Endpoint nội bộ không bảo vệ

3. **Bật HTTPS** khi triển khai:
   - Đặc biệt quan trọng với tính năng camera (getUserMedia) và bảo mật dữ liệu.

4. **CORS & Security Header**:
   - Một số trang đã thiết lập sẵn header như `X-Content-Type-Options`, `X-Frame-Options`, `X-XSS-Protection`, `Referrer-Policy`, `Permissions-Policy` ở mức meta HTML.
   - Khi triển khai production thực tế, nên cấu hình các header này từ phía server.

---

## 7. Định hướng phát triển

Một số hướng phát triển/hoàn thiện thêm:

- Tách code JavaScript ra file `.js` riêng thay vì nhúng inline trong HTML.
- Chuẩn hóa cấu trúc project (ví dụ: `src/` + bundler như Vite/Webpack nếu cần).
- Thêm unit test cho các hàm tính toán lãi suất, lịch trả nợ.
- Tích hợp thực tế với backend API (đăng ký hồ sơ, tra cứu kết quả).
- Bổ sung i18n (đa ngôn ngữ) nếu cần triển khai cho nhiều thị trường.

---

## 8. Bản quyền

- Giao diện, logo và nội dung mô phỏng theo thương hiệu **FE Credit**.
- Repo mang tính chất **demo / nội bộ** (tùy chính sách tổ chức).  
  Vui lòng kiểm tra lại chính sách sử dụng và chia sẻ mã nguồn trước khi public/triển khai diện rộng.
