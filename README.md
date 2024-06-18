mkdir marketing-ai
cd marketing-ai
npm init -y
npm install express axios dotenv
marketing-ai/
├── node_modules/
├── src/
│   ├── controllers/
│   │   └── aiController.js
│   ├── routes/
│   │   └── aiRoutes.js
│   ├── app.js
│   └── .env
├── package.json
└── server.js
const express = require('express');
const aiRoutes = require('./routes/aiRoutes');
const app = express();

app.use(express.json());
app.use('/api/ai', aiRoutes);

module.exports = app;
const app = require('./src/app');
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
OPENAI_API_KEY=your_openai_api_key
const axios = require('axios');

const generateCopywriting = async (req, res) => {
  const { prompt } = req.body;
  try {
    const response = await axios.post('https://api.openai.com/v1/engines/davinci-codex/completions', {
      prompt,
      max_tokens: 150,
    }, {
      headers: {
        'Authorization': `Bearer ${process.env.OPENAI_API_KEY}`,
      },
    });
    res.json(response.data);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

const generateContent = async (req, res) => {
  const { prompt } = req.body;
  try {
    const response = await axios.post('https://api.openai.com/v1/engines/davinci-codex/completions', {
      prompt,
      max_tokens: 500,
    }, {
      headers: {
        'Authorization': `Bearer ${process.env.OPENAI_API_KEY}`,
      },
    });
    res.json(response.data);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

module.exports = {
  generateCopywriting,
  generateContent,
};
const express = require('express');
const { generateCopywriting, generateContent } = require('../controllers/aiController');
const router = express.Router();

router.post('/copywriting', generateCopywriting);
router.post('/content', generateContent);

module.exports = router;
npx create-react-app marketing-ai-frontend
cd marketing-ai-frontend
npm install axios
