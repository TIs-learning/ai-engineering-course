# Roadmap AI Engineer
![Roadmap](blueprint.png)
---

## FASE 0: Persiapan (1-2 minggu)

### Materi
- Python 3.11+, Git & GitHub, command line dasar
- REST API concept: HTTP method, JSON, status code, endpoint, headers, authentication (API key/token)
- Async programming di Python (`async`/`await`, kenapa dibutuhkan untuk panggil API)

### Project
- Buat script Python yang memanggil API publik apa saja (misal API cuaca) dan tampilkan hasilnya dengan rapi.

---

## FASE 1: Fondasi Python & Dasar ML/DL secukupnya (4-5 minggu)

### Materi eksplisit

**Python untuk AI Engineering:**
- NumPy, Pandas untuk manipulasi data
- Virtual environment (`venv`/`conda`), dependency management (`pip`, `poetry`)
- Testing dasar (pytest), typing (`type hints`), Pydantic untuk validasi data

**Fondasi ML/DL (secukupnya, bukan untuk melatih model besar tapi untuk paham cara kerja & debug):**
- Bagaimana neural network belajar (forward/backward pass, loss, gradient descent) — level konsep, tidak perlu implementasi manual mendalam
- Bagaimana Transformer bekerja: tokenization, embedding, self-attention, positional encoding — ini WAJIB dipahami karena semua LLM berbasis ini
- Perbedaan model architecture: encoder-only (BERT), decoder-only (GPT-style), encoder-decoder (T5)
- Konsep training vs inference, parameter, context window, temperature, top-p/top-k sampling

### Sumber
- "Deep Learning Specialization" Andrew Ng (course 5: Sequence Models, cukup sampai bagian Transformer)
- Artikel "The Illustrated Transformer" - Jay Alammar
- Video "Let's build GPT" - Andrej Karpathy (YouTube)

### Project (menggabungkan materi fase ini)
- Buat notebook yang menjelaskan step-by-step bagaimana teks diubah jadi token, lalu jadi embedding, sampai bagaimana attention score dihitung untuk kalimat sederhana (pakai library `tiktoken` untuk tokenization, dan hitung attention secara manual dengan NumPy untuk 1 kalimat pendek).

---

## FASE 2: LLM API & Prompt Engineering (3-4 minggu)

### Materi eksplisit
- Cara kerja LLM API: Anthropic API, OpenAI API, Google Gemini API (request/response format, streaming, function calling/tool use)
- Prompt engineering: zero-shot, few-shot, chain-of-thought, role prompting, system prompt vs user prompt
- Structured output: memaksa model mengeluarkan JSON, penggunaan schema (Pydantic + function calling)
- Context management: cara handle context window terbatas, summarization untuk percakapan panjang
- Cost & latency awareness: token counting, memilih model sesuai kebutuhan (kecil vs besar)

### Sumber
- Dokumentasi resmi: docs.anthropic.com (prompt engineering guide), platform.openai.com/docs
- "Prompt Engineering Guide" - promptingguide.ai
- Course "ChatGPT Prompt Engineering for Developers" - DeepLearning.AI

### Project (menggabungkan materi fase ini)
**CLI Assistant dengan Structured Output**
- Buat aplikasi command-line yang menerima input natural language (misal "catat pengeluaran saya 50rb untuk makan siang")
- Gunakan tool/function calling agar model mengeluarkan output terstruktur (JSON tervalidasi Pydantic: kategori, jumlah, tanggal)
- Simpan hasil parsing ke file CSV/SQLite
- Tambahkan penanganan error saat model gagal mengeluarkan format yang benar (retry logic)

---

## FASE 3: RAG (Retrieval-Augmented Generation) (4-5 minggu)

### Materi eksplisit
- Kenapa RAG dibutuhkan (mengatasi keterbatasan knowledge cutoff & halusinasi)
- Embedding model: cara kerja text embedding, cosine similarity
- Vector database: ChromaDB, FAISS, Pinecone, atau Weaviate (indexing, similarity search, metadata filtering)
- Chunking strategy: fixed-size, semantic chunking, overlap, kenapa ukuran chunk penting
- Document loading & parsing: PDF, HTML, Markdown (library: `unstructured`, `PyPDF2`, `LangChain document loaders`)
- Reranking: menggunakan reranker model untuk memperbaiki hasil retrieval
- Framework: LangChain atau LlamaIndex (pahami konsepnya, jangan terlalu bergantung — banyak perusahaan mulai pakai pendekatan lebih ringan/manual)
- Evaluasi RAG: metrik retrieval (precision@k, recall@k), evaluasi jawaban (faithfulness, relevance) pakai tools seperti RAGAS

### Sumber
- "Building RAG Applications" - DeepLearning.AI (short course)
- Dokumentasi LangChain & LlamaIndex
- Artikel Pinecone Learning Center tentang vector search

### Project (menggabungkan materi fase ini)
**Chatbot RAG untuk Dokumen Internal**
- Kumpulkan 20-30 dokumen (PDF/Markdown) tentang topik tertentu (misal dokumentasi produk atau kumpulan artikel)
- Bangun pipeline: load → chunk → embed → simpan ke vector database
- Buat sistem retrieval + generation: user tanya, sistem ambil chunk relevan, LLM jawab berdasarkan konteks itu saja
- Tambahkan reranking untuk memperbaiki kualitas retrieval
- Evaluasi sistem dengan minimal 15 pasang pertanyaan-jawaban, ukur precision@k dan faithfulness
- Deploy sebagai web app sederhana (Streamlit/Gradio)

---

## FASE 4: AI Agents (4-6 minggu)

### Materi eksplisit
- Konsep agent: reasoning loop (ReAct pattern - reason + act), planning, tool use
- Multi-step tool calling: agent memutuskan kapan dan tool apa yang dipanggil
- Memory pada agent: short-term (conversation history) vs long-term (vector store)
- Multi-agent system: bagaimana beberapa agent berkolaborasi (orchestrator-worker pattern)
- Guardrails & safety: validasi output, mencegah prompt injection, membatasi aksi agent yang berbahaya
- Framework: LangGraph, CrewAI, atau Anthropic's agent SDK (pahami minimal satu secara mendalam)
- Model Context Protocol (MCP): standar untuk menghubungkan agent ke tools/data eksternal

### Sumber
- Dokumentasi LangGraph, artikel Anthropic "Building Effective Agents"
- Course "AI Agents in LangGraph" - DeepLearning.AI
- Dokumentasi Model Context Protocol (modelcontextprotocol.io)

### Project (menggabungkan materi fase ini)
**Agent Otomatisasi Riset**
- Buat agent yang menerima topik riset dari user, lalu secara otomatis: mencari informasi (tool web search), membaca & merangkum beberapa sumber, menyusun laporan terstruktur
- Tambahkan tool tambahan (misal kalkulator, akses database internal dari project RAG di fase 3)
- Implementasikan guardrail sederhana (validasi supaya agent tidak mengakses tool di luar yang diizinkan)
- Log setiap langkah reasoning agent untuk debugging

---

## FASE 5: Fine-tuning & Model Kecil (Opsional tapi bernilai, 3-4 minggu)

### Materi eksplisit
- Kapan fine-tuning dibutuhkan vs cukup dengan prompting/RAG
- Parameter-Efficient Fine-Tuning: LoRA, QLoRA (konsep + praktik)
- Dataset preparation untuk fine-tuning (format instruction-following)
- Fine-tuning model open-source kecil (Llama, Mistral, atau Qwen) pakai HuggingFace `transformers` + `peft`
- Quantization dasar (menjalankan model besar di hardware terbatas)

### Sumber
- HuggingFace course (huggingface.co/learn)
- Dokumentasi `peft` library dari HuggingFace

### Project (menggabungkan materi fase ini)
- Fine-tune model open-source kecil (misal Mistral-7B atau Llama-3-8B) dengan LoRA untuk tugas spesifik (misal menjawab dalam gaya/format tertentu), lalu bandingkan hasilnya dengan pendekatan prompting biasa pada model yang sama.

---

## FASE 6: LLMOps & Production (4-5 minggu)

### Materi eksplisit
- API serving: FastAPI untuk membungkus aplikasi AI jadi service
- Containerization: Docker
- Observability khusus LLM: logging prompt & response, tracing (LangSmith, Langfuse, atau Weights & Biases Weave)
- Evaluasi berkelanjutan: automated eval pipeline, A/B testing prompt
- Caching (mengurangi biaya panggilan API berulang), rate limiting
- Cost monitoring & optimization (memilih model sesuai task, prompt caching)
- CI/CD dasar (GitHub Actions)
- Keamanan: prompt injection defense, PII redaction, content moderation (guardrails library seperti Guardrails AI atau NeMo Guardrails)

### Sumber
- Dokumentasi Langfuse/LangSmith
- "LLMOps" course (DeepLearning.AI atau Coursera)
- Blog engineering perusahaan (Anthropic, OpenAI, Netflix Tech Blog tentang LLM in production)

### Project (menggabungkan materi fase ini)
**Produksi-kan Agent RAG dari Fase 3-4**
- Bungkus chatbot RAG + agent jadi REST API dengan FastAPI
- Tambahkan tracing lengkap (setiap request, retrieval, tool call, response tercatat)
- Implementasikan guardrail keamanan (deteksi prompt injection sederhana, redaksi data sensitif)
- Containerize dengan Docker, setup CI/CD sederhana
- Buat dashboard monitoring biaya (token usage per request) dan kualitas (skor evaluasi otomatis)
- Deploy ke cloud (Railway, Render, atau cloud provider pilihan)

---

## FASE 7: Portfolio & Persiapan Kerja (ongoing)

### Materi eksplisit
- System design untuk AI product (bagaimana menjawab "design a customer support AI agent for X")
- Studi kasus AI engineering dari perusahaan (Anthropic, OpenAI, Perplexity, blog engineering startup AI)
- Latihan coding interview dasar (Python, struktur data umum)
- Pemahaman trade-off bisnis: kapan pakai model besar mahal vs model kecil murah, kapan RAG vs fine-tuning vs prompting saja

### Project Capstone Akhir
Gabungkan semua fase jadi satu produk AI end-to-end:
1. Pilih masalah nyata (misal: asisten AI untuk industri tertentu — customer support, riset, atau produktivitas)
2. Bangun RAG untuk knowledge base spesifik
3. Tambahkan agent dengan minimal 3 tools berbeda
4. Evaluasi otomatis dengan metrik jelas
5. Deploy production-ready dengan monitoring, guardrail, dan cost tracking
6. Dokumentasikan arsitektur sistem (diagram) dan tulis artikel di Medium/LinkedIn

Portfolio minimal 3 project seperti di atas, terdokumentasi rapi di GitHub dengan README yang jelas.

---

## Estimasi Total Waktu
5-8 bulan dengan belajar konsisten 15-20 jam/minggu.

## Urutan Belajar Ringkas
Fase 0 → 1 (fondasi) → 2 (prompt & API) → 3 (RAG) → 4 (Agent) → 5 (opsional: fine-tuning) → 6 (production) → 7 (paralel dengan job hunting)

## Catatan Penting
Fondasi di Fase 1 sengaja dibuat ringkas dibanding roadmap ML Engineer — cukup untuk paham cara kerja model, bukan untuk melatih model dari nol. Kalau nanti kamu ingin mendalami training/fine-tuning model besar secara serius, itu baru masuk ranah ML Engineering/Research yang lebih dalam.
