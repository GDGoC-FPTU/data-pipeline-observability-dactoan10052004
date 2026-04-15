[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23572332&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** email@example.com
**Họ và tên:** Hồ Đắc Toàn
**Student ID:** AI20K-0057

---

## Mô tả

Bài lab này xây dựng một ETL Pipeline tự động hóa để xử lý dữ liệu sản phẩm từ file JSON:
- **Extract:** Đọc dữ liệu từ `raw_data.json`
- **Validate:** Loại bỏ record có giá <= 0 hoặc category rỗng
- **Transform:** Giảm giá 10%, chuẩn hóa category (Title Case), thêm timestamp
- **Load:** Lưu kết quả ra `processed_data.csv`

Ngoài ra, thí nghiệm Stress Test được thực hiện để chứng minh tầm quan trọng của Data Quality đối với AI Agent.

---

## Cách chạy (How to Run)

### Yêu cầu cài đặt
```bash
pip install pandas pytest
```

### Chạy ETL Pipeline
```bash
python solution.py
```

### Chạy Agent Simulation (Stress Test)
```bash
# Bước 1: Tạo dữ liệu rác
python generate_garbage.py

# Bước 2: Chạy Agent với cả 2 bộ dữ liệu
python agent_simulation.py
```

### Chạy Tests
```bash
pytest tests/
```

---

## Cấu trúc thư mục

```
├── solution.py              # Script ETL Pipeline
├── agent_simulation.py      # Mô phỏng RAG-like Agent
├── generate_garbage.py      # Script tạo garbage_data.csv
├── raw_data.json            # Dữ liệu đầu vào
├── processed_data.csv       # Output của pipeline (3 records)
├── garbage_data.csv         # Dữ liệu rác cho stress test
├── experiment_report.md     # Báo cáo thí nghiệm
└── README.md                # File này
```

---

## Kết quả

- **Tổng số records đọc vào:** 5
- **Records hợp lệ (processed):** 3 (Laptop, Chair, Monitor)
- **Records bị loại bỏ:** 2
  - id=3 (Mystery Box): giá âm (-10)
  - id=4 (Phone): category rỗng
- **Output:** `processed_data.csv` với các cột: id, product, price, category, discounted_price, processed_at
