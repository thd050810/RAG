# 🤖 RAG với LangChain - Phiên bản cải tiến

> Pipeline RAG đầy đủ: **PDF → Embedding → ChromaDB → LLM → Gradio UI**  
> Module 12 | AIVN

---

## 📋 Tổng quan

Hệ thống **Retrieval-Augmented Generation (RAG)** hỏi đáp tài liệu tiếng Việt, sử dụng:
- **LangChain** để orchestrate pipeline
- **ChromaDB** làm vector database
- **Qwen2.5-3B-Instruct** làm LLM (hỗ trợ tiếng Việt, chạy trên Colab T4 miễn phí)
- **Gradio** làm giao diện người dùng

---

## 🏗️ Kiến trúc Pipeline

```
PDF Files
   ↓
Document Loader (PyPDFLoader)
   ↓
Text Cleaner (chuẩn hóa Unicode tiếng Việt)
   ↓
Text Splitter (chunk_size=400, overlap=100)
   ↓
Embedding Model (paraphrase-multilingual-mpnet-base-v2)
   ↓
ChromaDB (Vector Store)
   ↓
MMR Retriever
   ↓
LLM (Qwen2.5-3B-Instruct)
   ↓
Gradio UI
```

---

## ✨ Tính năng nổi bật

| Tính năng | Chi tiết |
|-----------|----------|
| 🌏 Hỗ trợ tiếng Việt | Chuẩn hóa Unicode NFC, loại bỏ ký tự lỗi từ PDF |
| 🧠 Embedding nâng cao | `paraphrase-multilingual-mpnet-base-v2` (768 chiều) thay vì MiniLM |
| 🔍 Tìm kiếm MMR | Maximal Marginal Relevance — cân bằng relevance & diversity |
| ⚡ Tối ưu tốc độ | float16, InMemoryCache, max_new_tokens=250 |
| 💬 Giao diện chat | Lịch sử hội thoại, câu hỏi mẫu, Enter để gửi |
| 📊 Đánh giá pipeline | Đo latency, số chunk retrieved, chất lượng câu trả lời |

---

## 🛠️ Cài đặt

```bash
pip install \
  "torch>=2.0.0" \
  "transformers>=4.40.0" \
  "accelerate>=0.30.0" \
  "sentence-transformers>=2.7.0" \
  "langchain>=0.2.0" \
  "langchain-community>=0.1.0" \
  "chromadb>=0.5.0" \
  "langchain-chroma>=0.2.0" \
  "pypdf>=4.2.0" \
  "gradio>=5.0.0" \
  "langchain-huggingface" \
  "wget"
```

---

## 🚀 Cách sử dụng

1. **Mở notebook trên Google Colab** 

2. **Chạy lần lượt các cell:**
   - Cell 1–2: Cài đặt & khởi tạo project
   - Cell 3: Tải file PDF nguồn
   - Cell 4: Load LLM (Qwen2.5-3B-Instruct)
   - Cell 5–6: Xử lý văn bản & chia chunk
   - Cell 7: Tạo Vector Database (ChromaDB)
   - Cell 8: Xây dựng RAG Chain
   - Cell 9–10: Chạy thử & đánh giá
   - Cell 11: Khởi động giao diện Gradio

3. **Đặt câu hỏi** về tài liệu qua giao diện Gradio

---

## 📁 Cấu trúc thư mục

```
rag_langchain/
├── data_source/
│   └── generative_ai/      # Thư mục chứa file PDF
├── src/
│   ├── base/
│   └── rag/
└── RAGLangChain.ipynb      # Notebook chính
```

---

## ⚙️ Thông số kỹ thuật

| Thành phần | Giá trị |
|-----------|---------|
| LLM | `Qwen/Qwen2.5-3B-Instruct` |
| Embedding | `paraphrase-multilingual-mpnet-base-v2` |
| Chunk size | 400 ký tự |
| Chunk overlap | 100 ký tự (~25%) |
| Vector DB | ChromaDB |
| Search strategy | MMR (k=5) |
| Precision | float16 |

---

## 📊 Đánh giá

Pipeline được đánh giá tự động qua các chỉ số:
- **Latency** — thời gian phản hồi (giây)
- **Chunks retrieved** — số đoạn văn được truy xuất
- **Answer quality** — chấm điểm 1–5 dựa trên độ dài & cấu trúc câu trả lời
- **Answer length** — số từ trong câu trả lời

Kết quả được trực quan hóa bằng biểu đồ matplotlib.

---

