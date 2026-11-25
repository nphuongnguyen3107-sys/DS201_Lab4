# DS201 – Lab 4: Encoder–Decoder và Attention cho dịch máy Anh–Việt

Họ tên: Nguyễn Phương Nguyên – MSSV: 24521196

Dịch máy Anh → Việt trên small-PhoMT với ba mô hình Seq2Seq (encoder 3 lớp LSTM, decoder 3 lớp LSTM, hidden 256):

| Bài | Mô hình |
|---|---|
| 1 | Không attention |
| 2 | Bahdanau attention (Bahdanau et al., 2015) |
| 3 | Luong attention, global – general, có input feeding (Luong et al., 2015) |

Cấu hình chung: embedding 256, dropout 0.3, Adam lr 1e-3, batch 32, teacher forcing 0.5, tối đa 10 epoch, giữ checkpoint có ROUGE-L dev cao nhất; dùng toàn bộ train/dev/test sau khi lọc câu ≤ 50 từ (18.607 / 1.901 / 1.786 cặp). Độ đo chính: ROUGE-L F1 trên test.

Thí nghiệm thêm (không đổi cấu hình hay checkpoint của ba bài):
- ROUGE-L trên test chia theo độ dài câu nguồn (≤ 15, 16–30, 31–50 từ).
- Beam search (k = 5, chuẩn hóa độ dài α = 0.6) so với greedy.

## Cấu trúc

```
Lab4_assignment.ipynb   notebook chính (giữ nguyên output)
artifacts/              đồ thị, heatmap, bảng tổng hợp, bản dịch test của từng mô hình
report/                 báo cáo PDF
```

Thư mục `data/` và các checkpoint `.pt` không được đưa lên GitHub. Notebook tự tải dữ liệu từ link trong đề và kiểm tra SHA-256.

## Chạy lại

Mở `Lab4_assignment.ipynb` trên Google Colab, chọn GPU (Runtime → Change runtime type → T4 GPU), rồi Runtime → Run all.

## Kết quả

| Mô hình | ROUGE-L dev | ROUGE-L test | BLEU test | Số tham số | Giây/epoch |
|---|---|---|---|---|---|
| Bài 1. Không attention | 0.2298 | 0.2346 | 2.61 | 7,500,963 | 86.4 |
| Bài 2. Bahdanau | 0.2975 | 0.3138 | 5.83 | 8,025,763 | 147.1 |
| Bài 3. Luong | 0.2905 | 0.3054 | 5.52 | 7,959,971 | 147.4 |

Giây/epoch đo trên Google Colab, GPU Tesla T4. Beam search k = 5 nâng ROUGE-L test lên 0.2410 / 0.3253 / 0.3177.
