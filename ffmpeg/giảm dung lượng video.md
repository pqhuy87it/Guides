Để ép một video từ 270MB xuống dưới 100MB (giảm hơn 60% dung lượng), nguyên tắc cốt lõi là bạn phải **giảm Bitrate** (lượng dữ liệu trên mỗi giây), sau đó có thể cân nhắc hạ thêm độ phân giải (Resolution) hoặc số khung hình trên giây (FPS) nếu cần.

Dưới đây là 3 cách xử lý hiệu quả nhất, từ gõ lệnh nhanh gọn đến dùng giao diện trực quan:

### 1. Dùng FFmpeg qua Terminal (Nhanh và tối ưu nhất)

Chỉ cần mở WezTerm trên máy Mac của bạn lên, FFmpeg là công cụ vô địch để xử lý việc này mà không cần lằng nhằng giao diện.

* **Cách 1: Ép dung lượng bằng chỉ số CRF (Constant Rate Factor)**
`ffmpeg -i input.mp4 -vcodec libx264 -crf 28 output.mp4`
*(Mẹo: Điểm CRF dao động từ 0-51. Số càng cao thì dung lượng càng nhẹ nhưng hình càng mờ. Mức 28 là mức nén khá mạnh nhưng hình ảnh vẫn rất ổn).*
* **Cách 2: Nén bằng chuẩn H.265 (HEVC)**
Chuẩn này nén tốt hơn H.264 khoảng 50% mà vẫn giữ nguyên chất lượng. File xuất ra chạy cực kỳ mượt trên các thiết bị thuộc hệ sinh thái iOS/macOS.
`ffmpeg -i input.mp4 -c:v libx265 -crf 28 output.mp4`
* **Cách 3: Kết hợp hạ độ phân giải (Ví dụ hạ từ 1080p xuống 720p)**
`ffmpeg -i input.mp4 -vf scale=-1:720 -c:v libx264 -crf 26 output.mp4`

### 2. Dùng phần mềm HandBrake (Giao diện trực quan)

Nếu bạn muốn có UI để dễ kéo thả và xem trước thông số, **HandBrake** là phần mềm mã nguồn mở miễn phí tốt nhất.

1. Tải HandBrake và kéo thả file 270MB vào.
2. Ở mục **Preset**, chọn `Web` -> `Vimeo YouTube HQ 720p60` (hoặc `720p30` để nhẹ hơn nữa).
3. Chuyển sang tab **Video**, chú ý thanh trượt **Constant Quality**, hãy kéo nó về mức **26 đến 28**.
4. Chuyển sang tab **Audio**, hạ Bitrate âm thanh xuống **128kbps** (giúp tiết kiệm thêm một khoảng dung lượng kha khá).
5. Bấm **Start Encode**.

### 3. Dùng các trang web Online (Không cần cài đặt)

Nếu video không chứa mã nguồn dự án hay thông tin bảo mật và bạn không ngại chờ upload, các trang web này sẽ tự động tính toán giúp bạn:

* **FreeConvert.com:** Trang này cho phép tải lên file lớn hơn 200MB. Điểm hay nhất là nó có mục **Advanced Options**, cho phép bạn nhập đích danh "Target Size" (Dung lượng mục tiêu). Bạn chỉ cần gõ **95MB**, hệ thống sẽ tự động ép file xuống mức đó.
* **8mb.video:** Một trang web giao diện siêu tối giản, cho phép bạn chọn mức ép dung lượng trực tiếp xuống các mốc: 8MB, 50MB hoặc 100MB.

Video của bạn có thời lượng chính xác là bao nhiêu phút và giây? Nếu bạn cung cấp thời lượng, tôi có thể tính ra công thức Bitrate chuẩn xác nhất để bạn gõ lệnh xuất ra file vừa khít 99MB mà hình ảnh nét nhất có thể!
