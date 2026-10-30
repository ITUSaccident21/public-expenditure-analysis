-### 3.1. Data Collection

* **Subject:** Khảo sát mức thu nhập và các yếu tố ảnh hưởng đến lương của nhân sự ngành Khoa học Dữ liệu và AI trong khoảng thời gian từ **2020 đến 2023** (phân bổ số dòng theo năm: 2020: 72, 2021: 230, 2022: 1271, 2023: 1785; tổng: 3,358 dòng).
* **Source:** Nền tảng Kaggle (`https://www.kaggle.com/datasets/arnabchaki/data-science-salaries-2023/data`), tác giả đăng: `randomarnab`. Dữ liệu gốc được cung cấp từ nền tảng `aijobs.net`. Ngày truy cập: 23/10/2026.
* **License:** 
  * Áp dụng cấu trúc **ODbL** (Open Database License) cho cơ sở dữ liệu và **DbCL** (Database Contents License) cho nội dung bên trong.
  * *Quyền lợi:* Cho phép sử dụng, trích xuất, phân phối và phục vụ mục đích thương mại/học tập.
  * *Nghĩa vụ:* Bắt buộc ghi công (attribution) tác giả/nguồn, giữ nguyên trạng thái mở. Nhóm tuân thủ bằng cách trích dẫn rõ nguồn trong README và không commit file dữ liệu lớn lên git công khai.
* **Collection method:**
  * Thu thập dữ liệu thứ cấp từ các cuộc khảo sát thị trường và tổng hợp tin tuyển dụng định kỳ từ `aijobs.net` (2020–2023).
  * *Giới hạn và thiên lệch:*
    1. *Lệch địa lý:* Nhân sự tại Mỹ (`US`) chiếm **82.3%** tổng số bản ghi; top 5 quốc gia chiếm hơn **93%**.
    2. *Tự khai báo:* Dữ liệu thu thập tự nguyện qua khảo sát/tin tuyển dụng, dễ dẫn đến reporting bias (thiên vị mức lương cao).
    3. *Thiếu mã định danh:* Không có `employee_id` hay `company_id`, gây khó khăn trong việc kiểm tra trùng lặp cá nhân và phân chia tập dữ liệu huấn luyện độc lập.
    4. *Quy đổi tiền tệ:* Cột `salary_in_usd` dùng tỷ giá trung bình năm, làm mờ đi các biến động tỷ giá ngắn hạn và chưa phản ánh chính xác sức mua tương đương (PPP) từng vùng.
* **Motivation:** 
  * Sau khi làm sạch và loại bỏ bản ghi trùng lặp, dataset còn lại **3,155 dòng** và **11 cột** (gồm các biến số: `work_year`, `salary`, `salary_in_usd`, `remote_ratio` và các biến phân loại: `experience_level`, `employment_type`, `job_title`, `company_size`,...).
  * *Lý do chọn:* Cấu trúc gọn gàng, đa dạng loại biến, phù hợp để phân tích thống kê đa chiều và xây dựng mô hình dự đoán lương. Khác với các public notebook thông thường chỉ trực quan hóa cơ bản, nhóm tập trung khai thác các câu hỏi chuyên sâu (kiểm soát biến nhiễu, đánh giá tác động hình thức remote thực tế, xây dựng ML Pipeline chuẩn hóa).