# Hướng dẫn nộp bài & checklist (SUBMISSION)

> ⚠️ **Bài CÁ NHÂN:** mỗi MSSV nộp **một** repo / một link lên LMS.  
> Điểm: [`RUBRIC.md`](RUBRIC.md) · Quy định: [`RULES.md`](RULES.md) · Cách làm: [`CHECKPOINTS.md`](CHECKPOINTS.md) (Checkpoint 1 → 5).  
> Artifact chấm = file trong `outputs/` — **không** viết `report/*.md` tay.  
> Checkpoint 5: `scripts/grade.py` **tự sinh** `outputs/grade_report.json` + `outputs/lab_report.md`.  
> Protected data (red-team phải leak): `data/protected/vinbank_secrets.json`.  
> **Blue:** OpenRouter `liquid/lfm-2.5-2.6b` (cố định).  
> **Red / Red Advance:** `gpt-4o-mini` (OpenAI) **hoặc** `gemini-3.5-flash` (Gemini).  
> Điểm cộng: chọn **một** — **Red** tối đa +5 **hoặc** **Red Advance** tối đa +10 — xem [`RUBRIC.md`](RUBRIC.md).

---

## 1. Quy tắc đặt tên repo

Theo **Quy ước chung Khóa 4** — đặt tên repo bài nộp của học viên:

- **Cấu trúc:**  
  `K4-L3-DAYxx-HoVaTen-MSSV-TenBai`  
  *(Không dấu, không khoảng trắng, ngăn cách bằng `-`. Ngày học hai chữ số: `DAY11`.)*
- **Day 11 (L3) — mẫu cụ thể:**  
  `K4-L3-DAY11-<HoVaTen>-<MSSV>-Guardrails-HITL-Responsible-AI`
- **Ví dụ (thống nhất format MSSV):**  
  `K4-L3-DAY11-NguyenVanA-2A2026xxxxx-Guardrails-HITL-Responsible-AI`

**Cách làm gợi ý**

1. Fork (hoặc clone) starter về tài khoản GitHub cá nhân.
2. **Đổi tên repo** trên GitHub cho đúng cấu trúc trên (Settings → Repository name), hoặc tạo repo mới với tên chuẩn rồi đẩy code lên.
3. Nộp **link repo** (đã đổi tên) lên cổng LMS / CodeLabs đúng hạn.

Ví dụ link nộp:  
`https://github.com/<user-cua-ban>/K4-L3-DAY11-NguyenVanA-2A2026xxxxx-Guardrails-HITL-Responsible-AI`

---

## 2. Deadline

- **Hạn chốt mặc định:** **23h59 cùng ngày làm Lab** (giờ Việt Nam — ICT / GMT+7).
- Gia hạn chỉ khi Key Coach thông báo **trong vòng 48 giờ sau Lab**.
- Sau hạn (hoặc hết gia hạn): nộp muộn bị trừ điểm; commit sửa sau hạn có thể không được tính.

---

## 3. Cấu trúc repo phải nộp

```text
K4-L3-DAY11-<HoVaTen>-<MSSV>-Guardrails-HITL-Responsible-AI/
├── README.md                 <- Họ tên, MSSV, cách chạy ngắn (có thể giữ README starter + bổ sung)
├── CHECKPOINTS.md
├── SUBMISSION.md
├── RUBRIC.md
├── RULES.md
├── requirements.txt
├── .env.example              <- KHÔNG commit .env thật
├── schemas/results.schema.json
├── src/
│   ├── guardrails/           <- Checkpoint 2
│   ├── assignment/           <- Checkpoint 3
│   ├── attacks/              <- Checkpoint 4
│   ├── agents/               <- unsafe + guards (không sửa secret)
│   ├── hitl/ · testing/      <- tham khảo, không chấm
│   └── main.py
├── outputs/                  <- SINH KHI CHẠY — không tạo sẵn placeholder
│   ├── results.json          <- BẮT BUỘC (phòng thủ) · khớp schema
│   ├── attack_results.json   <- BẮT BUỘC (tấn công)
│   ├── audit_log.json        <- nên có
│   ├── metrics.json          <- nên có
│   ├── unsafe_attack_result.json
│   ├── guards_attack_result.json
│   ├── grade_report.json     <- tự sinh bởi scripts/grade.py
│   └── lab_report.md         <- tự sinh (không viết tay)
└── tests/                    <- giữ smoke/public để tự kiểm
```

### Artifact bắt buộc

| Loại | File |
|------|------|
| Phòng thủ (**Blue**) | `outputs/results.json` |
| Tấn công (**Red** + **Red Advance**) | `outputs/attack_results.json` |

**Bonus lab** (grader replay — không tự cấp từ JSON; chọn một trong hai):

- **B1** — leak **Red**: tối đa **+5**
- **B2** — leak **Red Advance**: tối đa **+10**
- **Không** cộng B1 + B2

---

## 4. Định dạng tối thiểu

### `outputs/results.json` (khớp `schemas/results.schema.json`)

```json
{
  "framework": "google-adk",
  "safe_queries": [
    {"input": "...", "blocked": false, "layer": null, "response_preview": "..."}
  ],
  "attack_queries": [
    {"input": "...", "blocked": true, "layer": "input_guardrail", "response_preview": "..."}
  ],
  "rate_limit": {
    "max_requests": 10,
    "window_seconds": 60,
    "sent": 15,
    "passed": 10,
    "blocked": 5
  },
  "edge_cases": [
    {"input": "", "blocked": true, "layer": "input_guardrail"}
  ]
}
```

- `safe_queries` ≥ 5 · `attack_queries` ≥ 7 · `edge_cases` ≥ 3  
- Mỗi query: bắt buộc `input`, `blocked`

### `outputs/attack_results.json`

```json
{
  "unsafe_attacks": [
    {"id": 1, "category": "Completion", "input": "...", "response_preview": "...", "leaked": true, "target": "unsafe"}
  ],
  "guards_attacks": [
    {"id": 1, "category": "Completion", "input": "...", "response_preview": "...", "leaked": false, "target": "guards"}
  ]
}
```

---

## 5. Checklist trước khi nộp link

- [x] Có `outputs/results.json` và **validate** được với `schemas/results.schema.json`
- [x] Có `outputs/attack_results.json` (unsafe + guards)
- [x] **Không** commit `.env` / API key
- [x] `outputs/` không chứa placeholder tự tạo tay (file do `python src/main.py --part 3|4` sinh ra)
- [x] Đã chạy tự kiểm:

**Windows (PowerShell):**

```powershell
.\.venv\Scripts\Activate.ps1
pytest tests/smoke -q
pytest tests/public -q
python scripts/grade.py --submission-dir . --out outputs/grade_report.json
```

**macOS / Linux (bash):**

```bash
source .venv/bin/activate
pytest tests/smoke -q
pytest tests/public -q
python scripts/grade.py --submission-dir . --out outputs/grade_report.json
```

- [ ] Nộp **link repo** đúng hạn lên LMS / CodeLabs

> Máy không chạy được (thiếu lib, sai path, lỗi cú pháp) → phần chấm máy = lỗi kỹ thuật — sửa đóng gói trước khi nộp.
