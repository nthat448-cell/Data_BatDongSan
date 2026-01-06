# 🏙️ VIETNAM REAL ESTATE LIQUIDITY PREDICTION
> **Đồ án môn học Data Analysis** - Xây dựng mô hình phân tích và dự báo tính thanh khoản Bất động sản.

![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)
![Library](https://img.shields.io/badge/Lib-Pandas%20|%20Sklearn%20|%20Seaborn-orange.svg)
![Status](https://img.shields.io/badge/Status-Completed-green.svg)

## 📑 MỤC LỤC
1. [Giới thiệu đề tài](#1-giới-thiệu-đề-tài)
2. [Mô tả dữ liệu](#2-mô-tả-dữ-liệu-data-dictionary)
3. [Quy trình xử lý & EDA](#3-quy-trình-xử-lý--eda)
4. [Xây dựng mô hình (K-Means & PCA)](#4-xây-dựng-mô-hình-clustering)
5. [Kết quả nghiên cứu](#5-kết-quả-nghiên-cứu)
6. [Hướng dẫn cài đặt](#6-hướng-dẫn-cài-đặt)

---

## 1. GIỚI THIỆU ĐỀ TÀI

### 📌 Vấn đề nghiên cứu
Thị trường Bất động sản Việt Nam có sự biến động lớn về giá và tính thanh khoản. Một bất động sản có thể được định giá cao nhưng lại mất hàng năm trời mới bán được (thanh khoản thấp), gây rủi ro chôn vốn cho nhà đầu tư.

### 🎯 Mục tiêu
Dự án này nhằm giải quyết câu hỏi: **"Làm sao phân loại được BĐS Dễ bán vs. Khó bán?"** thông qua dữ liệu lịch sử.
- **Input:** Đặc điểm BĐS (Giá, Diện tích, Vị trí...), thông tin người bán, chỉ số kinh tế vĩ mô.
- **Method:** Sử dụng học máy không giám sát (Unsupervised Learning) để phân khúc thị trường.
- **Output:** Nhãn phân cụm (Cluster Label) đại diện cho mức độ thanh khoản.

---

## 2. MÔ TẢ DỮ LIỆU (Data Dictionary)

Dữ liệu sau khi làm sạch được tổng hợp thành bảng Master, bao gồm các biến quan trọng sau:

| Tên biến (Variable) | Loại (Type) | Mô tả chi tiết | Vai trò |
| :--- | :--- | :--- | :--- |
| **size_m2** | `Float` | Diện tích sử dụng ($m^2$). | **Input** |
| **price_per_m2** | `Float` | Đơn giá theo mét vuông (VND). Biến số ảnh hưởng lớn nhất đến quyết định mua. | **Input** |
| **region_score** | `Float` | Điểm tiềm năng khu vực (Location Scoring) dựa trên hạ tầng và tiện ích. | **Input** |
| **owner_rating** | `Float` | Điểm tín nhiệm của người bán (0.0 - 1.0). | **Input** |
| **interest_rate** | `Float` | Lãi suất ngân hàng (%) tại thời điểm đăng tin. | **Input** |
| **liquidity_index** | `Float` | Chỉ số thanh khoản tính toán từ thời gian tồn tại của tin đăng. | **Input** |
| **Cluster_Label** | `Int` | Nhãn cụm (0, 1, 2) - Kết quả phân loại của mô hình. | **Output** |

---

## 3. QUY TRÌNH XỬ LÝ & EDA

Dự án tuân thủ quy trình 7 bước EDA cơ bản trong file `EDA1.py`:

### 🛠️ Bước 1-3: Làm sạch dữ liệu (Data Cleaning)
* **Missing Values:** Xử lý các giá trị rỗng bằng phương pháp điền trung bình (Mean Imputation) cho các biến liên tục.
* **Duplicates:** Loại bỏ 100% các bản ghi trùng lặp.
* **Data Type:** Chuyển đổi `transaction_date` sang định dạng `Datetime`, ép kiểu số học cho `price`.

### 📊 Bước 4-5: Phân tích đơn biến (Univariate)
* **Phân phối giá:** Biểu đồ Histogram cho thấy dữ liệu lệch phải (Right-skewed) -> Đa số nhà có giá trị trung bình thấp, xuất hiện một số ít BĐS siêu sang.
* **Tỷ lệ khu vực:** Biểu đồ tròn (Pie Chart) cho thấy nguồn cung tập trung chủ yếu tại TP.HCM và Hà Nội.

### 📉 Bước 6-7: Phân tích đa biến & Outlier
* **Correlation:** Ma trận tương quan cho thấy `interest_rate` có tương quan nghịch với số lượng giao dịch (lãi suất tăng -> giao dịch giảm).
* **Outlier Handling:** Sử dụng phương pháp **Z-score** với ngưỡng $threshold=3$ để loại bỏ các điểm dữ liệu nhiễu (ví dụ: nhà 10m2 nhưng giá 100 tỷ).

---

## 4. XÂY DỰNG MÔ HÌNH (Clustering)

### 🔹 Phương pháp 1: K-Means Clustering (Truyền thống)
* **Chuẩn hóa:** Sử dụng `StandardScaler` để đưa các biến về cùng một thang đo.
* **Tìm K tối ưu:** Sử dụng phương pháp **Elbow Method**, xác định điểm gãy tại **k=3**.
* **Kết quả:** Phân chia dữ liệu thành 3 nhóm khách hàng riêng biệt.

### 🔹 Phương pháp 2: PCA + K-Means (Nâng cao)
Để giải quyết vấn đề đa cộng tuyến và trực quan hóa trong không gian 3D:
1.  **PCA (Principal Component Analysis):** Giảm số chiều dữ liệu từ 6 biến xuống 3 thành phần chính (PC1, PC2, PC3).
2.  **Variance Explained:** 3 thành phần này giải thích được **>85%** thông tin của dữ liệu gốc.
3.  **Re-Clustering:** Chạy lại K-Means trên dữ liệu PCA giúp mô hình hội tụ tốt hơn và phân tách rõ ràng hơn.

---

## 5. KẾT QUẢ NGHIÊN CỨU

Sau khi phân tích, mô hình đã nhận diện được 3 phân khúc Bất động sản (Liquidity Segments):

#### 🟢 Cluster 0: "Phân khúc Bình dân & Dễ bán" (Thanh khoản Cao)
* **Đặc điểm:** Diện tích nhỏ (40-60m2), giá/m2 thấp, nằm ở khu vực đông dân cư.
* **Thanh khoản:** Rất tốt, thời gian bán trung bình < 30 ngày.

#### 🟡 Cluster 1: "Phân khúc Cao cấp & Kén khách" (Thanh khoản Thấp)
* **Đặc điểm:** Diện tích rất lớn (>200m2), tổng giá trị tài sản cao (High Ticket), `region_score` trung bình.
* **Thanh khoản:** Chậm, cần chiến lược marketing dài hạn.

#### 🔵 Cluster 2: "Phân khúc Đầu tư & Nhạy cảm Lãi suất"
* **Đặc điểm:** BĐS phụ thuộc mạnh vào biến động vĩ mô (`interest_rate`), thường là đất nền dự án.
* **Thanh khoản:** Biến động mạnh theo thị trường.

---

## 6. HƯỚNG DẪN CÀI ĐẶT

### Yêu cầu hệ thống
* Python 3.8+
* Jupyter Notebook

### Các bước chạy dự án
1.  **Clone repository:**
    ```bash
    git clone [https://github.com/Ten-Cua-Ban/Data_BatDongSan.git](https://github.com/Ten-Cua-Ban/Data_BatDongSan.git)
    ```
2.  **Cài đặt thư viện:**
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn
    ```
3.  **Chạy pipeline xử lý dữ liệu:**
    ```bash
    python clean_phase1.py
    python clean_phase2.py
    ```
4.  **Chạy mô hình phân tích:**
    Mở file `Clustering_Pipeline.ipynb` bằng Jupyter Notebook và chọn **Run All**.

---
**Tác giả:** [Tên Của Bạn]
**Giảng viên hướng dẫn:** [Tên Giảng Viên]
