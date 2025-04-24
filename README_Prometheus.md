# VSVTrend Strategy: Trend-Following Trading with Supertrend on TradingView

## Project Overview

This repository contains the **VSVTrend Strategy**, a trading strategy script written in Pine Script for use on TradingView. The primary purpose of this codebase is to provide an automated trading strategy that helps traders identify and act on market trends using the Supertrend indicator as a filter.

### Key Features
- **Supertrend Filter**: Utilizes the Supertrend indicator to determine market direction and filter trading signals.
- **Configurable Parameters**: Offers customizable inputs such as ATR period, factor for Supertrend calculation, stop loss, and take profit percentages.
- **Long and Short Entries**: Supports both long and short positions based on Supertrend direction.
- **Risk Management**: Includes built-in stop loss and take profit settings to manage risk.

### Benefits
- **Automation**: Automates entry and exit decisions, reducing emotional trading biases.
- **Flexibility**: Allows users to toggle the strategy on/off and adjust settings to suit different market conditions or personal risk tolerance.
- **Visual Feedback**: Plots the Supertrend indicator on the chart for visual confirmation of trend direction.

This project is ideal for traders looking for a simple yet effective trend-following strategy to integrate into their TradingView platform.

## Getting Started, Installation, and Setup

### Getting Started

This project contains a Pine Script (`VSVTrend.pine`) for use in TradingView, a platform for trading and technical analysis. The script is likely a custom indicator or strategy to assist with trend analysis.

**Quick Start Guide:**
1. Open TradingView in your browser (https://www.tradingview.com).
2. Access the Pine Editor by clicking on 'Pine Editor' at the bottom of the chart interface.
3. Copy the contents of `VSVTrend.pine` from this repository.
4. Paste the script into the Pine Editor.
5. Click 'Add to Chart' to apply the script to your current chart.
6. Adjust any input parameters if available in the script settings to customize the output.

For detailed instructions on installation and setup, refer to the section below.

### Installation and Setup

Since this project consists of a Pine Script for TradingView, there is no traditional installation or build process. Follow these steps to set up and use the script:

#### Using the Script in TradingView
1. **Obtain the Script**: Download or copy the contents of `VSVTrend.pine` from this repository.
2. **Access TradingView**: Log in to your TradingView account via a web browser or the TradingView app.
3. **Open Pine Editor**: In TradingView, navigate to the chart view and click on 'Pine Editor' at the bottom of the screen.
4. **Paste the Script**: Paste the copied code from `VSVTrend.pine` into the editor.
5. **Compile and Add to Chart**: Click the 'Add to Chart' button to compile the script and apply it to your active chart.
6. **Customize Settings**: If the script includes configurable inputs, adjust them via the settings panel that appears after adding the script to the chart.

#### Platform-Specific Instructions
- **Web Browser**: TradingView works on any modern browser (Chrome, Firefox, Safari, etc.). No additional setup is required.
- **Mobile App**: You can use the TradingView app on iOS or Android, but editing Pine Scripts might be limited. It's recommended to use a desktop browser for full functionality.

#### Development and Testing
- There is no separate 'development' environment for Pine Scripts. All testing and modifications are done directly in the TradingView Pine Editor.
- Save your script in TradingView by clicking 'Save' in the Pine Editor to avoid losing changes.

#### Production Use
- Pine Scripts do not have a 'build' or 'release' process. Once added to a chart in TradingView, the script is live and functional.
- If you wish to share or publish the script, you can do so via TradingView's 'Publish Script' feature, subject to their terms and community guidelines.

#### Dependencies
- There are no external dependencies or libraries required beyond access to the TradingView platform.

If you encounter issues or need to modify the script, refer to TradingView's Pine Script documentation for language-specific guidance.

## API Reference

This section provides a detailed reference for the `VSVTrend` strategy, which is implemented in Pine Script for use in TradingView. Below, you will find information on the strategy's configuration inputs and key components.

### Strategy Configuration

- **`VSVTrend Strategy`**
  - **Description**: A trading strategy that uses the Supertrend indicator as a filter to determine entry and exit points for long and short positions. The strategy can be toggled on or off and allows customization of parameters such as ATR period, factor, stop loss, and take profit percentages.
  - **Inputs**:
    - `show_strategy: bool` - Toggles the strategy on or off. Default is `true`.
      - **Description**: Enables or disables the strategy execution.
    - `use_supertrend: bool` - Determines whether to use the Supertrend filter. Default is `true`.
      - **Description**: When enabled, the Supertrend indicator is used to filter trades.
    - `atrPeriod: int` - The period for calculating the Average True Range (ATR). Default is `10`.
      - **Description**: Defines the lookback period for ATR calculation, affecting the Supertrend sensitivity.
    - `factor: float` - The multiplier for ATR in the Supertrend calculation. Default is `3.0`.
      - **Description**: Adjusts the width of the Supertrend bands; higher values create wider bands.
    - `sl: float` - Stop Loss percentage. Default is `1.5` (i.e., 1.5%).
      - **Description**: Sets the stop loss level as a percentage of the entry price.
    - `tp: float` - Take Profit percentage. Default is `3.0` (i.e., 3.0%).
      - **Description**: Sets the take profit level as a percentage of the entry price.
  - **Logic**:
    - The strategy enters a long position when the Supertrend direction is bullish (direction == 1) and the strategy is enabled.
    - The strategy enters a short position when the Supertrend direction is bearish (direction == -1) and the strategy is enabled.
    - Exits are managed with predefined stop loss and take profit levels for both long and short positions.
  - **Example Usage**:
    ```pinescript
    //@version=5
    strategy("VSVTrend Strategy", overlay=true, default_qty_type=strategy.percent_of_equity, default_qty_value=10)
    show_strategy = input.bool(true, title="Strategy ON/OFF")
    use_supertrend = input.bool(true, title="Supertrend filter")
    atrPeriod = input.int(10, title="ATR period")
    factor = input.float(3.0, title="factor")
    sl = input.float(1.5, title="Стоп Лос (%)") / 100
    tp = input.float(3.0, title="Тейк Профит (%)") / 100
    [supertrend, direction] = ta.supertrend(factor, atrPeriod)
    longCond = show_strategy and (direction == 1)
    shortCond = show_strategy and (direction == -1)
    if (longCond)
        strategy.entry("Long", strategy.long)
    if (shortCond)
        strategy.entry("Short", strategy.short)
    strategy.exit("TP/SL Long", from_entry="Long", profit=tp, loss=sl)
    strategy.exit("TP/SL Short", from_entry="Short", profit=tp, loss=sl)
    ```

### Indicators

- **`Supertrend`**
  - **Description**: An indicator used to identify trends and potential reversals. It is plotted on the chart when the `use_supertrend` option is enabled.
  - **Parameters**:
    - `factor: float` - Multiplier for ATR. Default is `3.0`.
    - `atrPeriod: int` - Period for ATR calculation. Default is `10`.
  - **Return Values**:
    - `supertrend: float` - The calculated Supertrend value.
    - `direction: int` - Indicates the trend direction (1 for bullish, -1 for bearish).
  - **Example Usage**:
    - The Supertrend indicator is automatically calculated and plotted in the strategy script when enabled. Users can adjust `factor` and `atrPeriod` to modify its behavior.

## Project Structure

This repository has a simple structure with the following key files:

### Key Files
- **VSVTrend.pine**: The main script file written in Pine Script, likely used for creating a custom indicator or strategy for TradingView.
- **LICENSE**: Contains the licensing information for the repository.
- **README.md**: The main documentation file for the project.

## Additional Notes

This section provides supplementary information about the VSVTrend Strategy, a TradingView Pine Script designed for trend-following trading.

### Strategy Details
The VSVTrend Strategy is built to identify and follow market trends using the Supertrend indicator as a filter. When enabled, it enters long or short positions based on the direction of the Supertrend. Key features include:
- **Supertrend Filter**: Optionally use the Supertrend indicator to determine the trend direction. When active, trades are only initiated in the direction of the trend.
- **ATR Period and Factor**: Configurable parameters for the Supertrend calculation, allowing users to adjust the sensitivity of the trend detection.
- **Position Sizing**: The strategy uses a percentage of equity for position sizing, with a default of 10% per trade.

### Customization
Users can customize the strategy through the input parameters directly in TradingView:
- Toggle the strategy on or off.
- Enable or disable the Supertrend filter.
- Adjust the ATR period and factor for the Supertrend.
- Set Stop Loss and Take Profit levels as percentages to manage risk and reward.

### Usage Considerations
- **Backtesting**: Before deploying this strategy in a live trading environment, thoroughly backtest it using historical data on TradingView to understand its performance characteristics.
- **Risk Management**: Ensure that the Stop Loss and Take Profit settings align with your risk tolerance. The default values (1.5% SL, 3.0% TP) are starting points and may need adjustment based on market conditions or personal strategy.
- **Market Conditions**: This trend-following strategy may perform differently in trending versus ranging markets. Monitor its effectiveness and adjust parameters as needed.

### Disclaimer
This strategy is provided for educational and informational purposes only. Trading involves significant risk, and past performance in backtesting is not indicative of future results. Always conduct your own research and consult with a financial advisor before engaging in trading activities.

## Contributing

We welcome contributions from the community to help improve **VSVTrend** and make it an even better trading strategy. Whether you have ideas for new features, bug fixes, or optimizations, your input is valuable.

### How to Contribute
1. **Fork the Repository**: Start by forking the repository to your own GitHub account.
2. **Clone the Repository**: Clone the forked repository to your local machine for development.
3. **Make Changes**: Implement your changes or additions to the codebase. Ensure that your modifications align with the project's goals.
4. **Test Your Changes**: Test your updates thoroughly to ensure they work as expected and do not introduce new issues.
5. **Submit a Pull Request**: Push your changes to your forked repository and submit a pull request to the main repository. Provide a clear description of your changes and why they are beneficial.

### Contribution Guidelines
- **Code Style**: Follow the conventions and structure present in the existing Pine Script code (`VSVTrend.pine`). Ensure your code is clean, readable, and well-commented.
- **Testing**: Any new features or changes should be tested to confirm they function correctly across different timeframes and market conditions.
- **Documentation**: Update any relevant documentation or comments within the code to reflect your changes.
- **Focus on Compatibility**: Ensure that your contributions are compatible with TradingView's Pine Script version 5.

We look forward to collaborating with you to enhance **VSVTrend** and achieve the goal of creating the perfect alpha indicator strategy!

## License

This project is licensed under the MIT License. For more details, please see the [LICENSE](./LICENSE) file.