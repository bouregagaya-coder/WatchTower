# WATCHTOWER

A secure Telegram-based managerial assistant for e-commerce inventory
optimisation and business intelligence. Master's thesis project in Artificial Intelligence.

## The problem
Small store owners struggle to get useful insights from raw sales data,
and they often decide when to restock by guesswork.

## What it does
- **Inventory optimisation:** computes optimal order quantities (Wilson EOQ)
  and reorder points with a safety stock at a 95% service level,
  and sends proactive Telegram alerts when stock falls below the threshold.
- **Business questions in plain language:** a four-stage query pipeline
  answers questions about your sales and stock data.
- **Security by design:** only one authorised manager can use the bot.

## Architecture

### 1. Zero Trust authentication
A Python decorator with rate limiting. It checks the Telegram User ID
against a whitelist and silently discards requests from anyone else.

### 2. Inventory optimisation engine
Wilson EOQ extended with safety stock (95% service level), computed from
historical sales data. Alert latency is under 2 seconds.

### 3. Natural language query pipeline
1. Prompt injection guard (17 patterns) blocks adversarial inputs
2. Common analytical questions are answered by direct Pandas computation
3. Other questions go to a locally hosted Llama 3 model through a
   custom OllamaLLM adapter
4. Retrieval Augmented Generation (RAG) keeps the answers grounded
   in verified store data

## Validation
Tested on six structured scenarios: authentication, EOQ/ROP numerical
accuracy, alert latency, prompt injection defence, and grounded
strategic analysis.

## How to run
1. Install libraries: `pip install -r requirements.txt`
2. Install Ollama and pull the Llama 3 model
3. Create a `.env` file with `TELEGRAM_TOKEN=...` and `AUTHORIZED_USER_ID=...`
4. Open `notebooks/watchtower.ipynb` and run the cells in order

## Note on persistence
The notebook uses a dual-write mechanism that mirrors a production
SQLite deployment.

## Tech stack
Python, Pandas, Ollama (Llama 3), Telegram Bot API, SQLite

## Author
Aya, Master's in Artificial Intelligence
