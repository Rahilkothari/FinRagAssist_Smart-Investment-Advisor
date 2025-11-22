# **FinRagAssist — Smart Investment Advisor**

A unified **Financial Intelligence System** that blends **ML-powered risk profiling**, **AI-enhanced stock analysis**, and **RAG-based market research** into a single Gradio app.

FinRagAssist helps users:

* Understand their **risk category** (High / Medium / Low)
* Receive **portfolio allocation guidance**
* View **5-year investment projections**
* Analyze **technical indicators** (SMA, EMA, RSI, MACD) for any stock
* Compare two stocks using **RAG + 3-Year metrics + Pros/Cons + Final Verdict**

This project integrates **XGBoost**, **OpenAI (GPT-4o-mini)**, **ChromaDB**, and **yfinance**, wrapped inside a clean Gradio UI.

## 🚀 **Features**

### 🧠 **1. ML-Based Risk Profiler (XGBoost)**

* Inputs: Age Group, Income Group, Loan Status, Employment Status, Investment Goal, Investment Amount
* Outputs:

  * Risk Tolerance (High / Medium / Low)
  * Confidence Score
  * Suggested portfolio allocation
  * 5-Year projected value
  * Pie chart + line chart visualization

### 📈 **2. Stock Analysis with Technical Indicators**

For any Indian or global stock (INFY, TCS, HDFCBANK, AAPL, TSLA, etc.):

* Clean NSE ticker correction (`HDFCBANK` → `HDFCBANK.NS`)
* Technicals computed:

  * **SMA-20 / EMA-20**
  * **RSI-14**
  * **MACD + Signal**
* Auto-generated “Chart Note” summary using GPT
* Price + indicator plots

### 🔍 **3. RAG-Powered Market Research**

Uses **ChromaDB + sentence-transformers** to retrieve relevant stock snippets.
If not found, it **fetches fresh yfinance data**, embeds it, and stores it permanently.

### 🆚 **4. Stock Comparison Engine**

Given two tickers (INFY vs TCS):

* RAG summaries for each
* Auto-cleaned 3-year history
* Key metrics:

  * 1-year return
  * 3-year return
  * Volatility
  * Sharpe-like score
  * Drawdowns
* GPT-based Pros & Cons
* Final Verdict for **conservative investors**

## 🧱 **Architecture**

```
                            ┌────────────────────────┐
                            │        User UI         │
                            │       (Gradio)         │
                            └──────────┬─────────────┘
                                       │
             ┌─────────────────────────┼─────────────────────────┐
             │                         │                         │
     ┌───────▼────────┐       ┌────────▼─────────┐      ┌────────▼──────────┐
     │ Risk Profiler   │       │ Stock Analyzer    │      │ Stock Comparator  │
     │ (XGBoost)       │       │ (Tech Indicators) │      │ (RAG + Metrics)   │
     └───────┬────────┘       └────────┬──────────┘      └────────┬──────────┘
             │                         │                         │
     ┌───────▼────────┐       ┌────────▼─────────┐      ┌────────▼──────────┐
     │ Preprocessing   │       │ yfinance Fetcher │      │ RAG Retrieval      │
     │ (Scaling/Enc)   │       └────────┬──────────┘      └────────┬──────────┘
     └───────┬────────┘                 │                         │
             │                    ┌─────▼─────┐             ┌──────▼─────┐
             │                    │ Indicators │             │ ChromaDB    │
             │                    └────────────┘             └─────────────┘
             │                         │                         │
     ┌───────▼────────┐       ┌────────▼─────────┐      ┌────────▼──────────┐
     │   Output ML    │       │ Charts (matplotlib│      │ GPT Explanations  │
     └────────────────┘       └───────────────────┘      └───────────────────┘
```

## 🛠 **Tech Stack**

| Component         | Technology                               |
| ----------------- | ---------------------------------------- |
| ML Model          | **XGBoost Classifier**                   |
| Embeddings        | **sentence-transformers (MiniLM-L6-v2)** |
| Vector DB         | **ChromaDB (Persistent)**                |
| LLM               | **GPT-4o-mini (OpenAI)**                 |
| Market Data       | **yfinance**                             |
| UI                | **Gradio**                               |
| Charts            | **matplotlib**                           |
| Data Processing   | **Pandas, NumPy**                        |
| Model Persistence | **joblib**                               |


## 📦 Installation

### **1. Clone repo**

```bash
git clone https://github.com/<your-username>/FinRagAssist.git
cd FinRagAssist
```

### **2. Install dependencies**

```bash
pip install -r requirements.txt
```

### **3. Add `.env` file**

Create:

```
OPENAI_API_KEY=your_key
TAVILY_API_KEY=optional
```

## 📂 Project Structure

```
FinRagAssist/
│── app.py
│── README.md
│── cleaned_data_final.csv
│── xgb_risk_model.joblib
│── chroma_db/
│── models/
│── prompts/
│── utils/
│── requirements.txt
```

## 🧪 Training the Risk Model

The XGBoost classifier uses a **simplified engineered feature set**:

```
AgeGroup  
IncomeGroup  
EmploymentStatus  
LoanStatus  
InvestmentGoal  
InvestmentAmount
```

To retrain:

```python
from finragassist_blocks import train_xgb_model, load_data

df = load_data()
train_xgb_model(df)
```

The model will automatically:

-Clean and normalize user inputs
-Encode categorical values
-Scale numerical features
-Train using a robust XGBoost pipeline
-Save the trained model bundle to disk for inference


## ▶️ Running the App

```bash
python app.py
```

Gradio UI launches at:

```
http://127.0.0.1:7860
```

## 📸 Screenshots

```
## 📸 Screenshots

### 🔹 Risk Profiler
<img width="1892" height="566" alt="Screenshot 2025-11-22 193248" src="https://github.com/user-attachments/assets/ad1982a4-5b49-4c2c-a3fe-75f319bb27e5" />
<img width="913" height="953" alt="Screenshot 2025-11-22 193302" src="https://github.com/user-attachments/assets/a9ec61c3-8c62-4f88-bc12-68e7b479939f" />

### 🔹 Technical Charts
<img width="1884" height="819" alt="Screenshot 2025-11-22 195159" src="https://github.com/user-attachments/assets/f910dcbd-675b-432a-b2bc-847bc978cbdb" />
<img width="996" height="994" alt="Screenshot 2025-11-22 195218" src="https://github.com/user-attachments/assets/fdf44bfe-4ea3-4c8e-845a-ce558ca38d31" />

### 🔹 Stock Comparison
<img width="1883" height="575" alt="Screenshot 2025-11-22 195255" src="https://github.com/user-attachments/assets/e08e53f7-629f-4023-8607-d8cd6a1e004f" />
<img width="1895" height="1021" alt="Screenshot 2025-11-22 195313" src="https://github.com/user-attachments/assets/310ee340-6b7e-41bd-ae6c-98b30cd710a0" />

```

## 🔮 Future Improvements

* Add live NSE news using news APIs
* Add Python backtesting engine
* Export PDF report of recommendations
* Add user authentication + personalization

## 📝 License

MIT License. Free for personal & commercial use.

## 🙌 Acknowledgements

* OpenAI for GPT models
* ChromaDB for vector storage
* yfinance for stock history
* Gradio for fast UI prototyping
