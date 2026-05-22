# Sổ Tay Du Lịch — PWA

Phrasebook tiếng Trung cho người Việt, tối ưu cho iPhone "Add to Home Screen". Chạy offline 100% sau lần load đầu nhờ service worker.

## Có gì trong này

```
sotay-pwa/
├── index.html              ← App chính (CSS + JS inline, dùng iOS system fonts)
├── manifest.webmanifest    ← PWA manifest (tên, icon, theme)
├── sw.js                   ← Service worker (cache-first cho offline)
├── icon.svg                ← Icon nguồn (vintage stamp style)
├── icon-192.png            ← Icon PWA 192×192
├── icon-512.png            ← Icon PWA 512×512
└── apple-touch-icon.png    ← Icon cho iOS Home Screen (180×180)
```

## Cách deploy lên GitHub Pages (free, ~5 phút)

### 1. Tạo GitHub repo

- Vào https://github.com/new
- Repository name: `sotay-pwa` (hoặc gì tùy bạn)
- Đặt **Public** (Pages free chỉ chạy với public repo)
- Bấm **Create repository**

### 2. Upload files

Cách dễ nhất là kéo thả qua web:

- Trên repo trống vừa tạo, bấm link **uploading an existing file**
- Kéo toàn bộ 7 file trong folder `sotay-pwa/` (kể cả file ẩn nếu có) vào trang
- Bấm **Commit changes**

Hoặc nếu rành git:

```bash
cd /Users/chuyenkute/Downloads/sotay-pwa
git init
git add .
git commit -m "init sotay-pwa"
git branch -M main
git remote add origin https://github.com/<USERNAME>/sotay-pwa.git
git push -u origin main
```

### 3. Bật GitHub Pages

- Repo → **Settings** → **Pages** (menu trái)
- Source: **Deploy from a branch**
- Branch: **main** / **/ (root)** → **Save**
- Đợi 1–2 phút, GitHub sẽ hiện URL dạng `https://<USERNAME>.github.io/sotay-pwa/`

### 4. Mở trên iPhone và Add to Home Screen

- Trên iPhone, mở **Safari** (phải là Safari, không phải Chrome) → vào URL trên
- Đợi trang load xong (~2 giây)
- Bấm nút **Share** (hình vuông mũi tên ↑ ở thanh dưới)
- Cuộn xuống → **Add to Home Screen**
- Đổi tên nếu muốn → **Add**

App giờ xuất hiện trên Home Screen với icon riêng. Mở từ icon = chạy fullscreen, không có thanh URL Safari.

### 5. Test offline

- Bật chế độ **Máy bay** trên iPhone
- Mở app từ Home Screen → vẫn hoạt động đầy đủ ✓
- Service worker đã cache toàn bộ file ngay từ lần mở đầu tiên.

## Các tính năng PWA đã thêm

- **iOS system fonts**: bỏ Google Fonts → dùng `New York` (serif vintage), `Songti SC` (chữ Trung kiểu cổ), `SF Pro` (UI). Không cần internet để load font.
- **Service worker**: cache toàn bộ assets cache-first → offline 100%.
- **Apple meta tags**: `apple-mobile-web-app-capable`, `apple-touch-icon`, `theme-color`, `apple-mobile-web-app-title` (= "Sổ Tay").
- **Safe area**: padding theo `env(safe-area-inset-*)` cho iPhone có notch / Dynamic Island.
- **Touch UX**: `-webkit-tap-highlight-color: transparent`, `touch-action: manipulation`, prevent zoom khi focus input.
- **TTS phát âm tiếng Trung**: nút loa trên mỗi thẻ — dùng `speechSynthesis` với `lang: zh-CN`, iOS có engine offline sẵn.

## Cập nhật app sau này

Khi bạn sửa code và push commit mới, service worker sẽ **không** tự động lấy bản mới vì đã cache. Để force update:

1. Sửa `CACHE` trong `sw.js` (vd: `sotay-v1` → `sotay-v2`)
2. Push lên GitHub
3. Trên iPhone, đóng app khỏi multitasking, mở lại — service worker mới sẽ kích hoạt và xóa cache cũ.

## Phương án host khác (nếu không thích GitHub Pages)

| Dịch vụ | Free | HTTPS | Setup |
|---|---|---|---|
| **GitHub Pages** | ✓ | ✓ | Như trên |
| **Cloudflare Pages** | ✓ | ✓ | Connect GitHub repo, auto deploy |
| **Vercel** | ✓ | ✓ | Drag drop folder vào vercel.com |
| **Netlify Drop** | ✓ | ✓ | Drag drop folder vào app.netlify.com/drop, không cần tài khoản |

Bất kỳ static host nào có HTTPS đều dùng được. PWA **yêu cầu HTTPS** (trừ localhost) — không thể chạy qua `file://` hoặc HTTP thường.
