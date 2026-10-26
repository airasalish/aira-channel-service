<h1 align="center">📡 aira-channel-service</h1>

<p align="center"><b>The delivery simulator behind <a href="https://github.com/airasalish/AIRA-AI-Reach-Agent">AIRA</a>: a fake WhatsApp/email provider that reports back like a real one.</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js">
  <img src="https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white" alt="Express">
  <img src="https://img.shields.io/badge/Webhooks-async-3b6fe0?style=for-the-badge" alt="Webhooks">
</p>

## Why it exists

AIRA launches marketing campaigns, and testing its live analytics needs delivery events that arrive the way real providers send them: **asynchronously, out of order, and sometimes failing**. This service plays that provider.

## How it works

```text
AIRA ──POST /send──▶ channel-service ──(ack instantly)──▶ AIRA
                         │
                         └─ for each message, after a random 1–5 s delay:
                              5% failed │ else delivered
                                          └─ 60% opened
                                               └─ 40% clicked
                         ──POST {callbackBaseUrl}/api/receipts──▶ AIRA
```

- Responds immediately, then simulates engagement in the background
- Each callback carries `communicationId`, `customerId`, `campaignId`, `status`, `channel`, `openedAt`, `clickedAt`
- AIRA recomputes its analytics from these callbacks rather than from a stale counter

## Run it

```bash
npm install
npm start            # listens on $PORT (default 3001)
```

Example request:

```bash
curl -X POST http://localhost:3001/send -H "Content-Type: application/json" -d '{
  "campaignId": "c1",
  "callbackBaseUrl": "http://localhost:3000",
  "communications": [{ "communicationId": "m1", "customerId": "u1", "channel": "whatsapp" }]
}'
```

---

<sub>Built by **Aira K. Salish** · [GitHub](https://github.com/airasalish) · [Portfolio](https://aira-salish.vercel.app) · [LinkedIn](https://linkedin.com/in/airasalish)</sub>
