Đây là hướng dẫn chi tiết từng bước để tạo file `key.properties` và cấu hình ký ứng dụng (Signing) cho Android trong Flutter. Việc này giúp bạn sửa lỗi build và chuẩn bị cho việc đẩy ứng dụng lên Google Play sau này.

### Bước 1: Tạo Keystore (Nếu bạn chưa có)

Nếu bạn chưa có file `.jks` (Keystore), bạn cần tạo nó trước. Nếu đã có rồi, hãy bỏ qua bước này.

1. Mở **Terminal** (Mac/Linux) hoặc **Command Prompt** (Windows).
2. Chạy lệnh sau (thay đổi `upload-keystore.jks` và `my-key-alias` theo ý bạn nếu muốn):
**Trên Mac/Linux:**
```bash
keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias

```


**Trên Windows:**
```cmd
keytool -genkey -v -keystore %USERPROFILE%\upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias

```


3. Nhập mật khẩu (nhớ kỹ mật khẩu này) và điền các thông tin (Tên, Đơn vị, Thành phố...).
4. Sau khi tạo xong, file `upload-keystore.jks` sẽ nằm ở thư mục gốc của User.
5. **Di chuyển file này** vào thư mục `android/app/` trong dự án Flutter của bạn.

---

### Bước 2: Tạo file `key.properties`

Đây là bước để sửa cái lỗi `No such file or directory` bạn vừa gặp.

1. Trong dự án Flutter, đi tới thư mục **`android/`** (Lưu ý: Là thư mục `android`, **không phải** `android/app`).
2. Tạo một file mới đặt tên là: **`key.properties`**.
3. Mở file đó và dán nội dung sau vào:

```properties
storePassword=mat_khau_ban_da_dat
keyPassword=mat_khau_ban_da_dat
keyAlias=my-key-alias
storeFile=../app/upload-keystore.jks

```

**Giải thích:**

* `storePassword` & `keyPassword`: Mật khẩu bạn đã nhập khi tạo keystore ở Bước 1.
* `keyAlias`: Tên alias bạn đặt ở Bước 1 (mặc định trong lệnh trên là `my-key-alias`).
* `storeFile`: Đường dẫn đến file keystore. Vì file `key.properties` nằm ở `android/` còn file `.jks` nằm ở `android/app/`, nên đường dẫn là `../app/ten_file.jks` (hoặc bạn có thể để file `.jks` ngay tại `android/` và sửa đường dẫn thành tên file).

---

### Bước 3: Cấu hình `build.gradle.kts`

Vì dự án của bạn dùng Kotlin DSL (`.kts`), bạn cần sửa file `android/app/build.gradle.kts` để đọc file trên.

1. Mở `android/app/build.gradle.kts`.
2. Thêm đoạn code load file ở phía trên phần `android { }` (trước dòng `android {`):

```kotlin
import java.util.Properties
import java.io.FileInputStream

val keystoreProperties = Properties()
val keystorePropertiesFile = rootProject.file("key.properties")
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(FileInputStream(keystorePropertiesFile))
}

```

3. Tìm đến khối `android { ... }` và thêm/sửa phần `signingConfigs`:

```kotlin
android {
    // ... các config khác ...

    signingConfigs {
        create("release") {
            // Chỉ load nếu file key.properties tồn tại để tránh lỗi
            if (keystorePropertiesFile.exists()) {
                keyAlias = keystoreProperties["keyAlias"] as String
                keyPassword = keystoreProperties["keyPassword"] as String
                storeFile = file(keystoreProperties["storeFile"] as String)
                storePassword = keystoreProperties["storePassword"] as String
            }
        }
    }

    buildTypes {
        getByName("release") {
            // Áp dụng cấu hình ký 'release' vừa tạo ở trên
            signingConfig = signingConfigs.getByName("release")
            isMinifyEnabled = true // Có thể là false tùy project
            proguardFiles(getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro")
        }
    }
}

```

---

### Bước 4: Bảo mật (Quan trọng)

Bạn **KHÔNG** nên để lộ mật khẩu keystore lên Github/Gitlab.

1. Mở file `.gitignore` trong thư mục `android/`.
2. Thêm 2 dòng này vào cuối file:

```text
key.properties
**/*.jks
**/*.keystore

```

### Bước 5: Kiểm tra lại

Sau khi làm xong, hãy chạy lệnh sau để đảm bảo mọi thứ hoạt động:

```bash
flutter clean
flutter run

```

Nếu bạn muốn kiểm tra build release:

```bash
flutter build apk --release

```
