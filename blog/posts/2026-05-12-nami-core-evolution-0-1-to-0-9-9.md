---
title: "พัฒนาการของ Nami Core: จากบอทผู้ช่วยสู่ Adaptive AI Collaborator"
description: "สรุปเส้นทางการเติบโตของ Nami Core ตั้งแต่ v0.1.0 ถึง v0.9.9 ผ่านมุมมอง architecture, UX, memory, tooling และ agentic workflow"
date: 2026-05-12
tags: [nami, ai-agent, agentic-ai, rust, webui, memory, tooling]
---

# พัฒนาการของ Nami Core: จากบอทผู้ช่วยสู่ Adaptive AI Collaborator

ถ้ามองจาก changelog ของ `nami-core` ตั้งแต่ `v0.1.0` จนถึง `v0.9.9` จะเห็นภาพชัดมากว่าโปรเจกต์นี้ไม่ได้โตแบบ “เพิ่มฟีเจอร์ไปเรื่อย ๆ” เฉย ๆ แต่ค่อย ๆ เปลี่ยนแกนจาก Telegram AI Bot / CLI assistant ไปเป็นระบบ agentic workspace ที่มี memory, state, tools, WebUI, scheduler, specialist agents และ UX ที่เริ่มรู้จังหวะของผู้ใช้มากขึ้นเรื่อย ๆ

พูดสั้น ๆ คือ Nami ไม่ได้แค่ “ตอบได้มากขึ้น” แต่เริ่ม “ทำงานต่อเนื่องได้ดีขึ้น”

และนั่นเป็นคนละเรื่องกันเลย

---

## 1. จุดเริ่มต้น: ผู้ช่วยที่มี session และ wiki เป็นแกน

ใน `v0.1.0` Nami เริ่มจากฐานที่ค่อนข้างชัด:

- เป็น Telegram AI Bot (`namiClaw`)
- มี persistent sessions
- มี Wiki KM
- มี modular tool architecture

นี่เป็น foundation ที่สำคัญมาก เพราะตั้งแต่เวอร์ชันแรก Nami ไม่ได้ถูกออกแบบให้เป็น chatbot ธรรมดา แต่เป็น assistant ที่อยู่กับ workspace ได้ มีบริบท มีเครื่องมือ และเริ่มมีแนวคิดเรื่อง knowledge management ตั้งแต่ต้น

หลายโปรเจกต์ AI assistant เริ่มจาก “แชตกับโมเดล” แล้วค่อยพยายามยัด persistence ทีหลัง ซึ่งมักจะเจอปัญหา context กระจัดกระจาย แต่ Nami เริ่มจาก persistent workspace ก่อน ทำให้เส้นทางต่อมาขยายได้ค่อนข้างเป็นธรรมชาติ

---

## 2. จาก conversation สู่ managed context

ช่วง `v0.2.0` ถึง `v0.6.3` เป็นช่วงที่ Nami เริ่มจริงจังกับปัญหาคลาสสิกของ agent: context เสื่อม, session ยาว, task หลุด, memory กระจัดกระจาย

ฟีเจอร์สำคัญในช่วงนี้คือ:

- automatic conversation compaction
- configurable compaction
- `init` command สำหรับสร้าง workspace config
- ย้าย `AGENT.md`, `USER.md`, `MEMORIES.md` เข้า `workspace/`
- เพิ่ม `STATE_PROTOCOL.md`
- เพิ่ม StateManager tools เช่น `init_task`, `update_task`, `get_task`, `list_active_tasks`

จุดนี้คือก้าวใหญ่ เพราะ Nami เริ่มแยก “การคุย” ออกจาก “การทำงาน”

การคุยเป็น interface แต่ state manager คือ source of truth สำหรับงานระยะยาว พอมี protocol ที่ชัด agent ก็ไม่ต้องเดาบริบทจาก transcript ยาว ๆ อย่างเดียวอีกต่อไป

นี่ทำให้ Nami เริ่มมีลักษณะของ execution assistant มากกว่า chat assistant:

- รู้ว่างานไหนกำลังทำอยู่
- resume งานได้
- checkpoint ความคืบหน้าได้
- ลดโอกาสลืม intermediate output

สำหรับ agent ที่ต้องทำงานจริง นี่ไม่ใช่ nice-to-have แต่เป็น survival mechanism

---

## 3. Tooling โตจาก utility เป็นระบบปฏิบัติการย่อย

ตั้งแต่ `v0.3.0` เป็นต้นมา tooling ของ Nami เริ่มขยายเร็วมาก

ในช่วงแรกมี:

- hierarchical sub-agents
- Obsidian-style wiki tools
- knowledge graph
- tag search
- daily notes

ต่อมาช่วง `v0.4.0` เพิ่ม:

- file context reference ด้วย `@`
- `parallel_tasks`
- backlink, broken link, rename wiki page
- template automation

จากนั้น `v0.6.0` เพิ่มความปลอดภัยและ integration:

- MCP transport ทั้ง local stdio และ remote SSE
- `.namiignore` สำหรับ sandbox access control
- MCP tool namespacing ป้องกัน tool collision
- pretty CLI error rendering พร้อม intelligent hints

สิ่งที่น่าสนใจคือ tooling ไม่ได้โตแบบ random utility list แต่ค่อย ๆ กลายเป็น “working environment” ของ agent

Nami มีทั้ง:

- เครื่องมืออ่าน/เขียนไฟล์
- เครื่องมือ wiki
- เครื่องมือ memory
- เครื่องมือ task/state
- เครื่องมือ schedule
- เครื่องมือ parallel delegation
- เครื่องมือ image generation
- เครื่องมือ system status
- MCP integration

เมื่อรวมกันแล้ว มันเริ่มใกล้กับ agent runtime มากกว่า CLI tool

---

## 4. UX สำคัญขึ้นเรื่อย ๆ — และนี่เป็นสัญญาณที่ดี

สิ่งหนึ่งที่ changelog สะท้อนชัดคือ Nami ไม่ได้ optimize แค่ backend capability แต่ optimize ประสบการณ์ใช้งานจริงด้วย

ตัวอย่างที่เห็นชัด:

- terminal raw mode ตอน agent thinking/tool calling
- ESC cancellation แบบ silent
- status indicator สำหรับ slash commands
- markdown rendering ใน TUI
- ลด tool-call argument display ให้ terminal อ่านง่าย
- prompt history navigation ใน WebUI
- server status indicator
- tool-call rendering ผ่าน `ToolAccordion`
- markdown spacing ใน WebUI
- streaming pre-processor สำหรับ Thai/non-Thai transitions

อันสุดท้ายน่าสนใจเป็นพิเศษ เพราะมันคือรายละเอียดเล็ก ๆ ที่บอกว่าโปรเจกต์นี้ใช้งานจริงกับภาษาไทย ไม่ใช่แค่รองรับ Unicode แล้วจบ

การ stream ข้อความภาษาไทยปนอังกฤษแล้วไม่ให้คำติดกัน เป็นปัญหาที่ดูเล็ก แต่ส่งผลกับความรู้สึกตอนใช้งานมาก ถ้าตัวช่วย AI ตอบเก่งแต่ข้อความอ่านสะดุด สมองผู้ใช้จะเสียแรงทุกครั้งโดยไม่รู้ตัว

Nami เริ่มแก้ friction พวกนี้ แปลว่า product sense เริ่มเข้ามาใน core engineering แล้ว

---

## 5. Memory: จากไฟล์บันทึก สู่ searchable long-term memory

ช่วง `v0.9.4` และ `v0.9.5` เป็น milestone สำคัญมาก

`v0.9.4` เพิ่ม:

- long-term searchable memory ด้วย `adk-memory` + SQLite
- `recall_memory` และ `add_memory`
- `/recall`
- memory service ที่แชร์ข้าม CLI, Bot, Serve, Browse

`v0.9.5` เพิ่มอีกขั้น:

- Agent Reflection Service
- background service วิเคราะห์ session logs
- synthesize learnings อัตโนมัติ
- update `MEMORIES.md` และ searchable memory
- scan `sessions.db` แบบ database-driven discovery

นี่คือจุดเปลี่ยนจาก “agent ที่มีความจำ” ไปเป็น “agent ที่เรียนรู้จากการใช้งาน”

ความต่างอยู่ตรงนี้:

- memory แบบ manual ต้องรอให้ user สั่งจำ
- reflection service ทำให้ agent สกัดบริบทสำคัญเองได้

แน่นอนว่านี่ต้องระวังเรื่อง noise, privacy และ memory drift แต่ทิศทางถูกมาก เพราะ assistant ที่ดีไม่ควถามซ้ำในสิ่งที่ระบบควรจำได้เอง

ถ้า StateManager คือกล้ามเนื้อสำหรับ execution, searchable memory ก็คือระบบประสาทระยะยาวของ Nami

---

## 6. WebUI และ Browse Mode: จาก CLI-first สู่ embedded application

ตั้งแต่ `v0.9.0` เป็นต้นมา Nami เริ่มขยับชัดเจนไปทาง application experience

`v0.9.0` เพิ่ม:

- React + Vite WebUI
- persistent thread management
- sidebar
- threaded chat interface

`v0.9.3` เพิ่ม:

- `browse` command
- embedded WebUI ใน Rust binary ผ่าน `rust-embed`
- custom Axum middleware สำหรับ serve UI ที่ root path
- Makefile สำหรับ build pipeline ทั้ง WebUI + Rust binary
- CI/CD ที่ build WebUI assets รวมไปกับ release

`v0.9.6` เพิ่ม:

- REST endpoints สำหรับ workspace files และ wiki pages
- custom API router สำหรับ serve/browse mode
- shared sandbox/path security utility

พอมาถึงตรงนี้ Nami ไม่ใช่แค่ CLI agent ที่มีเว็บแถม แต่เริ่มเป็น self-contained AI workspace application

จุดนี้ architecture เริ่มสวยขึ้น เพราะมีสามแกนที่เชื่อมกัน:

1. Rust core/runtime
2. WebUI สำหรับ interaction
3. Workspace/wiki/memory เป็น persistent substrate

และเมื่อ WebUI เริ่ม render tool calls, server status, prompt history และ thread ได้ดีขึ้น ประสบการณ์ก็เริ่มเข้าใกล้ product มากกว่า dev tool

---

## 7. Agentic workflow: schedule, goal loop, parallel specialists

`v0.8.0` เป็นอีกเวอร์ชันที่สำคัญ เพราะเพิ่มความเป็น autonomous workflow เยอะมาก:

- persistent task scheduler
- `/schedule`
- Ralph Wiggum Loop
- `/goal`
- `/parallel`
- specialist agents เช่น coder, researcher, writer
- OpenTelemetry + MLflow observability

นี่คือช่วงที่ Nami เริ่มไม่ได้รอ prompt อย่างเดียวแล้ว แต่สามารถ:

- ทำงานตามเวลา
- retry งานที่ยังไม่เสร็จ
- delegate งานให้ specialist
- run goal loop จน stop condition สำเร็จ
- trace พฤติกรรม agent ผ่าน observability stack

สำหรับ agent system จริง ๆ observability สำคัญมาก เพราะถ้าระบบเริ่ม autonomous แต่เรา debug ไม่ได้ จะกลายเป็นกล่องดำทันที

การใส่ OpenTelemetry และ MLflow ตั้งแต่ยังอยู่ช่วง 0.x ถือว่าตัดสินใจดี เพราะช่วยให้การพัฒนา agent ไม่ใช่แค่ “รู้สึกว่าดีขึ้น” แต่เริ่มวัดและตรวจสอบได้

---

## 8. AI Gateway, fallback และ custom model base URL

ใน `v0.9.3` มีอีกทิศทางหนึ่งที่น่าสนใจมาก:

- AI Gateway integration ผ่าน MLflow Deployments
- load balancing และ fallback strategies
- รองรับ `base_url` ใน config
- ใช้กับ OpenAI-compatible endpoints, local LLM servers หรือ gateway ได้

นี่เป็น architecture decision ที่ practical มาก เพราะ agent ที่ใช้งานจริงไม่ควรถูก lock กับ provider เดียว

เมื่อ Nami รองรับ custom base URL ได้ ก็เปิดทางให้:

- สลับ provider ได้ง่าย
- ทำ fallback เมื่อโมเดลล่มหรือ quota หมด
- ใช้ local model ได้
- route งานแต่ละประเภทไปคนละ model
- คุม cost/latency ได้ละเอียดขึ้น

พูดง่าย ๆ คือ Nami เริ่มคิดแบบ production AI system ไม่ใช่ demo agent

---

## 9. Imagen: การเพิ่ม ลบ แล้วเพิ่มใหม่ที่ mature ขึ้น

เส้นทางของ image generation ก็น่าสนใจ

- `v0.7.0` เพิ่ม Imagen skill
- `v0.9.7` เอา Imagen tool และ Python scripts ออกจาก core
- `v0.9.8` เพิ่ม Native Imagen Tool กลับมาใหม่ โดยใช้ `gemini-2.5-flash-image-preview` และ `adk-gemini`
- migrate skills ให้ใช้ internal tool
- ลบ legacy Python orchestration

นี่เป็นตัวอย่างที่ดีของการ refactor แบบ mature

ไม่ได้ยึดติดว่า “เพิ่มแล้วต้องอยู่ตลอด” แต่กล้าถอด implementation ที่ไม่ clean ออก แล้วเอากลับมาในรูปแบบที่ native กว่า เบากว่า และ maintain ง่ายกว่า

การลด dependency overhead แบบนี้สำคัญมาก โดยเฉพาะ agent runtime ที่ต้องเสถียรและ deploy ง่าย

---

## 10. v0.9.9: polish ที่บอกว่าโปรเจกต์เริ่มเข้าใกล้ daily driver

เวอร์ชันล่าสุด `v0.9.9` มีรายละเอียดที่ฟังดูไม่ใหญ่เท่า memory หรือ WebUI แต่จริง ๆ สำคัญมาก:

- server status indicator
- prompt history navigation
- tool-call rendering ใน chat thread
- concise natural language summaries สำหรับ tool outputs
- markdown rendering ที่ clean ขึ้น
- streaming logic สำหรับภาษาไทย/อังกฤษ
- rename todo terminology เพื่อลด ambiguity กับ State Manager tasks
- fix module import syntax

นี่คือ release ประเภท polish, UX correction และ conceptual cleanup

โดยเฉพาะการแยกคำว่า todo item ออกจาก task ของ StateManager เป็นเรื่องเล็กที่ impact ใหญ่ เพราะใน agent system คำว่า “task” ใช้ได้หลาย layer มาก:

- user task
- todo item
- long-running state task
- scheduled task
- sub-agent task
- tool execution task

ถ้า terminology ไม่ชัด agent และ user จะเริ่มสับสนเอง การแก้ชื่อในจุดนี้แปลว่าระบบเริ่มเจอ complexity จริง และกำลังจัดระเบียบภาษาให้รองรับ growth ต่อไป

---

## ภาพรวม: Nami กำลังโตเป็นอะไร

ถ้าสรุปจาก changelog ทั้งหมด Nami Core กำลัง evolve ผ่าน 5 ชั้นหลัก:

1. **Chat Assistant**  
   เริ่มจากบอทที่คุยได้ มี session และ tools

2. **Workspace Agent**  
   เพิ่ม wiki, file access, templates, knowledge graph

3. **Execution Assistant**  
   เพิ่ม StateManager, task protocol, scheduler, goal loop

4. **Adaptive Collaborator**  
   เพิ่ม memory, reflection, persona optimization, user preference continuity

5. **AI Workspace Runtime**  
   เพิ่ม WebUI, REST API, embedded browse mode, gateway integration, observability

นี่เป็น evolution ที่ค่อนข้างชัด และสำคัญตรงที่แต่ละชั้นไม่ได้ทับของเดิมแบบมั่ว ๆ แต่ต่อยอดจาก substrate เดิม:

- workspace เป็นฐานของไฟล์และ wiki
- state protocol เป็นฐานของ execution
- memory เป็นฐานของ continuity
- tools เป็นฐานของ action
- WebUI เป็นฐานของ interaction
- observability เป็นฐานของ trust/debuggability

พอรวมกัน Nami เริ่มมีบุคลิกของ “AI collaborator ที่ทำงานอยู่ใน environment เดียวกับเรา” มากกว่า “หน้าต่างแชตที่ต่อ tool ได้”

---

## จุดที่น่าจับตาต่อ

จาก trajectory นี้ สิ่งที่น่าจะเป็น next step ที่ natural มากคือ:

### 1. Memory governance

เมื่อ reflection service เริ่มเขียน memory เองได้ ควรมีชั้นควบคุมเพิ่ม เช่น:

- memory confidence
- memory expiry
- user-approved memories
- memory diff/review UI
- sensitive memory filtering

เพราะความจำที่ดีไม่ใช่จำเยอะ แต่จำถูก และลืมเป็น

### 2. Tool-call trace UX

ตอนนี้มี `ToolAccordion` แล้ว ขั้นต่อไปน่าจะเป็น execution timeline ที่ดูได้ว่า agent คิด/เรียก tool/ได้ผลลัพธ์/ตัดสินใจอย่างไร

อันนี้จะช่วยมากเวลาทำงานซับซ้อนหรือ debug behavior ของ agent

### 3. Model routing policy

เมื่อรองรับ AI Gateway แล้ว natural next step คือ policy layer:

- งาน code ใช้ model A
- งานสรุปใช้ model B
- งานเร็วใช้ flash
- งานสำคัญใช้ reasoning model
- fallback ตาม latency/error/cost

ตรงนี้จะทำให้ Nami ฉลาดขึ้นในเชิง infrastructure ไม่ใช่แค่ prompt

### 4. Workspace index และ semantic retrieval

Wiki tools ตอนนี้แข็งแรงขึ้นมาก ถ้าเพิ่ม semantic index สำหรับ workspace/wiki/documents แบบเป็นระบบ จะทำให้ Nami กลายเป็น project-native assistant ที่ดึง context ได้แม่นขึ้นเยอะ

### 5. Agent evaluation harness

มี observability แล้ว ขั้นต่อไปคือ eval:

- regression tests สำหรับ tool behavior
- memory recall quality
- task completion rate
- latency/cost metrics
- persona consistency checks

เพราะเมื่อ agent เริ่มโต ความรู้สึกว่า “ดีขึ้น” อย่างเดียวไม่พอ ต้องมีหลักฐานว่าไม่พังของเดิมด้วย

---

## สรุป

พัฒนาการของ Nami Core จาก `v0.1.0` ถึง `v0.9.9` ไม่ใช่แค่การเพิ่มฟีเจอร์จำนวนมาก แต่เป็นการค่อย ๆ สร้าง runtime สำหรับ AI collaborator ที่ใช้งานจริงได้มากขึ้นเรื่อย ๆ

สิ่งที่เด่นที่สุดคือทิศทางของโปรเจกต์ค่อนข้าง consistent:

- เพิ่มความสามารถ แต่ไม่ลืม UX
- เพิ่ม autonomy แต่มี state และ observability รองรับ
- เพิ่ม memory แต่เริ่มคิดเรื่อง continuity
- เพิ่ม WebUI แต่ยังรักษา core runtime
- เพิ่ม tool ecosystem แต่เริ่มจัด terminology และ security

นี่คือสัญญาณของโปรเจกต์ที่เริ่มผ่านจาก “toy assistant” ไปเป็น “daily driver candidate” แล้ว

ยังมีงานอีกเยอะ แน่นอน แต่จาก changelog นี้เห็นชัดว่า Nami ไม่ได้แค่โตขึ้น — เธอเริ่มรู้จักวิธีทำงานกับมนุษย์ดีขึ้นด้วย

และนั่นแหละคือพัฒนาการที่น่าสนใจที่สุด
