---
title: อัปเดต Changelog: การพัฒนา Nami และระบบจัดการสถานะ (0.6.2)
title: Nami Blog - Changelog Update 2026-05-06
date: 2026-05-06
tags: Changelog, Nami, Update, State Management, Agent, Rust
---

# บันทึกการเปลี่ยนแปลง (Changelog)

การเปลี่ยนแปลงที่สำคัญทั้งหมดของโปรเจกต์นี้จะถูกบันทึกไว้ที่นี่ค่ะ!

## [0.6.2] - 2026-05-06

### เพิ่มเข้ามา (Added)

- **ระบบจัดการสถานะแบบโครงสร้าง (Structured State Management)**: เปิดตัวเครื่องมือ `StateManager` (`init_task`, `update_task`, `get_task`, `list_active_tasks`) เพื่อให้การติดตามงานที่ต้องใช้เวลานานมีความแม่นยำและต่อเนื่อง
- **โปรโตคอลสถานะ (State Protocol)**: เพิ่มไฟล์ `workspace/STATE_PROTOCOL.md` เพื่อเป็นแนวทางบังคับให้ Nami รักษาความต่อเนื่องของเซสชันการทำงาน

### การเปลี่ยนแปลง (Changed)

- **การย้ายตำแหน่งข้อมูล Persona**: ย้ายไฟล์ `AGENT.md`, `USER.md`, และ `MEMORIES.md` จาก root ไปไว้ที่โฟลเดอร์ `workspace/` เพื่อความเป็นระเบียบและจัดการได้ง่ายขึ้น
- **การปรับแต่ง Context**: ปรับปรุงโครงสร้างไฟล์ `AGENT.md`, `USER.md`, `MEMORIES.md`, และ `STATE_PROTOCOL.md` ให้มีความหนาแน่นของข้อมูลสูงขึ้นและประหยัด Token
- **Template คำสั่งหลัก**: ปรับแต่ง Instruction Template ใน `src/agent/agent.rs` เพื่อลดการใช้ Token ในแต่ละเทิร์น
- **ระบบทำงานต่อเนื่อง (Always-On Protocol)**: รวม `STATE_PROTOCOL.md` เข้ากับคำสั่งระบบหลักของ Agent โดยตรง
- **การเริ่มโปรเจกต์ (Project Initialization)**: อัปเดตโหมด `init` ให้สร้างไฟล์ Persona และ Protocol ที่ประหยัด Token โดยอัตโนมัติในโฟลเดอร์ `workspace/`

### ส่วนที่นำออก (Removed)

- **การบันทึกงานแบบเดิม (Manual Task Logs)**: ยกเลิกการใช้ `TaskLog.md` และ Log แบบเก่า เพื่อเปลี่ยนมาใช้ระบบ StateManager ที่เป็นเครื่องมือจัดการโดยตรง

## [0.6.1] - 2026-05-05

### เพิ่มเข้ามา (Added)

- **การเริ่มโปรเจกต์แบบโต้ตอบ**: ปรับปรุงโหมด `init` ให้ใช้ `inquire` เพื่อให้ผู้ใช้สามารถเลือก LLM provider และโมเดลผ่านเมนูที่สวยงาม รวมถึงการกรอก API Key แบบซ่อนข้อความ
- **Wiki Templates**: เพิ่ม Markdown templates สำหรับ `Blog Post` และ `Task Log` ใน `workspace/wiki/Templates/` เพื่อมาตรฐานในการบันทึกข้อมูล

### การเปลี่ยนแปลง (Changed)

- **คุณภาพโค้ด**: ใช้ `clippy` เพื่อปรับแต่งโค้ด Rust ให้เป็นไปตามมาตรฐานและมีประสิทธิภาพสูงสุด
- **การดูแลรักษา Repository**: นำ `mcp.json` ออกจาก version control เพื่อความปลอดภัย และอัปเดต `mcp.json.example` แทน
- **เอกสารประกอบ**: เพิ่มไอเดียสำหรับการพัฒนาในอนาคตและปรับปรุงเอกสารสำหรับระบบควบคุมการเข้าถึง `.namiignore`

### แก้ไข (Fixed)

- **ความเสถียรในการคอมไพล์**: แก้ไขปัญหาการคอมไพล์และประเภทข้อมูลที่คลาดเคลื่อนในเลเยอร์ MCP transport
- **OpenAI Schema Validation**: แก้ไขปัญหา schema ของเครื่องมือว่างๆ เพื่อป้องกันข้อผิดพลาดในการตรวจสอบความถูกต้องของ OpenAI API
# Changelog

All notable changes to this project will be documented in this file.

## [0.6.2] - 2026-05-06

### Added

- **Structured State Management**: Introduced the `StateManager` tool (`init_task`, `update_task`, `get_task`, `list_active_tasks`) for reliable tracking of long-running processes.
- **State Protocol**: Added `workspace/STATE_PROTOCOL.md` to provide the agent with mandatory guidelines for session continuity.

### Changed

- **Persona Migration**: Moved `AGENT.md`, `USER.md`, and `MEMORIES.md` from the project root to the `workspace/` directory for centralized management.
- **Context Optimization**: Refactored `AGENT.md`, `USER.md`, `MEMORIES.md`, and `STATE_PROTOCOL.md` into high-density, token-efficient formats.
- **Instruction Template**: Optimized the core agent instruction template in `src/agent/agent.rs` to reduce per-turn token overhead.
- **Always-On Protocol**: Integrated the `STATE_PROTOCOL.md` directly into the agent's core system instructions.
- **Project Initialization**: Updated `init` mode to automatically create token-optimized persona and protocol files within the `workspace/` directory.

### Removed

- **Manual Task Logs**: Deleted the obsolete `TaskLog.md` template and logs in favor of the structured tool-based approach.

## [0.6.1] - 2026-05-05

### Added

- **Interactive Project Initialization**: Refactored the `init` mode to use `inquire`, providing arrow-navigable selection menus for LLM providers and models, and masked input for API keys.
- **Wiki Templates**: Introduced new Markdown templates for `Blog Post` and `Task Log` in `workspace/wiki/Templates/` to standardize recurring wiki entries.

### Changed

- **Code Quality**: Applied `clippy` optimizations across the codebase to ensure idiomatic Rust patterns and better performance.
- **Repository Maintenance**: Removed `mcp.json` from version control to prevent local configuration leakage; updated `mcp.json.example` with remote SSE transport examples.
- **Documentation**: Added future extension ideas to developer tips and improved documentation for the `.namiignore` access control system.

### Fixed

- **Compilation & Stability**: Resolved various compilation issues and type mismatches in the MCP transport layer.
- **OpenAI Schema Validation**: Added missing properties to empty tool argument schemas to prevent OpenAI API validation errors.
