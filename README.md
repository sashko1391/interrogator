# 🕵️ The Interrogator

**AI-powered text detective game where you interrogate a suspect to solve a murder case.**

🎮 **[Play Now](https://sashko1391.github.io/interrogator)**

---

## 🎯 About

The Interrogator is a browser-based detective game powered by Claude AI. You play as a detective interrogating Victoria Ashford, a suspect in the murder of her business partner Marcus Webb.

- Ask any question freely
- The suspect may lie, evade, or contradict herself
- Uncover the truth through clever questioning
- Limited energy — use your questions wisely

## 🌍 Languages

- 🇬🇧 English
- 🇺🇦 Українська

## 🎮 How to Play

1. Read the **Case Briefing** to understand the crime
2. Ask questions to the suspect in the chat
3. Watch the **Suspicion Meter** — it rises when you hit sensitive topics
4. You have **10 questions** (energy) per session
5. When ready, click **Make Accusation**
6. Choose wisely — wrong accusations lose the case!

## 🧠 Tips

- Pay attention to timeline inconsistencies
- Ask about relationships and motives
- Confront contradictions you discover
- The suspect becomes more nervous as suspicion rises

## 🛠️ Tech Stack

- Pure HTML/CSS/JavaScript (single file)
- Claude AI (Haiku 3.5) via Cloudflare Workers
- No frameworks, no dependencies

## 📁 Project Structure

```
interrogator/
├── index.html      # Complete game (EN/UA)
└── README.md
```

## 🚀 Self-Hosting

To run your own instance:

1. **Create Cloudflare Worker** at [dash.cloudflare.com](https://dash.cloudflare.com)

2. **Add Worker code:**
```javascript
export default {
  async fetch(request, env) {
    if (request.method === "OPTIONS") {
      return new Response(null, {
        headers: {
          "Access-Control-Allow-Origin": "*",
          "Access-Control-Allow-Methods": "POST, OPTIONS",
          "Access-Control-Allow-Headers": "Content-Type",
        },
      });
    }

    if (request.method !== "POST") {
      return new Response("Method not allowed", { status: 405 });
    }

    const body = await request.json();
    
    const response = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "x-api-key": env.ANTHROPIC_API_KEY,
        "anthropic-version": "2023-06-01",
      },
      body: JSON.stringify({
        model: body.model || "claude-3-5-haiku-20241022",
        max_tokens: 300,
        system: body.system || "",
        messages: body.messages || [],
      }),
    });

    const data = await response.json();

    return new Response(JSON.stringify(data), {
      headers: {
        "Content-Type": "application/json",
        "Access-Control-Allow-Origin": "*",
      },
    });
  },
};
```

3. **Add secret** in Worker Settings:
   - Name: `ANTHROPIC_API_KEY`
   - Value: Your API key from [console.anthropic.com](https://console.anthropic.com)

4. **Update `index.html`** — change `API_ENDPOINT` to your Worker URL

5. **Deploy to GitHub Pages** or any static hosting

## 💰 API Costs

Using Claude Haiku 3.5:
- ~$0.001 per game session (10 questions)
- $5 credit ≈ 5,000 games

## 📜 License

MIT License — feel free to fork and create your own cases!

## 🔮 Future Ideas

- [ ] Multiple cases with different difficulty
- [ ] Evidence collection system
- [ ] Multiple suspects
- [ ] Leaderboard (fastest solve time)
- [ ] Voice input/output

---

**Created with 🖤 and Claude AI**
