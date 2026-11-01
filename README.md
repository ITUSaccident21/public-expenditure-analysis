# Phân Tích Chi Tiêu Công & Mua Sắm Đô Thị Thành Phố Boulder
## Boulder City Public Expenditure & Procurement Analysis

**Môn học:** Lập trình cho Khoa học Dữ liệu (CSC17104)  
**Trường:** Đại học Khoa học Tự nhiên – ĐHQG-HCM (HCMUS / ITUS)  
**Học kỳ:** HK1, Năm học 2026–2027  

---

## 1. Giới Thiệu & Mục Tiêu Dự Án (Project Overview)

Chi tiêu công và mua sắm đô thị (Public Procurement & Accounts Payable) là bức tranh phản ánh trực tiếp và minh bạch nhất về cách một chính quyền vận hành ngân sách từ tiền thuế của người dân.

Dự án này thực hiện một chu trình Khoa học Dữ liệu hoàn chỉnh (End-to-End Data Science Pipeline) trên bộ dữ liệu chi trả công nợ thực tế của chính quyền **Thành phố Boulder, bang Colorado, Hoa Kỳ**. Thông qua phân tích khám phá (EDA), kiểm định thống kê chuyên sâu và xây dựng mô hình Học máy (Machine Learning), dự án nhằm giải quyết các bài toán:
- Nhận diện cơ cấu dòng tiền, đánh giá mức độ tập trung chi tiêu vào các nhà thầu lớn (Vendor Concentration).
- Kiểm chứng giả thuyết về hiện tượng dồn chi tiêu cuối kỳ ngân sách (Fiscal Year/Quarter-end Spikes - hiệu ứng "Use-it-or-lose-it").
- Phân tích cơ chế và rủi ro từ các giao dịch hoàn tiền / chứng từ ghi có (Credit Memos / Âm tiền).
- Xây dựng mô hình Machine Learning phân loại tự động danh mục chi tiêu phục vụ kiểm toán và hỗ trợ hạch toán tài chính công.

---

## 2. Thông Tin Nhóm & Phân Công Nhiệm Vụ (Team & Task Breakdown)

Nhóm gồm **3 thành viên**, mỗi thành viên đảm nhiệm chính **2 câu hỏi nghiên cứu** (đáp ứng đầy đủ các tiêu chí chiều sâu D1–D6 và bài toán Machine Learning):

| Thành viên | MSSV | Vai trò chính | Câu hỏi đảm nhiệm (Owner) | Đóng góp & Trách nhiệm quy trình |
| :--- | :---: | :--- | :--- | :--- |
| **Thành viên 1 (Lead)** | `[MSSV_1]` | Data Engineering & Machine Learning | **Q1** (D1, D3)<br>**Q6** (Machine Learning) | • Khảo sát nguồn dữ liệu, tải dữ liệu thô, thiết lập Repo.<br>• Phân tích thuộc tính định lượng (`transaction_amount`).<br>• Xây dựng và đánh giá ML Pipeline (Q6). |
| **Thành viên 2** | `[MSSV_2]` | Statistical Inference & Time Series | **Q2** (D2, D4)<br>**Q5** (D4, D6) | • Viết tài liệu thu thập dữ liệu & kiểm tra tính hợp pháp.<br>• Phân tích thuộc tính thời gian (`transaction_date`).<br>• Phân tích chu kỳ tài khóa và kiểm định độ vững (Robustness). |
| **Thành viên 3** | `[MSSV_3]` | EDA & Diagnostic Analytics | **Q3** (D3, D6)<br>**Q4** (D2, D5) | • Phân tích biến phân loại và dữ liệu khuyết thiếu (Missing Data).<br>• Phân tích dòng tiền âm / hoàn trả công nợ (Credit Memos).<br>• Đóng góp phần Error Analysis & Giải thích mô hình (Q6). |

---

## 3. Dữ Liệu Nghiên Cứu (Dataset Information)

### 3.1. Nguồn Gốc & Thu Thập Dữ Liệu
- **Chủ thể (Subject):** Dữ liệu chi trả công nợ và mua sắm công của chính quyền Thành phố Boulder, bang Colorado, Hoa Kỳ. Phản ánh chi tiết từng dòng hóa đơn (line-item) thanh toán cho nhà thầu/nhà cung ứng bên ngoài.
- **Nguồn dữ liệu (Source):** [City of Boulder Open Data - Accounts Payable Current](https://open-data.bouldercolorado.gov/datasets/Boulder::accounts-payable-current/about).
- **Ngày phát hành:** 04/02/2025 | **Ngày trích xuất dữ liệu:** 28/10/2026.
- **Quy mô dữ liệu thô:** **35,251 dòng** và **13 cột** (định dạng CSV, dung lượng ~6.5 MB).
- **Giấy phép (License):** **CC0 1.0 Universal (Public Domain Dedication)** — Dữ liệu thuộc phạm vi công cộng, cho phép tự do khai thác, phân tích và công bố phục vụ nghiên cứu và giáo dục.
- **Phương pháp thu thập:** Dữ liệu hành chính thứ cấp (Secondary Administrative Data) được trích xuất trực tiếp từ Hệ thống Quản trị Doanh nghiệp / Kế toán nội bộ (ERP) của Phòng Tài chính thành phố Boulder.

### 3.2. Giới Hạn & Thiên Lệch Dữ Liệu (Limitations & Biases)
1. **Thiên lệch địa lý (Geographic Bias):** Dữ liệu chỉ phản ánh cơ cấu hoạt động của một đô thị đặc thù (Boulder, Colorado), không khái quát hóa cho toàn bộ các bang hay nước Mỹ.
2. **Phạm vi chi phí (Scope Limitation):** Chỉ bao gồm chi trả cho bên thứ ba (Accounts Payable), không bao gồm chi phí chi trả lương trực tiếp cho đội ngũ công chức nội bộ (Payroll được bảo mật theo quy định pháp lý).
3. **Đặc thù kế toán (Accounting Mechanics):** Chứa các dòng giao dịch có giá trị âm (`transaction_amount < 0`) do các nghiệp vụ giảm trừ công nợ, hoàn trả hàng hóa hoặc chứng từ ghi có (Credit Memo).
4. **Không có mã hóa đơn duy nhất (No Unique Invoice ID):** Bản ghi ở cấp độ dòng chi tiết sản phẩm/dịch vụ nên xuất hiện nhiều dòng trùng lặp về mặt nghiệp vụ đối với các đơn hàng gồm nhiều hạng mục giống nhau.

### 3.3. Tuyên Bố Về Các Nghiên Cứu Công Khai Trước Đó (Declaration of Existing Work)
Bộ dữ liệu được tải trực tiếp từ cổng dữ liệu mở của chính quyền Thành phố Boulder. Tại thời điểm thực hiện đề tài, chưa có notebook công khai nào trên Kaggle hoặc GitHub thực hiện các phân tích chuyên sâu về kinh tế lượng, kiểm định giả thuyết và mô hình hóa danh mục chi tiêu như đồ án này. Toàn bộ câu hỏi nghiên cứu, phương pháp xử lý và mã nguồn đều do nhóm tự xây dựng từ đầu.

---

## 4. Danh Sách 6 Câu Hỏi Nghiên Cứu (Research Questions)

Các câu hỏi được thiết kế thỏa mãn yêu cầu mở rộng chiều sâu của đề cương đồ án:



---

## 5. Cấu Trúc Thư Mục Dự Án (Repository Structure)

```text
public-expenditure-analysis/
├── README.md                      # Báo cáo tổng quan dự án và phân công
├── CONTRIBUTING.md                # Quy chế làm việc nhóm, Git flow & Q&A prep
├── environment.yml                # File cấu hình môi trường Conda tái lập
├── LLM_USAGE.md                   # Nhật ký sử dụng công cụ AI minh bạch
├── slides.pdf                     # Slide thuyết trình bảo vệ đồ án
├── data/
│   ├── raw/                       # Dữ liệu gốc (Accounts_Payable_Current.csv)
│   └── processed/                 # Dữ liệu đã làm sạch & biến đổi (nếu có)
├── src/                           # Module Python tái sử dụng
│   ├── __init__.py
│   ├── data_loader.py             # Hàm tải và kiểm tra schema
│   ├── preprocessing.py           # Pipeline làm sạch và xử lý kiểu dữ liệu
│   ├── metrics.py                 # Hàm tính HHI, Bootstrap CI
│   └── visualization.py           # Hàm chuẩn hóa giao diện đồ thị Matplotlib/Seaborn
└── notebooks/
    ├── 01_collection_exploration.ipynb  # Thu thập dữ liệu, EDA và kế hoạch làm sạch
    ├── 02_analysis.ipynb                # Triển khai giải quyết 5 câu hỏi Non-ML (Q1 - Q5)
    └── 03_ml_question.ipynb             # Bài toán Machine Learning hoàn chỉnh (Q6)
```

---

## 6. Hướng Dẫn Tái Lập Môi Trường & Chạy Dự Án (How to Reproduce)

Dự án được thiết kế để có thể tái lập kết quả 100% trên môi trường Linux/macOS/Windows thông qua Conda.

### Bước 1: Khởi tạo môi trường Conda
```bash
# Clone repository
git clone <repository_url>
cd public-expenditure-analysis

# Tạo môi trường từ file cấu hình
conda env create -f environment.yml

# Kích hoạt môi trường
conda activate pfds-boulder
```

### Bước 2: Kiểm tra dữ liệu thô
Đảm bảo file dữ liệu thô `Accounts_Payable_Current.csv` đã nằm trong thư mục `data/raw/`.  
*(Nếu chưa có, tải trực tiếp từ [City of Boulder Open Data](https://open-data.bouldercolorado.gov/datasets/Boulder::accounts-payable-current/about) và đặt vào `data/raw/Accounts_Payable_Current.csv`).*

### Bước 3: Thực thi các Notebook theo thứ tự
Chạy lần lượt các notebook trong thư mục `notebooks/` để tái lập toàn bộ kết quả phân tích:
1. `notebooks/01_collection_exploration.ipynb`: Khám phá dữ liệu, kiểm định chất lượng, phân tích missing và phân phối.
2. `notebooks/02_analysis.ipynb`: Trực quan hóa và kiểm định thống kê cho các câu hỏi nghiên cứu từ Q1 đến Q5.
3. `notebooks/03_ml_question.ipynb`: Huấn luyện mô hình Machine Learning, tinh chỉnh siêu tham số và phân tích lỗi.

---

## 7. Các Phát Hiện Chính (Key Findings Preview)
1. **Phân phối cực đoan:** Dữ liệu chi tiêu bị lệch phải mạnh với giá trị trung vị là \$371, trong khi trung bình lên tới \$9,185 do sự xuất hiện của các hợp đồng đầu tư hạ tầng lớn.
2. **Tính tập trung ngân sách:** Hơn 60% tổng chi ngân sách tập trung vào 3 phòng ban chính: `Public Works / Utilities`, `Transportation & Mobility` và `Facilities & Fleet`.
3. **Hiện tượng dòng tiền đảo chiều:** Khoảng 0.5% giao dịch là dòng tiền âm (Credit Memos) phản ánh nghiệp vụ kế toán công hoàn trả và chiết khấu khối lượng, chủ yếu tập trung ở các đơn vị cung cấp linh kiện kỹ thuật và nhiên liệu.
