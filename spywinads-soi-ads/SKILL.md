---
name: spywinads-soi-ads
description: Soi quảng cáo video Facebook / Instagram của đối thủ bằng SpyWinAds MCP — tuổi THẬT của video (ngày file lên Meta lần đầu), bao nhiêu store đang chạy cùng video, store / Fanpage / sản phẩm đang đẩy gì, xếp hạng video đáng test, phân tích Hook · Offer · CTA, đề xuất 3 góc test và xuất CSV sản phẩm sang Shopify. Dùng khi nói "soi ads đối thủ", "spy ads", "check quảng cáo store này", "video này chạy bao lâu rồi", "bao nhiêu shop chạy video này", "Fanpage này chạy ads gì", "tìm sản phẩm win", "phân tích ads đối thủ", "góc test quảng cáo", "spy Facebook ads", "winning product", "copy sản phẩm Shopify", hoặc khi người dùng dán link Ad Library / link store / link Fanpage / link sản phẩm.
---

# Soi ads đối thủ với SpyWinAds

Skill miễn phí. Dữ liệu lấy từ **SpyWinAds MCP** (`https://spywinads.com/mcp`): kho quảng cáo video Meta tại Mỹ do người dùng SpyWinAds quét,
đã ghép **vân tay video** (cùng một video ở nhiều store được nhận ra là một) và **tuổi thật** (ngày file video lên Facebook lần đầu,
không phải ngày bật quảng cáo — store tắt rồi bật lại quảng cáo thì ngày chạy bị reset, tuổi file thì không).

## 0. Kiểm kết nối trước

- Có công cụ tên `spywin_*` không? Không có → dừng, hướng dẫn người dùng kết nối (mục **Cài đặt** ở cuối), không đoán số.
- Gói miễn phí: 30 lượt / ngày, 20 lượt / phút. Mỗi lần gọi công cụ là 1 lượt — **gom việc**, đừng gọi thừa.
- Công cụ báo hết lượt / chưa đăng nhập → nói đúng thông báo đó, kèm link `https://spywinads.com/developers`.

## 1. Đầu vào → công cụ

| Người dùng đưa | Gọi | Ghi chú |
|---|---|---|
| Tên miền store (`shop.com`) | `spywin_spy_store_ads` | QC đang chạy, QC mới 7 ngày, top video + sản phẩm |
| Link / ID Fanpage | `spywin_get_page_ads` | Video của Page + các store Page đẩy traffic về |
| Link Ad Library / ID quảng cáo | `spywin_lookup_video_ad` | Tuổi thật, số store chạy cùng video (đang chạy / từng chạy), store sớm nhất trong kho |
| Link sản phẩm Shopify | `spywin_export_shopify_product` | Tiêu đề, giá, biến thể, ảnh + link CSV nhập Shopify (24 giờ) + video QC của sản phẩm nếu kho có |
| **Từ khoá** sản phẩm | `spywin_queue_keyword_scan` → `spywin_get_scan_status` → `spywin_get_scan_results` | Chỉ có khi tài khoản bật quét qua AI + app SpyWinAds đang mở trên máy người dùng. Không có công cụ này → nói thẳng: "Tìm theo từ khoá cần app SpyWinAds (quét bằng mạng nhà bạn) — tải tại spywinads.com", rồi xin một store / Fanpage / link QC để soi ngay |
| Tab của chính họ (Win, Tiềm năng…) | `spywin_list_my_videos` / `spywin_get_my_overview` | Khi tài khoản có công cụ đọc tài khoản |

**Kho chưa có** (`found: false`) Page / store / QC → có `spywin_queue_page_scan` thì hỏi người dùng một câu rồi gọi
(`kind` = page / store / ad), sau đó theo dõi bằng `spywin_get_scan_status(request_id=…)`, xong thì gọi lại công cụ tra cứu.
Không có công cụ đó → đưa link quét miễn phí mà công cụ tra cứu trả về, và nói: cài app SpyWinAds để máy bạn quét ngay.

Từ khoá: `status` = `cached` → đọc kết quả ngay · `queued` → báo đang quét, hỏi lại sau vài phút bằng `spywin_get_scan_status` ·
`app_off` / `no_app` / `fb_paused` → nói đúng bước tiếp theo công cụ trả về (mở app, cài app, chờ tới giờ X). Không tự hứa thời gian.

## 2. Xếp hạng video đáng test

Từ các video công cụ trả về, chấm từng video và giải thích bằng số — **không** gọi video nào là "winner", "ra đơn", "lãi":

- **Bị chép nhiều** (`stores_running_now` ≥ 3): nhiều store khác nhau đang trả tiền chạy cùng video → sản phẩm đang được thử rộng.
- **Còn mới** (tuổi file ≤ 30 ngày) + **QC còn chạy** (`live_ads` > 0): đang được đẩy, chưa bão hoà.
- **Video cũ quay lại**: tuổi file cũ (> 60 ngày) nhưng QC đầu tiên mới → có người đang khai thác lại video cũ.
- **Một store đổ rất nhiều QC** (vd hàng trăm QC sống, 1 store): đang test mạnh / chia nhỏ nhóm QC — chưa chứng minh bán được.

Điểm gợi ý (để xếp thứ tự, nói rõ là ước lượng): `3 × store đang chạy + min(QC sống, 50) / 5 + (tuổi ≤ 14 ngày: 5 · ≤ 30: 3 · còn lại: 0)`.
Luôn kèm câu giới hạn: **số đếm là số QC quan sát được tối thiểu, không có chi tiêu, đơn hay doanh thu**.

## 3. Phân tích Hook · Offer · CTA

Công cụ trả **tên sản phẩm + trang đích**, không trả nguyên chữ / video quảng cáo. Vì vậy:

1. **Offer** — gọi `spywin_export_shopify_product` cho trang đích của video top 1–2 (shop hay chặn công cụ đọc web thường,
   công cụ này vẫn đọc được): tiêu đề thường chứa câu gấp gáp ("Last Day – 60% OFF"), giá, số biến thể, **và mọi video QC khác
   của cùng sản phẩm** (một sản phẩm thường có nhiều video — video có nhiều store chạy nhất là bản đang được chép).
   Có công cụ đọc web thì đọc thêm trang đích: combo mua nhiều, freeship, bảo hành, quà tặng.
2. Có trình duyệt / công cụ đọc web: mở link Ad Library của video để lấy chữ QC → **Hook** (câu đầu), **CTA**, **định dạng**.
3. Không đọc được → ghi "chưa đọc được", **không bịa**.

Bảng ra (mỗi video top 3–5):

| # | Sản phẩm | Tuổi thật | Store đang chạy / từng chạy | QC sống | Hook | Offer | CTA · Định dạng | Vì sao đáng xem |
|---|---|---|---|---|---|---|---|---|

## 4. Tổng hợp + 3 góc test

- **Pattern chung** giữa các video / store (cùng kiểu offer, cùng góc hook, cùng mùa vụ).
- **Khoảng trống**: góc chưa ai khai thác (đối tượng khác, nỗi đau khác, offer khác) — suy từ dữ liệu trên, nói rõ là đề xuất.
- **3 góc test** cụ thể: hook câu đầu · offer · định dạng video · đối tượng. Mỗi góc 2–3 dòng.

## 5. Bước tiếp theo (đề xuất ngắn, người dùng chọn)

- `spywin_lookup_video_ad` cho video top → xem các store khác đang chạy cùng video, soi tiếp store sớm nhất.
- `spywin_export_shopify_product` cho trang đích → CSV nhập Shopify.
- Muốn quét từ khoá tự động + tab Win / Tiềm năng / Scale cập nhật hằng ngày: app SpyWinAds (`https://spywinads.com`).

## Không làm

- Không bịa số, không suy doanh thu / lợi nhuận, không gọi "winner chắc chắn".
- Không gọi công cụ lặp cho cùng một đầu vào trong một lượt (tốn hạn mức).
- Không hướng dẫn cào Facebook, không đăng nhập Facebook hộ người dùng.

## Cài đặt (đưa người dùng khi chưa kết nối)

1. Tạo tài khoản miễn phí: `https://spywinads.com` (30 lượt / ngày).
2. Kết nối MCP `https://spywinads.com/mcp`:
   - **Claude Code**: `claude mcp add --transport http spywinads https://spywinads.com/mcp` rồi gõ `/mcp` để đăng nhập.
   - **Claude.ai / Claude Desktop**: Customize → Connectors → Add custom connector → dán link → "Sign in when needed".
   - **ChatGPT**: Plugins → More → Add custom MCP server → dán link.
   - **Cursor / VS Code / Codex / script**: khoá API ở `https://spywinads.com/developers`, gửi qua header `Authorization: Bearer <khoá>` (không đặt khoá trong link).
