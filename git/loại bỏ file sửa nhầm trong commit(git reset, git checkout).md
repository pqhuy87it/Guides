Để loại bỏ một file khỏi commit vừa tạo (ví dụ: bạn lỡ tay `git add .` rồi commit luôn cả file cấu hình hoặc file rác), cách xử lý phụ thuộc vào việc bạn **đã push code lên server hay chưa**.

Dưới đây là các trường hợp cụ thể:

### 1. Trường hợp phổ biến nhất: Chưa Push (Code vẫn ở máy)

Cách an toàn nhất là **"tháo" commit ra, bỏ file đó xuống, rồi commit lại**. Cách này giúp bạn **giữ lại những thay đổi** của file đó ở dưới máy (dưới dạng chưa add) để xử lý sau.

**Các bước thực hiện:**

1. **Hủy commit vừa rồi (nhưng giữ nguyên code):**
```bash
git reset --soft HEAD~1

```
kết quả là commit bị xoá, những file thay đổi trở về trạng thái trước commit

```bash
git reset --hard HEAD~1

```
kết quả là commit bị xoá, những file thay đổi cũng không còn

2. **Đưa file bạn muốn loại bỏ ra khỏi vùng Staging (Unstage):**
```bash
git reset <tên_file_muốn_bỏ>

```

*(Lúc này file đó sẽ chuyển sang màu đỏ - Modified, nhưng không còn nằm trong danh sách chờ commit nữa).*
3. **Commit lại các file còn lại:**
```bash
git commit -m "Message cũ hoặc mới tùy bạn"

```

---

### 2. Trường hợp muốn xóa hẳn thay đổi của file đó trong commit (Chưa Push)

Nếu bạn lỡ sửa sai file đó và muốn commit vừa rồi **coi như chưa từng chạm vào file đó** (trả file về trạng thái cũ của commit trước nữa):

```bash
# 1. Lấy nội dung cũ của file đó (từ commit trước) đè lên hiện tại
git checkout HEAD~1 -- <tên_file>

# 2. Cập nhật lại commit hiện tại
git commit --amend --no-edit

```

*Kết quả: Commit của bạn vẫn giữ nguyên ID (hash) nhưng file kia đã biến mất khỏi danh sách thay đổi.*

---
