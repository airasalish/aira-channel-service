# aira-channel-service

A small Express server that stands in for a WhatsApp/email provider when testing [AIRA](https://github.com/airasalish/AIRA-AI-Reach-Agent).

`POST /send` accepts a batch of messages and acknowledges it straight away. Then, for each message, after a random 1 to 5 second delay, it posts a delivery event back to `{callbackBaseUrl}/api/receipts`. About 5% of messages fail. Of the ones delivered, about 60% are opened, and about 40% of those are clicked. This lets AIRA's live analytics be tested against events that arrive late and out of order.

```bash
npm install
npm start   # port 3001 by default, or $PORT
```

```bash
curl -X POST http://localhost:3001/send -H "Content-Type: application/json" -d '{
  "campaignId": "c1",
  "callbackBaseUrl": "http://localhost:3000",
  "communications": [{ "communicationId": "m1", "customerId": "u1", "channel": "whatsapp" }]
}'
```
