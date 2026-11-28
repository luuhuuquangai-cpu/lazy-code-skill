# Lazy Code Skill — bộ skill 4S cho agent

Bộ skill này gói chuẩn **4S** lại thành lệnh dùng được: 4S là chuẩn code cho người và agent
trong team. Thay đổi nhỏ nhất mà vẫn giải quyết trọn vấn đề. Phần code còn lại phải tìm được,
sửa được, và tự khai tên khi hỏng.

| Mã | Luật | Ý nghĩa |
|:---|:---|:---|
| `S1` | Sạch Việc | Mỗi file, mỗi hàm làm đúng một việc |
| `S2` | Sát Nơi | Code của một việc nằm ở một chỗ |
| `S3` | Suôn Dòng | Dữ liệu đi một chiều, tầng trong không import tầng ngoài |
| `S4` | Số Danh | Log lỗi và `catch` luôn kèm `[module.ham]` |

Giới hạn cứng: file ≤ 200 dòng, hàm ≤ 30 dòng. Mỗi repo có một `LEXICON.md` để người và
agent dùng chung một nghĩa cho mỗi từ — viết bằng lời đời thường, người không biết code
(quản lý, khách hàng, người mới) đọc cũng hiểu.

## Skill

| Skill | Loại | Việc |
|:---|:---|:---|
| `4s` | chế độ, bật suốt session | Luật viết code. Cấp `lite`, `full` (mặc định), `ultra` |
| `4s-flow` | luồng hệ thống | Năm bước, hỏi dồn một chỗ, kỷ luật context, khi nào xoá context |
| `4s-interview` | quy trình trước khi sửa | Hỏi cho ra chuyện: vòng, cây quyết định, chốt nghĩa vào `LEXICON.md` |
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

## Hỏi lại trước khi sửa

Agent không được tự đoán rồi làm. Trước mỗi task code, `4s` chạy cổng `ASK`: gặp từ hiểu hai
kiểu, không rõ hành vi, hay không rõ sửa chỗ nào thì dừng và chạy `/4s-interview` để hỏi bạn
bằng 2–3 lựa chọn lấy từ chính code, mỗi câu kèm sẵn đề xuất để bạn gõ "ừ" là xong.

Đã hỏi mà chưa có câu trả lời thì **không sửa gì**. Không có đường "tự nêu giả định rồi vẫn làm".

## Luồng năm bước

`/4s-flow` cho biết đang ở bước nào:

```
1. HỎI  →  2. CHỐT  →  3. LÀM  →  4. KIỂM  →  5. GHI LẠI
```

Hai luật đáng nhớ: **hỏi dồn một chỗ** (mọi câu hỏi thuộc bước 1, đang code mà cần hỏi thì quay
lại hỏi cho hết) và **kỷ luật context** (giữ bước 1→3 trong một mạch; ở ranh giới bước có năm
lựa chọn xét theo thứ tự, nén context là cuối bảng). Chi tiết ở `/4s-flow`.

## Báo lỗi thì chưa được sửa

Cổng `FIX`: khi bạn báo một lỗi, lượt đó agent chỉ chẩn đoán và trình bày bốn thứ — nguyên nhân,
đề xuất sửa, ảnh hưởng tới chỗ khác, và từ điển có phải sửa theo không. Rồi dừng, chờ bạn duyệt.
Áp dụng cho cả `/4s-ui-debug`.

## Từ điển đọc được bởi mọi người

`LEXICON.md` là cuốn từ điển của **cả dự án**: người quản lý, khách hàng, người mới vào, và agent
đều đọc cùng một bản. Mỗi mục từ có 7 phần, viết bằng lời đời thường, có **Ví dụ** cụ thể và nói
rõ **KHÔNG phải** cái gì. Luật và mẫu ở `/4s-lexicon`.

Việc đang làm dở và câu hỏi chờ chốt ghi ở `KE_HOACH.md`, **không** ghi vào từ điển.

## Giấy phép

MIT. Xem [LICENSE](LICENSE).
