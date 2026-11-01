# Hướng Dẫn Đóng Góp & Phối Hợp Dự Án (CONTRIBUTING)
## Đồ án: Phân tích Chi tiêu Công Thành phố Boulder (CSC17104)

Chào mừng các thành viên nhóm! Tài liệu này quy định chuẩn mực làm việc với Git, kiểm thử, phân công trách nhiệm chéo và hướng dẫn chuẩn bị vấn đáp cá nhân nhằm đảm bảo **cả 3 thành viên đều nắm vững toàn bộ quy trình Khoa học Dữ liệu** và tự tin đạt điểm tối đa trong phần Q&A với hội đồng môn học.

---

## 1. 5 Điều Quan Trọng Nhất (Golden Rules)
1. **Không code chung trên cùng một Notebook:** Mỗi câu hỏi phân tích làm trên một notebook riêng hoặc nhánh riêng. Code dùng chung bắt buộc chuyển hóa thành hàm trong thư mục `src/`.
2. **Clear Output trước khi commit Notebook:** Trừ notebook báo cáo cuối cùng đã merge vào `main`, tất cả commit trong quá trình làm bắt buộc phải Clear Output (`Kernel -> Restart & Clear All Outputs`) để tránh xung đột Git JSON.
3. **Không đẩy thẳng lên `main`:** 100% thay đổi đi qua Pull Request (PR) và phải có ít nhất **1 đồng đội review + chạy kiểm thử (reproduce) thành công** mới được merge.
4. **Vectorization là bắt buộc:** Cấm tuyệt đối dùng vòng lặp `for` duyệt qua từng dòng của DataFrame (`iterrows`, `itertuples`) cho các thao tác mà NumPy/Pandas đã hỗ trợ toán tử mảng.
5. **Chất vấn chéo trước khi nộp:** Không có khái niệm "phần của ai người nấy biết". Mỗi tuần tổ chức 1 buổi Mock Q&A (vấn đáp thử), thành viên này phải phản biện được kết quả của thành viên khác.

---

## 2. Ma Trận Phân Công & Trách Nhiệm (RACI Matrix)

Để đảm bảo mọi thành viên đều nắm vững các khâu từ dữ liệu thô đến mô hình hóa:

### 2.1. Phân bổ 6 Câu hỏi Phân tích (2 câu/thành viên)
| Thành viên | Câu hỏi đảm nhiệm (Owner) | Tiêu chí chiều sâu | Phạm vi kỹ thuật & Mô hình |
| :--- | :--- | :--- | :--- |
| **TV1** *(Lead / Data Eng)* | **Q1:** Mức độ tập trung nhà thầu (Vendor Concentration & HHI)<br>**Q6:** Bài toán Machine Learning phân loại chi tiêu | **D1, D3**<br>*(ML Core)* | • Merge tập dữ liệu phân loại tổ chức, tính HHI index.<br>• Xây dựng Pipeline `scikit-learn` hoàn chỉnh, Temporal Split, ngừa Data Leakage. |
| **TV2** *(Stats & Temporal)* | **Q2:** Chu kỳ tài khóa & Dồn chi tiêu cuối kỳ<br>**Q5:** Đánh giá tính vững khi loại bỏ Outlier hạ tầng | **D2, D4**<br>**D4, D6** | • Resampling, Rolling window, Pivot/Melt.<br>• Kiểm soát biến nhiễu quy mô ngân sách.<br>• Phân tích đa ngưỡng Outlier (Robustness check). |
| **TV3** *(EDA & Diagnostic)* | **Q3:** Phân tích giao dịch âm (Credit Memos)<br>**Q4:** So sánh chi phí vận hành & Kiểm định Bootstrap CI | **D3, D6**<br>**D2, D5** + *(ML Interp)* | • Tính tỷ lệ hoàn trả, kiểm định độ nhạy ngưỡng giao dịch.<br>• Ước lượng 95% Bootstrap Confidence Intervals.<br>• Đóng góp phần Error Analysis & Permutation Importance vào Q6. |

### 2.2. Ma trận Trách nhiệm theo 5 Pha Dự án
| Pha thực hiện | TV1 | TV2 | TV3 |
| :--- | :--- | :--- | :--- |
| **Pha 1: Data Collection** | Khảo sát dữ liệu, tải dữ liệu thô `Accounts_Payable_Current.csv` vào `data/raw/`, khởi tạo repo. | Soạn thảo tài liệu nguồn gốc dữ liệu, kiểm tra License CC0 1.0 và các thiên lệch dữ liệu hành chính. | Thiết lập môi trường `environment.yml` và cấu hình Git LFS / `.gitignore` chuẩn. |
| **Pha 2: Data Exploration & Cleaning** | Khảo sát phân phối thuộc tính định lượng (`transaction_amount`), phát hiện giá trị cực trị/âm. | Khảo sát chuỗi thời gian (`transaction_date`), chuẩn hóa định dạng ngày, phát hiện năm sai lệch. | Khảo sát biến phân loại (`department`, `vendor_name`) và phân tích hiện tượng thiếu dữ liệu đồng loạt. |
| **Pha 3: Question Formulation** | Đề xuất khung Q1 (D1+D3) và bài toán ML Q6 (Phân loại danh mục chi tiêu). | Đề xuất khung Q2 (D2+D4) và Q5 (D4+D6). | Đề xuất khung Q3 (D3+D6) và Q4 (D2+D5). |
| **Pha 4: Core Analysis & Modeling** | Code Q1 và dựng khung Pipeline Scikit-learn, huấn luyện Baseline & 2 Models cho Q6. | Code Q2 & Q5, viết hàm resample chuỗi thời gian và kiểm định tính vững. | Code Q3 & Q4, viết module Bootstrap CI trong `src/metrics.py`, hoàn thiện Error Analysis cho Q6. |
| **Pha 5: Synthesis & Defense Prep** | Tổng hợp Notebook chung, kiểm tra Reproducibility trên môi trường sạch (Clean run). | Thiết kế Slide thuyết trình (Data Storytelling, 15 phút bảo vệ). | Chủ trì biên soạn bộ câu hỏi Mock Q&A và tổ chức buổi phản biện nội bộ trước khi nộp. |

---

## 3. Quy Trình Làm Việc Với Git & Jupyter Notebook

### 3.1. Các bước thực hiện một Task
```bash
# 1. Kéo code mới nhất từ main
git switch main && git pull origin main

# 2. Tạo nhánh tính năng theo mã định danh
# Cú pháp: feat/q<số_câu>-<mô_tả_ngắn> hoặc feat/phase<số_pha>-<mô_tả_ngắn>
git switch -c feat/q1-vendor-concentration

# 3. Cài đặt nbstripout để tự động dọn metadata/outputs thừa của notebook
conda activate pfds-boulder
pip install nbstripout
nbstripout --install

# 4. Commit theo đúng quy chuẩn Conventional Commits
git add notebooks/02_analysis.ipynb src/metrics.py
git commit -m "feat(q1): compute HHI index and vendor concentration curves (D1, D3)"

# 5. Cập nhật nhánh main mới nhất và kiểm tra conflict
git fetch origin && git merge origin/main

# 6. Đẩy lên GitHub và tạo Pull Request (PR)
git push -u origin feat/q1-vendor-concentration
```

### 3.2. Quy ước Commit
- Cú pháp: `loại(phạm_vi): nội dung tóm tắt`
- Các loại (`type`):
  - `feat`: Thêm câu hỏi phân tích, biểu đồ mới, tính năng mới.
  - `fix`: Sửa lỗi công thức, sửa lỗi rò rỉ dữ liệu, sửa bug logic.
  - `refactor`: Tách code từ notebook thành các hàm Python trong `src/`.
  - `docs`: Cập nhật markdown phân tích, README, LLM_USAGE.md.
  - `chore`: Cấu hình môi trường conda, gitignore.
- Các phạm vi (`scope`): `data`, `eda`, `clean`, `q1`, `q2`, `q3`, `q4`, `q5`, `q6-ml`, `src`.

---

## 4. Cơ Chế Review Code Chéo (Bắt Buộc Trước Khi Merge)

| Người tạo PR (Tác giả) | Reviewer chính | Nhiệm vụ bắt buộc của Reviewer |
| :---: | :---: | :--- |
| **TV1** | **TV2** | Kéo nhánh về máy, chạy lại notebook từ trên xuống dưới (`Restart and Run All`) đảm bảo ra cùng kết quả. |
| **TV2** | **TV3** | Kiểm tra biểu đồ: Đủ Title, Axis labels, Units, Legend chưa? Số liệu trong bài viết markdown có khớp biểu đồ không? |
| **TV3** | **TV1** | Kiểm tra cấu trúc code: Đã vector hóa chưa? Có phụ thuộc biến toàn cục (global state) giữa các ô không? |

### Checklist 5 Tiêu chuẩn Khi Duyệt PR:
- [ ] **Reproducibility:** Chạy `Restart & Run All` thành công 100%, không phát sinh lỗi đường dẫn tuyệt đối (bắt buộc dùng đường dẫn tương đối từ gốc project).
- [ ] **No Data Leakage:** Trong bài toán ML, các bước `fit` của Scaler/Encoder nằm hoàn toàn trong tập Train hoặc bọc trong `Pipeline`. Phân chia tập dữ liệu bằng Temporal Split.
- [ ] **Vectorization Check:** Không dùng vòng lặp `for row in df.iterrows()`. Mọi phép tính toán cột đều dùng Pandas/NumPy vectorization.
- [ ] **Chart Polish:** Biểu đồ rõ ràng, đầy đủ nhãn trục, đơn vị tính, tiêu đề và chú giải. Nhận xét markdown nêu số liệu định lượng cụ thể.
- [ ] **Q&A Comprehension:** Người review đã đọc hiểu toàn bộ phân tích và sẵn sàng trả lời thay tác giả nếu được hỏi.

---

## 5. Quy Chuẩn Kỹ Thuật (Data Science Code & Data Hygiene)

1. **Quản lý dữ liệu:**
   - Thư mục `data/raw/` và `data/processed/` **tuyệt đối không commit lên Git** nếu dung lượng lớn (đưa vào `.gitignore`).
   - Cung cấp link tải dữ liệu chính thức từ City of Boulder Open Data Portal trong README.
2. **Module hóa mã nguồn (`src/`):**
   - Các hàm tính chỉ số HHI, hàm lấy mẫu Bootstrap, hàm vẽ biểu đồ phân phối chuẩn phải được viết vào file `.py` trong thư mục `src/` và import vào notebook để tái sử dụng.
3. **Quy chuẩn Machine Learning (Q6):**
   - Bắt buộc thiết lập Dummy Baseline để làm mốc so sánh.
   - Bắt buộc giải thích lý do chọn metric (Weighted F1-score & Macro F1-score vì danh mục chi tiêu mất cân bằng nghiêm trọng).
   - Bắt buộc có phân tích lỗi (Error Analysis) và diễn giải thuộc tính (Permutation Importance).

---

## 6. Chiến Lược Luyện Tập Vấn Đáp (Oral Q&A Defense Strategy)

Để đảm bảo không thành viên nào bị "bắt bí" trong 10 phút hỏi đáp riêng từng người của hội đồng:

### 6.1. Ngân Hàng Câu Hỏi Giảng Viên Thường Xoáy Vào Đồ Án
1. **Khâu Dữ liệu & Tiền xử lý (Mọi thành viên đều phải trả lời được):**
   - *"Tại sao lại xuất hiện 484 dòng khuyết đồng thời ở 7 cột phân loại? Nhóm xử lý thế nào và tại sao không drop?"*
   - *"Các khoản chi có giá trị âm phản ánh nghiệp vụ kế toán gì? Xử lý chúng như thế nào khi tính tổng ngân sách?"*
   - *"Phát hiện outlier bằng phương pháp nào? Outlier trong chi tiêu công có phải là lỗi dữ liệu không hay là hợp đồng hạ tầng hợp lệ?"*
2. **Khâu Phân tích Câu hỏi Chuyên sâu (D1 – D6):**
   - *"Yếu tố nhiễu (confounder) ở câu hỏi của bạn là gì? Nếu không kiểm soát quy mô ngân sách phòng ban thì kết luận bị thiên lệch ra sao?"*
   - *"Chỉ số HHI đo lường điều gì? Ngưỡng nào được coi là thị trường nhà thầu tập trung cao độ?"*
   - *"Tại sao lại dùng Bootstrap Confidence Interval thay vì chỉ so sánh giá trị trung bình đơn thuần?"*
3. **Khâu Machine Learning:**
   - *"Tại sao lại chia tập Train/Test theo thời gian (Temporal Split) mà không dùng `train_test_split` ngẫu nhiên?"*
   - *"Làm thế nào để chứng minh mô hình không bị rò rỉ dữ liệu (Data Leakage) khi xử lý các nhãn nhà thầu mới xuất hiện?"*
   - *"Mô hình dự đoán sai ở những danh mục chi tiêu nào nhiều nhất? Nguyên nhân nghiệp vụ là gì?"*

### 6.2. Kế hoạch Mock Defense Nội bộ (3 ngày trước buổi báo cáo)
- **Buổi 1 (Day T-3):** TV1 đóng vai Giảng viên chất vấn TV2 và TV3 về Tiền xử lý, EDA và các câu hỏi D1–D6.
- **Buổi 2 (Day T-2):** TV2 & TV3 đóng vai Giảng viên chất vấn TV1 về Pipeline ML, Baseline, Metric và Data Leakage.
- **Buổi 3 (Day T-1):** Tổng duyệt bài thuyết trình đúng 15 phút, tập trung kể câu chuyện dữ liệu (Data Storytelling).
