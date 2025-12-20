Cách hiệu quả và phổ biến nhất để quản lý nhiều phiên bản Flutter trên macOS là sử dụng **FVM (Flutter Version Management)**. Đây là công cụ chuẩn ngành giúp bạn chuyển đổi phiên bản Flutter theo từng dự án (per-project) hoặc toàn hệ thống một cách dễ dàng.

Dưới đây là hướng dẫn chi tiết từng bước:

---

### Cách 1: Sử dụng FVM (Khuyên dùng)

FVM cho phép mỗi dự án sử dụng một phiên bản Flutter riêng biệt mà không gây xung đột.

#### Bước 1: Cài đặt FVM

Cách đơn giản nhất trên macOS là dùng Homebrew:

```bash
brew tap leoafarias/fvm
brew install fvm

```

#### Bước 2: Cài đặt các phiên bản Flutter

Sau khi cài xong FVM, bạn có thể cài đặt các phiên bản Flutter mà bạn cần:

```bash
fvm install stable    # Cài bản stable mới nhất
fvm install 3.19.3    # Cài phiên bản cụ thể
fvm install 3.10.0    # Cài phiên bản cũ hơn

```

Để xem danh sách các phiên bản đã cài:

```bash
fvm list

```

#### Bước 3: Sử dụng trong dự án

Di chuyển vào thư mục dự án của bạn và chọn phiên bản muốn dùng:

```bash
cd du_an_cua_toi
fvm use 3.19.3

```

Lệnh này sẽ tạo ra một thư mục ẩn `.fvm/` trong dự án của bạn, chứa các liên kết (symlinks) đến phiên bản Flutter đã chọn.

#### Bước 4: Cấu hình IDE (Quan trọng)

Để VS Code hoặc Android Studio hiểu được bạn đang dùng phiên bản nào từ FVM, bạn cần cấu hình lại một chút:

**Đối với VS Code:**
Tạo hoặc sửa file `.vscode/settings.json` trong thư mục gốc của dự án và thêm dòng sau:

```json
{
  "dart.flutterSdkPath": ".fvm/flutter_sdk",
  "search.exclude": {
    "**/.fvm": true
  },
  "files.watcherExclude": {
    "**/.fvm": true
  }
}

```

*Lưu ý: Việc này giúp VS Code trỏ đúng vào phiên bản Flutter nằm trong folder `.fvm` thay vì phiên bản cài đặt toàn cục.*

**Đối với Android Studio:**

1. Vào **Settings (Preferences)** -> **Languages & Frameworks** -> **Flutter**.
2. Tại mục **Flutter SDK path**, trỏ đường dẫn đến thư mục `.fvm/flutter_sdk` nằm trong dự án của bạn.

---

### Cách 2: Sử dụng Puro (Giải pháp mới & nhanh hơn)

Nếu bạn thấy FVM hơi chậm hoặc tốn dung lượng ổ cứng, **Puro** là một giải pháp thay thế mới nổi, cài đặt nhanh hơn và quản lý tài nguyên tốt hơn.

1. **Cài đặt Puro:**
```bash
curl -o- https://puro.dev/install.sh | bash

```


2. **Tạo môi trường:**
```bash
puro create my-env 3.19.0

```


3. **Sử dụng trong dự án:**
```bash
puro use my-env

```



Puro tự động cấu hình cho VS Code và Android Studio, nên bạn thường không cần chỉnh sửa thủ công như FVM.

---

### Cách 3: Sử dụng ASDF (Nếu bạn là Dev đa năng)

Nếu bạn đã quen dùng `asdf` để quản lý Node.js, Ruby, Python... thì bạn cũng có thể dùng nó cho Flutter.

1. **Cài plugin Flutter:**
```bash
asdf plugin add flutter

```


2. **Cài đặt phiên bản:**
```bash
asdf install flutter 3.19.0

```


3. **Set phiên bản cho dự án:**
```bash
asdf local flutter 3.19.0

```



---

### Tóm lại: Nên chọn cách nào?

| Công cụ | Ưu điểm | Nhược điểm | Dành cho ai |
| --- | --- | --- | --- |
| **FVM** | Phổ biến nhất, cộng đồng lớn, hỗ trợ IDE tốt qua config. | Cần cấu hình IDE thủ công một chút. | Đa số lập trình viên Flutter. |
| **Puro** | Siêu nhanh, tiết kiệm dung lượng ổ cứng, tự động config IDE. | Mới hơn, ít người dùng hơn FVM. | Người thích công nghệ mới, máy cấu hình thấp. |
| **ASDF** | Quản lý "tất cả trong một" (Node, Git, Flutter...). | Cấu hình ban đầu hơi phức tạp. | Full-stack developer. |

**Lời khuyên:** Hãy bắt đầu với **FVM**. Đó là tiêu chuẩn hiện tại và dễ dàng tìm kiếm sự hỗ trợ nếu gặp lỗi.
