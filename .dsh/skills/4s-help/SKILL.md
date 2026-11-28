---
name: 4s-help
description: >
  Thẻ tra cứu nhanh bộ 4S: cấp độ, các skill, mã luật, cú pháp shortcut, cách bật tắt. Hiện một
  lần. Dùng cho /4s-help, "4s help", "4s dùng thế nào", "mã S1 S2 là gì".
---

# 4S Help

Hiện thẻ này khi được gọi. Một lần: không đổi chế độ, không ghi file.

## Cấp độ

| Cấp | Lệnh | Hành vi |
|:---|:---|:---|
| **lite** | `/4s lite` | Làm đúng yêu cầu, vi phạm 4S chỉ nêu ở cuối câu trả lời |
| **full** | `/4s` | Mặc định. Luật 4S là luật cứng: vi phạm phải sửa hoặc khai `shortcut:` |
| **ultra** | `/4s ultra` | Như full, thêm phản biện yêu cầu trước khi làm, và không tự khai `shortcut:` khi chưa được đồng ý |

Cấp giữ nguyên tới khi đổi hoặc hết session. Ba cổng `LEX`, `ASK`, `FIX` áp dụng ở cả ba cấp.

## Skill

| Skill | Lệnh | Việc |
|:---|:---|:---|
| **4s** | `/4s` | Chế độ code: ít code mới nhất, theo bốn luật, câu trả lời nêu việc đã bỏ qua và rủi ro |
| **4s-flow** | `/4s-flow` | Luồng năm bước, hỏi dồn một chỗ, kỷ luật context và năm lựa chọn ở ranh giới bước |
| **4s-interview** | `/4s-interview` | Hỏi cho ra chuyện: hỏi theo vòng và theo cây quyết định, chốt nghĩa vào `LEXICON.md` |
| **4s-lexicon** | `/4s-lexicon` | Tạo và sửa `LEXICON.md`, từ điển chung viết cho người không biết code cũng hiểu |
| **4s-review** | `/4s-review` | Review một thay đổi theo 4S, đánh số phát hiện |
| **4s-check** | `/4s-check` | Kiểm cả repo theo 4S, phép kiểm `C1`–`C8` |
| **4s-debt** | `/4s-debt` | Sổ nợ: gom các comment `shortcut:` |
| **4s-ui-debug** | `/4s-ui-debug` | Quy trình bug UI: trình duyệt thật trước, chẩn đoán rồi chờ duyệt mới sửa |
| **4s-help** | `/4s-help` | Thẻ này |

## Mã luật

| Mã | Nghĩa |
|:---|:---|
| `S1` | Sạch Việc: mỗi file, mỗi hàm một việc |
| `S2` | Sát Nơi: một việc nằm ở một chỗ |
| `S3` | Suôn Dòng: dữ liệu một chiều, tầng trong không import tầng ngoài |
| `S4` | Số Danh: log lỗi và `catch` kèm `[module.ham]` |
| `L200` `L30` | File ≤ 200 dòng, hàm ≤ 30 dòng |
| `LEX` | Từ điển dự án `LEXICON.md` |
| `ASK` | Cổng hỏi lại: `ASK-1` nghĩa từ, `ASK-2` ranh giới hành vi, `ASK-3` phạm vi nơi sửa |
| `FIX` | Cổng báo lỗi: chỉ chẩn đoán + đề xuất, chờ duyệt rồi mới sửa code hoặc `LEXICON.md` |
| `FLOW` | Luồng năm bước: HỎI → CHỐT → LÀM → KIỂM → GHI LẠI |
| `GATE` | Hỏi dồn một chỗ: mọi câu hỏi thuộc bước 1, vào bước 3 thì không hỏi nữa (trừ khi yêu cầu đổi hẳn) |
| `CTX` | Kỷ luật context: vùng còn sáng ~150k token, năm lựa chọn ở ranh giới bước |
| `SC` | Vi phạm có khai báo bằng comment `shortcut:` |
| `C1`–`C8` | Phép kiểm của `4s-check` |

Thứ tự trước khi sửa: `LEX` (đọc từ điển) → `ASK` (hỏi dồn một chỗ) → ghi `LEXICON.md` → code.
Người dùng báo lỗi: `FIX` (chẩn đoán) → chờ duyệt → sửa. Trình tự đầy đủ ở `4s-flow`.

Tầng: `UI → Controller → Use Case → Entity`, và `Controller → Infrastructure → Entity`.

## Cú pháp nợ

```
// shortcut: [mã luật] <giới hạn đang chấp nhận>, <khi nào phải nâng cấp>
// shortcut: S1 file vừa parse vừa gửi, tách khi thêm kênh gửi thứ hai
```

## Bật, tắt

Tắt: nói "stop 4s" hoặc "normal mode". Bật lại: `/4s`.

Tự bật mỗi session: thêm dòng sau vào `AGENTS.md` của repo, hoặc `~/.dsh/AGENTS.md` cho mọi
dự án trên máy (cần cài skill ở `~/.dsh/skills` trước):

```
Bật skill 4s ở cấp full cho mọi task code.
```

## Luật nằm ở file nào

Sửa luật thì mở đúng một file này, các file khác chỉ trỏ về:

| Luật | Bản gốc |
|:---|:---|
| Bốn luật `S1`–`S4`, giới hạn `L200`/`L30`, nợ `SC`, ba cổng | `4s` |
| Trình tự năm bước, hỏi dồn, kỷ luật context, việc nhỏ | `4s-flow` |
| Cách hỏi, cây quyết định, chốt nghĩa, chưa trả lời thì dừng | `4s-interview` |
| Cấu trúc `LEXICON.md`, 7 phần, một từ một nghĩa, một khái niệm một tên | `4s-lexicon` |
| Kiểm từ điển sau khi sửa bug | `4s-lexicon` mục 8 |
