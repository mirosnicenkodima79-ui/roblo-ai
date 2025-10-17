const express = require("express");
const axios = require("axios");
const app = express();

app.use(express.json());

const HF_API_URL = "https://api-inference.huggingface.co/models/ai-forever/ruGPT-3.5-13B";
const HF_API_KEY = "YOUR_HUGGING_FACE_API_KEY"; // Замените на ваш токен

app.post("/chat", async (req, res) => {
  try {
    const response = await axios.post(
      HF_API_URL,
      {
        inputs: req.body.message,
      },
      {
        headers: {
          Authorization: `Bearer ${HF_API_KEY}`,
        },
      }
    );
    res.json({ response: response.data[0]?.generated_text || "Извините, я не понял ваш запрос." });
  } catch (error) {
    console.error(error);
    res.status(500).json({ response: "Произошла ошибка при обработке запроса." });
  }
});

app.listen(3000, () => {
  console.log("Сервер запущен на порту 3000");
});
