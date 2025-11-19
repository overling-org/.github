OverLing — 多言語 OCR オーバーレイ翻訳ツール
<img width="200" alt="Image" style="float: left;" src="https://github.com/user-attachments/assets/a4bee303-24dd-416f-8a36-e6b7a563497f" />

OverLing は、Web ページ上のテキストや画像内の文字を OCR で抽出し、
複数の言語を自動判別して 元の位置に重ねて翻訳を表示する Chrome 拡張機能です。

「文脈を崩さず、自然に読める翻訳体験」を実現します。

✨ 主要機能
🔍 1. 画像・テキスト両対応のマルチ言語 OCR

Web ページ、画像、スクリーンショットなど、あらゆる文字を自動抽出。

🎯 2. オーバーレイ翻訳

抽出した文字の位置（bbox）に、翻訳結果をそのまま重ねて表示。
ホバーすると翻訳/原文を切り替え可能。

🧠 3. 自動言語判別

ページ内に複数の言語が混在していてもブロックごとに判定。

⚡ 4. 高速・軽量

FastAPI + PaddleOCR による高速パイプライン。

🏗️ アーキテクチャ
Chrome Extension（Popup + Content Script）
        │ captureVisibleTab
        ▼
      Server（FastAPI）
 OCR → 言語判定 → 翻訳
        ▲
        │ JSON Response
        ▼
  Overlay Renderer（DOM オーバーレイ）

📦 ディレクトリ構成
overling/
  extension/
    manifest.json
    src/
      popup/
        popup.html
        popup.ts
      content/
        content.ts
      overlay/
        overlay.ts
        overlay.css
    package.json

  server/
    app.py
    requirements.txt
    services/
      ocr.py
      translate.py
      segment.py
    models/
      schemas.py
    utils/
      image.py
      metrics.py

  docs/
    api-spec.md
    architecture.md
    roadmap.md

  README.md

🚀 セットアップ
1. クローン
git clone https://github.com/overling/overling.git
cd overling

2. サーバー起動
cd server
pip install -r requirements.txt
uvicorn app:app --reload


http://localhost:8000
 で実行されます。

3. Chrome 拡張の読み込み
cd extension
npm install
npm run build


Chrome → 拡張機能 → 「デベロッパーモードをオン」 →
「パッケージ化されていない拡張機能を読み込む」 → extension/dist を選択。

🔌 API 仕様（MVP）
POST /translate

Request

{
  "image_base64": "data:image/png;base64,...",
  "target_lang": "ja",
  "options": {
    "return_lang_tag": true,
    "min_confidence": 0.6
  }
}


Response

{
  "viewport": { "width": 1280, "height": 720, "dpr": 2 },
  "results": [
    {
      "text_original": "Bonjour",
      "text_translated": "こんにちは",
      "src_lang": "fr",
      "confidence": 0.93,
      "bbox": [210,340,180,32]
    }
  ],
  "meta": {
    "ocr_ms": 420,
    "lang_detect_ms": 15,
    "translate_ms": 90,
    "pipeline_ms": 560
  }
}

🗺️ ロードマップ
v1.0

OCR + オーバーレイ翻訳

ホバーで原文/翻訳切替

ターゲット言語選択

v1.1

翻訳品質調整

オーバーレイのライト/ダークテーマ

行単位・段落単位のセグメンテーション改善

v2.0

スクロール連動でリアルタイム翻訳

クライアント側での WASM OCR

Firefox / Edge 対応

個人情報の自動マスキング

🤝 コントリビュート

Issue・Pull Request 歓迎します。

📄 ライセンス

MIT License.


-----------------------------------------------------------------------------------------------------------

OverLing — Multilingual OCR Overlay Translator
<img width="200" alt="Image" style="float: left;" src="https://github.com/user-attachments/assets/a4bee303-24dd-416f-8a36-e6b7a563497f" />

OverLing is a Chrome extension that captures visible text—including text inside images—detects multiple languages in a single page, and overlays translated text directly on top of the original content.

Designed to provide a seamless reading experience where translations appear exactly where the original text is.

✨ Key Features
🔍 OCR for Text & Images

Extract text from HTML elements, images, screenshots, and mixed-language environments using a multi-language OCR engine.

🎯 Inline Overlay Translation

Translated text is rendered at the exact screen position of the original text (using bounding boxes).
Hover to toggle original/translated text.

🧠 Automatic Language Detection

Each block of text is analyzed independently to determine its source language.

⚡ Fast & Lightweight

Powered by FastAPI + PaddleOCR.
Optimized for quick round-trip translation.

🏗️ Architecture
Chrome Extension (Popup, Content Script)
        │ captureVisibleTab
        ▼
      Server (FastAPI)
 OCR → Language Detection → Translation
        ▲
        │ JSON Response
        ▼
  Overlay Renderer (Absolute-position DOM)

📦 Directory Structure
overling/
  extension/
    manifest.json
    src/
      popup/
        popup.html
        popup.ts
      content/
        content.ts
      overlay/
        overlay.ts
        overlay.css
    package.json

  server/
    app.py
    requirements.txt
    services/
      ocr.py
      translate.py
      segment.py
    models/
      schemas.py
    utils/
      image.py
      metrics.py

  docs/
    api-spec.md
    architecture.md
    roadmap.md

  README.md

🚀 Getting Started
1. Clone
git clone https://github.com/overling/overling.git
cd overling

2. Start the server
cd server
pip install -r requirements.txt
uvicorn app:app --reload


The server runs at http://localhost:8000.

3. Load the Chrome extension
cd extension
npm install
npm run build


Open Chrome → Extensions → Enable Developer Mode →
"Load unpacked" → select extension/dist.

🔌 API (MVP)
POST /translate

Request

{
  "image_base64": "data:image/png;base64,...",
  "target_lang": "en",
  "options": {
    "return_lang_tag": true,
    "min_confidence": 0.6
  }
}


Response

{
  "viewport": { "width": 1280, "height": 720, "dpr": 2 },
  "results": [
    {
      "text_original": "Bonjour",
      "text_translated": "Hello",
      "src_lang": "fr",
      "confidence": 0.93,
      "bbox": [210,340,180,32]
    }
  ],
  "meta": {
    "ocr_ms": 420,
    "lang_detect_ms": 15,
    "translate_ms": 90,
    "pipeline_ms": 560
  }
}

🗺️ Roadmap
v1.0

OCR + translation overlay

Hover to toggle source/translated text

Target language selection

v1.1

Translation quality tuning

Light/Dark overlay themes

Better segmentation for multi-line text

v2.0

Real-time translation while scrolling

Client-side OCR (WebAssembly)

Firefox / Edge support

Privacy auto-masking

🤝 Contributing

Pull requests are welcome.
Open an issue to discuss new features or improvements.

📄 License

MIT License.
