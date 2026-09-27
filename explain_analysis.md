# Bản Phân Tích So Sánh Chỉ Số EXPLAIN Trước Và Sau Khi Tối Ưu

## 1. Trước khi tối ưu (Sử dụng hàm YEAR() - Non-SARGable)
- **Chỉ số `type`:** `ALL` (Full Table Scan).
- **Chỉ số `rows`:** Quét qua toàn bộ số lượng dòng hiện có trong bảng (ví dụ: `100,000` dòng).
- **Lý do:** Khi sử dụng hàm biến đổi dữ liệu bọc quanh cột `transaction_date`, MySQL bắt buộc phải tính toán giá trị cho từng dòng một, khiến hệ thống không thể sử dụng Index có sẵn.

## 2. Sau khi tối ưu (Sử dụng so sánh khoảng & Composite Index)
- **Chỉ số `type`:** `range` (Range Scan) hoặc `ref`.
- **Chỉ số `rows`:** Giảm mạnh chỉ còn quét qua các dòng nằm trong khoảng năm 2026 (ví dụ: `2,500` dòng).
- **Kết luận:** Việc chuyển đổi sang câu lệnh đạt chuẩn SARGable kết hợp Composite Index giúp bộ tối ưu hóa (Optimizer) định vị chính xác vùng dữ liệu cần tìm, tăng tốc độ phản hồi hệ thống lên gấp nhiều lần.
