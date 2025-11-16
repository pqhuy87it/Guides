`git cherry-pick` là một lệnh Git rất hữu ích, cho phép bạn "chọn" (pick) một commit cụ thể từ một nhánh và áp dụng nó lên một nhánh khác.

Không giống như `merge` hay `rebase` (áp dụng toàn bộ lịch sử của nhánh), `cherry-pick` chỉ lấy đúng commit mà bạn chỉ định.

## 🎯 Mục đích chính

Lệnh này sao chép **nội dung thay đổi** (patch) của một commit và tạo ra một **commit mới** (với mã hash mới) trên nhánh hiện tại của bạn.

## Scenario Khi nào nên dùng `git cherry-pick`?

1.  **Vá lỗi (Hotfix):** Bạn sửa một lỗi nghiêm trọng trên nhánh `main` (hoặc `master`). Bạn muốn áp dụng bản vá (commit sửa lỗi) đó cho nhánh `develop` hoặc nhánh `feature` của bạn ngay lập tức mà không cần merge toàn bộ nhánh `main`.
2.  **Commit nhầm nhánh:** Bạn vô tình commit một tính năng lên nhánh `develop` trong khi đáng lẽ phải commit nó vào nhánh `feature/my-feature`. Bạn có thể dùng `cherry-pick` để "chuyển" commit đó sang nhánh `feature` (sau đó reset lại nhánh `develop`).
3.  **Lấy một phần tính năng:** Một đồng nghiệp làm 5-6 commit trên nhánh `feature/A`, nhưng bạn chỉ cần *một* trong số các commit đó cho nhánh `feature/B` của mình.

-----

## 🛠 Hướng dẫn sử dụng cơ bản

Giả sử bạn có mã hash (SHA) của commit muốn lấy (bạn có thể tìm bằng lệnh `git log` trên nhánh kia).

**Kịch bản:** Bạn muốn lấy commit `a865b4d` từ nhánh `feature/A` và áp dụng nó vào nhánh `develop`.

**Bước 1: Lấy mã commit**

Trước tiên, hãy `checkout` qua nhánh chứa commit bạn muốn:

```bash
git checkout feature/A
git log --oneline
```

Bạn sẽ thấy danh sách commit, ví dụ:

```
a865b4d Sửa lỗi nút bấm bị lệch
c390f21 Thêm tính năng đăng nhập
...
```

Mã commit bạn cần là `a865b4d`.

**Bước 2: Chuyển về nhánh đích**

Chuyển về nhánh mà bạn muốn *nhận* commit đó (nhánh `develop` trong ví dụ này):

```bash
git checkout develop
```

**Bước 3: Thực hiện "cherry-pick"**

Dùng lệnh `git cherry-pick` với mã hash của commit:

```bash
git cherry-pick a865b4d
```

Git sẽ lấy các thay đổi từ commit `a865b4d` và tạo một commit *mới* trên nhánh `develop` với cùng nội dung thay đổi và cùng message (nhưng mã hash sẽ khác).

-----

## 🌟 Các tùy chọn (Options) hữu ích

### 1\. "Chọn" nhiều commit

Bạn có thể "chọn" nhiều commit cùng lúc bằng cách liệt kê các mã hash, theo thứ tự từ cũ đến mới:

```bash
# Áp dụng commit A, rồi B, rồi C
git cherry-pick <hash-commit-A> <hash-commit-B> <hash-commit-C>
```

### 2\. "Chọn" một dải commit (Range)

Bạn có thể "chọn" một loạt các commit liên tiếp.

```bash
# Lấy các commit từ <hash-A> (không bao gồm A) đến <hash-B> (bao gồm B)
git cherry-pick <hash-A>..<hash-B>

# Lấy các commit từ <hash-A> (bao gồm A) đến <hash-B> (bao gồm B)
# Dấu ^ có nghĩa là "bao gồm cả A"
git cherry-pick <hash-A>^..<hash-B>
```

### 3\. Chỉnh sửa commit message (`-e`)

Nếu bạn muốn chỉnh sửa lại commit message (ví dụ: thêm ghi chú là "cherry-picked từ nhánh A"), hãy dùng cờ `-e`:

```bash
git cherry-pick -e <commit-hash>
```

Trình soạn thảo code của bạn sẽ mở ra, cho phép bạn sửa message trước khi commit.

### 4\. Áp dụng thay đổi nhưng không commit (`-n`)

Nếu bạn muốn lấy các thay đổi nhưng *chưa* vội commit (ví dụ: để gộp chung với các thay đổi khác), hãy dùng cờ `-n` (hoặc `--no-commit`):

```bash
git cherry-pick -n <commit-hash>
```

Các thay đổi sẽ được đưa vào **Staging Area**. Bạn có thể kiểm tra lại (`git status`) rồi `git commit` thủ công sau.

-----

## 💥 Giải quyết xung đột (Conflict)

`cherry-pick` rất dễ gây ra xung đột (conflict) nếu thay đổi của commit bạn chọn bị "đụng hàng" với code trên nhánh hiện tại.

Khi điều này xảy ra, Git sẽ tạm dừng:

1.  **Sửa file:** Mở các file bị báo "conflict" và sửa chúng (xóa các dấu `<<<<<`, `>>>>>` và chọn code đúng).
2.  **Add file:** Sau khi sửa xong, dùng `git add` để báo cho Git là bạn đã giải quyết xong:
    ```bash
    git add <ten-file-vua-sua>
    ```
3.  **Tiếp tục:** Ra lệnh cho `cherry-pick` tiếp tục:
    ```bash
    git cherry-pick --continue
    ```

> **Cách hủy bỏ:**
> Nếu bạn thấy conflict quá phức tạp và muốn "quay xe", hãy dùng lệnh:
>
> ```bash
> git cherry-pick --abort
> ```
>
> Mọi thứ sẽ trở lại trạng thái như trước khi bạn gõ lệnh `cherry-pick`.

Bạn có muốn tìm hiểu thêm về cách hoạt động của `git rebase` hoặc `git merge` để so sánh không?
