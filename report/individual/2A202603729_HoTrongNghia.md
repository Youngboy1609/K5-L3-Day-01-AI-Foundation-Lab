# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV:*2A202603729*
- **Họ và tên:*Hồ Trọng Nghĩa*
- **SEED** (in ở ô đầu notebook): `8296`

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | ☑ |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | ☑ |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | ☑ |

## 2. Kết quả chính

| Mô hình | F1 test | F1 SUB 40 | p95 (ms/tin) | USD/1k |
| --- | ---: | ---: | ---: | ---: |
| BoW + LogReg | 0.931 | 0.857 | 0.138 | 0 |
| BoW + MLP | 0.980 | 0.947 | 0.405 | 0 |
| Embedding + kNN | 0.933 | 0.974 | 58.910 | 0 |
| Embedding + LogReg | 0.975 | 1.000 | 36.975 | 0 |
| Embedding + MLP head | 0.966 | 1.000 | 33.590 | 0 |
| LLM nhỏ zero-shot | 0.000 | 0.000 | 1402.289 | 0.007780 |
| LLM lớn zero-shot | 0.000 | 0.000 | 1550.583 | 0.077800 |
| LLM nhỏ few-shot | 0.000 | 0.000 | 1090.621 | 0.020045 |

Notebook chạy với `SEED=8296`; bộ kiểm tra đạt `14/15`. LLM đã chạy và có số đo chi phí, nhưng check `4.1 + 4.4 call_openai / llm_classify_batch` chưa đạt; vì vậy F1 LLM bằng 0 cần được kiểm tra lại output/parse trước khi dùng làm kết luận chất lượng.

## 3. Dự đoán và thực tế

Trong thử thách cá nhân, BoW đúng 3/4 tin và Embedding + LogReg cũng đúng 3/4. BoW nhầm tin yêu cầu đăng nhập thành ham; Embedding + LogReg nhầm tin giao hàng thành ham. Điều này cho thấy cả hai mô hình vẫn cần dữ liệu đủ đa dạng cho các mẫu lừa đảo tự nhiên.

## 4. Phân tích 50 case khó

Kết quả notebook cho thấy các mô hình embedding đạt F1 SUB cao hơn F1 test, trong khi BoW + LogReg giảm từ 0.931 xuống 0.857 trên SUB. Các kết quả LLM đều có F1 bằng 0 dù đã có chi phí và độ trễ, nên cần kiểm tra check 4.1/4.4 và output thô trước khi kết luận LLM kém. Cần thận trọng khi diễn giải vì SUB chỉ có 40 tin; stress test 50 case mới là nơi đánh giá các kiểu diễn đạt khác phân phối.

## 5. Thử thách cá nhân

Đề của MSSV này là **Embedding #5**. Bốn tin tự viết cho kết quả: BoW đúng 3/4, Embedding + LogReg đúng 3/4; F1 của cả hai là `0.667`. BoW bắt đúng tin giao hàng nhưng bỏ sót tin giả danh hỗ trợ; Embedding + LogReg làm ngược lại. Hai tin ham có `free`/`win` đều được cả hai mô hình phân loại đúng.

## 6. Phương án cho sản phẩm thật

Embedding + kNN không đạt SLA với p95 `58.910 ms/tin`; các mô hình local còn lại đạt `p95 < 50 ms/tin`. Tôi chọn Embedding + LogReg vì F1 test `0.975`, F1 SUB `1.000`, p95 `36.975 ms/tin` và chi phí `0 USD/1k`; đánh đổi là throughput thấp hơn BoW và cần encoder. LLM có p95 từ `1090.621` đến `1550.583 ms/tin`, không đạt SLA, đồng thời chi phí ước tính từ `0.007780` đến `0.077800 USD/1k`. Đề xuất router: mô hình rẻ xử lý tin chắc chắn, chỉ chuyển tin có xác suất `p ∈ [0.3, 0.7]` hoặc có URL/yêu cầu dữ liệu nhạy cảm sang tầng kiểm tra thứ hai.

## 7. AI log và điều đã học

AI được dùng để gợi ý cấu trúc ô thử thách và xử lý lỗi chạy không tương tác khi thiếu API key; tôi đã kiểm tra lại các hàm có sẵn và chạy toàn bộ notebook. Điều quan trọng nhất là BoW nhạy với từ khoá nhưng yếu với cách diễn đạt mới; embedding dùng ngữ nghĩa tốt hơn trên tập SUB nhưng vẫn phụ thuộc dữ liệu và cần đo trên stress test.
