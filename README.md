# 🏠 PREDICTING REAL ESTATE LIQUIDITY (DỰ BÁO THANH KHOẢN BĐS)

## 1. Giới thiệu bài toán (Problem Statement)
Thanh khoản (Liquidity) là rủi ro lớn nhất trong đầu tư Bất động sản. Dự án này nhằm mục đích xây dựng quy trình xử lý dữ liệu và mô hình học máy để trả lời câu hỏi: **"Bất động sản này có dễ bán hay không?"** dựa trên các đặc điểm của nó và thị trường.

**Mục tiêu cụ thể:**
* Làm sạch và chuẩn hóa dữ liệu đa nguồn (Transactions, Owners, Market Indicators).
* Phân tích các yếu tố tác động đến thanh khoản (Giá, Vị trí, Lãi suất).
* Ứng dụng **K-Means Clustering** & **PCA** để phân khúc thị trường, nhận diện các nhóm BĐS có đặc tính thanh khoản cao/thấp.

---

## 2. Mô tả dữ liệu (Data Description)

Dữ liệu đầu vào (Features) được lựa chọn kỹ càng để phản ánh cung - cầu và tâm lý thị trường.

### Input Features (Biến đầu vào):
| Biến số | Kiểu dữ liệu | Ý nghĩa trong thanh khoản |
| :--- | :--- | :--- |
| **price_per_m2** | `Float` | Đơn giá càng cao kén người mua -> ảnh hưởng thanh khoản. |
| **size_m2** | `Float` | Diện tích quá lớn hoặc quá nhỏ thường khó bán hơn. |
| **region_score** | `Float` | Điểm hấp dẫn của vị trí (Hạ tầng, Tiện ích). |
| **owner_rating** | `Float` | Uy tín người bán (ảnh hưởng đến niềm tin người mua). |
| **interest_rate** | `Float` | Lãi suất vay (Yếu tố vĩ mô tác động mạnh đến sức mua). |
| **days_on_market** | `Integer` | (Dữ liệu lịch sử) Số ngày tin đăng tồn tại trên sàn. |

### Output Target (Biến mục tiêu):
* **Liquidity Label/Index:** Nhãn phân loại khả năng thanh khoản (Dựa trên thời gian bán được hàng).

---

## 3. Quy trình EDA & Tiền xử lý (Exploratory Data Analysis)
Quy trình được thực hiện qua các bước trong `clean_phase1.py`, `clean_phase2.py` và `EDA1.py`:

1.  **Data Cleaning:** Xử lý dữ liệu khuyết (Missing data) ở các cột quan trọng như Giá và Diện tích.
2.  **Feature Engineering:** Tạo biến mới `region_score` từ dữ liệu vị trí và `owner_rating` từ lịch sử giao dịch.
3.  **Outlier Detection:** Loại bỏ các BĐS có giá trị bất thường (dùng Z-score) để tránh làm nhiễu mô hình dự báo.
4.  **Trực quan hóa (Visualization):**
    * Phân tích mối tương quan (Correlation) giữa Lãi suất ngân hàng và Số lượng giao dịch.
    * Biểu đồ phân phối thời gian bán hàng (Days on Market) theo từng Quận/Huyện.

---

## 4. Phân khúc thanh khoản bằng K-Means (Clustering Results)

Thay vì dự báo tuyến tính, chúng tôi sử dụng thuật toán **K-Means** (trong `Clustering_Pipeline.ipynb`) để gom nhóm các BĐS có đặc điểm tương đồng.

**Kết quả phân cụm (Liquidity Segments):**
Dữ liệu được chia thành 3 nhóm (Clusters) chính:
* **Cluster 0 (Thanh khoản thấp):** Thường là BĐS diện tích lớn, tổng giá trị cao (High ticket size), kén khách.
* **Cluster 1 (Thanh khoản cao):** BĐS giá tầm trung, diện tích vừa phải (60-90m2), vị trí đông dân cư.
* **Cluster 2 (Thanh khoản trung bình):** BĐS ở vùng ven, giá rẻ nhưng xa trung tâm, phụ thuộc nhiều vào quy hoạch.

---

## 5. Tối ưu hóa mô hình với PCA
Để trực quan hóa các nhóm thanh khoản trong không gian đa chiều, kỹ thuật **PCA (Principal Component Analysis)** được áp dụng:

1.  **Giảm chiều:** Rút gọn 6 biến đầu vào thành 3 thành phần chính (Principal Components) đại diện cho >80% thông tin.
2.  **Visualization 3D:** Biểu đồ 3D cho thấy ranh giới rõ ràng giữa nhóm "Dễ bán" và "Khó bán", chứng minh các biến đầu vào đã chọn lọc có hiệu quả phân loại tốt.

---
*Đồ án môn học Data Analysis - Dự báo Thanh khoản Bất động sản Việt Nam.*
