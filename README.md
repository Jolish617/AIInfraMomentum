# Decoding the AI Supply Chain: Infrastructure Momentum Predictor

This repository contains the code, exploratory data analysis, and backtesting results for a quantitative finance final project. The project utilizes a multi-output Stacked Long Short-Term Memory (LSTM) neural network to predict the momentum of secondary AI infrastructure equities (Cooling, Memory, and Power) based on the primary AI hardware catalyst (NVIDIA).

## Project Overview
While the market hyper-focuses on primary AI chip designers, the massive physical demands of these chips create a highly correlated—yet potentially lagging—secondary market in thermal management and infrastructure. Traditional linear models struggle to capture these shifting, non-stationary relationships. This project demonstrates how deep learning architectures, combined with risk-parity position sizing, can decode market lag and generate significant algorithmic alpha out-of-sample.

### Technologies Used
* **Data:** `yfinance` API (5-year daily interval)
* **Target Basket:** Vertiv (VRT), Micron (MU), Constellation Energy (CEG)
* **Catalyst Filter:** NVIDIA (NVDA)
* **Deep Learning Framework:** TensorFlow / Keras

---

## Repository Figures and Strategy Evolution

All figures in the `blog_figures` directory were generated via the central Jupyter Notebook (`.ipynb`). They document the end-to-end exploratory process, from data validation to strategy optimization.

### 1. Data Validation
Before deploying the neural network, the structural interdependency of the AI infrastructure basket was validated.
* **`Correlation Heat Map.png`**: Establishes the static baseline, proving strong positive cross-correlations between the primary catalyst (NVDA) and its supply chain.
* **`60-Day Rolling Correlation.jpg`**: Proves that the lead/lag relationship is highly volatile over time. This temporal instability invalidates traditional linear models (like ARIMA) and justifies the use of an LSTM to decode shifting regimes.

### 2. Model Diagnostics
* **`Huber Loss Function.png`**: Displays the training vs. validation loss curve. Aggressive regularization techniques (30% Dropout, Batch Normalization) and a Huber Loss function were utilized to prevent the model from overfitting to the extreme noise of the 2022-2023 market data.

### 3. Algorithmic Strategy Evolution (Backtesting)
The core of this research is the iterative refinement of the trading logic, moving from a basic directional predictor to a sophisticated risk-manager.

* **Iteration 1 (`L:S.png`): The Long/Short Pitfall** The initial strategy shorted the market when anticipating mean reversion. It drastically underperformed as it was repeatedly crushed by the unprecedented, secular AI bull market.
  
* **Iteration 2 (`L Cash.jpg`): The Long/Cash Pivot**
  To protect capital, the strategy was adjusted to exit to cash (0% return) on predicted down-days. While safer, the model suffered from "whipsawing" during sideways consolidation periods, resulting in opportunity cost.

* **Iteration 3 (`Risk Parity Opt.png`): Final Risk-Parity Optimization**
  The final, successful iteration shifted focus from purely predicting price to actively managing risk. By introducing an **NVDA Trend Filter** and **Inverse Volatility Scaling** (reducing position sizes during high-volatility regimes), the model achieved a highly stable equity curve. 
  * **Final Out-of-Sample Sharpe Ratio:** 1.56
  * **Final Directional Edge:** ~56%
  * **Result:** Material outperformance against the Buy & Hold market baseline with significantly reduced drawdowns.
