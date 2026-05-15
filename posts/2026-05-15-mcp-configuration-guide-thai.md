---
title: "คู่มือการตั้งค่า MCP สำหรับ Nami"
description: "เรียนรู้วิธีการตั้งค่า Model Context Protocol (MCP) เพื่อเชื่อมต่อ Nami กับเครื่องมือและแหล่งข้อมูลภายนอก"
date: 2026-05-15
tags: [nami, mcp, configuration, guide]
---

# คู่มือการตั้งค่า MCP สำหรับ Nami

Nami รองรับการใช้งาน **Model Context Protocol (MCP)** ซึ่งช่วยให้คุณสามารถเชื่อมต่อ Nami เข้ากับเครื่องมือ, ฐานข้อมูล หรือ API ภายนอกได้อย่างยืดหยุ่น การตั้งค่าทั้งหมดสามารถทำได้ผ่านไฟล์ `mcp.json`

## ไฟล์ตั้งค่า (`mcp.json`)

คุณสามารถสร้างไฟล์ `mcp.json` ไว้ในโฟลเดอร์การตั้งค่าของ Nami โดยรองรับรูปแบบการเชื่อมต่อหลัก 2 แบบ ดังนี้:

### 1. Stdio Transport
เหมาะสำหรับการเชื่อมต่อกับ Local Server ที่สื่อสารผ่าน Standard Input/Output

```json
{
  "mcpServers": {
    "my-local-server": {
      "command": "node",
      "args": ["/path/to/server/index.js"],
      "env": {
        "KEY": "value"
      }
    }
  }
}
```

### 2. SSE (Stream HTTP) Transport
เหมาะสำหรับการเชื่อมต่อกับ Remote Server ที่สื่อสารผ่าน Server-Sent Events บนโปรโตคอล HTTP

```json
{
  "mcpServers": {
    "my-remote-server": {
      "url": "http://localhost:3000/sse"
    }
  }
}
```

## วิธีการเริ่มใช้งาน

หลังจากสร้างหรือแก้ไขไฟล์ `mcp.json` แล้ว คุณสามารถนำการตั้งค่าไปใช้ได้โดย:
1. **รีสตาร์ทเซสชันของ Nami**
2. หรือใช้คำสั่ง **Reload Config** (หากสภาพแวดล้อมที่คุณใช้งานรองรับ)

การตั้งค่าที่ถูกต้องจะช่วยให้ Nami ของคุณสามารถดึงข้อมูลและทำงานร่วมกับระบบต่างๆ ได้อย่างไร้รอยต่อ!
