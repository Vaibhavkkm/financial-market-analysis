# Financial Market Analysis: Stochastic Models and Agent-Based Modeling

## Overview

This project provides comprehensive implementations of financial market simulations using both **stochastic models** and **agent-based modeling (ABM)**. The analysis explores how different mathematical models and agent behaviors can simulate realistic stock price movements and market dynamics over time.

The project demonstrates four distinct approaches to modeling financial markets:
1. **Geometric Brownian Motion (GBM)** - Classical stochastic model
2. **Heston Model** - Advanced stochastic volatility model
3. **Agent-Based Model (ABM)** - Multi-agent market simulation
4. **Hybrid Model** - Combined GBM and ABM approach with diverse agent types

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Models Description](#models-description)
  - [1. Geometric Brownian Motion (GBM)](#1-geometric-brownian-motion-gbm)
  - [2. Heston Model](#2-heston-model)
  - [3. Agent-Based Model (ABM)](#3-agent-based-model-abm)
  - [4. Hybrid Model (GBM + ABM)](#4-hybrid-model-gbm--abm)
- [Agent Types](#agent-types)
- [Parameters Explanation](#parameters-explanation)
- [Output and Visualizations](#output-and-visualizations)
- [File Structure](#file-structure)
- [References](#references)

## Features

- **Multiple Stochastic Models**: Implementation of GBM and Heston models for realistic price simulations
- **Agent-Based Modeling**: Simulation of market dynamics with heterogeneous agents
- **Diverse Agent Types**: Includes fundamentalists, chartists, noise traders, contrarians, and institutional traders
- **Hybrid Approach**: Combines stochastic processes with agent behavior for comprehensive market simulation
- **Visualization**: Detailed plots showing price evolution, volatility dynamics, and agent interactions
- **Reproducible Results**: All simulations use seeded random number generators for consistency

## Requirements

- Python 3.x
- NumPy - For numerical computations and random number generation
- Matplotlib - For data visualization and plotting
- Jupyter Notebook/JupyterLab - For running the interactive notebook

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Vaibhavkkm/financial-market-analysis.git
   cd financial-market-analysis
   ```

2. **Install required packages:**
   ```bash
   pip install numpy matplotlib jupyter
   ```

   Or using conda:
   ```bash
   conda install numpy matplotlib jupyter
   ```

## Usage

1. **Launch Jupyter Notebook:**
   ```bash
   jupyter notebook FinancialMarketAnalysis.ipynb
   ```

2. **Run the cells sequentially** to see each model in action:
   - Execute cells in order from top to bottom
   - Each section is self-contained and demonstrates a different modeling approach
   - Visualizations will appear inline showing price movements and market dynamics

3. **Modify parameters** as needed to experiment with different market conditions:
   - Adjust volatility, drift, and other model parameters
   - Change the number of agents or their initial wealth
   - Modify trading days or time horizons

## Models Description

### 1. Geometric Brownian Motion (GBM)

The GBM model simulates stock price movements using a stochastic differential equation. It's the foundation of the Black-Scholes option pricing model.

**Key Formula:**
```
dS = μS dt + σS dW
```

Where:
- `S` = Stock price
- `μ` = Drift (expected return)
- `σ` = Volatility (standard deviation of returns)
- `dW` = Wiener process (Brownian motion)

**Implementation Details:**
- Simulates 252 trading days (one year)
- Uses discrete-time approximation: `S(t) = S(t-1) * exp(drift + shock)`
- Drift component: `(μ - 0.5σ²)dt` ensures proper mean
- Shock component: `σ * N(0, √dt)` adds random fluctuations

**Default Parameters:**
- Initial Price: $100
- Annual Drift (μ): 10% (0.10)
- Annual Volatility (σ): 20% (0.20)
- Trading Days: 252

### 2. Heston Model

The Heston model extends GBM by introducing **stochastic volatility**, allowing volatility itself to vary randomly over time. This captures real market phenomena like volatility clustering.

**Key Features:**
- Volatility follows a mean-reverting square root process
- Correlation between price and volatility movements
- More realistic modeling of market behavior

**Implementation Details:**
- Uses two correlated Brownian motions
- Volatility equation: `dv = κ(θ - v)dt + σ√v dW₂`
- Price equation: `dS = μS dt + √v S dW₁`
- Ensures non-negative volatility

**Default Parameters:**
- Initial Price (S₀): $100
- Initial Volatility (v₀): 0.04
- Drift (μ): 0.10
- Mean Reversion Speed (κ): 2.0
- Long-term Variance (θ): 0.04
- Volatility of Volatility (σ): 0.3
- Correlation (ρ): -0.7 (negative correlation typical in markets)
- Time Horizon (T): 1 year
- Time Step (dt): 1/252

### 3. Agent-Based Model (ABM)

The ABM simulates a financial market where individual agents make trading decisions based on their beliefs and strategies. Price emerges from the collective behavior of all agents.

**Agent Types:**
1. **Fundamentalists** (40%): Trade based on fundamental value
2. **Chartists** (30%): Follow technical trends and momentum
3. **Noise Traders** (30%): Trade randomly

**Mechanism:**
- Each agent decides to buy or sell based on their strategy
- Orders are aggregated to calculate net demand
- Price adjusts based on net demand: `price_change = 0.1 * net_demand`
- Includes random market noise for realism

**Implementation Details:**
- 100 agents with initial wealth of $1,000 each
- Agents make decisions every trading day
- Price impact function translates demand to price changes
- Historical prices influence chartist decisions

### 4. Hybrid Model (GBM + ABM)

The Hybrid model combines the strengths of both approaches: stochastic price evolution from GBM with behavioral impacts from agent trading.

**Five Agent Types:**
1. **Fundamentalists** (25%): Buy when price < $100, sell when price > $100
2. **Chartists** (25%): Follow momentum (buy if trending up, sell if trending down)
3. **Noise Traders** (20%): Random trading with no strategy
4. **Contrarians** (15%): Trade against trends (buy when price drops, sell when price rises)
5. **Institutional Traders** (15%): Large orders based on longer-term moving averages

**Price Evolution:**
```
price(t) = price(t-1) * exp(drift + shock + agent_impact)
```

Where:
- `drift` = (μ - 0.5σ²)dt (GBM component)
- `shock` = σ * N(0, √dt) (stochastic component)
- `agent_impact` = 0.001 * net_demand (behavioral component)

**Default Parameters:**
- Initial Price: $100
- Annual Drift (μ): 0.10
- Annual Volatility (σ): 0.20
- Number of Agents: 100
- Agent Impact Factor: 0.001
- Trading Days: 252

## Agent Types

### Fundamentalists
- **Strategy**: Compare current price to fundamental value ($100)
- **Behavior**: Buy when undervalued, sell when overvalued
- **Order Size**: 5-15 units (ABM), 5-10 units (Hybrid)
- **Market Impact**: Stabilizing force, pushes price toward fundamental value

### Chartists
- **Strategy**: Technical analysis based on recent price trends
- **Behavior**: Buy in uptrends, sell in downtrends
- **Order Size**: 3-8 units (ABM), 5-10 units (Hybrid)
- **Analysis Window**: Last 5-10 days
- **Market Impact**: Can amplify trends, creating momentum

### Noise Traders
- **Strategy**: Random, uninformed trading
- **Behavior**: Randomly buy or sell regardless of price
- **Order Size**: 1-5 units (ABM), 1-5 units (Hybrid)
- **Market Impact**: Adds randomness and liquidity to the market

### Contrarians (Hybrid Model Only)
- **Strategy**: Counter-trend trading
- **Behavior**: Buy after price drops, sell after price increases
- **Order Size**: 5-10 units
- **Market Impact**: Can dampen extreme price movements

### Institutional Traders (Hybrid Model Only)
- **Strategy**: Long-term moving average analysis
- **Behavior**: Large orders based on 20-day moving average
- **Order Size**: 10-20 units (much larger than other agents)
- **Market Impact**: Significant price impact due to order size

## Parameters Explanation

### GBM Parameters
- **Initial Price (S₀)**: Starting stock price (typically $100)
- **Drift (μ)**: Expected annual return (e.g., 0.10 = 10%)
- **Volatility (σ)**: Annual standard deviation of returns (e.g., 0.20 = 20%)
- **Trading Days**: Number of time steps (252 = one trading year)
- **Time Step (dt)**: Fraction of year per step (1/252)

### Heston Parameters
- **Mean Reversion Speed (κ)**: How quickly volatility returns to long-term mean
- **Long-term Variance (θ)**: Target variance level
- **Vol of Vol (σ)**: Volatility of the variance process
- **Correlation (ρ)**: Correlation between price and volatility (-1 to 1)

### ABM Parameters
- **Number of Agents**: Total market participants (100 in implementation)
- **Agent Distribution**: Percentage allocation to each agent type
- **Risk Aversion**: Agent's willingness to take risk
- **Initial Wealth**: Starting capital for each agent ($1,000)
- **Price Impact Factor**: How much net demand affects price

### Hybrid Parameters
- **All GBM parameters** (drift, volatility, time step)
- **All ABM parameters** (agents, distribution, impact factor)
- **Agent Impact Scaling**: Weight of agent behavior vs. stochastic component

## Output and Visualizations

Each model generates detailed visualizations:

### GBM Output
- **Line plot**: Stock price evolution over 252 trading days
- **X-axis**: Trading days (0-252)
- **Y-axis**: Stock price ($)
- **Title**: "Simulated Stock Prices (GBM)"

### Heston Model Output
- **Two subplots**:
  1. Stock price trajectory over time
  2. Volatility evolution showing stochastic volatility
- Demonstrates volatility clustering and mean reversion

### ABM Output
- **Line plot**: Price evolution driven purely by agent behavior
- Shows emergent price dynamics from individual agent decisions
- Highlights impact of heterogeneous trading strategies

### Hybrid Model Output
- **Line plot**: Combined effect of stochastic and behavioral factors
- More realistic price movements incorporating both randomness and agent actions
- Demonstrates complex market dynamics

All plots include:
- Grid lines for easy reading
- Clear labels and titles
- Proper axes scaling
- Legend where applicable

## File Structure

```
financial-market-analysis/
│
├── FinancialMarketAnalysis.ipynb    # Main Jupyter notebook with all implementations
├── README.md                         # This file - Project documentation
├── LICENSE                           # MIT License
├── MATH_MODELLING_MANGROLIYA.pptx   # Presentation materials
└── ODD_MATHMODELLING_MANGROLIYA.pdf # Mathematical modeling documentation
```

## References

### Mathematical Finance
- Black, F., & Scholes, M. (1973). "The Pricing of Options and Corporate Liabilities"
- Heston, S. L. (1993). "A Closed-Form Solution for Options with Stochastic Volatility"
- Hull, J. C. "Options, Futures, and Other Derivatives"

### Agent-Based Modeling
- LeBaron, B. (2006). "Agent-based Computational Finance"
- Cont, R., & Bouchaud, J. P. (2000). "Herd Behavior and Aggregate Fluctuations in Financial Markets"
- Farmer, J. D., & Foley, D. (2009). "The Economy Needs Agent-Based Modelling"

### Stochastic Processes
- Shreve, S. E. "Stochastic Calculus for Finance"
- Øksendal, B. "Stochastic Differential Equations"

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

**Vaibhav Kumar**
- GitHub: [@Vaibhavkkm](https://github.com/Vaibhavkkm)

## Acknowledgments

This project demonstrates the intersection of quantitative finance, stochastic calculus, and computational economics. It's designed for educational purposes to help understand:
- How stochastic models capture market randomness
- How agent behavior influences price discovery
- The complex dynamics of financial markets
- The integration of mathematical and computational approaches to finance
