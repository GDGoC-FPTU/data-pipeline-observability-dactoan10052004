# Báo Cáo Thí Nghiệm: Ảnh Hưởng của Data Quality đối với AI Agent

**Student ID:** AI20K-0057
**Họ và tên:** Hồ Đắc Toàn
**Ngày thực hiện:** 2026-04-15

---

## 1. Kết quả thí nghiệm

Chạy `agent_simulation.py` với 2 bộ dữ liệu và ghi lại kết quả:

| Kịch bản | Phản hồi của Agent | Độ chính xác (1-10) | Ghi chú |
|----------|--------------------|---------------------|---------|
| Clean Data (`processed_data.csv`) | "Agent: Based on my data, the best choice is Laptop at $1200." | 9 | Agent trả lời chính xác, tìm được sản phẩm đúng nhất trong danh mục Electronics. Dữ liệu đã được chuẩn hóa và loại bỏ các record không hợp lệ. |
| Garbage Data (`garbage_data.csv`) | "Agent: Based on my data, the best choice is Nuclear Reactor at $999999." | 2 | Agent trả lời sai hoàn toàn vì Garbage Data chứa Outlier là "Nuclear Reactor" với giá $999,999. |

---

## 2. Phan tich & Nhận xét

### Tại sao Agent trả lời sai khi dùng Garbage Data?

Khi Agent sử dụng bộ dữ liệu `garbage_data.csv`, nó gặp phải nhiều vấn đề chất lượng nghiêm trọng:

- **Outlier (Giá trị bất thường):** Record "Nuclear Reactor" có giá $999,999 là một outlier cực đoan. Vì logic của Agent là tìm sản phẩm có giá cao nhất trong danh mục "electronics", nó sẽ chọn ngay outlier này, dẫn đến kết quả hoàn toàn sai lệch so với nhu cầu thực tế của người dùng.

- **Duplicate IDs:** Có 2 record cùng ID=1 (Laptop và Banana). Điều này gây ra sự nhầm lẫn trong việc truy xuất dữ liệu, có thể dẫn đến kết quả không nhất quán nếu Agent xử lý theo ID.

- **Sai kiểu dữ liệu (Wrong Data Types):** Giá của "Broken Chair" là chuỗi "ten dollars" thay vì số. Nếu Agent có logic tính toán trên giá, nó sẽ bị crash hoặc cho ra kết quả `NaN`.

- **Giá trị Null (Null Values):** Record "Ghost Item" có ID=None và category=None. Các giá trị null này có thể khiến Agent bị lỗi khi truy cập thuộc tính, dẫn đến exception không mong muốn.

Tóm lại, Garbage Data làm cho Agent mất khả năng phân biệt đâu là thông tin thực sự có giá trị, dẫn đến các phân tích sai lệch và câu trả lời không đáng tin cậy.

---

## 3. Kết luận

**Data Quality > Quality Prompt?** ✅ Đồng ý.

Một Agent dù có prompt tốt đến đâu cũng sẽ cho ra kết quả sai nếu dữ liệu đầu vào có chất lượng kém. Trong thí nghiệm này, chỉ cần một record Outlier là Agent đã bị đổi hướng hoàn toàn. Điều này chứng tỏ rằng **Data Quality là nền tảng cốt lõi** — một ETL Pipeline mạnh mẽ với validation chặt chẽ là điều kiện tiên quyết trước khi tối ưu hóa bất kỳ prompt hay model nào.
