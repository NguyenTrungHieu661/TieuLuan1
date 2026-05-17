# 📝 Tiểu Luận: Nghiên Cứu và Triển Khai Mô Hình Tóm Tắt Văn Bản (BART-base & T5-small) trên Bộ Dữ Liệu CNN/DailyMail

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![Framework](https://img.shields.io/badge/Framework-HuggingFace%20%7C%20PyTorch-orange.svg)](https://huggingface.co/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

Repository này chứa toàn bộ mã nguồn, tài liệu triển khai và kết quả thực nghiệm thuộc đề tài tiểu luận: **"Ứng dụng kiến trúc Transformer (BART và T5) cho bài toán tóm tắt văn bản tự động"**. Dự án được thực hiện và chạy thực nghiệm hoàn toàn trên môi trường Google Colab với hạ tầng tăng tốc phần cứng GPU T4.

---

## 📌 Thành Phần Hệ Thống (Repository Structure)

Kho lưu trữ bao gồm các file Notebook (`.ipynb`) độc lập xử lý từng phân đoạn của dự án:

* **`BART_base..ipynb`**: Quy trình tiền xử lý, tinh chỉnh (Fine-tuning) mô hình `facebook/bart-base` trên subset dữ liệu và trích xuất các chỉ số đánh giá.
* **`T5_small.ipynb`**: Quy trình cấu hình, huấn luyện mô hình `t5-small` và đóng gói kết quả đầu ra.
* **`Interface.ipynb`**: Xây dựng giao diện đồ họa tương tác (Web UI) bằng thư viện **Gradio**, cho phép người dùng nhập bài báo bất kỳ và nhận kết quả tóm tắt trực quan từ mô hình đã huấn luyện.
* **`train_subset.csv` & `test_subset.csv`**: Bộ dữ liệu thực nghiệm rút gọn tuân thủ nghiêm ngặt cấu trúc gốc phục vụ kiểm thử hệ thống nhanh (đáp ứng giới hạn dung lượng lưu trữ của GitHub).

---

## 📊 Bộ Dữ Liệu Thực Nghiệm (Dataset)

Dự án sử dụng bộ dữ liệu chuẩn quốc tế **CNN/DailyMail (v3.0.0)** gồm các bài báo tiếng Anh kèm từ khóa/câu tóm tắt điểm tin do con người biên soạn (`highlights`).

* **Nguồn dữ liệu gốc:** [Hugging Face Datasets - CNN/DailyMail](https://huggingface.co/datasets/cnn_dailymail)
* **Chiến lược huấn luyện:** Để tối ưu hóa tài nguyên tính toán trên môi trường Colab miễn phí, mã nguồn cấu hình trích xuất ngẫu nhiên một phân đoạn dữ liệu (`subset`) bao gồm **10,000 mẫu huấn luyện** (`train`) và **500 mẫu kiểm thử** (`test`).
* **Cơ chế nạp dữ liệu:** Hệ thống hỗ trợ 2 phương thức nạp linh hoạt:
    1. Tải trực tiếp thời gian thực (Real-time Cloud) từ Hugging Face Hub thông qua hàm `load_dataset`.
    2. Đọc cục bộ các file thực nghiệm dạng `.csv` có sẵn trong repository để kiểm tra tính toàn vẹn của pipeline.

---

## ⚙️ Hướng Dẫn Cài Đặt & Chạy Thực Nghiệm (Installation & Usage)

Để chạy lại thực nghiệm hoặc kiểm tra giao diện demo, bạn có thể thực hiện theo các bước sau:

### Bước 1: Môi trường chạy code
Khuyến khích mở trực tiếp các file `.ipynb` bằng **Google Colab** và chuyển đổi loại môi trường (Runtime type) sang **GPU T4** để tăng tốc độ xử lý.

### Bước 2: Cài đặt các thư viện phụ thuộc
Hệ thống yêu cầu các thư viện xử lý ngôn ngữ tự nhiên tối tân nhất từ Hugging Face. Đoạn mã cài đặt tự động đã được tích hợp ở đầu mỗi file notebook:
```bash
pip install transformers datasets evaluate rouge_score sacrebleu matplotlib gradio pandas openpyxl -q
