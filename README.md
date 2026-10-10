# 25C02034_DataViz_Midterm

Trang web trực quan hóa tương tác cho bộ dữ liệu *Urban Retail Sales – HCMC 2025* (240 giao dịch).

## Cấu trúc

| File | Vai trò |
|---|---|
| `index.html` | Trang web: HTML, CSS và toàn bộ code D3.js |
| `data.js` | Dữ liệu CSV đã chuyển sang JSON (`window.SALES_DATA`); ô trống được chuyển thành `null` |
| `urban_retail_sales_hcmc_2025_240.csv` | Dữ liệu gốc, để tham khảo |

Thư viện D3 v7 được tải từ cdnjs, không cần build.

## Việc cần làm trước khi nộp

1. Thay `[Họ và tên]` và `[MSHV]` trong `index.html` bằng tên và MSHV của bạn (có 3 chỗ `[MSHV]` và 1 chỗ `[Họ và tên]`).
2. Đổi tên thư mục từ `MSHV_DataViz_Midterm` thành `<MSHV>_DataViz_Midterm`.
3. Deploy (chọn một cách):
   - **Netlify Drop** (dễ nhất): vào https://app.netlify.com/drop, kéo thả cả thư mục vào. Sau đó vào *Site settings → Change site name* và đặt tên `<mshv>-dataviz-midterm`.
   - **GitHub Pages**: tạo repo tên `<MSHV>_DataViz_Midterm`, upload 3 file, rồi vào *Settings → Pages → Deploy from branch → main / root*.
   - **Vercel**: `npx vercel` trong thư mục, hoặc import repo GitHub.
4. Mở link vừa deploy ở cửa sổ ẩn danh và kiểm tra biểu đồ hiện đầy đủ.
5. Tạo file `<MSHV>.pdf` gồm họ tên, MSHV và URL (để link bấm được), rồi nộp lên Moodle.

## Chạy thử trên máy

Có thể mở thẳng `index.html` bằng trình duyệt (cần có mạng để tải D3 và font). Hoặc chạy qua server:

```bash
python3 -m http.server 8000
```

Sau đó mở http://localhost:8000

---

## Ghi chú để bảo vệ (giải thích bằng lời của bạn)

### 6 insight và lý do chọn biểu đồ

| # | Insight | Biểu đồ | Vì sao chọn |
|---|---|---|---|
| 1 | Electronics doanh thu cao nhất (63.8M) nhưng margin thấp nhất (15.3%); Fashion lãi nhiều nhất (17.4M, 33.6%) | Grouped bar (revenue và profit), có nút chuyển sang margin | So sánh 2 đại lượng trên 5 nhóm rời rạc → bar là phù hợp nhất |
| 2 | District 1 doanh thu cao nhất nhưng margin thấp nhất (21.5%) vì 53% doanh thu là Electronics | Stacked bar (có chế độ %) và heatmap store × category | Stacked bar cho thấy *cơ cấu*; heatmap cho thấy margin trong từng ô giống các store khác, tức nguyên nhân là mix sản phẩm chứ không phải vận hành |
| 3 | Discount càng cao margin càng giảm (34% → −1%), r = −0.71 | Scatter (mỗi giao dịch) và bar theo mức discount | Scatter thể hiện quan hệ giữa 2 biến số và outlier; bar giúp đọc trung bình từng mức |
| 4 | Cả 6 giao dịch lỗ đều có discount ≥ 25% trên hàng margin thấp (4 Electronics, 2 Protein Drink) | Parallel coordinates có brushing | Cần nhìn 7 biến cùng lúc để thấy "chân dung" chung của nhóm bất thường |
| 5 | Giao hàng càng lâu rating càng thấp (4.51 → 3.63), r = −0.68; Thu Duc giao chậm nhất nên rating thấp nhất | Strip plot và đường trung bình; 100% stacked bar theo store | Strip plot thấy được phân bố thật chứ không chỉ trung bình; 100% stacked so sánh tỉ lệ giữa các store |
| 6 | Q4 bán chạy nhất (60.5M) nhưng tháng 12 discount trung bình 17.6% nên margin chỉ còn 20.2% | Stacked bar theo tháng và line chart (margin, discount) | Dữ liệu theo thời gian → line; cố ý tách 2 panel để không dùng biểu đồ 2 trục y (dual-axis gây hiểu sai) |

### Công thức quan trọng (insight 3–4)

- Margin = Profit ÷ Revenue. Margin của nhóm = Σprofit ÷ Σrevenue (không lấy trung bình các margin).
- Nếu `c` = Cost ÷ (Quantity × Unit_Price), tức tỉ lệ giá vốn trên giá niêm yết, thì margin khi giảm giá `d` là **1 − c/(1 − d)**.
  - Điểm hòa vốn: **d = 1 − c**.
  - Electronics có c ≈ 0.75 → hòa vốn ở d ≈ 25%. Food & Beverage có c ≈ 0.68 → d ≈ 32%. Fashion có c ≈ 0.58 → d ≈ 42%.
  - Các đường cong "model" trên scatter plot chính là công thức này.

### Tương tác có trên trang

- Thanh filter dính trên đầu trang (store, category, customer, khoảng tháng). Mọi biểu đồ và KPI cập nhật theo filter.
- Click vào cột category (Insight 1), store (Insight 2, 5) hoặc tháng (Insight 6) để lọc toàn trang (coordinated views).
- Hover lên legend category để làm nổi category đó trên mọi biểu đồ.
- Brushing trên scatter (Insight 3) và trên các trục parallel coordinates (Insight 4). Hai biểu đồ được liên kết với nhau và với bảng Explorer.
- Nút "Select the losses" chọn nhanh 6 giao dịch lỗ.
- Tooltip trên mọi điểm, cột và ô.
- Toggle Revenue/Margin và Absolute/% để đổi chế độ xem.
- Bảng Explorer sort được theo từng cột (details on demand).

### Xử lý dữ liệu

- Có 10 ô trống: 6 ở Customer_Rating, 4 ở Delivery_Days. Các dòng này chỉ bị loại khỏi phép tính trung bình rating và delivery, vẫn được tính vào doanh thu và lợi nhuận.
- Đã kiểm tra: Revenue = Qty × Price × (1 − Discount) và Profit = Revenue − Cost đúng với cả 240 dòng.
- Hạn chế: mẫu nhỏ; một số ô store × category có n < 5.
