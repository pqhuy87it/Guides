Chào bạn, sau chuỗi ngày "đánh vật" với các lỗi xung đột phiên bản (từ Kotlin, Analyzer đến các thư viện của iOS/Android) vừa qua, việc bạn quyết định tìm hiểu cách quản lý phiên bản Flutter là một bước đi **cực kỳ chính xác và mang tính bước ngoặt**. 

Trong thế giới Flutter, việc nâng cấp SDK thường xuyên làm hỏng code cũ. Công cụ "quốc dân" và chuẩn chỉ nhất hiện nay mà các team chuyên nghiệp sử dụng là **FVM (Flutter Version Management)**. 

Thay vì cài một bản Flutter duy nhất cho cả máy tính, FVM cho phép **mỗi dự án của bạn sử dụng một phiên bản Flutter riêng biệt**, và bạn có thể chuyển đổi qua lại giữa chúng chỉ trong 1 giây.

Dưới đây là hướng dẫn từ A-Z cách thiết lập và sử dụng FVM:

### Bước 1: Cài đặt FVM lên máy tính
Vì máy bạn đã có sẵn Dart, bạn chỉ cần mở Terminal và chạy lệnh sau để cài FVM như một công cụ toàn cầu (global):

```bash
dart pub global activate fvm
```

*(Lưu ý: Sau khi cài xong, nếu Terminal báo vàng yêu cầu thêm đường dẫn vào biến môi trường `PATH`, bạn hãy làm theo hướng dẫn trên màn hình nhé. Thường là thêm `export PATH="$PATH":"$HOME/.pub-cache/bin"` vào file `~/.zshrc` hoặc `~/.bash_profile`)*.

### Bước 2: Tải các phiên bản Flutter bạn muốn
Bây giờ, thay vì lên web tải Flutter, bạn dùng FVM để tải. Ví dụ:

```bash
# Tải phiên bản ổn định mới nhất
fvm install stable

# Tải một phiên bản cụ thể (ví dụ bản cũ để fix lỗi dự án cũ)
fvm install 3.19.6

# Xem danh sách các bản đã tải về máy
fvm list
```

### Bước 3: Áp dụng phiên bản cho dự án
Bạn mở Terminal, đi vào thư mục dự án của bạn (ví dụ cái app Github Search vừa rồi) và gõ:

```bash
fvm use 3.19.6
```

**Điều gì vừa xảy ra?** FVM sẽ tạo ra một thư mục ẩn tên là `.fvm` bên trong dự án của bạn. Trong đó chứa một cái "lối tắt" (symlink) trỏ đúng đến bộ SDK 3.19.6. Bất kỳ ai clone code của bạn về cũng sẽ biết dự án này dùng bản 3.19.6.

### Bước 4: Thay đổi thói quen gõ lệnh
Từ giây phút này, khi làm việc trong dự án đã cấu hình FVM, bạn **đừng gõ chữ `flutter` đứng một mình nữa**, mà hãy thêm chữ `fvm` lên trước:

* Thay vì `flutter clean` ➔ Gõ `fvm flutter clean`
* Thay vì `flutter pub get` ➔ Gõ `fvm flutter pub get`
* Thay vì `dart run build_runner...` ➔ Gõ `fvm dart run build_runner...`

### Bước 5: Cấu hình IDE (Bắt buộc để không bị lỗi đỏ code)
Vì bạn dùng FVM, IDE (Android Studio hoặc VS Code) của bạn cần biết đường dẫn Flutter mới để phân tích code, nếu không nó sẽ vẫn dùng bản cũ của máy tính.

**Nếu bạn dùng VS Code:**
Tạo (hoặc mở) file `.vscode/settings.json` trong dự án và dán đoạn này vào:
```json
{
  "dart.flutterSdkPath": ".fvm/flutter_sdk",
  "search.exclude": {
    "**/.fvm": true
  }
}
```

**Nếu bạn dùng Android Studio:**
1. Vào **Settings** (Mac: `Cmd + ,`).
2. Tìm đến mục **Languages & Frameworks** -> **Flutter**.
3. Ở ô **Flutter SDK path**, bạn trỏ đường dẫn tuyệt đối đến thư mục `.fvm/flutter_sdk` nằm ngay bên trong dự án của bạn (Ví dụ: `/Users/phamhuy/.../my_project/.fvm/flutter_sdk`).
4. Bấm Apply và OK.

---

**⚠️ Lưu ý cuối cùng:** Đừng quên mở file `.gitignore` của dự án và thêm dòng này vào để tránh đẩy cả bộ SDK lên GitHub:
```text
.fvm/flutter_sdk
```

Bạn có muốn tôi hướng dẫn thêm cách gỡ bỏ bản Flutter cũ đang cài trực tiếp trên máy để nhường toàn quyền quản lý lại cho FVM không?
