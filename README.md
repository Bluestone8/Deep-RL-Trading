# Deep Reinforcement Learning for Cryptocurrency Trading

A comprehensive collection of deep reinforcement learning models for algorithmic trading on cryptocurrency markets, specifically focused on ETH/KRW trading using Korean exchange APIs.

## Project Overview

This repository contains multiple deep RL implementations exploring different architectures (CNN, LSTM, Transformer) and algorithms (DQN, DDPG, SAC) for cryptocurrency trading. All models have been tested on real historical data from Korean exchanges (Bithumb, Coinone).

## Repository Structure

### Core Trading Models

#### 1. **01_cnn_dqn_hourly_trading.ipynb**

- **Architecture**: CNN-based DQN
- **Data**: Hourly ETH/KRW data
- **Features**:
  - 1D CNN for price pattern extraction
  - Experience replay buffer
  - Epsilon-greedy exploration
  - 2-year training/validation split
- **Actions**: 3 discrete (Hold, Buy, Sell)
- **Training**: 200-1000 episodes

#### 2. **02_cnn_dqn_with_dropout.ipynb**

- **Architecture**: CNN-DQN with regularization
- **Improvements**:
  - Dropout layers (0.3) for overfitting prevention
  - AdamW optimizer with weight decay
  - Transaction fees (0.1%)
  - Random episode start positions
  - Increased replay buffer size (20,000)
- **Results**: 34.52% return on validation

#### 3. **03_cnn_dqn_improved.ipynb**

- **Architecture**: Enhanced CNN-DQN
- **Improvements**:
  - Double DQN to reduce maximization bias
  - Huber loss instead of MSE
  - Soft target network updates (Polyak averaging)
  - Action space constraints (prevent consecutive buys/sells)
  - Lower learning rates (1e-4 for critic)
- **Performance**: 5.39% return

#### 4. **04_cnn_dqn_7actions.ipynb**

- **Architecture**: CNN-DQN with partial position management
- **Actions**: 7 discrete actions
  - Hold, Buy 10%, Buy 25%, Buy 50%
  - Sell 10%, Sell 25%, Sell 50%
- **Features**: Long-term PnL bonus rewards
- **Transaction costs**: 0.1% fee rate
- **Results**: Mixed performance with partial position scaling

#### 5. **05_ddpg_sac_comparison.ipynb**

- **Architectures**: DDPG and SAC for continuous actions
- **Environments**:
  - Continuous action space [-1, 1]
  - Partial position scaling
  - Drawdown risk penalties
  - Long-term PnL bonuses
- **Features**:
  - Actor-Critic with target networks
  - Experience replay
  - Gaussian exploration noise
  - Validation loss tracking
- **Results**: DDPG achieved 3.5% return on test data

#### 6. **06_cnn_dqn_synthetic.ipynb**

- **Architecture**: CNN-DQN on synthetic data
- **Purpose**: Proof of concept
- **Data**: Geometric Brownian Motion
- **Actions**: SELL (-1), HOLD (0), BUY (+1)
- **Features**: Basic DQN with experience replay

#### 7. **07_lstm_dqn_synthetic.ipynb**

- **Architecture**: LSTM-based DQN
- **Purpose**: Time-series modeling alternative
- **Features**:
  - 2-layer LSTM (hidden_dim=64)
  - Sequential pattern learning
  - Dropout (0.1)

#### 8. **08_transformer_dqn_synthetic.ipynb**

- **Architecture**: Transformer-based DQN
- **Purpose**: Attention mechanism exploration
- **Features**:
  - Positional encoding
  - Multi-head attention (4 heads)
  - 2-layer encoder
- **Results**: 114.73 test reward on synthetic data

#### 9. **09_transformer_dqn_real_data.ipynb**

- **Architecture**: Transformer-DQN on real Coinone data
- **Data**: Real cryptocurrency market data
- **Features**: Fallback to synthetic data on API errors

## Technical Stack

- **Deep Learning**: PyTorch
- **RL Algorithms**: DQN, Double DQN, DDPG, SAC
- **Architectures**: CNN, LSTM, Transformer
- **APIs**: Bithumb, Coinone
- **Data**: ETH/KRW hourly candlestick data

## Key Features Across Models

### Environment Characteristics

- **State Space**: 30-hour rolling window of normalized prices + features (balance ratio, position, volatility)
- **Reward Functions**:
  - Log returns on net worth
  - Long-term PnL bonuses
  - Drawdown risk penalties
- **Risk Management**: Transaction costs, position limits, drawdown tracking

### Training Improvements

- **Regularization**: Dropout, weight decay, gradient clipping
- **Stability**: Target networks, soft updates, experience replay
- **Exploration**: Epsilon-greedy, Gaussian noise, action constraints
- **Evaluation**: Train/validation splits, loss monitoring, greedy policy testing

## Results Summary

| Model       | Architecture | Returns | Key Features                          |
| ----------- | ------------ | ------- | ------------------------------------- |
| CNN-DQN v1  | Basic CNN    | 7.52%  | Daily trading, dropout regularization |
| CNN-DQN v2  | Enhanced CNN | 5.39%   | Double DQN, Huber loss, soft updates  |
| DDPG        | Actor-Critic | 3.5%    | Continuous actions, risk penalties    |
| Transformer | Attention    | Mixed   | Best on synthetic data                |

## Getting Started

### Prerequisites

```bash
pip install torch numpy matplotlib requests pandas
```

### Data Access

Models use Korean exchange APIs (Bithumb, Coinone) for real market data. Historical data spans 2-3 years of hourly ETH/KRW candles.

### Running a Model

1. Open any numbered notebook
2. Execute cells sequentially
3. Monitor training progress and validation metrics
4. Evaluate on held-out test data

## Key Learnings

1. **Architecture Impact**: CNN performed best on price patterns, Transformers showed promise on synthetic data
2. **Regularization Critical**: Dropout and weight decay essential for generalization
3. **Exploration Balance**: Too little exploration leads to poor policies, too much wastes training time
4. **Risk Management**: Transaction costs, drawdown penalties crucial for realistic performance
5. **Data Quality**: Synthetic data easier to optimize, real market data much noisier

## Future Work

- [ ] Multi-asset portfolio optimization
- [ ] Ensemble methods combining architectures
- [ ] Online learning for adaptation
- [ ] Risk-adjusted metrics (Sharpe ratio, Max drawdown)
- [ ] Production deployment with real-time trading
- [ ] Transaction cost optimization
- [ ] Market regime detection

## License

Educational/research purposes only. Not financial advice.

## Contact

For questions or collaborations, please open an issue or contact the repository owner.
