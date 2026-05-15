---
title: "Integrating Nami with LINE Messaging API"
description: "A comprehensive guide to integrating Nami, your AI agent, with the LINE Messaging API for interactive chat bot experiences."
date: 2026-05-13
tags: [linebot, integration, nami, api, chat-bot, development]
---
# Integrating Nami with LINE Messaging API

Good news for all Nami users! Your AI agent now officially supports integration with the LINE Messaging API. This means you can interact with Nami directly through a LINE chat bot, bringing your AI assistant closer to where you communicate most.

This guide will walk you through the necessary steps to get your LINE Bot up and running with Nami.

## Configuration

Before you can run your LINE Bot, you'll need to configure your LINE Channel credentials. You have two primary ways to do this:

### 1. Using `nami init` (Recommended)

The easiest way to set up your credentials is during Nami's initialization process. Simply run the following command:

```bash
cargo run -- init
```

The system will then prompt you to enter your **LINE Channel Secret** and **LINE Channel Access Token**. Follow the on-screen instructions.

### 2. Manual Configuration

If you prefer to configure it manually, you can directly add your credentials to your `.env` file. Open your `.env` file and append the following lines, replacing the placeholder values with your actual secret and token:

```env
LINE_CHANNEL_SECRET=your_channel_secret_here
LINE_CHANNEL_ACCESS_TOKEN=your_channel_access_token_here
```

## Running the Bot

Once configured, you can start the LINE bot webhook server. This server will listen for incoming messages from LINE.

To start it, use the command:

```bash
cargo run -- line
```

By default, the server will run on `http://localhost:8080`. If you need to use a different port, you can specify it with the `--port` flag:

```bash
cargo run -- line --port 9090
```
This would run the server on `http://localhost:9090`.

## Webhook Setup (Crucial for Public Access)

LINE's Messaging API requires a public HTTPS endpoint for its webhooks. This means your local development server needs to be accessible from the internet. Here's how to set that up, typically using `ngrok`:

1.  **Expose your local server with ngrok**:
    If your Nami LINE bot server is running on port `8080` (the default), open a new terminal and run:

    ```bash
    ngrok http 8080
    ```
    `ngrok` will provide you with a public HTTPS URL (e.g., `https://<your-ngrok-id>.ngrok-free.app`).

2.  **Configure LINE Developers Console**:
    *   Log in to your [LINE Developers Console](https://developers.line.biz/console/).
    *   Navigate to your **Provider** > **Channel** > **Messaging API** settings.
    *   Locate the **Webhook URL** section.
    *   Set the Webhook URL to the `ngrok` URL you obtained, followed by `/callback`. For example: `https://<your-ngrok-id>.ngrok-free.app/callback`
    *   Ensure that **Use webhook** is enabled.
    *   Click the **Verify** button. This will send a test request to your Nami bot, confirming that it's receiving requests correctly. If successful, you'll see a confirmation message.

## Implementation Details for Developers

For those curious about the underlying implementation, the LINE integration logic resides in `src/modes/line.rs`. It leverages several Rust crates:

*   **Axum**: Used for building the robust and efficient HTTP server that handles incoming webhook requests at the `/callback` endpoint.
*   **HMAC-SHA256**: Crucial for security, this is used to verify the `x-line-signature` header in every incoming request, ensuring that messages genuinely originate from the LINE platform and haven't been tampered with.
*   **AgentRunner**: This is Nami's core, responsible for processing the text received from LINE and generating intelligent AI responses.
*   **Reqwest**: Utilized for making HTTP requests back to the LINE API to send reply messages to users.