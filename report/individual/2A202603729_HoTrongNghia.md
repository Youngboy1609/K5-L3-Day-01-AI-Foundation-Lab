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
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | ☐ |

## 2. Kết quả chính

| Mô hình | F1 test | F1 SUB 40 | p95 (ms/tin) | USD/1k |
| --- | ---: | ---: | ---: | ---: |
| BoW + LogReg | 0.931 | 0.857 | 0.020 | 0 |
| BoW + MLP | 0.980 | 0.947 | 0.206 | 0 |
| Embedding + kNN | 0.933 | 0.974 | 7.965 | 0 |
| Embedding + LogReg | 0.975 | 1.000 | 5.148 | 0 |
| Embedding + MLP head | 0.966 | 1.000 | 6.946 | 0 |

Notebook chạy với `SEED=8296`; bộ kiểm tra đạt `13/15`. Hai mục LLM và phép tính chi phí LLM không chạy vì không cấu hình API key.

## 3. Dự đoán và thực tế

Trong thử thách cá nhân, BoW đúng 3/4 tin và Embedding + LogReg cũng đúng 3/4. BoW nhầm tin yêu cầu đăng nhập thành ham; Embedding + LogReg nhầm tin giao hàng thành ham. Điều này cho thấy cả hai mô hình vẫn cần dữ liệu đủ đa dạng cho các mẫu lừa đảo tự nhiên.

## 4. Phân tích 50 case khó

Kết quả notebook cho thấy các mô hình embedding đạt F1 SUB cao hơn F1 test, trong khi BoW + LogReg giảm từ 0.931 xuống 0.857 trên SUB. Cần thận trọng khi diễn giải vì SUB chỉ có 40 tin; stress test 50 case mới là nơi đánh giá các kiểu diễn đạt khác phân phối.

## 5. Thử thách cá nhân

Đề của MSSV này là **Embedding #5**. Bốn tin tự viết cho kết quả: BoW đúng 3/4, Embedding + LogReg đúng 3/4; F1 của cả hai là `0.667`. BoW bắt đúng tin giao hàng nhưng bỏ sót tin giả danh hỗ trợ; Embedding + LogReg làm ngược lại. Hai tin ham có `free`/`win` đều được cả hai mô hình phân loại đúng.

## 6. Phương án cho sản phẩm thật

Tất cả năm mô hình local đều đạt `p95 < 50 ms/tin`. Tôi chọn Embedding + LogReg vì F1 test `0.975`, F1 SUB `1.000`, p95 `5.148 ms/tin` và chi phí `0 USD/1k`; đánh đổi là throughput thấp hơn BoW và cần encoder. Đề xuất router: mô hình rẻ xử lý tin chắc chắn, chỉ chuyển tin có xác suất `p ∈ [0.3, 0.7]` hoặc có URL/yêu cầu dữ liệu nhạy cảm sang tầng kiểm tra thứ hai.

## 7. AI log và điều đã học

AI được dùng để gợi ý cấu trúc ô thử thách và xử lý lỗi chạy không tương tác khi thiếu API key; tôi đã kiểm tra lại các hàm có sẵn và chạy toàn bộ notebook. Điều quan trọng nhất là BoW nhạy với từ khoá nhưng yếu với cách diễn đạt mới; embedding dùng ngữ nghĩa tốt hơn trên tập SUB nhưng vẫn phụ thuộc dữ liệu và cần đo trên stress test.
