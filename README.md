# 🏙️ VIETNAM REAL ESTATE DATA ANALYSIS

## 1. Vấn đề nghiên cứu
Dự án phân tích thị trường bất động sản tại 5 thành phố lớn (Hà Nội, TP.HCM, Đà Nẵng, Cần Thơ, Hải Phòng). Mục tiêu là làm sạch dữ liệu thô, phân tích xu hướng giá và ứng dụng K-Means Clustering để phân khúc thị trường.

## 2. Dữ liệu Input & Output
Dữ liệu được tổng hợp từ các file: Properties, Transactions, Owners, Market Indicators.

### Input (Dữ liệu đầu vào cho mô hình):
- **size_m2** (Float): Diện tích ($m^2$).
- **price_per_m2** (Float): Đơn giá (VND/$m^2$).
- **region_score** (Float): Điểm tiềm năng khu vực.
- **owner_rating** (Float): Điểm tín nhiệm chủ nhà.
- **interest_rate** (Float): Lãi suất ngân hàng (%).
- **liquidity_index** (Float): Chỉ số thanh khoản.

### Output (Kết quả):
- **Cluster_Label**: Nhãn phân cụm (0, 1, 2) đại diện cho phân khúc BĐS.

## 3. Quy trình xử lý (EDA)
1. **Làm sạch:** Xử lý dữ liệu thiếu (Missing), loại bỏ trùng lặp (Duplicate) bằng `clean_phase1.py` và `clean_phase2.py`.
2. **Chuẩn hóa:** Đổi tỷ giá USD sang VND (25,400), định dạng ngày tháng (dd/mm/yyyy).
3. **Trực quan hóa:**
   - Biểu đồ tròn: Tỷ lệ BĐS theo thành phố.
   - Histogram: Phân phối giá (phát hiện lệch phải).
4. **Outlier:** Loại bỏ nhiễu bằng phương pháp Z-score trong file `EDA1.py`.

## 4. Kết quả Gom cụm (K-Means)
Sử dụng phương pháp Elbow xác định số cụm tối ưu ($k$).
- **Cụm 0:** BĐS bình dân, vùng ven.
- **Cụm 1:** BĐS trung cấp, thanh khoản cao.
- **Cụm 2:** BĐS cao cấp, vị trí đắc địa.

## 5. Phân tích nâng cao (PCA)
- Giảm chiều dữ liệu từ 6 biến xuống 3 thành phần chính (PC1, PC2, PC3).
- Trực quan hóa không gian 3D cho thấy các nhóm khách hàng tách biệt rõ ràng (Xem chi tiết trong `Clustering_Pipeline.ipynb`).

---
*Đồ án môn học Data Analysis.*
