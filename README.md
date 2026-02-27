# Project README: Trader Performance and Behavior Analysis with Market Sentiment

## 1. Introduction
This project analyzes trader performance and behavior using historical trading data and a Fear & Greed Index to understand how market sentiment influences trading outcomes and decisions. The primary goal is to identify trader segments and propose actionable trading strategies.

## 2. Data Preparation

To prepare the data for analysis, the following steps were taken:

*   **Data Loading:** Both `historical_data.csv` (trades) and `fear_greed_index.csv` (sentiment) datasets were loaded into pandas DataFrames.
*   **Initial Documentation:** Basic summaries including row/column counts, missing values, and data types were documented for both datasets.
*   **Timestamp Conversion & Alignment:** The `Timestamp IST` column in the `trades_df` and the `date` column in `sentiment_df` were converted to datetime objects. A daily `date` column was created for both DataFrames to enable daily-level aggregation and merging.
*   **Key Metric Creation:** The `trades_df` was aggregated to a daily level (`daily_trades`), calculating:
    *   `total_pnl`: Sum of daily 'Closed PnL'.
    *   `num_trades`: Count of daily 'Trade ID'.
    *   `avg_trade_size`: Mean of daily 'Size USD'.
    *   `daily_wins`: Count of trades with 'Closed PnL' > 0.
    *   `Buy_count` and `Sell_count`: Number of buy and sell trades per day.
    *   `win_rate`: Ratio of `daily_wins` to `num_trades`.
    *   `long_short_ratio`: Ratio of `Buy_count` to `Sell_count`.
*   **Data Merging:** The `daily_trades` DataFrame was merged with the `sentiment_df` on the `date` column to combine trading metrics with market sentiment classifications.

## 3. Analysis and Insights

The analysis focused on understanding the interplay between market sentiment and trader performance/behavior, and identifying distinct trader segments.

### Performance by Sentiment

*   **Total Daily PnL:** Highest during 'Extreme Fear' (average $41,830) and 'Fear' (average $29,677) days, significantly lower during 'Greed' (average $6,445) periods. This suggests higher profit opportunities in volatile, down-trending markets.
*   **Average Win Rate:** Relatively consistent across most sentiments (around 32-33%), with 'Extreme Greed' days showing a slightly higher average win rate (45%). This indicates that the *proportion* of winning trades does not drastically change, though PnL does.
*   **Average Trade Size:** Remained relatively stable across sentiments, with 'Neutral' days showing the largest (average $7,472) and 'Extreme Fear' days the smallest (average $4,233). This might reflect cautious position sizing during peak fear.

### Trader Behavior by Sentiment

*   **Average Number of Trades:** Significantly higher during 'Extreme Fear' (average 600 trades) and 'Fear' (average 402 trades) days, indicating increased trader activity and market volatility.
*   **Average Trade Size:** Largely aligns with performance observations, being smallest in 'Extreme Fear' periods, suggesting traders may reduce risk by decreasing position sizes when market fear is high.

### Identified Trader Segments

By analyzing individual account metrics, we observed wide variations in:

*   `total_pnl_account`
*   `total_trades_account`
*   `avg_trade_size_account`
*   `win_rate_account`

These variations highlight diverse trading styles and success levels among different accounts, providing a basis for further segmentation (e.g., high-frequency vs. low-frequency, consistent winners vs. inconsistent).

## 4. Actionable Trading Strategies

Based on the derived insights, two actionable strategies are proposed:

### Strategy 1: Counter-Sentiment Aggression (with Caution)
*   **Recommendation:** Strategically increase trading frequency and potentially position size (if risk management allows) during periods of 'Extreme Fear' or 'Fear'. Focus on fundamentally sound assets that might be oversold. Conversely, reduce exposure and trading frequency during 'Greed' periods.
*   **Rationale:** The data indicates that the highest average total PnL is achieved during 'Extreme Fear' and 'Fear' days, despite consistent win rates. This implies larger profit magnitudes from winning trades during market downturns, suggesting opportunities for buying low. Increased trading activity during these periods also suggests higher volatility, which can present more entry/exit points. Prudent risk management, such as cautiously adjusting position size (as suggested by the slightly lower average trade size during 'Extreme Fear'), is crucial.

### Strategy 2: Diversified/Adaptive Sizing (based on sentiment and individual skill)
*   **Recommendation:** Implement a dynamic strategy where average trade size is adjusted based on both prevailing market sentiment and an individual trader's proven historical win rate. Traders with consistently high individual win rates might maintain larger position sizes regardless of sentiment. Other traders should scale down during 'Extreme Fear' or 'Greed' to mitigate risk and potentially scale up during 'Neutral' or 'Fear' periods where PnL opportunities are present but the market is not excessively speculative.
*   **Rationale:** While the win rate is relatively stable across most sentiments, total PnL fluctuates significantly, suggesting that trade sizing plays a critical role in overall profitability. Individual account metrics demonstrate that some traders consistently achieve higher win rates. This supports an adaptive sizing approach: experienced, successful traders might leverage their skill more aggressively, while others should adjust their risk exposure based on market conditions, increasing size when conditions are favorable and reducing it when risk is higher.
