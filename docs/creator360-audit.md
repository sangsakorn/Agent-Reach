# 🔍 Creator360.studio — Audit สิ่งที่มีอยู่แล้ว (Existing Assets Audit)

> จุดประสงค์: สำรวจงานที่ทำไว้แล้วทั้งหมดก่อนสร้าง Agent ใหม่ เพื่อ **ลดงานซ้ำซ้อนและไม่เสียทรัพยากรโดยไม่จำเป็น**
> วันที่สำรวจ: 2026-10-09 | สำรวจโดย: Cline Agent | GitHub account: `sangsakorn`

---

## 1. ภาพรวม: creator360.studio ประกอบด้วย 5 Worlds

| # | World | ระบบ | สถานะจากเว็บ |
|---|-------|------|--------------|
| 01 | Commerce World | Shopee Video AI — ผลิตวิดีโออัตโนมัติจากแคตตาล็อก 40,000+ | ✅ Live |
| 02 | Neural Core | LINE OA Smart Agents — ตอบแชท/ปิดการขาย 24/7 + RAG Memory | ✅ Live |
| 03 | Amber Pulsar | VIP Media Lounge — สตรีม 4K/Hi-Res + AI สั่งงานด้วยเสียง | ✅ Live |
| 04 | Radiant Core | Infra: Gemini 2.5 Flash, Vertex AI, Cloud Run, Cloudflare | ✅ Live |
| 05 | Signal Outpost | Enterprise Onboarding / Consult | ✅ Live |

---

## 2. Repos ที่มีอยู่แล้วบน GitHub (sangsakorn)

### 🟢 Core Creator360 (เกี่ยวข้องโดยตรง)

| Repo | Stack | คืออะไร | สถานะ |
|------|-------|---------|-------|
| **Creator360Ecosystem** | JS / FastAPI | 📋 **Blueprint & Docs hub** — README อธิบาย ecosystem ทั้งหมด + backend MVP (FastAPI + `ai_service.py`: GPT-4o script gen, Shopee product analysis, image gen) + `memory/PRD.md` | Phase 1 MVP เสร็จแล้ว |
| **Creator360editor-vdo** | JS / FastAPI | 🎬 Video Editor (CapCut alternative) — React editor + FFmpeg pipeline | Beta |
| **Creator360AI.studio** | TS / Vite | 🤖 Google AI Studio app — frontend เชื่อม Gemini | Deployable |
| **music.studio** | JS / FastAPI | 🎵 Media backend (`youtube_service.py`, `file_service.py`) + Dockerfile + cloudbuild.yaml → ตรงกับ **World 03 VIP Media Lounge** | Deployed (Cloud Build) |
| **creator360-content-empire** (+ `-1`) | HTML/Shell | 📝 AI-Powered Content Creation System | เก่า (2025) |
| **creator360-premium-site** | Python | 🌐 เว็บ premium — มี 3 ส่วน: `creator360_ai`, `creator360_fastapi`, `creator360_streamlit` + Azure deploy | เก่า (2025) |

### 🟡 Agent / AI Infrastructure (ใช้ต่อได้ ไม่ต้องสร้างใหม่)

| Repo | คืออะไร | ใช้กับ Creator360 ยังไง |
|------|---------|------------------------|
| **Agent-Reach** (workspace นี้) | CLI ให้ AI agent อ่าน/ค้น 13 แพลตฟอร์ม (Twitter, Reddit, YouTube, GitHub, Bilibili, XHS, web, RSS, Exa) | = หน้าที่ของ agent **"OpenClaw" (Research & trend intel)** ที่ระบุไว้ใน README — มีอยู่แล้ว ไม่ต้องเขียนใหม่ |
| **MoneyPrinterTurbo** | AI สร้างวิดีโอสั้น HD จาก topic/keyword อัตโนมัติ | = pipeline วิดีโอสำหรับ **World 01 Shopee Video AI** — ใช้เป็น render engine ได้เลย |
| **obsidian-mind** | Persistent memory สำหรับ AI coding agents | สอดคล้องกับ **OPC Memory Sync** |
| **goose** | Open-source extensible AI agent framework | ตัวเลือก agent runtime |
| **superpowers** | Agentic skills framework | ตัวเลือก skills/methodology |
| **opik** | LLM evaluation + tracing platform | ใช้วัดคุณภาพ agent (QA — หน้าที่ "OpenClaude") |
| **n8n-docs** | เอกสาร n8n automation | reference สำหรับ workflow automation |

---

## 3. สถาปัตยกรรม Production (จาก Creator360Ecosystem README)

```
Frontend : React 19 + TS 5.9 + Vite 7 + TailwindCSS v4 + shadcn/ui + TanStack Query + Wouter
Backend  : Express.js + tRPC + Drizzle ORM + MySQL + jose (JWT/Google OAuth) + LINE Messaging API
AI       : Gemini 2.5 Flash (main) → Vertex AI → Gemma4 → Ollama  [failover 4 ชั้น]
           Veo 3.1 (video gen) | SDXL (image gen) | FFmpeg 7.1.1
Infra    : Cloud Run asia-southeast3 | Cloudflare Tunnel | AWS S3 / Cloudflare R2
Agents   : OpenClaw (research, 2 tools) | Hermes (script+scheduler, 3 tools, 7 crons) | OpenClaude (QA, 6 tools)
Memory   : OPC Brain (Notion) + agents/opc_memory_sync.py
Pricing  : Starter ฿0 / Creator ฿149 / Pro ฿499 / Studio ฿999
Deploy   : C360 → line-automation rev 00071-vhr | CHIM → line-automation-c360 rev 00038-gf8
```

---

## 4. ⚠️ ประเด็นสำคัญที่พบ (Risks & Gaps)

### 🔴 R1 — `line_automation_system` (Production Core) ไม่มีบน GitHub
- README ระบุว่าเป็น "🔴 PRODUCTION CORE (Private)" แต่ **ไม่พบ repo นี้ใน GitHub account** (404)
- โค้ด production (LINE webhook, aiAgentService.ts, tRPC routes, Drizzle schema, agents/) น่าจะมีแค่บน local/Cloud Run เท่านั้น
- **ความเสี่ยงสูงสุด**: ถ้าเครื่อง local หาย = โค้ด production หายทั้งระบบ
- 👉 **Action แรกที่ควรทำ: push `line_automation_system` ขึ้น GitHub (private repo)**

### 🟠 R2 — มี AI stack 2 ชุดที่ไม่ตรงกัน
- `Creator360Ecosystem/backend/ai_service.py` ใช้ **Emergent LLM Key + GPT-4o + gpt-image-1**
- แต่ production stack ระบุ **Gemini 2.5 Flash + Vertex AI failover**
- 👉 ต้องตัดสินใจว่า stack ไหนคือ canonical (แนะนำ Gemini ตาม production) แล้ว migrate/ปิดอีกตัว

### 🟠 R3 — Video pipeline ซ้ำกัน 3 จุด
- `Creator360Ecosystem/backend` (FastAPI + Emergent)
- `Creator360editor-vdo` (FFmpeg editor)
- `MoneyPrinterTurbo` (auto short-video generator)
- 👉 ควรกำหนด **1 canonical video pipeline** แล้วให้ที่เหลือเป็น component ย่อย

### 🟡 R4 — Frontend 3 รูปแบบ
- CRACO React (Ecosystem) / React 19 + Vite + tRPC (production) / AI Studio app
- 👉 canonical = React 19 + Vite (ตาม production)

### 🟢 R5 — สิ่งที่ยังไม่มี = โอกาสสำหรับ Agent ใหม่ (ไม่ซ้ำของเดิม)
จาก README "Project Status" ที่ยัง ⏳ Pending:
- [ ] **Multi-agent Dispatch** ← ช่องว่างหลักสำหรับ Agent ใหม่
- [ ] Cloudflare R2 Storage
- [ ] Git Cleanup (~190 files)
- [ ] Video export/render จริง (PRD backlog P0)
- [ ] Stripe integration (P1)
- [ ] Auto-subtitle (P1)

---

## 5. ข้อเสนอแนะลำดับถัดไป (ไม่ให้ทำงานซ้ำ)

1. **กู้ `line_automation_system` ขึ้น GitHub ก่อน** — ถ้าไม่มี backup งานใหม่ทั้งหมดมีความเสี่ยง
2. **ตัดสินใจ canonical stack** (แนะนำ: Gemini + Express/tRPC + React19/Vite ตาม production)
3. **สร้าง Agent ใหม่ในส่วนที่ว่างจริง** เช่น Multi-agent Dispatch โดยใช้ของที่มี:
   - Research → ใช้ **Agent-Reach** (มีแล้วใน workspace นี้)
   - Video render → ใช้ **MoneyPrinterTurbo** เป็น engine
   - QA/Eval → ใช้ **opik**
   - Memory → ใช้แนวทาง **OPC Memory Sync / obsidian-mind**
4. เขียน Agent ให้ deploy บน Cloud Run asia-southeast3 ตาม infra เดิม
