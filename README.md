# Nội dung truyện trả phí

Thư mục này được đẩy lên **một repo GitHub public riêng** (app tải từ đó, không đóng gói vào app).

```
manifest.json                 # danh sách truyện trả phí (tên, bìa, thời lượng, productId)
stories/<id>/story.json       # nội dung truyện (câu, mốc thời gian, chạm tương tác)
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

Mỗi truyện là một sản phẩm **Non-Consumable** giá 1,99 $ trên App Store Connect / Google Play, với id `story.<id có dấu _>` (ví dụ `story.little_turtle`). Thêm các sản phẩm đó vào RevenueCat.
