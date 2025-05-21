# SoundConverterOfficial

Bài tập Java cuối kỳ

# 🎵 Sound Converter – AI Audio Tool

Ứng dụng desktop giúp **phân tích giọng nói, chỉnh sửa nội dung, và trộn các đoạn âm thanh lại thành file mới** với sự hỗ trợ của AI (Whisper CPP) và FFmpeg. Giao diện thân thiện, hỗ trợ tiếng Việt.

---

## ⚙️ HƯỚNG DẪN CÀI ĐẶT (Setup)

### 🧩 A. Cài đặt môi trường

| Yêu cầu                    | Ghi chú                          |
| -------------------------- | -------------------------------- |
| Java JDK 17+               | Bắt buộc                         |
| MySQL Server 8+            | Dùng để lưu phiên âm             |
| FFmpeg                     | Đặt sẵn trong `resources/ffmpeg` |
| Whisper CPP + model `.bin` | Đặt trong `resources/whisper/`   |

#### 1. Cài Java JDK

- Tải JDK từ [Adoptium](https://adoptium.net) hoặc [Oracle](https://www.oracle.com/java/)
- Cài đặt, rồi thiết lập biến môi trường `JAVA_HOME`
- Kiểm tra bằng:

```bash
java -version
```

#### 2. Cài MySQL

- Tải tại [MySQL Download](https://dev.mysql.com/downloads/)
- Tạo người dùng `root` hoặc user riêng
- Mở cổng `3308` nếu cần (điều chỉnh theo sql server của máy chạy theo port nào, thường là 3306)

#### 3. Tạo file cấu hình

Tạo file tại:
`src/main/resources/config.properties`

```properties
db.url=jdbc:mysql://localhost:3308/soundconverter?createDatabaseIfNotExist=true&useUnicode=true&characterEncoding=UTF-8
db.user=root
db.password=your_password
```

> ✅ Ứng dụng sẽ tự động tạo cơ sở dữ liệu & bảng khi chạy lần đầu.

---

### 🔧 B. Cài đặt mã nguồn & build

Tải thư mục dự án về, rồi build như sau:

```bash
cd SoundConverterOfficial
mvn clean package
```

Sau khi build xong, bạn sẽ có file:

```
target/SoundConverterOfficial-1.0-SNAPSHOT.jar
```

---

### 🧠 C. Cài Whisper CPP & FFmpeg

1. **Whisper CLI**: tải `whisper-cli.exe` từ:
   [https://github.com/ggerganov/whisper.cpp/releases](https://github.com/ggerganov/whisper.cpp/releases)

2. **Model AI**: ví dụ:

   - `ggml-base-q8_0.bin`
   - `ggml-tiny.en.bin`

> Đặt vào thư mục: `src/main/resources/whisper/models`

3. **FFmpeg**: tải `ffmpeg.exe` và `ffprobe.exe` tại:
   [https://ffmpeg.org/download.html](https://ffmpeg.org/download.html)

> Đặt vào thư mục: `src/main/resources/ffmpeg`

---

### 🚀 D. Chạy ứng dụng

```bash
java -jar target/SoundConverterOfficial-1.0-SNAPSHOT.jar
```

Hoặc vào file src/main/MainLauncher.java để chạy

---

## 🧪 HƯỚNG DẪN Sử DỤNG (How to Use)

### 1. Nhập file âm thanh

- Mở tab **Audio Segments**
- Nhấn nút `Import`
- Chọn file `.mp3`, `.wav`, `.ogg`, v.v.

### 2. Phân tích nội dung bằng AI

- Chọn ngôn ngữ (mặc định là **Tiếng Việt**)
- Nhập độ dài phân đoạn (hoặc để `0` để AI tự chia)
- Nhấn `Analyze AI`

> Kết quả: nội dung lời nói được chia thành từng đoạn ngắn và hiển thị dưới dạng bảng.

### 3. Chỉnh sửa đoạn phiên âm

- Nhấn đúp vào cột **Text** để sửa nội dung
- Có thể sửa sai chính tả, thêm dấu, v.v.

### 4. Ghép các đoạn lại thành file mới

- Chọn đoạn → `Add to Merge`
- Vào tab **Merge & Export**
- Sắp xếp thứ tự bằng `Up / Down`
- Nhấn `Preview` để nghe thử
- Đặt tên file (ví dụ: `output_remix.mp3`)
- Nhấn `Merge` để xuất file mới

> File sẽ được lưu tại thư mục `output/`

---

## 🧠 Tùy chọn nâng cao

### 📁 Lưu / Tải lại cấu hình trộn

- Dùng `Save` để lưu danh sách đoạn đã chọn
- Dùng `Load` để tải lại sau

### 🧠 Chọn model AI phù hợp

| Model         | Gợi ý sử dụng          |
| ------------- | ---------------------- |
| tiny.en.bin   | Máy yếu, chỉ tiếng Anh |
| base-q8_0.bin | Đa ngôn ngữ, phổ thông |
| medium.bin    | Cần độ chính xác cao   |

> Thay đổi bằng cách chỉnh trong `WhisperService.java`

---

## ❌ Xử lý lỗi thường gặp

| Lỗi                     | Cách xử lý                                 |
| ----------------------- | ------------------------------------------ |
| Không chạy được app     | Kiểm tra JDK, FFmpeg, Whisper đã đúng chưa |
| Không kết nối MySQL     | Kiểm tra `config.properties`, mở port 3306 |
| Whisper báo lỗi model   | Model `.bin` bị thiếu hoặc sai định dạng   |
| Không tạo được file mới | Kiểm tra quyền ghi thư mục `output/`       |

---

## 📊 Phím tắt tiện dụng

| Tổ hợp phím | Chức năng     |
| ----------- | ------------- |
| Ctrl+I      | Nhập audio    |
| Ctrl+A      | Phân tích AI  |
| Ctrl+E      | Sửa phiên âm  |
| Ctrl+M      | Merge file    |
| Ctrl+S      | Save cấu hình |
| Ctrl+L      | Load cấu hình |
| Ctrl+P      | Preview file  |

---

## 📢 Liên hệ & Hỗ trợ

- Email: **[truongtrivn123@gmail.com](mailto:truongtrivn123@gmail.com)**

---
