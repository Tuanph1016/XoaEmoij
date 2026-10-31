# remove-emoji-zh

Công cụ dòng lệnh xóa **emoji / icon** khỏi văn bản tiếng Trung, chỉ giữ lại **chữ Hán và dấu câu**.
A tiny CLI tool that strips emoji from Chinese text and keeps only Chinese characters and punctuation.

- Không cần cài thư viện, chỉ cần Python 3.8+
- Xử lý được emoji phức tạp: skin tone 👍🏻, ghép ZWJ 👨‍👩‍👧, cờ 🇨🇳, keycap 1️⃣, `❤️`, `‼️`
- Giữ nguyên dấu câu Trung & Latin: `。，、！？；：“”‘’《》【】（）……——·～`
- Giữ nguyên xuống dòng

## Ví dụ

**Trước**

```
连遮一遮回家都很不方便。你着急，我帮你问老师请个假好不好？😊他断然拒绝，
```

**Sau**

```
连遮一遮回家都很不方便。你着急，我帮你问老师请个假好不好？他断然拒绝，
```

## Cài đặt

```bash
git clone https://github.com/<ten-ban>/remove-emoji-zh.git
cd remove-emoji-zh
```

## Bản web (GitHub Pages)

Dùng thử online: **https://&lt;ten-ban&gt;.github.io/remove-emoji-zh/**

File `index.html` là bản web độc lập (HTML + JavaScript thuần, không thư viện, không gửi dữ liệu lên server).
Dán văn bản, bấm **Xóa icon** (hoặc `Ctrl+Enter`), rồi **Sao chép** hoặc **Tải về .txt**.

Bật GitHub Pages cho repo của bạn:

1. Vào repo trên GitHub → **Settings** → **Pages**
2. Mục **Build and deployment** → **Source**: chọn **Deploy from a branch**
3. **Branch**: `main`, thư mục `/ (root)` → **Save**
4. Đợi khoảng 1 phút, link sẽ hiện ở đầu trang Pages

## Giao diện đồ họa (GUI)

```bash
python remove_emoji_zh_gui.py
```

1. Dán văn bản vào ô **Văn bản gốc** (hoặc bấm **Dán từ clipboard** / **Mở file…**)
2. Bấm **Xóa icon** (hoặc `Ctrl+Enter`)
3. Bấm **Sao chép kết quả** hoặc **Lưu file…**

Có thể bật **Tự động sao chép kết quả** để khỏi phải bấm bước 3.
Các checkbox (giữ chữ số, chữ Latin, ký hiệu, gộp dấu cách) sẽ cập nhật kết quả ngay khi đổi.

> GUI dùng `tkinter` (có sẵn khi cài Python trên Windows/macOS).
> Linux: `sudo apt install python3-tk`.
> Trên Windows có thể đổi tên file thành `remove_emoji_zh_gui.pyw` để double-click mà không hiện cửa sổ đen.

### Đóng gói thành file .exe (tùy chọn)

```bash
pip install pyinstaller
pyinstaller --onefile --windowed remove_emoji_zh_gui.py
```

File chạy nằm trong thư mục `dist/`, gửi cho người không cài Python vẫn dùng được.

## Cách dùng dòng lệnh

```bash
# 1. Xử lý một đoạn văn bản
python remove_emoji_zh.py -t "你好😊世界🎉"

# 2. Xử lý file (tạo file mới input.clean.txt)
python remove_emoji_zh.py input.txt

# 3. Chỉ định file đầu ra
python remove_emoji_zh.py input.txt -o output.txt

# 4. Ghi đè file gốc
python remove_emoji_zh.py input.txt -i

# 5. Nhiều file (hỗ trợ wildcard, kể cả trên Windows)
python remove_emoji_zh.py "*.txt" -i

# 6. Dùng với pipe
cat input.txt | python remove_emoji_zh.py > output.txt
```

### Tùy chọn

| Tùy chọn | Mô tả |
| --- | --- |
| `-t, --text` | Xử lý trực tiếp một đoạn văn bản |
| `-o, --output` | File đầu ra (chỉ dùng với 1 file đầu vào) |
| `-i, --in-place` | Ghi đè file gốc |
| `-e, --encoding` | Encoding file đầu vào (mặc định `utf-8`) |
| `--keep-digits` | Giữ chữ số `0-9` |
| `--keep-latin` | Giữ chữ cái Latin `A-Z a-z` |
| `--keep-symbols` | Giữ ký hiệu toán học / tiền tệ (`￥` `$` ...) |
| `--collapse-spaces` | Gộp nhiều dấu cách liền nhau, xóa dấu cách cuối dòng |
| `-q, --quiet` | Không in thống kê |

> Mặc định tool **chỉ giữ chữ Hán + dấu câu + khoảng trắng**. Chữ số và chữ Latin sẽ bị xóa
> trừ khi bạn bật `--keep-digits` / `--keep-latin`.

## Dùng như thư viện Python

```python
from remove_emoji_zh import clean_text

clean_text("你好😊世界")                      # '你好世界'
clean_text("共15人😊", keep_digits=True)       # '共15人'
```

## Chạy test

```bash
python -m unittest -v
```

## Cách hoạt động

Tool dùng cơ chế **whitelist**: chỉ giữ lại ký tự thuộc các khoảng chữ Hán (CJK) hoặc
nhóm dấu câu của Unicode (`P*`), khoảng trắng và xuống dòng. Mọi thứ khác (emoji, variation selector,
ZWJ, skin tone, cờ, ký hiệu...) đều bị loại bỏ, nên cũng bắt được cả emoji mới ra trong tương lai.

## License

MIT
