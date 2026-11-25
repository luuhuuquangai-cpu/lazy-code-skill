---
name: 4s-debt
description: >
  Gom mọi comment `shortcut:` (và `ponytail:` cũ) thành sổ nợ: chỗ nào đang cố ý vi phạm 4S, giới
  hạn đang chấp nhận, khi nào phải sửa. Báo cáo một lần, không sửa code. Dùng cho "sổ nợ 4s",
  "4s debt", "còn nợ gì", "liệt kê shortcut", /4s-debt.
---

Mỗi lần cố ý vi phạm 4S (file vượt 200 dòng, một file làm hai việc, tạm bỏ qua một tầng), skill
`4s` bắt để lại một comment `shortcut:` nêu giới hạn và điều kiện nâng cấp. Skill này gom các
comment đó vào một sổ, để món nợ tạm thời không âm thầm thành vĩnh viễn.

## Quét

Tìm marker trong comment, bỏ qua `.git`, mọi thứ `.gitignore` loại (dependency, build, cache),
và tài liệu `.md`:

```
git grep -nE '(#|//|/[*]|--) ?(shortcut|ponytail):' -- ':!*.md'
```

Repo chưa có git: thêm `--no-index`. Stack dùng ký hiệu comment khác thì thêm vào biểu thức.
Người dùng nêu từ khoá riêng (`/4s-debt TODO`) thì grep từ đó.

Mỗi kết quả là một dòng nợ. Bỏ kết quả không phải món nợ, như ghi chú về phím tắt bàn phím.

## Đầu ra

Một dòng mỗi món nợ, nhóm theo file:

`<file>:<dòng> [mã luật]. giới hạn: <giới hạn đang chấp nhận>. nâng cấp khi: <điều kiện>.`

Cú pháp comment là `shortcut: [mã luật] <giới hạn>, <khi nào phải nâng cấp>`, nên lấy thẳng từ
comment. Comment không ghi mã luật thì đoán từ nội dung (`S1` tách việc, `S2` gom chỗ, `S3` tầng,
`S4` log, `L200`, `L30`), và đánh dấu `(đoán)`. Cần chủ nợ thì thêm `git blame -L<dòng>,<dòng>`.

Gắn nhãn `không-điều-kiện` cho món nợ không nêu khi nào phải nâng cấp. Đó là những món sẽ mục.

Kết thúc: `<N> món nợ, <M> không có điều kiện nâng cấp.` Nhóm theo mã luật nếu có từ 5 món trở
lên, để thấy luật nào đang bị nợ nhiều nhất.

Không có kết quả: `Không có nợ 4S. Sổ sạch.`

## Ranh giới

Chỉ đọc và báo cáo, không sửa gì. Muốn lưu lại thì người dùng nói, skill ghi sổ ra file
(vd `4S-DEBT.md`). Báo cáo một lần.
