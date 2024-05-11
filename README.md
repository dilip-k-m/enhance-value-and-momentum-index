# enhance-value-and-momentum-index
Project: Enhanced Value and Momentum Index

Concept of the product:
	value and mom investment through index investment in india stock exchange(NSE)
	Usually last 30 years data shows that N50, Mid150, Sml250 gives return of 12%, 20% and 27% cagr
	respectively and their risk/volatility are in increasing order.
	
	Indices we considered: Nifty 50, Mid cap 150, Small cap 250, Gold ETF
	
			N50(2-3 yr), Mid150(3-5 yrs), Sml250(>7 yr) 
	cagr:	12%pa,			 20				  30

	profit incurred by an asset over a period of time is directly proportional to Risk associated with that asset

	value = enter at low(est) pt and exit at high(est) pt
	mom = invest in relatively faster moving index

	Periodically rebalance portfolio as per risk appetite
	As per risk appetite of the investor, we will invest the capital based on current risk score analyzer.
	core-logic:
			logic of exit(SELL): at highest point of the market[ when market mood is in Greedy/extreem greedy zone and RSI is above 75 and pe is in overbought zone]
	
			logic of Enter(BUY): at lowest point of the market[ when market mood is in market mood is in fear/extreem fear zone and RSI is below 40 and pe is in oversold zone]
	
Product Lifecycle:
3 phases of deployment:
1. Initial load: Start of investment.
	Find the nearest date suitable to enter into the market to get into the index while it is cheaper and market mood is in fear/extreem fear zone.
	
	2 job is responsible for this:
	1. Daily job: Predict the Lowest and highest [i.e. point of entry and exit as per above logic] in forthcoming week for all the 3 indices.
	2. If today is the lowest point in the week for an index, open position allocating %age of capital for that index as calculated by the risk score analyzer
	
	risk score analyzer: RiskScoreMixFunc(tot-risk-score) using standard deviation[=risk factor] of all the 5 indices 
	 returns ratio of N50, SML, Mid, Gold, Emergency_Fund(Safest Debt/liquid fund) in mixture to achive the given risk-score
	

2. For Portfolio rebalance:

	3 jobs are responsible for this activity:
	N.B. Rule for selling of reserve to buy indices: First we need to exhaust Emergency_Fund and then go for selling of GOLD ETF. And vice versa as rule of buying of reserve after selling the indices
	1. Monthly Job: Predict the Lowest and highest [i.e. point of entry and exit as per above logic] in forthcoming quarter for all the 3 indices.
	2. Daily job: Predict the Lowest and highest [i.e. point of entry and exit as per above logic] in forthcoming week for all the 3 indices.
	3. Capital movement job::[Take action at real time pricing]:
		Enter(BUY): If today is the actual lowest point in the week for an index as predicted by job:1 and 2, and Index pe is in over-sold zone and market mood index is in fear/extreem-fear zone and RSI of the index is below 40 BUY the yet to be allocated %age of capital for that index as calculated by the risk score analyzer by selling that amount of GOLD/Emergency_Fund.
			If that position is already achieved, while Index pe is in over-sold zone and market mood index is in fear/extreem-fear zone, then follow a rule of buying the index: BUY of 10% GOLD+Emergency_Fund price equivalent for every 2% fall of the index to tackle war or covid like exceptional situation as per the rule for selling of reserve to buy indices. 
		Exit(Sell): If today is the actual highest point in the quarter for an index as predicted by job:1 and 2 and Index pe is in over-bought zone and market mood index is in Greedy zone and and RSI of the index is above 75, SELL the index upto the yet to be allocated %age of capital for that index as calculated by the risk score analyzer by buying that amount of GOLD/Emergency_Fund.
			If that position is already achieved while Index pe is in over-bought zone and market mood index is in Greedy zone and RSI of the index is above 75, then follow a rule of selling the index: sell the index of amount equivalent to 5% of should be allocated for GOLD+Emergency_Fund price, for every 2% rise of the index to profit from extreem bullish cycle as per the rule for buying of reserve after selling the indices. 
			
3. Partial Withdrawal(SWP):
		Follow the rule of selling the index as in Exit(Sell) phase of phase.2-step.3[at highest point of the market[ when market mood is in Greedy/extreem greedy zone and RSI is above 75 and pe is in overbought zone]]. withdraw the amount from all the 5 indexes as per the risk score analyzer's allocation %age reccomendation in each quarter.
		
4. delta load(SIP):
		Follow the rule of buying the index as in Enter(BUY) phase of phase.2-step.3[at lowest point of the market[ when market mood is in market mood is in fear/extreem fear zone and RSI is below 40 and pe is in oversold zone]]. Buy the amount from all the 5 indexes as per the risk score analyzer's allocation %age reccomendation in each quarter.
		
Code-base:
Monthly Job predicts next 90 days' closing price and daily job predicts next 7 days' closing price of all the 5 indexes using ARIMA and LSTM and uses most accurate ones by using last 30 years of the index data along with past 30 years of following dependent index/feature data:
	1. World's top 5 economy major indexes: like Shanghai Composite Index, Nikkei 225, Dow Jones Industrial Average, the S&P 500, the Nasdaq Composite index, India VIX, FTSE 100 Index 2.International Gold price and Gold price in India, 
	3.  FTSE World Government Bond Index (WGBI), SGI Double Short 10Y US Treasuries and US and India's 10 year GSEC etc
	4. India's GDP per capita
	5. India's export volume index of foreign trade
	
Why we used ARIMA and LSTM for our future price predictions?
Determining whether ARIMA or LSTM (Long Short-Term Memory) models are better depends on the specific characteristics of the data and the forecasting task at hand. Here are some considerations for when ARIMA might be favored over LSTM:

Linear Patterns: ARIMA models are well-suited for capturing linear relationships in time series data. If the underlying patterns in the data are relatively simple and linear, ARIMA can often perform well without the complexity of neural networks like LSTM.

Stationarity: ARIMA models require the data to be stationary or made stationary through differencing. If the data exhibits clear trends or seasonality that can be easily differenced out, ARIMA can be effective. LSTM models, on the other hand, do not require stationary data and can handle more complex non-linear relationships.

Interpretability: ARIMA models provide more transparent insights into the factors driving the forecasts. The coefficients of the AR and MA terms in an ARIMA model can offer direct interpretation in terms of the impact of past observations and past forecast errors on future values. This interpretability can be valuable in certain applications where understanding the drivers of the forecast is crucial.

Data Size: ARIMA models are often more suitable for small to medium-sized datasets. They are less computationally intensive compared to LSTM models, which require training large neural networks, especially when dealing with large volumes of data.

Short-term Forecasting: ARIMA models are generally better suited for short-term forecasting tasks where the underlying patterns are relatively stable over time. They can effectively capture short-term trends and seasonality without overfitting to noise in the data.

However, there are scenarios where LSTM models might be preferred:

Non-linear Patterns: If the underlying patterns in the data are highly complex or non-linear, LSTM models can outperform ARIMA. LSTMs are capable of capturing long-term dependencies in sequential data and can learn intricate patterns that ARIMA might struggle to capture.

Large-Scale Data: For large-scale datasets with complex temporal dependencies, LSTM models can be more effective. They are designed to handle sequential data of varying lengths and can automatically learn relevant features from the data through their architecture.

Long-term Forecasting: LSTM models are better suited for long-term forecasting tasks where the underlying patterns might evolve over time. They can capture complex trends and relationships that extend over longer time horizons.
