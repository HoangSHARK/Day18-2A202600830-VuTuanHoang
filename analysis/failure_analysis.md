# Failure Analysis Report

Dựa trên kết quả chạy RAGAS Evaluation (`ragas_report.json`), dưới đây là 5 câu hỏi có điểm số trung bình (avg_score) thấp nhất và cách khắc phục tương ứng:

## 1. Phụ cấp ăn trưa hàng tháng là bao nhiêu?
- **Worst Metric**: faithfulness
- **Score**: 0.0 (Avg: 0.0)
- **Diagnosis**: LLM hallucinating (LLM tự bịa ra câu trả lời hoặc ảo giác do context không chứa thông tin).
- **Suggested Fix**: Tighten prompt, lower temperature (Sửa lại prompt để ép LLM trả lời "Không biết" nếu không tìm thấy, đồng thời giảm temperature xuống 0).

## 2. Nếu cần mua một chiếc laptop 30 triệu cho nhân viên mới, ai phê duyệt và cần gì từ phòng CNTT?
- **Worst Metric**: faithfulness
- **Score**: 0.0 (Avg: 0.0)
- **Diagnosis**: LLM hallucinating.
- **Suggested Fix**: Tighten prompt, lower temperature.

## 3. Nhân viên tạm ứng 15 triệu, sau 20 ngày mới thanh toán. Bị phạt bao nhiêu?
- **Worst Metric**: faithfulness
- **Score**: 0.0 (Avg: 0.0)
- **Diagnosis**: LLM hallucinating.
- **Suggested Fix**: Tighten prompt, lower temperature.

## 4. Nhân viên được nghỉ bao nhiêu ngày phép năm?
- **Worst Metric**: faithfulness
- **Score**: 0.0 (Avg: 0.125)
- **Diagnosis**: LLM hallucinating.
- **Suggested Fix**: Tighten prompt, lower temperature.

## 5. Lương thử việc của nhân viên Junior mức cao nhất là bao nhiêu?
- **Worst Metric**: faithfulness
- **Score**: 0.0 (Avg: 0.125)
- **Diagnosis**: LLM hallucinating.
- **Suggested Fix**: Tighten prompt, lower temperature.

---
**Nhận xét tổng quan:** 
Hầu hết các câu hỏi bị điểm thấp nhất đều dính lỗi `faithfulness` = 0.0 (độ trung thực của câu trả lời so với context). Điều này chứng tỏ LLM có xu hướng "ảo giác" (hallucinate) hoặc cố gắng trả lời dựa trên kiến thức được huấn luyện sẵn thay vì từ tài liệu cung cấp. 
**Hành động tiếp theo**: Cần sửa lại prompt hệ thống khắt khe hơn: *"CHỈ sử dụng context được cung cấp. Tuyệt đối không tự bịa câu trả lời. Nếu context không có, hãy trả lời 'Không tìm thấy'."* và cài đặt `temperature=0`.
