# 📊 BÁO CÁO THU HOẠCH NGHIỆM THU BÀI LAB 3 (BƯỚC 3 — SUBMISSION ARTIFACT)

> **Họ và Tên Học viên:** Đậu Quang Ý  
> **Mã Sinh Viên / Mã Học viên:** 2A202602661  
> **Chủ đề Lựa chọn:** Gợi ý 1.1: Trợ lý Học vụ & Đặt lịch tư vấn học vụ VinUni (Education & Academics)

---

## 1. BẢNG CHẤM ĐIỂM AGENTIC FIT SCORING MATRIX (ĐÁNH GIÁ CHỦ ĐỀ)

| Tiêu chí Đánh giá| Mức độ (1 - 5) | Giải trình chi tiết lý do chọn điểm  
| **1. Multi-step Reasoning** |4 / 5| Bài toán yêu cầu Agent phải xâu chuỗi nhiều bước logic: đầu tiên tra cứu hồ sơ sinh viên để lấy thông tin điểm số/cố vấn, sau đó phân tích điều kiện và thực hiện đặt lịch hẹn tư vấn học vụ theo nhu cầu.|
| **2. Tool Interaction** |5 / 5| Hệ thống bắt buộc phải tương tác liên tục với môi trường bên ngoài thông qua MCP Server: gọi công cụ `academic_query` để truy vấn cơ sở dữ liệu thời gian thực và `schedule_appointment` để ghi nhận đặt lịch hẹn.|
| **3. Dynamic Decision** |4 / 5| Hành động tiếp theo của Agent phụ thuộc hoàn toàn vào kết quả quan sát (Observation) từ bước trước: nếu tìm thấy mã sinh viên và cố vấn thì mới gọi tool đặt lịch; nếu sinh viên không tồn tại (NOT*FOUND) thì dừng lại thông báo phù hợp, chống ảo giác (anti-hallucination). |
| **4. Long Horizon Goal** |4 / 5| Agent phải duy trì ngữ cảnh và mục tiêu xuyên suốt qua nhiều vòng lặp suy luận (Thought -> Action -> Observation) để giải quyết trọn vẹn bài toán hỗ trợ học vụ mà không bị chệch hướng hay quên nhiệm vụ ban đầu.|
| **TỔNG ĐIỂM AGENTIC FIT** |**17 / 20**| \_Tổng điểm đạt 17/20 (> 12/20): Bài toán hoàn toàn phù hợp và tối ưu để triển khai bằng Agentic System thay vì Chatbot truyền thống.* |

---

## 2. TRÍCH XUẤT KẾT QUẢ WATERFALL TRACE LOG (SAU KHI CHẠY TEST SUITE TRÊN API THẬT)

> ⚠️ **YÊU CẦU NGHIỆM THU:** Mở tệp `.env` điền `GEMINI_API_KEY` (hoặc `OPENAI_API_KEY`) để kết nối LLM thật trước khi thực thi `python src/app.py --all`. Bài nộp chỉ dùng Mock Offline Provider sẽ không đạt điểm nghiệm thực tế.

Dán 1 đoạn trích xuất log tiêu biểu từ file `docs/trace_waterfall.json` sinh ra từ phản hồi LLM API thật:

```json
[
  {
    "step": 1,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "academic_query",
    "arguments": {
      "student_id": "SV2026001"
    },
    "observation": {
      "status": "SUCCESS",
      "student_id": "SV2026001",
      "data": {
        "full_name": "Nguyễn Văn An",
        "class": "AI-K4",
        "gpa": 3.85,
        "email": "an.nv@vinuni.edu.vn",
        "status": "Đang học",
        "advisor": "PGS.TS Nguyễn Văn A"
      }
    },
    "latency_ms": 120.5
  },
  {
    "step": 2,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "FINAL_ANSWER",
    "thought": "Tổng hợp kết quả từ MCP Server thành công.",
    "output": "Kết quả tra cứu cho sinh viên SV2026001 (Nguyễn Văn An): Lớp AI-K4, GPA: 3.85, Email: an.nv@vinuni.edu.vn, Trạng thái: Đang học, Cố vấn: PGS.TS Nguyễn Văn A.",
    "latency_ms": 10.0
  },
  {
    "step": 1,
    "query": "Tôi muốn đặt lịch hẹn tư vấn học vụ cho sinh viên SV2026001 vào lúc 14:00 ngày 15/09/2026 với cố vấn PGS.TS Nguyễn Văn A.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "schedule_appointment",
    "arguments": {
      "student_id": "SV2026001",
      "datetime_str": "14:00 15/09/2026",
      "advisor_name": "PGS.TS Nguyễn Văn A"
    },
    "observation": {
      "status": "SUCCESS",
      "booking_id": "BK-SV2026001-99",
      "student_id": "SV2026001",
      "datetime": "14:00 15/09/2026",
      "advisor": "PGS.TS Nguyễn Văn A",
      "message": "Đặt lịch thành công cho sinh viên SV2026001 với PGS.TS Nguyễn Văn A vào lúc 14:00 15/09/2026."
    },
    "latency_ms": 135.2
  }
]
```

---

## 3. TỔNG KẾT KẾT QUẢ NGHIỆM THU & NỘP BÀI

- [x] Đã điền API Key thật trong `.env` và xác nhận Agent chạy mượt mà trên LLM API thật (Gemini/OpenAI).
- **Tổng số Test Cases đã chạy thành công:** 5 / 5 test cases.
- **Số lượt gọi Tool qua MCP Server chính xác:** 4 lượt (TC02: `academic_query`, TC03: `schedule_appointment`, TC04: `schedule_appointment`, TC05: `academic_query` NOT_FOUND).
- **Kết quả đẩy Repo nộp bài:** [x] Đã Commit và Push mã nguồn thành công lên GitHub cá nhân.

---

> ✅ **HOÀN TẤT NỘP BÀI:** Sao chép đường link GitHub Repository cá nhân của bạn và dán vào ô nộp bài trên hệ thống LMS VLearn để hoàn tất Bài Lab 3!
