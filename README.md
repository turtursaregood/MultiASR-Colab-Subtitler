# MultiASR-Colab-Subtitler

**三合一高速 AI 語音轉字幕 Google Colab 工具**  
支援 **YouTube 影片網址下載**、**本地音檔/影片上傳**、**雲端硬碟路徑讀取**，並整合目前開源界表現最優異的三大語音辨識引擎（**Faster-Whisper**、**SenseVoice**、**WhisperX**），一鍵自動產出 `.srt` 字幕檔、`.txt` 純文字稿與含時間戳記的閱讀稿。

---

## 快速啟動 (Open in Colab)

請根據需求點擊下方按鈕直接在 Google Colab 開啟：

| 版本分支 | 說明 | 啟動連結 |
| :--- | :--- | :--- |
| **`main` (穩定版)** | 經驗證可正常運作的穩定版本 | [![Open In Colab (main)](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/turtursaregood/MultiASR-Colab-Subtitler/blob/main/MultiASR_Colab_Subtitler.ipynb) |
| **`dev` (測試版)** | 最新功能開發與測試分支 | [![Open In Colab (dev)](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/turtursaregood/MultiASR-Colab-Subtitler/blob/dev/MultiASR_Colab_Subtitler.ipynb) |

> **提示**：開啟 Colab 後，請確認上方選單 **「執行階段 (Runtime)」 $\rightarrow$ 「變更執行階段類型 (Change runtime type)」** 已選擇 **T4 GPU**。

---

## 核心特色

1. **三大頂尖語音模型隨選隨用（動態按需安裝）**
   - 不必一次安裝所有龐大套件，選哪個模型才裝哪個套件，徹底避免 Colab 套件版本衝突。
   - **自動顯存釋放 (VRAM Cleanup)**：在同一次 Colab 連線中切換不同模型時，自動清空前一個模型的 GPU 記憶體，不會發生 OOM 崩潰。
2. **FFmpeg 自動音訊標準化預處理**
   - 無論是 YouTube 下載的 `.m4a`、上傳的 `.mp3` / `.mp4` / `.mkv` / `.flac`，皆自動轉為標準 `16kHz 單聲道 WAV`，解決舊版常見的 `Soundfile is malformed` 解碼報錯問題。
3. **內建 OpenCC 繁體中文自動轉換**
   - 支援將模型輸出的簡體字與詞彙自動轉為台灣繁體慣用詞（`s2twp`）。
4. **一次產出三種常用格式**
   - `transcript.txt`：純文字逐字稿
   - `transcript.srt`：標準影音時間軸字幕檔
   - `timestamped.txt`：帶 `[MM:SS -> MM:SS]` 時間標記的閱讀稿

---

## 內建三大模型比較與選用建議

| 引擎選項 | 底層模型 | 速度 | 特色與適用情境 |
| :--- | :--- | :---: | :--- |
| **Faster-Whisper (`large-v3-turbo`)** | OpenAI Whisper Large-v3-Turbo (CTranslate2) | 極快 | **預設首選**。兼具頂級準確率與高推論速度，內建 VAD 過濾空白與音樂，防止重複幻覺字幕。 |
| **Faster-Whisper (`large-v3`)** | OpenAI Whisper Large-v3 | 快 | 完整版最大參數模型，適合對生僻詞或複雜口音有極致要求時使用。 |
| **SenseVoice-Small (`FunASR`)** | 阿里達摩院 SenseVoice + FSMN-VAD | 光速 | 非自回歸架構，轉錄速度極快，對**中英夾雜 (Code-switching)**、粵語、日語、韓語辨識極強。 |
| **WhisperX (`large-v3-turbo`)** | Faster-Whisper + Wav2Vec2 音素對齊 | 快 | 透過強制音素對齊（Forced Alignment）提供**毫秒級精準度的 SRT 時間軸**。 |

---

## 使用步驟

1. **步驟 1（取得音訊）**：選擇「下載 YouTube 影片音訊」、「從電腦上傳新音檔」或「手動輸入檔案路徑」，點擊執行完成下載與 16kHz WAV 轉檔。
2. **步驟 2（語音辨識）**：從下拉選單選擇想要的 ASR 模型與語言（預設 `auto` 自動偵測），點擊執行即可自動生成三種字幕檔案。
3. **步驟 3（選用備份）**：一鍵將產出的字幕檔備份至您的 Google Drive。
