---
name: 4s-check
description: >
  Kiểm cả repo theo 4S: file thùng rác (utils/helpers), một việc rải nhiều nơi, import ngược
  tầng, log lỗi và catch thiếu tên module, file quá 200 dòng, hàm quá 30 dòng, từ chưa có trong
  LEXICON.md, nợ shortcut không có điều kiện nâng cấp. Báo cáo xếp hạng, đánh số, không sửa code.
  Dùng cho "kiểm tra 4s", "audit 4s", "repo có đạt 4s không", trước khi merge hoặc bàn giao,
  /4s-check.
---

Kiểm repo như người vừa nhận bàn giao và sẽ bị gọi dậy khi nó hỏng. Thứ tự quan trọng:
`S4` (lỗi bị giấu) → `S3` (sai chiều phụ thuộc) → `S2` → `S1` → `LEX` → giới hạn dòng.
Đây là báo cáo người dùng yêu cầu, nên đưa đủ.

## 1. Bản đồ trước

- Kiểm phần người dùng chỉ định: một thư mục, một package, hoặc cả repo. Không chỉ định: cả repo.
- Đọc `LEXICON.md` (tên tầng thật ở mục 3, danh từ nghiệp vụ ở mục 2), README, entry point
  (main, route, handler, job, CLI). Không có `LEXICON.md`: ghi `C7` FAIL, vẫn kiểm tiếp.
- Repo lớn: đi sâu ở chỗ sai tốn nhất (tiền, auth, ghi dữ liệu, job nền), không đọc từng file.
  Nói rõ phần nào chưa đọc.

## 2. Phép kiểm

Mọi lệnh dưới đây chỉ dùng `git`, chạy y hệt trong bash và PowerShell. Luôn giới hạn loại file
code, nếu không grep sẽ bắt cả tài liệu `.md`. Thêm đuôi file của stack nếu thiếu.

**`C1` `S4` log lỗi thiếu tên → FAIL**

```
git grep -nE -e 'console\.(error|warn)|log(ger|ging)?\.(error|warn|exception)' --and --not -e '\[[A-Za-z_$][A-Za-z0-9_$.]*\.[A-Za-z0-9_$]+\]' -- '*.js' '*.jsx' '*.ts' '*.tsx' '*.vue' '*.py'
git grep -n -A3 -E '\bcatch\b *(\(|\{)|^\s*except\b' -- '*.js' '*.jsx' '*.ts' '*.tsx' '*.vue' '*.py'
```

Lệnh hai in mỗi `catch`/`except` kèm 3 dòng sau. Chỉ xét thân của chính `catch` đó: `catch`
viết trên một dòng thì chỉ xét dòng đó, dòng sau thuộc lệnh khác. Thân không có `[module.ham]`
là FAIL: `catch (e) { console.log(e) }`, `catch { return null }`, `except: pass`. Thân dài hơn
3 dòng thì đọc file để xác nhận.

**`C2` `S1` file thùng rác → FAIL**

```
git ls-files -- ':(icase,glob)**/utils.*' ':(icase,glob)**/util.*' ':(icase,glob)**/helpers.*' ':(icase,glob)**/helper.*' ':(icase,glob)**/common.*' ':(icase,glob)**/misc.*' ':(icase,glob)**/shared.*'
```

Tên theo việc như `helper_thiet_bi.ts` không bị bắt và không phải lỗi. Package `shared/`
dùng chung giữa nhiều app là ngoại lệ hợp lệ nếu `LEXICON.md` mục 4 ghi nó.

**`C3` `S3` import ngược tầng → FAIL**

Thay tên thư mục thật từ `LEXICON.md` mục 3. Entity và Use Case không được import Controller,
UI hay Infrastructure. Mẫu chỉ khớp đúng tên thư mục, nên `quick_ui_less` không bị bắt nhầm.

```
git grep -nE '(from|require\() *.[^ ]*(^|/)(db|ui|api|handlers|adapters)(/|.$|.\)|.;)' -- '*domain*' '*usecase*'
```

Import vòng: chọn 5 module trung tâm, đọc import, đi theo chuỗi. Ghi rõ module nào chưa đi.

**`C4` `S2` một việc rải nhiều nơi → WARN**

Với 3 danh từ nghiệp vụ quan trọng nhất trong `LEXICON.md` mục 2:

```
git grep -l -i -e '<danh_tu>' -- '*.js' '*.ts' '*.py'
```

Hơn 5 file ở hơn 2 thư mục khác nhau → WARN. Cùng một luật (phép tính, điều kiện) viết ở hai
nơi → FAIL, vì hai bản sẽ lệch nhau.

**`C5` `L200` file quá 200 dòng**

```
git grep -c -e '^' -- '*.js' '*.jsx' '*.ts' '*.tsx' '*.vue' '*.py'
```

Đọc các file trên 200 dòng. Làm nhiều việc → FAIL, tách theo việc. Làm một việc và có `SC` →
không phải phát hiện. Làm một việc, không có `SC` → WARN.

**`C6` `L30` hàm quá 30 dòng → WARN**

Không đếm tự động chắc chắn được. Đọc các file lớn nhất từ `C5`. Chỉ báo hàm vừa dài vừa làm
nhiều bước; bảng tra cứu dài là bình thường. Ghi "đã đọc N/M file".

**`C7` `LEX` từ điển**

Không có `LEXICON.md` → FAIL. Có rồi: so 10 danh từ xuất hiện nhiều nhất trong tên file, thư
mục, bảng với từ điển. Danh từ thiếu định nghĩa → WARN. Một từ có hai nghĩa → FAIL.
Mục từ thiếu **Ví dụ** hoặc **KHÔNG phải** → WARN. Nghĩa viết tắt hoặc dùng từ kỹ thuật chưa
giải thích, người không biết code không đọc hiểu → WARN (xem `4s-lexicon` mục 3, 4).

**`C8` `SC` nợ không có điều kiện nâng cấp → WARN**

```
git grep -nE '(#|//|/[*]|--) ?(shortcut|ponytail):' -- ':!*.md'
```

Dòng nào không nêu "khi nào phải nâng cấp" → WARN. Danh sách đầy đủ: skill `4s-debt`.

## 3. Kiểm trước khi báo

- Mỗi phát hiện cần một trường hợp cụ thể: "tình huống này dẫn tới kết quả sai này".
  Không có trường hợp cụ thể thì không phải phát hiện.
- Trước khi nói code không ai dùng: grep cả repo, kể cả test, fixture, config, chuỗi động.
- Vi phạm có `shortcut:` nêu rõ giới hạn là quyết định, không phải phát hiện, trừ khi tải thật
  đã vượt giới hạn đó.
- Đề xuất cách sửa nhỏ nhất. Ưu tiên cách sửa xoá code. Không thêm tầng hay interface.
- Không báo sở thích style, không "nên cân nhắc".

## 4. Đầu ra

Tiếng Việt đơn giản, câu ngắn. Giải thích thuật ngữ ở lần đầu dùng.

Mở đầu: `Repo này làm gì:` hai ba câu.

Rồi ba nhóm, quan trọng trước, bỏ nhóm rỗng:
- **Phải sửa:** mọi FAIL.
- **Nên sửa:** WARN có rủi ro thật (logic rủi ro không có test, một việc rải nhiều nơi).
- **Nhỏ:** WARN còn lại.

Đánh số xuyên suốt để người dùng nói "sửa 2 và 5". Tối đa 20 phát hiện; bỏ bớt thì nói bỏ bao
nhiêu. Mỗi phát hiện đủ bốn phần, mỗi phần một hai câu:

```
2. [C3 S3] Use Case tự gọi DB (src/usecase/tao_don.js:1)
   - Đây là gì: hàm tạo đơn hàng, được controller gọi khi khách bấm đặt hàng.
   - Vấn đề: nó import thẳng ../db/client, nên không test được nếu không có DB thật,
     và đổi DB là phải sửa cả logic đơn hàng.
   - Sửa: controller đọc DB rồi truyền dữ liệu vào tao_don(input); xoá dòng import.
   - Nếu bỏ qua: mỗi lần đổi bảng đơn hàng là phải sửa và test lại cả nghiệp vụ.
```

Kết thúc bằng:
- `Kết luận:` một dòng: đạt chuẩn 4S, hoặc sửa gì trước.
- `Không kiểm:` phần chưa đọc hoặc không chạy được (ví dụ `C6 đã đọc 12/58 file`).

Không có phát hiện: `Repo này làm gì:`, rồi `Đạt chuẩn 4S.` và một dòng về những gì đã kiểm.

Báo cáo một lần, không sửa code.
