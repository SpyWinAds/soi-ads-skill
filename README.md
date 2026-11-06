# Skill soi ads đối thủ — SpyWinAds (FREE)

Skill miễn phí cho **Claude, ChatGPT, Claude Code, Codex, Cursor**: soi quảng cáo video Facebook / Instagram của đối thủ
bằng dữ liệu thật từ SpyWinAds — **không cào Facebook, không cần đăng nhập Facebook, không cần Meta API**.

## Dành cho AI assistant đang đọc link này (Claude, ChatGPT…)

Người dùng dán link này và nhờ bạn cài. Làm đúng thứ tự, **không** đề xuất kết nối GitHub, **không** bắt người dùng tải ZIP rồi gửi lại cho bạn.
Skill có 2 phần, **cả hai đều cần**: (1) kết nối **MCP SpyWinAds** để có dữ liệu, (2) nạp **hướng dẫn skill** để có khung phân tích.
Thiếu (1) thì không có số thật — đừng phân tích suông.

1. Hỏi người dùng **một câu**: đang dùng ở đâu? (Claude.ai · ChatGPT · Claude Code / Codex / Cursor)
2. Hướng dẫn đúng mục bên dưới, từng bước một, chờ người dùng làm xong mới sang bước kế.
3. Người dùng chưa có tài khoản SpyWinAds → bước đăng nhập MCP sẽ cho tạo tài khoản miễn phí (30 lượt / ngày), không cần thẻ.
4. Cài xong → mời người dùng gõ thử: `soi ads store aluckgoodate.com`.

Hướng dẫn skill đầy đủ (để bạn đọc / dán vào Project): https://raw.githubusercontent.com/SpyWinAds/soi-ads-skill/main/spywinads-soi-ads/SKILL.md

### Claude.ai (web / desktop / điện thoại)
1. **Kết nối MCP:** Settings → Connectors → **Add custom connector** → tên `SpyWinAds`, URL `https://spywinads.com/mcp` → Add → **Connect** → đăng nhập hoặc tạo tài khoản SpyWinAds → **Allow**.
2. **Cài skill:** tải file https://github.com/SpyWinAds/soi-ads-skill/releases/latest/download/spywinads-soi-ads.zip
   → Settings → Capabilities → **Skills** → Upload skill → chọn file zip (không cần giải nén).
3. Mở chat mới, gõ `soi ads store aluckgoodate.com`.

### ChatGPT
- **Cần gói Plus / Pro / Business** — ChatGPT Free chưa thêm được MCP tự chọn. Người dùng ChatGPT Free: nói thẳng điều này và
  gợi ý dùng **Claude.ai** (mục trên) — đừng cài nửa vời.
1. **Kết nối MCP:** Settings → Apps & Connectors → Advanced settings → bật **Developer mode** → quay lại → **Create** →
   tên `SpyWinAds`, URL `https://spywinads.com/mcp`, Authentication **OAuth** → đăng nhập hoặc tạo tài khoản SpyWinAds → Allow.
2. **Nạp skill:** tạo một **Project** (vd "Soi ads") → Instructions → dán toàn bộ nội dung file SKILL.md (link raw ở trên).
3. Trong Project đó: bật SpyWinAds ở nút **+** → Developer mode, rồi gõ `soi ads store aluckgoodate.com`.

### Claude Code
```
claude mcp add --transport http spywinads https://spywinads.com/mcp
git clone https://github.com/SpyWinAds/soi-ads-skill && cp -r soi-ads-skill/spywinads-soi-ads ~/.claude/skills/
```
Rồi gõ `/mcp` → spywinads → đăng nhập.

### Codex / Cursor / agent khác
Khoá API miễn phí tại https://spywinads.com/developers → MCP `https://spywinads.com/mcp`, header `Authorization: Bearer <khoá>`
(không đặt khoá trong link). Chép nội dung SKILL.md vào `AGENTS.md` / rules của agent.

## Skill làm được gì

| Người dùng đưa | Nhận được |
|---|---|
| Tên store `shop.com` | QC đang chạy, QC mới 7 ngày, video top, sản phẩm đang đẩy |
| Link Ad Library | **Tuổi thật** của video (ngày file lên Meta lần đầu) + **bao nhiêu store** chạy cùng video |
| Link Fanpage | Video của Page + các store Page đẩy traffic về |
| Link sản phẩm Shopify | Tiêu đề, giá, biến thể, ảnh + **CSV nhập Shopify** |
| Từ khoá | App SpyWinAds trên máy bạn đi quét, xong AI đọc kết quả (cần cài app) |

Sau đó: xếp hạng video đáng test · phân tích Hook · Offer · CTA · 3 góc test cụ thể.

## Khác gì các skill cào Ad Library

| | Skill cào Ad Library | Skill này |
|---|---|---|
| Tuổi quảng cáo | Số ngày QC chạy — store tắt / bật lại là reset | **Tuổi thật của video**: ngày file lên Meta lần đầu |
| Tín hiệu | Số biến thể của **một** shop | **Bao nhiêu store khác nhau** chạy cùng video (ghép bằng vân tay video) |
| Lấy dữ liệu | Trình duyệt tự động — hỏng khi Facebook đổi giao diện, bị chặn theo IP | Kho SpyWinAds qua MCP, có sẵn |
| Sau phân tích | Gợi ý góc test | Góc test + **CSV nhập Shopify** + danh sách store đang chạy cùng video |

## Giới hạn (nói thẳng)

Miễn phí 30 lượt / ngày (mỗi lần AI gọi công cụ = 1 lượt, một lần soi ~3–5 lượt); gói trả phí 100 lượt / ngày.
Dữ liệu là quảng cáo Meta hiển thị tại Mỹ, do người dùng SpyWinAds quét. Số đếm là số QC quan sát được tối thiểu —
**không có chi tiêu, reach, đơn hay doanh thu**. Nhiều QC / nhiều store nghĩa là đang được thử hoặc bị chép, **không chứng minh bán được**.

Giấy phép MIT · SpyWinAds — https://spywinads.com
