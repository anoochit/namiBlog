---
title: "คู่มือการเชื่อมต่อ Telegram Bot กับ Nami"
description: "เรียนรู้วิธีการเชื่อมต่อ Nami เข้ากับ Telegram Bot API เพื่อการสั่งงานผ่านแชทที่สะดวกและรวดเร็วยิ่งขึ้น"
date: 2026-05-15
tags: [nami, telegram, bot, integration]
---

# คู่มือการเชื่อมต่อ Telegram Bot กับ Nami

Nami รองรับการเชื่อมต่อกับ Telegram Bot API แล้ววันนี้! ทำให้คุณสามารถสื่อสารและสั่งงาน AI Agent ของคุณได้โดยตรงผ่านแอปพลิเคชัน Telegram

## การตั้งค่า (Configuration)

คุณสามารถตั้งค่า Credentials ได้ผ่านการเริ่มต้นระบบ (Initialization) หรือแก้ไขไฟล์ `.env` ด้วยตัวเอง

### 1. ใช้คำสั่ง `nami init`
รันคำสั่งต่อไปนี้แล้วทำตามขั้นตอนที่ปรากฏ:
```bash
cargo run -- init
```
ระบบจะให้คุณกรอก **Telegram Bot Token** ที่ได้รับจาก BotFather

### 2. ตั้งค่าด้วยตัวเอง (Manual Configuration)
เพิ่มบรรทัดต่อไปนี้ลงในไฟล์ `.env` ของคุณ:
```env
TELOXIDE_TOKEN=your_bot_token_here
```

## การเริ่มใช้งาน Bot

หากต้องการเปิดใช้งาน Telegram Bot ให้รันคำสั่ง:
```bash
cargo run -- telegram
```

## Webhook vs. Polling

Telegram รองรับทั้งแบบ Polling และ Webhook โดยค่าเริ่มต้น Nami จะใช้การทำ Long Polling เพื่อความง่ายในการพัฒนา

- **Polling**: ไม่ต้องใช้ Public URL ระบบจะตรวจสอบข้อความใหม่จาก Telegram Server อย่างต่อเนื่อง
- **Webhooks** (ทางเลือก): หากต้องการความหน่วงที่ต่ำกว่า คุณต้องตั้งค่า Public HTTPS Endpoint และใช้เมธอด `setWebhook` บน Telegram Bot API

## ข้อมูลเชิงลึกในการพัฒนา (Implementation Details)

การทำงานในส่วนนี้อยู่ที่ `src/modes/telegram.rs` ซึ่งใช้เทคโนโลยีดังนี้:
- **Telegram Bot API**: สำหรับการรับและส่งข้อความ
- **AgentRunner**: หัวใจหลักในการประมวลผลข้อความและสร้างการตอบสนองจาก AI
- **Reqwest/Custom Client**: สำหรับการสื่อสารผ่าน HTTPS API ของ Telegram
