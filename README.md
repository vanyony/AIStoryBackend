# AIStoryBackend
AI Story Generator Backend
[package.json](https://github.com/user-attachments/files/23689276/package.json)
{
  "name": "aistorybackend",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "type": "module",
  "dependencies": {
    "axios": "^1.13.2",
    "cors": "^2.8.5",
    "express": "^5.1.0"
  }
}
[server.js](https://github.com/user-attachments/files/23689297/server.js)
import express from "express";
import axios from "axios";
import cors from "cors";

const app = express();
app.use(express.json());
app.use(cors()); // 允许小程序访问

app.post("/api/ai", async (req, res) => {
  const { text } = req.body;

  try {
    const aiRes = await axios.post(
      "https://api.deepseek.com/v1/chat/completions",
      {
        model: "deepseek-chat",
        messages: [
          { role: "system", content: "你是一个故事续写助手，请简洁、有创意地续写用户输入。" },
          { role: "user", content: text }
        ]
      },
      {
        headers: { Authorization: `Bearer ${process.env.DEEPSEEK_API_KEY}` }     
      }
    );

    res.json({
      reply: aiRes.data.choices[0].message.content
    });

  } catch (err) {
    console.log(err);
    res.status(500).json({ error: "AI请求失败" });
  }
});

const PORT = 3000;
app.listen(PORT, () => console.log(`🚀 后端运行在 http://localhost:${PORT}`));
