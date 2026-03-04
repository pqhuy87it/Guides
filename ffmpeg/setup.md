Để cài đặt FFmpeg, cách nhanh và sạch nhất là sử dụng các trình quản lý gói (package manager) qua command line.

Chỉ cần mở WezTerm (hoặc bất kỳ terminal nào bạn đang dùng) lên và chạy lệnh tương ứng với hệ điều hành:

### 1. Trên macOS (Khuyên dùng Homebrew)

Hầu như máy dev nào cũng cài sẵn Homebrew. Bạn chỉ cần chạy lệnh sau:

```bash
brew install ffmpeg

```

*(Lưu ý: Quá trình cài đặt có thể mất vài phút vì nó sẽ kéo theo khá nhiều thư viện xử lý video/audio phụ thuộc).*

Nếu máy bạn chưa có Homebrew, hãy chạy lệnh này trước để cài brew: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

### 2. Trên Linux (Ubuntu/Debian)

Nếu bạn đang cần cài trên server hoặc môi trường Linux:

```bash
sudo apt update
sudo apt install ffmpeg

```

### 3. Trên Windows

Nếu bạn dùng Windows, cách gõ lệnh nhanh nhất là dùng `winget` (có sẵn trên Windows 10/11):

```powershell
winget install ffmpeg

```

Hoặc nếu bạn có dùng Scoop hay Chocolatey:

* Scoop: `scoop install ffmpeg`
* Chocolatey: `choco install ffmpeg`

---

### Kiểm tra cài đặt

Sau khi terminal chạy xong lệnh cài đặt, bạn gõ lệnh sau để xác nhận FFmpeg đã được đưa vào biến môi trường (PATH) thành công chưa:

```bash
ffmpeg -version

```

Nếu màn hình in ra một loạt thông tin về phiên bản (như `ffmpeg version 6.x.x...`) và cấu hình build là bạn đã thành công.

Bạn có muốn tôi viết luôn câu lệnh FFmpeg hoàn chỉnh để ép dung lượng file mp4 270MB kia xuống dưới 100MB bằng chuẩn H.265 không?
