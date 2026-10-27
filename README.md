# File2Excel

Trang web tĩnh 1 file (`index.html`) để chuyển đổi **PDF / ảnh / Excel (XLSX, XLS, CSV)** sang **Excel** — chạy 100% trên trình duyệt, không cần server riêng.

## Cách dùng

1. Vào trang (GitHub Pages) hoặc mở trực tiếp `index.html` trong trình duyệt.
2. Kéo thả (hoặc chọn) 1 hoặc nhiều file PDF / ảnh / Excel.
3. Bấm **Chuyển đổi** → xem trước → bấm **Tải Excel**.

## Chức năng

- **Excel → Excel:** đọc trực tiếp bằng SheetJS, mỗi sheet gốc thành 1 sheet xuất ra.
- **PDF → Excel:** dùng pdf.js lấy vị trí chữ trên từng trang, ghép thành hàng/ô theo layout (chế độ "Tự động") hoặc lấy văn bản thô mỗi dòng 1 ô (chế độ "Văn bản thô").
- **Ảnh → Excel:** gửi ảnh tới **OCR API ngoài** do bạn tự cấu hình (mục ⚙️ Cấu hình OCR). Lưu ý: nội dung ảnh sẽ được gửi tới API đó — các loại file khác không rời khỏi máy.

## Cấu hình OCR (cho ảnh)

- **OCR API Endpoint:** API nhận POST, body JSON:
  ```json
  { "imageBase64": "…", "mimeType": "image/png" }
  ```
  Trả về một trong các dạng:
  ```json
  { "text": "…văn bản…\ncách dòng…", } // tab/dấu , hoặc ; auto tách ô
  { "lines": [["a","b"],["c","d"]] }
  { "rows": [["a","b"],["c","d"]] }
  ```
- **API Key:** tùy chọn, được gửi theo header `Authorization: Bearer …`.
- Cấu hình lưu trong `localStorage` của trình duyệt, không có server lưu lại.

## Deploy (GitHub Pages)

Repo này dùng GitHub Pages từ nhánh chính:
- Settings → Pages → Deploy from a branch → `main` / `(root)`.

Hoặc deploy lại bằng lệnh:
```bash
gh repo create datkep92/file2excel --public --source=. --push
gh api repos/datkep92/file2excel/pages -X POST -f source='{"branch":"main","path":"/"}' --input - 2>/dev/null || true
```
Sau đó trang chạy tại: `https://datkep92.github.io/file2excel/`

## Thư viện (CDN)

- [SheetJS xlsx](https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js)
- [pdf.js 3.11](https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js)

## Giới hạn đã biết

- PDF scan (ảnh quét, không có lớp chữ) cần OCR — hiện chỉ hỗ trợ OCR qua API cấu hình, chưa có OCR nội bộ cho PDF scan.
- Ảnh → Excel phụ thuộc chất lượng API OCR bạn cấu hình; chưa có OCR mặc định.
