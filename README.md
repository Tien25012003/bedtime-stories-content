# Nội dung truyện trả phí

Thư mục này được đẩy lên **một repo GitHub public riêng** (app tải từ đó, không đóng gói vào app).

```
manifest.json                 # danh sách truyện trả phí (tên, bìa, thời lượng, productId)
manifest.en.json              # danh sách truyện tiếng Anh
stories/<id>/story.json       # nội dung truyện (câu, mốc thời gian, chạm tương tác)
stories/<id>/story.en.json    # bản tiếng Anh (cùng tranh, mốc thời gian và giọng đọc riêng)
stories/<id>/scene-N.jpg      # tranh
stories/<id>/<nhân-vật>.jpg   # avatar nhân vật
stories/<id>/narration-N-<giọng>.m4a
```

## Đưa lên GitHub
```bash
cd remote-content
git init && git add . && git commit -m "Paid stories"
git branch -M main
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```
Sau đó sửa `CONTENT_BASE_URL` trong `src/config.ts` thành `https://raw.githubusercontent.com/<user>/<repo>/main/`.

## Thêm / sửa truyện
1. Viết truyện trong `scripts/content/stories.mjs` (`free: false`).
2. `node scripts/gen-art.mjs <id>`: vẽ tranh.
3. `node scripts/tts.mjs minh <id>` (tạo mốc thời gian), rồi `tu`, `ut`, `female`.
4. `node scripts/build-story.mjs`, rồi commit và push thư mục này.

## Bản tiếng Anh
1. Viết lời tiếng Anh trong `scripts/content/stories.en.mjs` (mỗi cảnh đúng số câu như bản Việt, cùng thứ tự).
2. Truyện mới: `node scripts/tts.mjs ava` trước (tạo mốc thời gian `timings.en.json`, audio không dùng). Rồi các giọng: `max` (edge, không giới hạn), `luna`, `ivy`, `leo` (Gemini, khoảng 10 lượt/ngày mỗi model, chạy lại để làm tiếp).
3. `node scripts/build-story.mjs` tạo `story.en.json` + `manifest.en.json`, rồi commit và push.

## Mua truyện bằng coin
Truyện có `productId` là truyện trả phí, mở bằng 20 coin (`STORY_COINS` trong `src/config.ts`). `productId` chỉ là khoá đánh dấu, **không** cần tạo sản phẩm trên cửa hàng cho từng truyện.

Coin bán theo 3 gói, sản phẩm **Consumable** trên App Store Connect / Google Play, thêm vào RevenueCat (`COIN_PACKS` trong `src/config.ts`):

| id | Gói | Coin | Giá |
|---|---|---|---|
| `coins_basic` | Basic | 40 | 3,99 $ (99.000đ) |
| `coins_plus` | Plus | 100 + 20 tặng | 9,99 $ (249.000đ) |
| `coins_premium` | Premium | 200 + 100 tặng | 19,99 $ (499.000đ) |

Sau đó dán public SDK key của RevenueCat vào `REVENUECAT_IOS_KEY` / `REVENUECAT_ANDROID_KEY`. Key trống thì app chạy chế độ thử (bấm mua là cộng coin ngay).
