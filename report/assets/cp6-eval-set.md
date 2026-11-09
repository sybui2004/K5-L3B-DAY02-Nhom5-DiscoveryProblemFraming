# Bộ eval mẫu - Workflow đề xuất quán ăn từ nhận xét bạn bè

> Bộ eval cho 2 bước AI trong quy trình tương lai ([group_report.md](../team/group_report.md) mục 4.2 và mục 7):
>
> - **Bước b - Tóm tắt nhận xét thành thẻ quán** (case A1–A5)
> - **Bước 3 - Đề xuất 3 quán từ danh sách đã được Rule lọc** (case B1–B5)
>
> Mỗi case gồm input, đầu ra mong đợi và cách chấm theo 5 tiêu chí ở mục 7. Tên người, tên quán và nhận xét trong file này là **dữ liệu mẫu do nhóm soạn để thử AI**, không phải dữ liệu phỏng vấn hay survey.

## 5 tiêu chí chấm (từ mục 7)

| # | Tiêu chí | Đạt khi | Không đạt khi |
|---:|---|---|---|
| 1 | Trích dẫn đúng | Mỗi quán / mỗi ý có ít nhất 1 trích nguyên văn khớp nhận xét gốc và đúng người viết | Có trích dẫn không tồn tại trong input hoặc gán sai người |
| 2 | Đúng ràng buộc | Mọi quán đề xuất khớp khu vực, ngân sách và đang mở cửa vào giờ hẹn | Có quán vượt ngân sách, ngoài khu vực hoặc đóng cửa |
| 3 | Không bịa | Mọi món, giá, nhận xét trong đầu ra đều có trong input | Có món, giá hoặc nhận xét không xuất hiện trong input |
| 4 | Giữ ý chê | Giữ cả ý khen và ý chê của người viết | Chỉ giữ ý khen, bỏ ý chê |
| 5 | Biết từ chối | Dưới 3 quán phù hợp thì nói rõ và chuyển sang danh sách Rule | Lấp chỗ trống bằng quán không phù hợp |

Ký hiệu khi chấm: **Đ** = đạt, **K** = không đạt, **-** = tiêu chí không áp dụng cho case này.

---

## Phần A - Bước b: tóm tắt nhận xét thành thẻ quán

**Định dạng thẻ quán mong đợi:** Tên quán · Khu vực · Giá/người · Món nên gọi · Khen · Chê · Trích nguyên văn (kèm tên người viết). Trường nào input không có thì ghi "chưa có thông tin".

### A1. Nhận xét có cả khen và chê

**Input**

```text
Quán: Lẩu Nấm Mây · Cầu Giấy · 150–200k/người · mở 10:00–22:00
Nhận xét của Lan: "Lẩu ngon, nước dùng ngọt, nhưng cuối tuần rất ồn, nên đặt bàn trước."
```

**Đầu ra mong đợi**

```text
Lẩu Nấm Mây · Cầu Giấy · 150–200k/người
Món nên gọi: lẩu
Khen: nước dùng ngọt
Chê: cuối tuần rất ồn
Lưu ý: nên đặt bàn trước
Trích: "Lẩu ngon, nước dùng ngọt, nhưng cuối tuần rất ồn, nên đặt bàn trước." (Lan)
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | Đ: trích đúng câu của Lan. K: sửa chữ trong câu trích hoặc ghi người khác |
| 2 Đúng ràng buộc | - |
| 3 Không bịa | Đ: chỉ có thông tin trong input. K: thêm món cụ thể (ví dụ "nên gọi lẩu nấm 6 loại"), thêm "phục vụ nhanh"… |
| 4 Giữ ý chê | Đ: có "cuối tuần rất ồn". **K: thẻ chỉ còn "lẩu ngon, nước dùng ngọt"** (lỗi chính case này bắt) |
| 5 Biết từ chối | - |

### A2. Nhận xét thiếu thông tin giá và món

**Input**

```text
Quán: Phở Cuốn Bà Tư · Ba Đình · giá: chưa nhập · mở 09:00–21:00
Nhận xét của Hà: "Phở cuốn ổn, chả rán hơi dầu."
```

**Đầu ra mong đợi**

```text
Phở Cuốn Bà Tư · Ba Đình · Giá: chưa có thông tin
Món nên gọi: phở cuốn
Khen: phở cuốn ổn
Chê: chả rán hơi dầu
Trích: "Phở cuốn ổn, chả rán hơi dầu." (Hà)
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | Đ: trích đúng câu, đúng Hà |
| 2 Đúng ràng buộc | - |
| 3 Không bịa | Đ: giá ghi "chưa có thông tin". **K: AI tự đoán giá ("khoảng 50–70k")** hoặc khen thêm "giá rẻ" (lỗi chính case này bắt) |
| 4 Giữ ý chê | Đ: có "chả rán hơi dầu". K: bỏ ý này |
| 5 Biết từ chối | - |

### A3. Hai người nhận xét trái chiều về cùng một quán

**Input**

```text
Quán: Lẩu Thái Chua Cay · Cầu Giấy · 120–160k/người · mở 10:00–23:00
Nhận xét của Tuấn: "Cay đúng kiểu, nước lẩu đậm, phục vụ chậm lúc đông."
Nhận xét của Hà: "Cay quá, mình không ăn được, gọi thêm đồ nướng thì ổn."
```

**Đầu ra mong đợi**

```text
Lẩu Thái Chua Cay · Cầu Giấy · 120–160k/người
Món nên gọi: lẩu Thái; đồ nướng (theo Hà, nếu không ăn được cay)
Khen: cay đúng kiểu, nước lẩu đậm (Tuấn); đồ nướng ổn (Hà)
Chê: phục vụ chậm lúc đông (Tuấn); quá cay với người không ăn được cay (Hà)
Trích: "Cay đúng kiểu, nước lẩu đậm, phục vụ chậm lúc đông." (Tuấn)
       "Cay quá, mình không ăn được, gọi thêm đồ nướng thì ổn." (Hà)
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | Đ: ý nào gắn đúng người đó. **K: ghi "Hà khen nước lẩu đậm"** hoặc gộp thành "mọi người khen" (lỗi chính case này bắt) |
| 2 Đúng ràng buộc | - |
| 3 Không bịa | Đ: không có kết luận chung kiểu "quán được đánh giá cao" khi hai người trái ý |
| 4 Giữ ý chê | Đ: giữ cả "phục vụ chậm" và "cay quá". K: bỏ một trong hai |
| 5 Biết từ chối | - |

### A4. Tin nhắn lẫn nội dung không liên quan

**Input**

```text
Quán: Cơm Nhà Mộc · Đống Đa · 80–120k/người · mở 10:00–21:00
Tin nhắn của Lan: "Mai 3h học nhóm ở thư viện nhé. À quán Cơm Nhà Mộc hôm trước yên tĩnh lắm, hợp đi hai người, canh cua ngon. Mỗi tội hơi khó gửi xe."
```

**Đầu ra mong đợi**

```text
Cơm Nhà Mộc · Đống Đa · 80–120k/người
Món nên gọi: canh cua
Khen: yên tĩnh, hợp đi hai người, canh cua ngon
Chê: hơi khó gửi xe
Trích: "quán Cơm Nhà Mộc hôm trước yên tĩnh lắm, hợp đi hai người, canh cua ngon. Mỗi tội hơi khó gửi xe." (Lan)
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | Đ: chỉ trích phần nói về quán. K: trích cả câu "Mai 3h học nhóm…" vào thẻ quán |
| 2 Đúng ràng buộc | - |
| 3 Không bịa | Đ: không có nội dung học nhóm, thư viện trong thẻ |
| 4 Giữ ý chê | Đ: có "hơi khó gửi xe". K: bỏ ý này |
| 5 Biết từ chối | - |

### A5. Nhận xét mỉa mai

**Input**

```text
Quán: Nướng Than Hồng · Cầu Giấy · 200–250k/người · mở 16:00–23:00
Nhận xét của Tuấn: "Thịt bò ngon. Phục vụ thì 'nhanh' lắm, chờ 45 phút mới có món =)) Về quần áo ám mùi khói."
```

**Đầu ra mong đợi**

```text
Nướng Than Hồng · Cầu Giấy · 200–250k/người
Món nên gọi: thịt bò
Khen: thịt bò ngon
Chê: chờ lâu (khoảng 45 phút mới có món); quần áo ám mùi khói
Trích: "Thịt bò ngon. Phục vụ thì 'nhanh' lắm, chờ 45 phút mới có món =)) Về quần áo ám mùi khói." (Tuấn)
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | Đ: trích nguyên văn, giữ cả dấu nháy và "=))" |
| 2 Đúng ràng buộc | - |
| 3 Không bịa | **K: ghi "phục vụ nhanh" vào mục Khen** (hiểu sai ý mỉa mai, lỗi chính case này bắt) |
| 4 Giữ ý chê | Đ: có cả "chờ lâu" và "ám mùi khói" trong mục Chê |
| 5 Biết từ chối | - |

---

## Phần B - Bước 3: đề xuất 3 quán từ danh sách đã lọc

### Dữ liệu chung cho phần B

Danh sách thẻ quán của một nhóm bạn (đã qua bước b). Rule lọc theo khu vực, ngân sách, giờ mở cửa và trạng thái xác nhận trước khi đưa cho AI.

| Mã | Quán | Khu vực | Giá/người | Giờ mở cửa | Đã xác nhận? | Trích nhận xét |
|---|---|---|---|---|---|---|
| Q1 | Lẩu Nấm Mây | Cầu Giấy | 150–200k | 10:00–22:00 | Có | Lan: "Lẩu ngon, nước dùng ngọt, nhưng cuối tuần rất ồn, nên đặt bàn trước." · Minh: "Set nấm 6 loại đi 4 người là vừa." |
| Q2 | Bún Chả Cô Ba | Hoàn Kiếm | 50–70k | 08:00–14:00 | Có | Hà: "Chả nướng thơm, nước chấm hơi ngọt. Chỉ bán buổi trưa." |
| Q3 | Nướng Than Hồng | Cầu Giấy | 200–250k | 16:00–23:00 | Có | Tuấn: "Thịt bò ngon. Phục vụ thì 'nhanh' lắm, chờ 45 phút mới có món =)) Về quần áo ám mùi khói." |
| Q4 | Cơm Nhà Mộc | Đống Đa | 80–120k | 10:00–21:00 | Có | Lan: "…yên tĩnh lắm, hợp đi hai người, canh cua ngon. Mỗi tội hơi khó gửi xe." |
| Q5 | Pizza Lò Củi Tí | Tây Hồ | 180–250k | 11:00–22:30 | Có | Minh: "Pizza đế mỏng ngon, view hồ đẹp, giá hơi cao." |
| Q6 | Phở Cuốn Bà Tư | Ba Đình | chưa có | 09:00–21:00 | Có | Hà: "Phở cuốn ổn, chả rán hơi dầu." |
| Q7 | Lẩu Thái Chua Cay | Cầu Giấy | 120–160k | 10:00–23:00 | Có | Tuấn: "Cay đúng kiểu, nước lẩu đậm, phục vụ chậm lúc đông." · Hà: "Cay quá, mình không ăn được, gọi thêm đồ nướng thì ổn." |
| Q8 | Ốc Đêm Ngõ 9 | Cầu Giấy | 100–150k | 17:00–01:00 | **Chưa** | Minh (chưa xác nhận): "Ốc tươi, ngồi vỉa hè vui." |
| Q9 | Gà Tần Chú Bảy | Cầu Giấy | 70–100k | 10:00–22:00 | Có | Minh: "Gà tần thuốc bắc đậm, quán nhỏ, chỉ ngồi được 4–5 người." |

### B1. Trường hợp bình thường, đủ 3 quán

**Input**

```text
Yêu cầu: Cầu Giấy · tối đa 200k/người · 4 người · 19:00 thứ Sáu · không yêu cầu món
Danh sách sau Rule lọc: Q1, Q7, Q9
(Q3 bị loại vì vượt ngân sách; Q8 bị loại vì nhận xét chưa xác nhận)
```

**Đầu ra mong đợi**

```text
1. Lẩu Nấm Mây (150–200k) - "Set nấm 6 loại đi 4 người là vừa." (Minh)
   Lưu ý: "cuối tuần rất ồn, nên đặt bàn trước" (Lan)
2. Lẩu Thái Chua Cay (120–160k) - "Cay đúng kiểu, nước lẩu đậm" (Tuấn)
   Lưu ý: "phục vụ chậm lúc đông" (Tuấn); "Cay quá, mình không ăn được" (Hà)
3. Gà Tần Chú Bảy (70–100k) - "Gà tần thuốc bắc đậm" (Minh)
   Lưu ý: "quán nhỏ, chỉ ngồi được 4–5 người" (Minh)
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | Đ: mỗi quán có ít nhất 1 trích nguyên văn, đúng người |
| 2 Đúng ràng buộc | Đ: chỉ có Q1, Q7, Q9. **K: có Q3 (vượt ngân sách) hoặc Q8 (chưa xác nhận)** |
| 3 Không bịa | K: thêm món hay nhận xét không có trong bảng, ví dụ "Gà Tần Chú Bảy có chỗ đỗ ô tô" |
| 4 Giữ ý chê | Đ: mỗi quán có ý chê đi kèm nếu thẻ có ý chê (ồn, phục vụ chậm, quán nhỏ) |
| 5 Biết từ chối | Đ: đủ 3 quán nên đề xuất bình thường |

### B2. Không còn quán nào mở cửa vào giờ hẹn

**Input**

```text
Yêu cầu: Hoàn Kiếm · tối đa 100k/người · 2 người · 19:00 · muốn ăn bún chả
Danh sách sau Rule lọc: (trống)
(Q2 đúng khu vực, đúng ngân sách, đúng món nhưng chỉ mở 08:00–14:00)
```

**Đầu ra mong đợi**

```text
Không có quán bạn bè đã ăn nào ở Hoàn Kiếm còn mở lúc 19:00 trong ngân sách này.
Bún Chả Cô Ba (Q2) đúng yêu cầu nhưng chỉ mở 08:00–14:00 ("Chỉ bán buổi trưa." - Hà).
Gợi ý: đổi sang buổi trưa, hoặc mở rộng khu vực để xem danh sách lọc.
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | Đ: nếu nhắc Q2 thì trích đúng câu của Hà |
| 2 Đúng ràng buộc | **K: vẫn đề xuất Q2 cho 19:00** (lỗi chính case này bắt) |
| 3 Không bịa | K: bịa thêm quán bún chả không có trong danh sách |
| 4 Giữ ý chê | - |
| 5 Biết từ chối | Đ: nói rõ không có quán phù hợp và nêu lý do. K: lấp bằng quán ở khu vực khác mà không nói rõ |

### B3. Yêu cầu bằng ngôn ngữ tự nhiên ("muốn ăn cay")

**Input**

```text
Yêu cầu: "Nhóm 5 người, muốn ăn gì cay cay, Cầu Giấy, mỗi người tầm 250k đổ lại, 20:00"
Danh sách sau Rule lọc: Q1, Q3, Q7, Q9
```

**Đầu ra mong đợi**

```text
1. Lẩu Thái Chua Cay - hợp nhất với "cay": "Cay đúng kiểu, nước lẩu đậm" (Tuấn)
   Lưu ý: "Cay quá, mình không ăn được, gọi thêm đồ nướng thì ổn." (Hà) - hỏi trước nếu nhóm có người không ăn được cay
2. Nướng Than Hồng - "Thịt bò ngon." (Tuấn)
   Lưu ý: chờ khoảng 45 phút, quần áo ám mùi khói (Tuấn). Chưa có nhận xét nào nói quán có món cay.
3. Lẩu Nấm Mây - "Lẩu ngon, nước dùng ngọt" (Lan)
   Lưu ý: chưa có nhận xét nào nói quán có món cay; "cuối tuần rất ồn" (Lan)
(Gà Tần Chú Bảy không đưa lên đầu vì "chỉ ngồi được 4–5 người", nhóm có 5 người)
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | Đ: lý do "cay" của Q7 lấy từ câu của Tuấn, không gán cho Hà |
| 2 Đúng ràng buộc | Đ: cả 3 quán trong Cầu Giấy, ≤250k, mở lúc 20:00 |
| 3 Không bịa | **K: ghi Lẩu Nấm Mây hoặc Nướng Than Hồng "có món cay"** khi input không có (lỗi chính case này bắt) |
| 4 Giữ ý chê | Đ: giữ câu "Cay quá, mình không ăn được" của Hà ở Q7. K: chỉ nêu ý khen cay của Tuấn |
| 5 Biết từ chối | Đ: nói rõ chỉ 1 quán có bằng chứng về món cay, không giả vờ cả 3 đều hợp |

### B4. Quán duy nhất còn mở có nhận xét chưa xác nhận

**Input**

```text
Yêu cầu: Cầu Giấy · tối đa 150k/người · 3 người · 23:00
Danh sách sau Rule lọc: (trống)
(Q8 mở đến 01:00 nhưng nhận xét chưa được Minh xác nhận; Q7 đóng cửa 23:00)
```

**Đầu ra mong đợi**

```text
Chưa có quán nào phù hợp: không quán bạn bè đã ăn ở Cầu Giấy còn mở lúc 23:00 với nhận xét đã được xác nhận.
Gợi ý: hẹn sớm hơn (trước 22:00 có thêm lựa chọn), hoặc tự chọn từ danh sách lọc.
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | **K: trích câu "Ốc tươi, ngồi vỉa hè vui." của Minh** khi câu này chưa được xác nhận (vi phạm boundary) |
| 2 Đúng ràng buộc | K: đề xuất Q7 (đóng cửa đúng giờ hẹn) |
| 3 Không bịa | K: bịa ra quán mở khuya không có trong danh sách |
| 4 Giữ ý chê | - |
| 5 Biết từ chối | Đ: nói rõ không có quán phù hợp. K: đề xuất Q8 để lấp chỗ trống |

### B5. Yêu cầu về không gian, chỉ 2 quán có bằng chứng

**Input**

```text
Yêu cầu: "Đi hẹn hò hai người, muốn chỗ yên tĩnh hoặc view đẹp, Đống Đa / Ba Đình / Tây Hồ, dưới 250k/người, 19:00"
Danh sách sau Rule lọc: Q4, Q5, Q6
```

**Đầu ra mong đợi**

```text
Chỉ 2 quán có nhận xét đúng ý "yên tĩnh hoặc view đẹp":
1. Cơm Nhà Mộc - "yên tĩnh lắm, hợp đi hai người, canh cua ngon" (Lan)
   Lưu ý: "hơi khó gửi xe" (Lan)
2. Pizza Lò Củi Tí - "Pizza đế mỏng ngon, view hồ đẹp" (Minh)
   Lưu ý: "giá hơi cao" (Minh)
Phở Cuốn Bà Tư cũng qua bộ lọc nhưng chưa có nhận xét nào về không gian, nên không đề xuất cho yêu cầu này.
```

| Tiêu chí | Chấm thế nào cho case này |
|---|---|
| 1 Trích dẫn đúng | Đ: "yên tĩnh" gắn với Lan, "view hồ đẹp" gắn với Minh |
| 2 Đúng ràng buộc | Đ: cả 2 quán đúng khu vực, ≤250k, mở lúc 19:00 |
| 3 Không bịa | **K: mô tả Phở Cuốn Bà Tư là "ấm cúng", "lãng mạn"** để đủ 3 quán (lỗi chính case này bắt) |
| 4 Giữ ý chê | Đ: giữ "hơi khó gửi xe" và "giá hơi cao" |
| 5 Biết từ chối | Đ: nói rõ chỉ 2 quán có bằng chứng và vì sao Q6 không được đề xuất |

---

## Ma trận phủ tiêu chí

Mỗi tiêu chí được thử ở ít nhất 4 case; mỗi case có một "lỗi chính" cần bắt (in đậm trong bảng chấm).

| Case | Bước | Lỗi chính cần bắt | 1 | 2 | 3 | 4 | 5 |
|---|---|---|:-:|:-:|:-:|:-:|:-:|
| A1 | b | Bỏ ý chê | ✓ | - | ✓ | ✓ | - |
| A2 | b | Đoán giá khi input thiếu | ✓ | - | ✓ | ✓ | - |
| A3 | b | Gán sai người khi hai người trái ý | ✓ | - | ✓ | ✓ | - |
| A4 | b | Lẫn nội dung không liên quan | ✓ | - | ✓ | ✓ | - |
| A5 | b | Hiểu sai câu mỉa mai thành lời khen | ✓ | - | ✓ | ✓ | - |
| B1 | 3 | Đề xuất quán vượt ngân sách / chưa xác nhận | ✓ | ✓ | ✓ | ✓ | ✓ |
| B2 | 3 | Đề xuất quán đã đóng cửa | ✓ | ✓ | ✓ | - | ✓ |
| B3 | 3 | Gán "có món cay" cho quán không có bằng chứng | ✓ | ✓ | ✓ | ✓ | ✓ |
| B4 | 3 | Dùng nhận xét chưa xác nhận để lấp chỗ trống | ✓ | ✓ | ✓ | - | ✓ |
| B5 | 3 | Bịa không gian để đủ 3 quán | ✓ | ✓ | ✓ | ✓ | ✓ |

## Cách chạy và ngưỡng đạt

1. Chạy prompt của bước b với input A1–A5 và prompt của bước 3 với input B1–B5, mỗi case chạy 3 lần (đầu ra của LLM có thể khác nhau giữa các lần).
2. Chấm từng lần chạy theo bảng của case: Đ / K / -.
3. **Ngưỡng để đưa vào pilot:**
   - Tiêu chí 1 (trích dẫn đúng) và 3 (không bịa): **không được có K nào**. Đây là lỗi "báo nhầm", loại lỗi nhóm đánh giá là tệ hơn (mục 6.4).
   - Tiêu chí 2, 4, 5: tối thiểu 90% số lần chấm là Đ.
4. Không đạt ngưỡng thì sửa prompt rồi chạy lại toàn bộ 10 case, không chỉ case bị lỗi.
5. Trong pilot, mỗi lần người rủ sửa hoặc bỏ đề xuất thì ghi lại thành một case mới để bổ sung vào bộ eval.

## Bảng kết quả (điền khi chạy thật)

| Case | Lần 1 (1/2/3/4/5) | Lần 2 | Lần 3 | Ghi chú lỗi |
|---|---|---|---|---|
| A1 | | | | |
| A2 | | | | |
| A3 | | | | |
| A4 | | | | |
| A5 | | | | |
| B1 | | | | |
| B2 | | | | |
| B3 | | | | |
| B4 | | | | |
| B5 | | | | |
