# 4S — bộ skill chuẩn code cho agent

4S là chuẩn code cho người và agent trong team, kế thừa tinh thần
[ponytail](https://github.com/DietrichGebert/ponytail): thay đổi nhỏ nhất mà vẫn giải quyết trọn
vấn đề. Phần code còn lại phải tìm được, sửa được, và tự khai tên khi hỏng.

| Mã | Luật | Ý nghĩa |
|:---|:---|:---|
| `S1` | Sạch Việc | Mỗi file, mỗi hàm làm đúng một việc |
| `S2` | Sát Nơi | Code của một việc nằm ở một chỗ |
| `S3` | Suôn Dòng | Dữ liệu đi một chiều, tầng trong không import tầng ngoài |
| `S4` | Số Danh | Log lỗi và `catch` luôn kèm `[module.ham]` |

Giới hạn cứng: file ≤ 200 dòng, hàm ≤ 30 dòng. Mỗi repo có một `LEXICON.md` để người và
agent dùng chung một nghĩa cho mỗi từ.

## Skill

| Skill | Loại | Việc |
|:---|:---|:---|
| `4s` | chế độ, bật suốt session | Luật viết code. Cấp `lite`, `full` (mặc định), `ultra` |
| `4s-lexicon` | sửa `LEXICON.md` | Tạo và bảo trì từ điển dự án |
| `4s-review` | báo cáo một lần | Review một thay đổi |
| `4s-check` | báo cáo một lần | Kiểm cả repo, phép kiểm `C1`–`C8` |
| `4s-debt` | báo cáo một lần | Sổ nợ: gom các comment `shortcut:` |
| `4s-ui-debug` | quy trình | Bug UI: trình duyệt thật trước, code sau |
| `4s-help` | thẻ tra cứu | Cấp độ, lệnh, mã luật |

Mỗi skill nằm ở `.dsh/skills/<tên>/SKILL.md`. Mỗi file làm một việc và không quá 200 dòng.

## Cài đặt

**Một dự án:** copy thư mục `.dsh/skills/` vào gốc dự án. DeepSeek Harness đọc skill trong
`.dsh/skills` của thư mục gần nhất có `.git`.

**Mọi dự án trên máy:** copy từng thư mục skill vào `~/.dsh/skills/`.

```powershell
Copy-Item .dsh\skills\* "$env:USERPROFILE\.dsh\skills\" -Recurse
```

```bash
cp -r .dsh/skills/* ~/.dsh/skills/
```

Các agent khác đọc chuẩn `SKILL.md` (như Claude Code) thì copy vào thư mục skill của agent đó.

## Tự bật mỗi session

Thêm dòng sau vào `AGENTS.md` ở gốc repo (cả team dùng chung), hoặc vào `~/.dsh/AGENTS.md`
(mọi dự án trên máy, cần cài skill toàn máy trước):

```
Bật skill 4s ở cấp full cho mọi task code.
```

Tắt trong một session: nói "stop 4s". Đổi cấp: `/4s lite`, `/4s ultra`.

## Bắt đầu với một repo

1. `/4s-lexicon` để tạo `LEXICON.md`. Skill `4s` dừng mọi task cho tới khi file này có.
2. `/4s-check` để xem repo đang lệch chuẩn ở đâu.
3. Code bình thường khi `4s` đang bật. Trước khi merge, chạy `/4s-review`.

## Chạy cùng ponytail

Không cần cài ponytail. `4s` đã chứa thang chọn giải pháp, phần trả lời kết thúc bằng
"đã bỏ qua / rủi ro", và luật "không có trường hợp cụ thể thì không phải phát hiện".
Bật cả hai cũng không xung đột. Riêng chuyện tách hay gom file thì theo 4S.
