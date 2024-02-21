```javascript
const express = require('express');
const cors = require('cors');

const app = express();

// CORS 설정
app.use(cors({
  origin: 'https://frontend.example.com', // 클라이언트의 도메인
  credentials: true, // 인증정보(쿠키 등)을 함께 전송할 수 있도록 설정
}));

// FE 서버의 요청을 받는 엔드포인트
app.get('/api/data', (req, res) => {
  // 요청 처리 로직
});

// 서버 시작
app.listen(3000, () => {
  console.log('Server is running on port 3000');
});

```
