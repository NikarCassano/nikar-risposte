
const express = require("express");
const bodyParser = require("body-parser");

const app = express();
const VERIFY_TOKEN = "nikar123"; // il token che userai in Meta Developers

app.use(bodyParser.json());

// 1. Verifica iniziale webhook
app.get("/webhook", (req, res) => {
  let mode = req.query["hub.mode"];
  let token = req.query["hub.verify_token"];
  let challenge = req.query["hub.challenge"];

  if (mode && token && mode === "subscribe" && token === VERIFY_TOKEN) {
    console.log("Webhook verificato ✅");
    res.status(200).send(challenge);
  } else {
    res.sendStatus(403);
  }
});

// 2. Ricezione messaggi
app.post("/webhook", (req, res) => {
  let data = req.body;
  console.log("📩 Nuovo messaggio:", JSON.stringify(data, null, 2));
  res.status(200).send("EVENT_RECEIVED");
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server attivo su porta ${PORT}`));
