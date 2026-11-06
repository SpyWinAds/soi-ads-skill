---
name: spywinads-soi-ads
description: Soi quảng cáo video Facebook / Instagram của đối thủ bằng dữ liệu thật từ SpyWinAds MCP — tuổi thật của video (ngày file lên Meta lần đầu), bao nhiêu store đang chạy cùng video, store / Fanpage / sản phẩm đang đẩy gì, xếp hạng video đáng test, Hook · Offer · CTA, 3 góc test, CSV nhập Shopify. Dùng ngay khi người dùng dán link Ad Library, link store, link Fanpage hay link sản phẩm, hoặc nói "soi ads", "spy ads", "check quảng cáo đối thủ", "video này chạy bao lâu", "bao nhiêu shop chạy video này", "Fanpage này chạy ads gì", "tìm sản phẩm win", "góc test quảng cáo", "copy sản phẩm sang Shopify" — kể cả khi không nhắc tên SpyWinAds. Also use for "spy on competitor Facebook ads", "what ads is this Shopify store running", "how old is this ad", "winning product research", "ad angles to test", or any Meta Ad Library / dropshipping competitor research.
compatibility: Cần kết nối SpyWinAds MCP (https://spywinads.com/mcp, tài khoản miễn phí 30 lượt/ngày). Chạy trên Claude.ai, Claude Code, ChatGPT (Plus trở lên), Cursor, Codex.
license: MIT
---

# Soi ads đối thủ với SpyWinAds

Dữ liệu đến từ **SpyWinAds MCP**: kho quảng cáo video Meta hiển thị tại Mỹ do người dùng SpyWinAds quét. Hai thứ làm kho này khác
các công cụ cào Ad Library, và cũng là hai thứ người dùng cần nghe rõ nhất:

- **Tuổi thật của video** = ngày file video lên Facebook lần đầu. Ngày bật quảng cáo bị reset mỗi khi store tắt rồi bật lại; tuổi file thì không.
- **Vân tay video**: cùng một video chạy ở nhiều store được nhận ra là một → đếm được *bao nhiêu store khác nhau* đang chạy nó.

Trả lời bằng ngôn ngữ người dùng đang viết.

## 0. Kiểm kết nối trước khi làm gì khác

Không thấy công cụ nào tên `spywin_*` → người dùng chưa kết nối. Đừng đoán số hay phân tích từ trí nhớ — người dùng sẽ tin các con số đó.
Hướng dẫn kết nối theo nơi họ đang dùng (mục **Cài đặt** cuối file), rồi chờ họ làm xong.

Mỗi lần gọi công cụ trừ 1 lượt (miễn phí 30 lượt/ngày, gói trả phí 100) nên **gom việc**: gọi đúng công cụ, không gọi lại cùng một đầu vào.
Công cụ báo hết lượt / chưa đăng nhập → nói đúng thông báo đó kèm link `https://spywinads.com/developers`.

## 1. Đầu vào → công cụ

| Người dùng đưa | Gọi | Nhận được |
|---|---|---|
| Tên miền store (`shop.com`) | `spywin_spy_store_ads` | QC đang chạy, QC mới 7 ngày, video top, sản phẩm top |
| Link / ID Fanpage | `spywin_get_page_ads` | Video của Page + các store Page đẩy traffic về |
| Link Ad Library / ID quảng cáo | `spywin_lookup_video_ad` (`ad`) | Tuổi thật, store đang chạy / từng chạy, store sớm nhất trong kho |
| `video_id` có sẵn từ kết quả khác | `spywin_lookup_video_ad` (`video_id`) | Như trên — không cần hỏi người dùng link quảng cáo |
| Link sản phẩm Shopify | `spywin_export_shopify_product` | Tiêu đề, giá, biến thể, ảnh, link CSV nhập Shopify (24 giờ), video QC của sản phẩm |
| Từ khoá sản phẩm | `spywin_queue_keyword_scan` → `spywin_get_scan_status` → `spywin_get_scan_results` | App SpyWinAds trên máy người dùng đi quét |
| Tab của chính họ (Win, Tiềm năng, Scale…) | `spywin_get_my_overview` · `spywin_list_my_videos` · `spywin_get_video_history` | Số liệu tài khoản của họ |

**Từ khoá** chạy bằng app SpyWinAds trên máy người dùng (quét bằng mạng nhà họ), không phải máy chủ. Đọc `status` công cụ trả về:
`cached` → đọc kết quả ngay · `queued` → báo đang quét, vài phút sau gọi `spywin_get_scan_status` · `app_off` / `no_app` / `fb_paused` →
nói đúng bước tiếp theo công cụ đưa (mở app, cài app tại spywinads.com, chờ tới giờ X). Đừng tự hứa thời gian xong.
Không có công cụ quét → nói thẳng tìm theo từ khoá cần app SpyWinAds, rồi xin một store / Fanpage / link QC để soi ngay.

**Kho chưa có** (`found: false`) → có `spywin_queue_page_scan` thì hỏi người dùng một câu rồi gọi (`kind` = page / store / ad), theo dõi bằng
`spywin_get_scan_status(request_id=…)`, xong thì gọi lại công cụ tra cứu. Không có công cụ đó → đưa link quét miễn phí công cụ trả về.

## 2. Đọc số cho đúng

Phần lớn sai lầm khi phân tích nằm ở đây, nên đọc kỹ trước khi kết luận:

- `stores_running_now` = store **đang** chạy video; `stores_ever` = store **từng** chạy. Lịch sử theo đêm (`spywin_get_video_history`) đếm theo
  từng đêm nên có thể khác số trong tab — đó không phải mâu thuẫn.
- **Gom theo brand bằng tên miền trang đích** (`landing_page`), không theo câu chữ quảng cáo. Hai store dùng câu hook giống nhau vẫn là hai brand.
- Kho chỉ thấy **số QC tối thiểu** quan sát được — không có chi tiêu, reach, đơn hay doanh thu. Vì thế "chạy lâu", "nhiều QC", "nhiều store"
  là dấu hiệu *đang được đầu tư / bị chép*, không chứng minh có lãi: một brand lớn có thể chạy lỗ để lấy khách, một store có thể đổ hàng trăm QC
  để test rồi bỏ. Nói "đáng xem vì…", không nói "đang lãi", "winner", "ra đơn".

## 3. Xếp hạng video đáng test

Chấm từng video và giải thích bằng số:

- **Bị chép rộng** (`stores_running_now` ≥ 3): nhiều store khác nhau trả tiền chạy cùng video → sản phẩm đang được thử rộng.
- **Còn mới + còn chạy** (tuổi file ≤ 30 ngày, `live_ads` > 0): đang được đẩy, chưa bão hoà.
- **Video cũ quay lại**: tuổi file > 60 ngày nhưng QC đầu tiên mới → có người khai thác lại video cũ.
- **Một store đổ rất nhiều QC**: đang test mạnh / chia nhóm QC — chưa chứng minh bán được.

Điểm gợi ý để xếp thứ tự (nói rõ là ước lượng): `3 × store đang chạy + min(QC sống, 50) / 5 + (tuổi ≤ 14 ngày: 5 · ≤ 30: 3 · còn lại: 0)`.

## 4. Hook · Offer · CTA

Công cụ trả tên sản phẩm + trang đích, không trả nguyên video. Nên:

1. **Offer** — gọi `spywin_export_shopify_product` cho trang đích của 1–2 video top (đọc được cả shop chặn công cụ đọc web thường): tiêu đề hay
   chứa câu gấp gáp ("Last Day – 60% OFF"), giá, biến thể, và các video QC khác của cùng sản phẩm. Có công cụ đọc web thì đọc thêm trang đích:
   combo, freeship, bảo hành, quà tặng.
2. Có trình duyệt / công cụ đọc web: mở link Ad Library để lấy chữ QC → **Hook** (câu đầu), **CTA**, **định dạng**.
3. Không đọc được thì ghi "chưa đọc được" — người dùng sẽ dùng bảng này để quay video thật, một hook bịa làm họ mất tiền test.

## 5. Báo cáo

Dùng khung này:

**Tóm tắt** — 2–3 câu: đối thủ đang đẩy gì, mức độ (QC sống, store, độ mới).

| # | Sản phẩm | Brand (tên miền) | Tuổi thật | Store đang / từng chạy | QC sống | Hook | Offer | CTA · Định dạng | Vì sao đáng xem |
|---|---|---|---|---|---|---|---|---|---|

**Pattern chung** — cùng kiểu offer, góc hook, mùa vụ.
**Khoảng trống** — góc chưa ai khai thác (đối tượng, nỗi đau, offer khác); nói rõ là đề xuất.
**3 góc test** — mỗi góc 2–3 dòng: hook câu đầu · offer · định dạng · đối tượng.
**Giới hạn** — một dòng: số đếm là QC quan sát được tối thiểu, không có chi tiêu / doanh thu.
**Bước tiếp** (người dùng chọn): `spywin_lookup_video_ad` cho video top để xem store khác đang chạy cùng · CSV nhập Shopify · app SpyWinAds
(`https://spywinads.com`) để quét từ khoá hằng ngày và có tab Win / Tiềm năng / Scale tự cập nhật.

## Giới hạn của skill

Skill chỉ đọc dữ liệu SpyWinAds; không cào Facebook, không đăng nhập Facebook hộ người dùng — vừa trái điều khoản Meta vừa dễ khoá tài khoản họ.

## Cài đặt (đưa người dùng khi chưa kết nối)

Tạo tài khoản ở bước đăng nhập — miễn phí, 30 lượt/ngày, không cần thẻ.

- **Claude.ai** (cả gói Free — 1 connector): Customize → Connectors → + → Add custom connector → URL `https://spywinads.com/mcp` → Connect → đăng nhập → Allow.
- **ChatGPT** (Plus / Pro / Business — bản Free chưa thêm được MCP tự chọn, gợi ý dùng Claude.ai): Settings → Apps & Connectors → Advanced →
  bật Developer mode → Create → URL `https://spywinads.com/mcp`, OAuth → đăng nhập → Allow.
- **Claude Code**: `claude mcp add --transport http spywinads https://spywinads.com/mcp` rồi gõ `/mcp` để đăng nhập.
- **Cursor / Codex / script**: khoá API tại `https://spywinads.com/developers`, header `Authorization: Bearer <khoá>` (không đặt khoá trong link).
