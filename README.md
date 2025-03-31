# LuxAI: Strategic Resource Management AI Agent

## Overview

LuxAI is a sophisticated AI agent designed for the Lux AI Challenge, a multi-agent resource management and city-building competition that tests advanced algorithmic and strategic thinking.

![Game Mechanics](https://raw.githubusercontent.com/Lux-AI-Challenge/Lux-Design-S1/master/assets/daynightshift.gif)

## Key Features

- **Hybrid Strategy**: Combines rule-based heuristics and machine learning
- **Advanced Resource Management**
- **Dynamic City Building**
- **Adaptive Decision-Making**

## Game Mechanics

The Lux AI Challenge takes place on a procedurally generated 2D grid where two teams compete to:
- Collect resources (Wood, Coal, Uranium)
- Build and expand cities
- Research technologies
- Survive day and night cycles

### Resource Hierarchy
1. **Wood**: Basic resource (1 fuel per unit)
2. **Coal**: Mid-tier resource (10 fuel per unit, requires 50 research points)
3. **Uranium**: Advanced resource (40 fuel per unit, requires 200 research points)

### Key Challenges
- Limited resource collection rates
- Day/Night cycle management
- Fuel consumption
- Technology research
- Competitive resource gathering

## Agent Architecture

### Core Components
1. **Game State Manager**
   - Tracks resources, units, and game state
   - Calculates strategic metrics

2. **Mission System**
   - Assigns and manages unit tasks
   - Prioritizes actions based on game conditions

3. **Heuristic Engine**
   - Makes strategic decisions
   - Identifies optimal resource clusters
   - Evaluates city-building locations

4. **Imitation Learning Module**
   - Uses neural network for advanced decision-making
   - Processes game state into action probabilities

### Decision-Making Process
The agent combines multiple approaches to make optimal decisions:
- Rule-based strategic planning
- Machine learning-guided action selection
- Dynamic resource management
- Adaptive mission assignment

## Installation

### Prerequisites
- Python 3.7+
- NumPy
- PyTorch

### Setup
```bash
git clone https://github.com/DRuanli/LuxAI.git
cd LuxAI
pip install numpy torch lux-ai-2021
```

## Usage

### Running Matches
```bash
luxai-s2 path/to/agentA path/to/agentB --seed 42
```

### Submission
```bash
tar -czvf submission.tar.gz agent.py heuristics.py model.pth lux/
```

## Contributing
1. Fork the repository
2. Create a feature branch
3. Implement changes
4. Run local matches
5. Submit a Pull Request

## License
MIT License

## Acknowledgments
- Lux AI Challenge Organizers
- Kaggle Community
- Open-Source Contributors