mo-phong-bcr/
├── index.html
├── package.json
├── vite.config.js
├── src/
│   ├── main.jsx
│   ├── App.jsx
│   └── styles.css
{
  "name": "mo-phong-bcr",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.2.0",
    "vite": "^5.2.0"
  }
}
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
});
<!doctype html>
<html lang="vi">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mô phỏng BCR nâng cao</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './styles.css';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
import React, { useState } from 'react';
import './styles.css';

export default function App() {
  const [history, setHistory] = useState([]);
  const [prediction, setPrediction] = useState('');

  const addResult = (result) => {
    const newHistory = [...history, result];
    setHistory(newHistory);
    localStorage.setItem('bcr_history', JSON.stringify(newHistory));
    simulatePrediction(newHistory);
  };

  const simulatePrediction = (h) => {
    if (h.length < 3) return setPrediction('Chưa đủ dữ liệu');
    const red = h.filter(r => r === 'Đỏ').length;
    const blue = h.filter(r => r === 'Xanh').length;
    const tie = h.filter(r => r === 'Hòa').length;

    const most = Math.max(red, blue, tie);
    if (most === red) setPrediction('Dự đoán: Đỏ');
    else if (most === blue) setPrediction('Dự đoán: Xanh');
    else setPrediction('Dự đoán: Hòa');
  };

  const reset = () => {
    setHistory([]);
    localStorage.removeItem('bcr_history');
    setPrediction('');
  };

  return (
    <div className="app">
      <h1>Mô phỏng BCR nâng cao (Cột cầu)</h1>
      <div className="controls">
        <button className="red" onClick={() => addResult('Đỏ')}>Đỏ</button>
        <button className="blue" onClick={() => addResult('Xanh')}>Xanh</button>
        <button className="green" onClick={() => addResult('Hòa')}>Hòa</button>
        <button onClick={reset}>Reset</button>
      </div>

      <div className="prediction">{prediction}</div>

      <div className="bead-road">
        {history.map((r, i) => (
          <div
            key={i}
            className={`bead ${r === 'Đỏ' ? 'red' : r === 'Xanh' ? 'blue' : 'green'}`}
          />
        ))}
      </div>
    </div>
  );
}
body {
  font-family: sans-serif;
  background: #f7f9fc;
  margin: 0;
  padding: 20px;
}

.app {
  text-align: center;
  max-width: 600px;
  margin: 0 auto;
}

.controls button {
  margin: 5px;
  padding: 10px 20px;
  border: none;
  font-size: 16px;
  border-radius: 8px;
  cursor: pointer;
  color: #fff;
}

button.red { background-color: #e74c3c; }
button.blue { background-color: #3498db; }
button.green { background-color: #2ecc71; }

.prediction {
  margin: 15px;
  font-weight: bold;
  font-size: 18px;
}

.bead-road {
  display: grid;
  grid-template-columns: repeat(auto-fill, 20px);
  gap: 5px;
  justify-content: center;
}

.bead {
  width: 20px;
  height: 20px;
  border-radius: 50%;
}

.bead.red { background-color: #e74c3c; }
.bead.blue { background-color: #3498db; }
.bead.green { background-color: #2ecc71; }
