# Reflection - RAG Pipeline (Module 1-5)
**Họ và tên:** Vũ Tuấn Hoàng

## Phần 1 - Mapping bài giảng
Qua quá trình lập trình 5 modules, em đã đối chiếu được các lý thuyết trên lớp vào thực tế như sau:
1. **Module 1 (Chunking):** Việc chia nhỏ văn bản không chỉ đơn thuần là cắt chuỗi theo số lượng ký tự, mà cần áp dụng **Semantic Chunking** và **Hierarchical Chunking** để giữ nguyên vẹn ngữ nghĩa của đoạn văn, giúp vector embedding hiểu đúng context hơn.
2. **Module 2 (Hybrid Search):** Thay vì chỉ dùng Vector Search (Dense) rất dễ bỏ sót các từ khóa đặc thù (mã nhân viên, tên riêng), việc kết hợp thêm BM25 (Keyword Search) và trộn điểm bằng thuật toán **RRF (Reciprocal Rank Fusion)** giúp lấy được các tài liệu chuẩn xác từ cả 2 phía.
3. **Module 3 (Reranking):** Vector Search mặc dù tìm nhanh nhưng độ liên quan không cao (vì thuật toán dot-product/cosine đơn giản). Sử dụng mô hình **Cross-Encoder Reranker** để xem xét chéo query và document giúp sắp xếp lại Top K chính xác hơn rất nhiều.
4. **Module 4 (RAGAS Evaluation):** Đã hiểu cách tự động hóa việc chấm điểm hệ thống bằng bộ 4 chỉ số: *Faithfulness* (độ trung thực), *Answer Relevancy* (đúng trọng tâm), *Context Precision* (tài liệu đúng nằm ở top đầu) và *Context Recall* (lấy đủ tài liệu).
5. **Module 5 (Enrichment):** Các kỹ thuật như *Contextual Prepend*, *Summarize*, và *HyQA* giúp làm giàu dữ liệu gốc. Chunk có thêm câu hỏi giả định hoặc câu bối cảnh sẽ giúp match với query của người dùng dễ dàng hơn.

## Phần 2 - Khó khăn & Giải quyết
1. **Lỗi Encoding trên Windows Terminal:** 
   - **Lỗi:** Bị crash `UnicodeEncodeError: 'charmap' codec can't encode characters` khi in icon emoji `⚠️` trong file Python.
   - **Cách giải quyết:** Set biến môi trường `$env:PYTHONIOENCODING="utf-8"` trong PowerShell trước khi chạy lệnh Python để hỗ trợ hiển thị ký tự UTF-8.
2. **Lỗi tương thích RAGAS và Langchain:**
   - **Lỗi:** `No module named 'langchain_core.pydantic_v1'` do phiên bản Langchain v0.3 đã loại bỏ thư viện này.
   - **Cách giải quyết:** Hạ cấp Langchain xuống bản `<0.3` (`pip install -U "langchain-core<0.3" "langchain<0.3"`).
3. **Lỗi RAGAS gọi nhầm OpenAI Model (Error 400):**
   - **Lỗi:** Thư viện RAGAS ngầm định gọi `gpt-3.5-turbo` lên endpoint của DeepSeek, dẫn đến API từ chối do không hỗ trợ model đó. Tương tự, DeepSeek cũng không có API Embedding.
   - **Cách giải quyết:** Inject trực tiếp Custom LLM (`ChatOpenAI` dùng model `deepseek-v4-flash`) và Custom Embedding (`HuggingFaceEmbeddings` dùng mô hình local `all-MiniLM-L6-v2`) vào thẳng hàm `evaluate()` của Ragas.

## Phần 3 - Action Plan
Dựa trên những kiến thức đã thực hành, em dự định áp dụng vào dự án thực tế sắp tới như sau:
- **Xây dựng Hybrid Search làm tiêu chuẩn:** Sẽ không dùng Dense Search đơn thuần nữa mà luôn đi kèm với BM25 + RRF trong Qdrant/ElasticSearch để không bị trượt các truy vấn có tính chất tra cứu mã số, tên miền cụ thể.
- **Áp dụng Contextual Prepend:** Khi cắt tài liệu dài thành chunk ngắn, sẽ dùng LLM gán thêm một câu "Tiêu đề/Ngữ cảnh" vào đầu mỗi chunk để tránh hiện tượng mất gốc ngữ nghĩa (ví dụ: đang nói về "quyền lợi" nhưng không biết của "nhân viên chính thức" hay "thực tập sinh").
- **Tích hợp RAGAS vào CI/CD:** Sử dụng tự động các kịch bản test RAGAS mỗi khi có thay đổi về Prompt hoặc thay đổi chunk size để đo lường định lượng xem pipeline mới tốt lên hay tệ đi.
