# openwa-n8n-workflows

Import-ready [n8n](https://n8n.io/) workflows for [OpenWA](https://github.com/rmyndharis/OpenWA), the self-hosted WhatsApp API gateway.

## What's here

[`workflows/whatsapp-ai-bot-3-varieties.json`](workflows/whatsapp-ai-bot-3-varieties.json): one OpenWA webhook, three branches, a local AI reply.

| Branch | Fires when | Use it for |
|---|---|---|
| Variety 1: Personal Contact | `data.contact.pushName` equals the name you set | A private AI assistant for one person |
| Variety 2: Group Message | `data.isGroup` is true | Group handling |
| Variety 3: @agent Mention | group message that contains `@agent` | A group bot that only answers when called |

Stack: OpenWA (`message.received` webhook, `send-text` endpoint) → n8n AI Agent → Ollama `llama3.2:3b` with a 30-message memory window keyed on `chatId`. No paid API, no data leaves your server.

Delete the branches you don't need.

## Use it

1. OpenWA running on the same Docker network as n8n (the workflow calls `http://openwa-api:2785`).
2. Ollama running with the model pulled: `ollama pull llama3.2:3b`.
3. In n8n: Workflows → ⋯ → **Import from JSON** → paste the file.
4. Re-select credentials (they're `REPLACE_ME` in the file): the OpenWA API key on **Send WhatsApp Message**, Ollama on **Ollama Chat Model**.
5. Variety 1: replace `YourContactName` with the sender's real `pushName` from a webhook payload, not the name saved in your phone.
6. In the OpenWA dashboard → Webhooks → add `https://<your-n8n>/webhook/wa-webhook` for `message.received`.
7. Activate.

OpenWA retries webhooks. If a duplicate reply matters, add a dedup step on `idempotencyKey` ([OpenWA n8n docs](https://github.com/rmyndharis/OpenWA/blob/main/docs/22-n8n-integration.md)).

## Guides

Full write-ups, including the pitfalls I hit:

- [Self-host a free WhatsApp API with n8n and OpenWA](https://abhishekrajpurohit.in/blog/complete-guide-self-hosting-n8n-openwa-whatsapp-api): the whole setup on one page
- [3 WhatsApp + n8n workflow varieties](https://abhishekrajpurohit.in/blog/whatsapp-n8n-workflow-varieties): how each branch behaves and what to change
- [Connecting n8n to OpenWA](https://abhishekrajpurohit.in/blog/n8n-openwa-integration)
- [OpenWA API reference for n8n workflows](https://abhishekrajpurohit.in/blog/openwa-api-reference-n8n-workflows)
- [Backing up the OpenWA data volume](https://abhishekrajpurohit.in/blog/openwa-data-volume-backup)

Community project, not affiliated with OpenWA.

## License

MIT
