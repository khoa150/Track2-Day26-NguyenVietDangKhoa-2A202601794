# COLOSSEUM · Đấu Trường Agent

### Day 26 · MCP/A2A Infrastructure & Agentic Routing — the competitive lab

> Bản MCP 2026-07-28 đã bỏ handshake. Trong một trận đấu, điều đó còn có nghĩa là **không có
> đình chiến**.
>
> *The 2026-07-28 MCP spec removed the handshake. In a fight, it also means there is no truce.*

---

## 1. Bạn xây gì · What you build

Bạn xây **backend của một VLearn tutor**: một learner hỏi về khoá AI20K, và tutor phải trả lời
**có căn cứ** trong chính kho tài liệu của khoá — 24 slide deck đang dùng, 66 deck canonical,
35 file RESEARCH, 16 GLOSSARY — qua một bề mặt MCP/A2A **đang nói dối nó**.

*You build the backend of a VLearn tutor. A learner asks about the AI20K course; your tutor must
answer it grounded in the real course corpus, through an MCP/A2A surface that is lying to it.*

**Ba nhiệm vụ · Three tasks**, mỗi vòng đấu bạn làm cả ba:

```
   TASK 1 · ATTACK      deck/     soạn một ASK + một mutation manifest. Bắn vào đối thủ.
   TASK 3 · DEFEND      agent/    agent CỦA BẠN, dưới đòn tấn công của họ: suy luận →
                                  gọi MCP/A2A → một câu trả lời có dẫn chứng, có guardrail
   TASK 2 · PROSECUTE   eval/     bạn nhận trace của họ. Nộp cáo buộc: invariant nào vỡ,
                                  ở sự kiện nào, chứng minh ra sao
```

### ⚖ Luật quyết định tất cả · The rule that defines the format

> **Không chỉ ra được thì không có sát thương.**
> *No claim, no damage.*

Trọng tài chỉ chấm những gì **được nêu ra và chứng minh**. Một đòn đánh trúng mà bạn không giải
thích được thì **bằng không**. Một cáo buộc sai làm bạn mất `0.8 × trọng số` của chính lớp lỗi đó.

*An attack that lands but that you cannot explain earns nothing. Being right is not enough; you
must prove it.*

### Một hiệp, năm giai đoạn · One exchange, five stages

![COLOSSEUM — the five stages of one exchange](docs/img/spar-five-stages.png)

Đây là `spar.html` phát lại một hiệp thật. Đọc từ trái sang phải, một hàng là một đội:

*This is `spar.html` replaying a real exchange. Read it left to right; one row per side.*

|                      |                                                        |                                                                  |
| -------------------- | ------------------------------------------------------ | ---------------------------------------------------------------- |
| **1 QUESTION** | lá bài của đối thủ hỏi gì                      | `which_day_covers · baggage · needs course_day,track,anchor` |
| **2 ACTION**   | agent gọi gì,**gateway của BẠN xử ra sao**  | `slides.query > forward` · `slides.get_frame > forward`     |
| **3 ANSWER**   | câu trả lời đi ra, và nó dẫn nguồn gì         | `0 spans · 0 anchors · **UNCITED**`                          |
| **4 EVAL**     | đối thủ cáo buộc gì, dựa vào bằng chứng nào | `protocol_misuse · evt:0009`                                  |
| **5 REFEREE**  | trọng tài phán, và mất bao nhiêu máu            | **`VERIFIED −6hp`**                                     |

Hàng dưới trống vì hiệp này bên kia chưa phòng thủ. Ô **EVAL** ghi `no claim, no damage` khi
công tố không nộp gì — đó chính là luật của thể thức, hiện ngay trên màn hình.

*The bottom row is empty because that side has not defended yet in this exchange. The **EVAL**
column reads `no claim, no damage` when a prosecutor filed nothing — the rule of the format,
visible on screen.*

### Màn hình lớn · The projector

![COLOSSEUM — the projector view](docs/img/projector-five-stages.png)

Giảng viên chiếu `projector.html`: cùng năm giai đoạn đó, nhưng của trận đang
đấu. Ảnh này lấy từ một trận thật (`q-s01-operator`, hiệp 4, máu 89–84).

*The instructor projects `projector.html`: the same five stages, for the duel
currently on camera. This capture is from a real duel — round 4, HP 89–84.*

|                      |                                      |                                             |
| -------------------- | ------------------------------------ | ------------------------------------------- |
| **1 QUESTION** | `define_term`                      | cần`definition.anchor` · lá `atk_04` |
| **2 ACTION**   | `glossary.define` → forward ×2   | `slides.search`                           |
| **3 ANSWER**   | `2 spans · 1 anchor · 235 chars` | `streamable-http-transport`               |
| **4 EVAL**     | `wrong_answer` @ `answer.span:0` | `unflagged_conflict` @ `answer.span:1`  |
| **5 REFEREE**  | `UNPROVEN −10hp`                  | `FALSE −7.5hp`                           |

Nhìn giai đoạn 5: **công tố nộp hai cáo buộc, không cáo buộc nào được công nhận,
và chính họ mất 17.5 máu vì phản đòn.** Cáo buộc sai có giá — đó là lý do
"không cáo buộc thì không có sát thương" cắt theo cả hai chiều.

*Look at stage 5: **the prosecutor filed two claims, neither was upheld, and the
recoil cost them 17.5 HP.** A false claim has a price — which is why "no claim,
no damage" cuts both ways.*

Ở ảnh trên, ô 3 ghi `UNCITED`: agent trả lời mà **không dẫn một anchor nào**, và ô 5 cho thấy
cái giá của nó. Cột 2 và cột 5 là hai thứ bạn thực sự bị chấm.

*In the shot above, stage 3 says `UNCITED` — the agent answered with no anchors at all — and
stage 5 shows what that cost. Columns 2 and 5 are the two you are actually scored on.*

---

## 2. Bắt đầu trong 5 phút · Start in 5 minutes

```bash
make install          # python 3.12, zero third-party deps
make doctor           # kiểm tra sẵn sàng · readiness check
make spar BOT=rookie  # đấu với bot dễ nhất · fight the easiest bot
make ui               # mở màn hình pixel · open the pixel battle view
```

### ⚠️ Thế giới (corpus) KHÔNG nằm trong repo này · The world is NOT in this repo

`make doctor` sẽ báo lỗi này ở lần chạy đầu, và đó là **bình thường**:

```
no world in kit/world/ - ask your instructor for the world artifact
```

Corpus (~12 MB) không commit vào repo — tải từ **Releases**:

```bash
gh release download world-df8c55dabb35 --pattern '*.zip'
unzip colosseum-world-df8c55dabb35.zip     # -> kit/world/df8c55dabb35/
rm colosseum-world-df8c55dabb35.zip
make doctor                                # -> world df8c55dabb35 - 12375 pages
```

Kiểm tra toàn vẹn (khuyến khích):

```bash
gh release download world-df8c55dabb35 --pattern '*.sha256'
shasum -a 256 -c colosseum-world-df8c55dabb35.zip.sha256
```

Dùng `gh` vì repo đang private — `curl` thẳng link release trả **404** cho tới
khi repo mở public.

*Download the corpus from the repo's Releases with `gh` (a plain `curl` to the
release URL returns 404 while this repo is private). Verify the sha256. Unzip
the whole directory so `kit/world/df8c55dabb35/manifest.json` exists.*

Thư mục phải trông như `kit/world/<world_id>/manifest.json`. Nếu `make doctor`
vẫn báo thiếu, bạn giải nén lồng sai một cấp.

*The corpus (~12 MB) ships separately from your instructor — it is deliberately
not committed. Unzip the whole `<world_id>/` directory into `kit/world/` so that
`kit/world/<world_id>/manifest.json` exists, then re-run `make doctor`. If it
still reports a missing world, you have one directory level too many.*

> Bộ world của sinh viên **không bao giờ chứa `truth.json`** — `make doctor` sẽ
> từ chối chạy nếu thấy nó. Nếu file đó có trong bản bạn nhận được, báo giảng
> viên ngay: bạn đang cầm bản của giám khảo.
>
> *A student world never contains `truth.json`, and `make doctor` refuses to run
> if it finds one. If yours has it, tell your instructor — you have the
> referee's copy.*

**Không cần API key.** `spar.py` chạy hoàn toàn trên `MockBroker`. Bộ kit này **không chứa HTTP
client nào có thể gọi tới model endpoint** — đó là một tính chất công bằng, không phải chỉ là vệ
sinh: model không bao giờ lọt vào đường chấm điểm mà sinh viên nhìn thấy.

*No API key needed, ever. The kit contains no HTTP client that can reach a model endpoint.*

---

## 3. Ba con bot · The three bots

|                  | Bot           | Bạn học được gì                                                                                                                                                                                              |
| ---------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **EASY**   | `rookie`    | Tin mọi thứ, luôn`mcp list`, không guardrail. **Thua Rookie nghĩa là bạn có bug, không phải thiếu chiến thuật.**                                                                              |
| **MEDIUM** | `operator`  | Đọc được, và là**artifact dạy nhiều nhất trong kit — vì lỗi của nó là những lỗi hợp lý**. Nó pin và diff, truyền `traceparent` nhưng không verify, nhầm identity với authority. |
| **HARD**   | `adversary` | Bốn lớp kiểm tra identity, pin liên tục, ghi exactly-once, kỷ luật`mcp search`. **Bạn học nó bằng cách dò — đó chính là kỹ năng trận đấu chấm điểm.**                            |

```bash
make spar BOT=operator AS=defender     # bạn thủ
make spar BOT=adversary AS=prosecutor  # bạn luận tội
make spar BOT=operator AS=all          # cả ba nhiệm vụ
```

---

## 4. Thế giới nói dối như thế nào · How the world lies

Ba áp lực này **có thật và đã đo được**, không phải dựng lên (`CORPUS-FACTS.md` trong repo giảng viên):

- **`day18` thực sự lệch giữa hai replica** — 45 content frame ở bản working, 31 ở canonical,
  cùng track, cùng subtitle. `replica_flip` là hành vi tự nhiên của kho, không phải lỗi tiêm vào.
- **Số ngày KHÔNG phải khoá bền** — `day01..day12` mỗi số xuất hiện ở 2–3 file canonical, và
  đúng 15 file Track-3 mâu thuẫn với chính subtitle của nó. Hỏi "ngày nào dạy X?" là một bài toán
  **phân giải**, không phải tra cứu.
- **Kho tài liệu lớn gấp ~7 lần một context window.** Truy vấn hẹp không phải sở thích — nó là
  thứ duy nhất chạy được.

---

## 5. Bạn bị chấm trên cái gì · What you are judged on

**17 lớp lỗi, 5 nhóm.** Trả lời đúng là chưa đủ: có thể đúng mà vẫn bịa citation, đúng mà vẫn lộ
dữ liệu, đúng mà vẫn đọc từ replica cũ.

| Nhóm                        | Các lớp                                                                                                                 |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **A · Hạ tầng**     | `enforcement_failure` 10 · `stale_read` 8 · `write_violation` 8 · `protocol_misuse` 6                          |
| **B · Sự thật**     | `wrong_answer` 8 · `fabricated_citation` 8 · `hallucination` 7 · `ungrounded` 5 · `unsupported_precision` 4 |
| **C · An toàn**      | `authority_exceeded` 10 · `guardrail_breach` 8 · `privacy_leak` 8                                                 |
| **D · Chất lượng** | `unflagged_conflict` 6 · `overreach` 5 · `incoherent` 4 · `non_responsive` 4                                   |
| **E · Kinh tế**      | `wasteful` 3                                                                                                            |

`enforcement_failure` và `authority_exceeded` nặng nhất vì đó là **luận đề của Ngày 26: cái hạ tầng
của bạn *thực thi*, không phải cái agent của bạn *nói*.**

---

## 6. Gọi tool khó ở chỗ nào · Why the tools are hard

Gọi đúng tool là chưa đủ — bạn phải gọi **cho đúng cách**. Tám cơ chế, mỗi cái lấy từ hành vi thật
của MCP/HTTP API:

|   | Cơ chế                                                                                                      | Nếu sai                                                                                     |
| - | ------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| 1 | **Giá động** `cost = base + Σ(field_weight) + rows × row_weight`                                 | `wasteful`; và nếu bạn bỏ field khỏi mask rồi lại trích dẫn nó → `ungrounded` |
| 2 | **Lease** — `get_frame` cần vé từ một `query` gần đó, sống được 3 lệnh                 | `protocol_misuse`. Cache frame ID từ vòng 2 rồi lấy ở vòng 7 = không được gì    |
| 3 | **Precondition** — mọi lệnh ghi cần `If-Match` etag từ `registry.provenance`                   | `409 conflict`; retry mà không đọc lại = `write_violation`                          |
| 4 | **Kết quả một phần** — `{"partial": true, "continuation": …}`                                   | coi partial là đủ =`protocol_misuse`                                                    |
| 5 | **Cửa sổ rate riêng từng tool** — `citation-checker` 2 lần / 3 vòng                            | `rate_limited`, **không hoàn tiền**                                               |
| 6 | **Lỗi mờ** — `{"code": "unavailable"}`, không lý do, không bao giờ có                         | phải đàm phán trên**sự kiện thất bại**, không phải lý do nó đưa ra      |
| 7 | **Đàm phán ngôn ngữ** — `lang` sai thì **âm thầm** trả về mục ngôn ngữ kia        | một câu trả lời sai với anchor trông rất hợp lệ                                     |
| 8 | **Phiên bản deprecated** — `slides.search` còn chạy, nhưng `slides.query` mới là kế nhiệm | `wasteful`                                                                                 |

**Quyết định chuyển từ "gọi tool nào" thành "gọi tool nào, với mask gì, theo thứ tự nào, cầm vé gì,
trong cửa sổ nào"** — và mỗi thứ trong đó là một lớp lỗi có thể bị luận tội.

---

## 7. Nộp bài · Submit

```bash
make test                    # bộ test công khai — đây là conformance check của bạn
make validate                # deck hợp lệ chưa? 14 lá, cân bằng lớp, lethality band
make submit TEAM=<tên-đội>   # đóng gói agent/ deck/ eval/ thành bundle đã seal
```

`make submit` **bắt buộc** có `TEAM=`. Bundle nằm ở `submissions/<tên-đội>.bundle`.

*`make submit` requires `TEAM=`. The sealed bundle lands in
`submissions/<team>.bundle`. `agent/` and `deck/` lock at session start;
`eval/` you keep working on.*

> `make qualify` đã bị bỏ. Nó gọi một `qualify.py` chưa từng được viết, ghi ra
> `submissions/radar.json` mà **không có gì đọc**. Dùng `make test`.
>
> *`make qualify` is retired — it called a `qualify.py` that was never written,
> producing a file nothing reads. `make test` is the conformance check.*

`make qualify` cho bạn **bốn trục độc lập** — Defence, Attack, Prosecution, Integrated — chấm trên
**cùng một bộ fixture** cho mọi đội. Đó mới là thứ đo được năng lực từng phần; bảng đấu đo cả hệ
thống dưới áp lực.

Đọc `RULES.md` trước khi nộp. Vi phạm hợp đồng = loại bài, không phải trừ điểm.
