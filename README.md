# Skill soi ads đối thủ — SpyWinAds (FREE)

Skill miễn phí cho **Claude Code, Codex, Claude.ai, ChatGPT, Cursor**: soi quảng cáo video Facebook / Instagram của đối thủ
bằng dữ liệu thật từ SpyWinAds — **không cào Facebook, không cần đăng nhập Facebook, không cần Meta API**.

## Khác gì các skill cào Ad Library

| | Skill cào Ad Library | Skill này |
|---|---|---|
| Tuổi quảng cáo | Số ngày QC chạy — store tắt / bật lại là reset | **Tuổi thật của video**: ngày file lên Meta lần đầu |
| Tín hiệu | Số biến thể của **một** shop | **Bao nhiêu store khác nhau** chạy cùng video (ghép bằng vân tay video) |
| Lấy dữ liệu | Trình duyệt tự động — hỏng khi Facebook đổi giao diện, bị chặn theo IP | Kho SpyWinAds qua MCP, có sẵn |
| Đầu vào | Từ khoá / Page ID | Store · Fanpage · link QC · link sản phẩm · từ khoá (qua app SpyWinAds) |
| Sau phân tích | Gợi ý góc test | Góc test + **CSV nhập Shopify** + danh sách store đang chạy cùng video |

## Cài đặt (2 phút)

1. **Tài khoản SpyWinAds miễn phí** (30 lượt / ngày): https://spywinads.com
2. **Kết nối MCP** `https://spywinads.com/mcp`
   - Claude Code: `claude mcp add --transport http spywinads https://spywinads.com/mcp` → gõ `/mcp` → đăng nhập
   - Claude.ai / Desktop: Customize → Connectors → Add custom connector → dán link
   - ChatGPT: Plugins → More → Add custom MCP server → dán link
   - Cursor / VS Code / Codex: khoá API tại https://spywinads.com/developers, header `Authorization: Bearer <khoá>`
3. **Cài skill**
   - Claude Code: chép thư mục `spywinads-soi-ads` vào `~/.claude/skills/`
   - Claude.ai (gói có Skills): nén thư mục `spywinads-soi-ads` thành zip rồi tải lên ở mục Skills trong cài đặt
   - Codex / agent khác: chép nội dung `SKILL.md` vào `AGENTS.md` hoặc thư mục skill của agent

## Dùng

> soi ads store aluckgoodate.com
> video này chạy bao lâu rồi, bao nhiêu shop đang chạy: https://www.facebook.com/ads/library/?id=...
> Fanpage này đang đẩy sản phẩm gì: https://www.facebook.com/...
> copy sản phẩm này sang Shopify: https://shop.com/products/...

Ví dụ thật (04/11/2026, store `aluckgoodate.com`): 377 QC đang chạy, 217 QC mới trong 7 ngày, 74 video, 58 sản phẩm.
Sản phẩm "Jesus Light of Heaven Christmas Wall Hanging" ($39,99, tiêu đề "Last Day – 60% OFF") có 6 video;
một video file lên Meta 24/09 đang được **21 store** chạy (50 store từng chạy, 128 Fanpage) → bản đang bị chép rộng nhất.

## Giới hạn (nói thẳng)

Dữ liệu là quảng cáo Meta hiển thị tại Mỹ, do người dùng SpyWinAds quét. Số đếm là số QC quan sát được tối thiểu —
**không có chi tiêu, reach, đơn hay doanh thu**. Nhiều QC / nhiều store nghĩa là đang được thử hoặc bị chép, **không chứng minh bán được**.

Giấy phép MIT · SpyWinAds — https://spywinads.com
