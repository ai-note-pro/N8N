
# 🧠 AI News Generation Workflow (n8n)
An **automated AI-powered workflow** built in n8n that fetches the latest AI news, summarizes them using **OpenAI GPT-4.1-mini**, and publishes the digest to **Notion**.

---
## ⚙️ How It Works
1. **Trigger** – Starts manually (can be automated later).  
2. **RSS Feed Reader** – Fetches articles from [the-decoder.com/feed](https://the-decoder.com/feed/).  
3. **Code Node (JavaScript)** – Formats and cleans up article data.  
4. **OpenAI GPT-4.1-mini** – Summarizes the news and extracts 3 key terms.  
5. **Notion Node** – Creates a new Notion page titled `AI News YYYY-MM-DD`.

---
💻 Code Node (JavaScript): Take first 5 articles and format them  
```const paragraphs = items.slice(0, 5).map(item => {
  const data = item.json;
  const pubDate = new Date(data.isoDate).toLocaleDateString("en-US", {
    weekday: "short", year: "numeric", month: "short", day: "numeric"
  });
  const categories = data.categories ? data.categories.join(", ") : "";
  return `📅 ${pubDate}
👤 Author: ${data["dc:creator"]}
📰 Categories: ${categories}
${data.contentSnippet}
🔗 Read more: ${data.guid}`;
});
return [{ json: { allText: paragraphs.join("\n\n") } }]```;  

---
🤖 OpenAI Node (Prompt)  
```You are an AI news assistant.
Your task is to process the provided text and produce a concise, well-structured output in two sections:
📰 AI News Today
Create a bullet-point list with 2–3 sentence summaries.
📘 Key Terms
Extract EXACTLY 3 technical terms from the text and explain each in ≤25 words.
The entire output must not exceed 1500 characters.
Here is the text: {{ $json.allText }}```
