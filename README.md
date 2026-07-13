# WhatsApp Chatbot Setup (Twilio + FastAPI + ngrok)

## Prerequisites

- Python 3.10+
- Twilio Account
- Twilio WhatsApp Sandbox
- ngrok
- Git

---

## 1. Clone the Repository

```bash
git clone <repository_url>
cd <repository_name>
```

---

## 2. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 3. Configure Environment Variables

Create a `.env` file and add the required API keys and configuration values.
```bash

# Twilio
TWILIO_ACCOUNT_SID=your_twilio_account_sid
TWILIO_AUTH_TOKEN=your_twilio_auth_token
TWILIO_WHATSAPP_NUMBER=whatsapp:+14155238886

# Add any other API keys used by the project
```


---

## 4. Start the FastAPI Server

Run the application:

```bash
python main.py
```

or if using Uvicorn:

```bash
uvicorn main:app --reload
```

The server will start on:

```
http://127.0.0.1:8000
```

You can verify it by opening:

```
http://127.0.0.1:8000/docs
```

---

## 5. Expose the Local Server using ngrok

Install ngrok if not already installed.

Start a tunnel on port **8000**:

```bash
ngrok http 8000
```

You will receive a public URL similar to:

```
https://abcd-1234.ngrok-free.app
```

Keep this terminal running.

---

## 6. Configure Twilio WhatsApp Sandbox

Login to your Twilio Console.

Navigate to:

```
Develop → Messaging → Try it Out → Send a WhatsApp Message
```

Open **Sandbox Settings**.

For **When a message comes in**, enter:

```
https://your-ngrok-url.ngrok-free.app/webhook
```

Example:

```
https://abcd-1234.ngrok-free.app/webhook
```

Set the HTTP Method to:

```
POST
```

Save the configuration.

---

## 7. Join the Sandbox

On WhatsApp, send the following message:

```
join condition-shut
```

to the Twilio Sandbox number:

```
+1 415 523 8886
```

You will receive a confirmation message indicating that your WhatsApp account has joined the sandbox.

---

## 8. Start Chatting

Send:

```
Hi
```

The chatbot will begin the conversation and guide you through:

1. Date of Birth
2. Birth Time
3. Birth City
4. Astrology Analysis
5. Career
6. Relationship
7. Health
8. Life Overview

---

## Project Flow

```
WhatsApp User
        │
        ▼
Twilio WhatsApp Sandbox
        │
        ▼
ngrok Public URL
        │
        ▼
FastAPI (/webhook)
        │
        ▼
State Manager
        │
        ▼
Validators
        │
        ▼
Swiss Ephemeris Chart Generator
        │
        ▼
Astrology Interpreter
        │
        ▼
Twilio Response
        │
        ▼
WhatsApp User
```

---

## API Endpoint

| Method | Endpoint | Description |
|---------|----------|-------------|
| POST | `/webhook` | Receives WhatsApp messages from Twilio |

---

## Troubleshooting

### `{"detail":"Not Found"}`

This is expected when opening the root URL.

Your webhook endpoint is:

```
https://your-ngrok-url.ngrok-free.app/webhook
```

---

### ngrok shows

```
dial tcp localhost:8000: connection refused
```

Your FastAPI server is not running.

Start it first:

```bash
python main.py
```

or

```bash
uvicorn main:app --reload
```

---

### No reply on WhatsApp

Check that:

- FastAPI server is running.
- ngrok is running.
- Twilio Sandbox webhook points to:
  ```
  https://your-ngrok-url.ngrok-free.app/webhook
  ```
- HTTP Method is **POST**.
- You have joined the sandbox by sending:
  ```
  join condition-shut
  ```
  to:
  ```
  +1 415 523 8886
  ```

---

## Architecture

```
WhatsApp
   │
Twilio Sandbox
   │
ngrok
   │
FastAPI (/webhook)
   │
SQLite Memory
   │
State Manager
   │
Input Validators
   │
Swiss Ephemeris
   │
LLM Astrology Interpreter
   │
Twilio XML Response
   │
WhatsApp
```
