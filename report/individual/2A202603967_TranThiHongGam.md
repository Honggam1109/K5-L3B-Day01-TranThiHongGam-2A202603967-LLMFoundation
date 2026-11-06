# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV:** 2A202603967
- **Họ và tên:** Trần Thị Hồng Gấm
- **SEED** (in ở ô đầu notebook):

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | ☐ |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | ☐ |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | ☐ |

## 2. Kết quả chính

*Chép bảng so sánh cuối Phần 5 (F1 test, F1 SUB 40, p95 ms/tin, USD/1k tin) của chính bạn.*

                                     f1  f1_SUB
1. BoW + LogReg (tự code)        0.905   0.919
2. BoW + MLP (deep learning)     0.950   1.000
3a. Encoder (MiniLM) + kNN       0.911   1.000
3b. Encoder (MiniLM) + LogReg    0.962   0.976
3c. Encoder (MiniLM) + MLP head  0.957   0.976
4a. LLM nhỏ zero-shot            0.976   0.976
4b. LLM lớn zero-shot            0.950   0.950
4c. LLM nhỏ few-shot             0.950   0.950

## 3. Dự đoán và thực tế

Dự đoán 1: LLM lớn sẽ có F1 cao hơn LLM nhỏ khoảng 0.05–0.10. Thực tế LLM nhỏ đạt 0.976, LLM lớn đạt 0.950. Điều này cho thấy model lớn hơn chưa chắc chính xác hơn.

Dự đoán 2: Few-shot giúp tăng F1. Thực tế F1 giảm từ 0.976 xuống 0.950 khi thêm ví dụ. Nguyên nhân có thể do các ví dụ chưa phù hợp.

Dự đoán 3: Không mô hình nào đáp ứng SLA 50 ms. Thực tế BoW + LogReg và BoW + MLP đều đáp ứng. Tôi đã đánh giá thấp tốc độ của các mô hình truyền thống.

## 4. Phân tích 50 case khó

BoW dễ nhầm các tin nhắn có từ khóa gây nhiễu vì chủ yếu dựa vào từ vựng. LLM có thể bị ảnh hưởng bởi prompt injection.

Hai ví dụ:
- Tin nhắn bình thường chứa từ "free" hoặc "win" có thể bị BoW phân loại nhầm thành spam.
- Tin nhắn spam chứa chỉ dẫn "Ignore previous instructions" có thể khiến LLM đưa ra nhãn sai.
## 5. Thử thách cá nhân

Cách làm: tạo 2 tin nhắn spam giả danh ngân hàng, thông báo trúng thưởng và 2 tin nhắn ham chứa từ "free", "win". Sau đó sử dụng hai mô hình để dự đoán và so sánh với nhãn thật.

Kết quả: Đã xây dựng code kiểm thử 4 tin nhắn. Chưa có kết quả dự đoán thực tế để xác định mô hình nào chính xác hơn.

Giải thích:BoW chủ yếu dựa vào từ khóa nên dễ nhầm lẫn. Embedding có khả năng hiểu ngữ cảnh tốt hơn, nhưng có thể gặp khó khăn vì mô hình được huấn luyện trên dữ liệu SMS tiếng Anh.

## 6. Phương án cho sản phẩm thật

*Với yêu cầu p95 < 50 ms/tin cho 10 triệu SMS/ngày: chọn phương án nào, dẫn số liệu nào, chấp nhận đánh đổi gì.*
chọn BoW + MLP vì có F1 test = 0.950, F1 SUB 40 = 1.000 và p95 khoảng 8.72 ms, đáp ứng SLA dưới 50 ms/tin.

Có thể kết hợp LLM để xử lý các tin nhắn khó phân loại. Cách này giúp cân bằng tốc độ, chất lượng và chi phí.

Đánh đổi: Cần kiểm chứng thêm trên dữ liệu thực tế và giám sát các trường hợp phân loại sai.

7. AI log và điều đã học

## 7. AI log và điều đã học

sử dụng AI để hỗ trợ viết code, sửa lỗi, giải thích kết quả và gợi ý cách làm. Sau đó, chạy thử và kiểm tra lại kết quả. Qua bài học, hiểu được mô hình lớn chưa chắc tốt hơn và cần cân nhắc độ chính xác, tốc độ, chi phí khi lựa chọn mô hình.

