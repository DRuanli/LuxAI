# LuxAI

A sophisticated AI agent for the Lux AI Challenge, a multi-agent resource management and city-building competition designed to test algorithmic and strategic thinking.

## Overview

The Lux AI Challenge is a competition where players design agents to compete in a resource management game inspired by concepts like the tragedy of the commons and climate change. This agent implements a hybrid strategy combining rule-based heuristics and machine learning to efficiently gather resources, build cities, and outcompete opponents.

The game takes place on a procedurally generated 2D grid where two teams compete to collect resources, build cities, and research technologies to unlock more valuable resources. The winning condition is based on city and unit count after 360 turns, with ties determined by total resources collected.

## Installation

### Prerequisites

- Python 3.7+
- NumPy
- PyTorch (for the imitation learning component)

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/LuxAI.git
   cd LuxAI
   ```

2. Install the required dependencies:
   ```bash
   pip install numpy torch lux-ai-2021
   ```

3. Ensure the model file is available:
   ```bash
   # The model.pth file should be in the root directory
   ls model.pth
   ```

4. Verify the directory structure:
   ```bash
   # Ensure the lux module and its components are correctly set up
   ls lux/
   ```

## Usage

### Running Matches

To run a local match between two agents:

```bash
luxai-s2 path/to/agentA path/to/agentB --seed 42
```

Options:
- `--seed [number]`: Set a specific seed for reproducible matches
- `--out [filename]`: Save a replay file
- `--debug`: Enable debug mode for more detailed output
- `--maxtime [ms]`: Set the maximum time per turn (default: 3000ms)

### Submission

For Kaggle or other competition submissions, package the agent with:

```bash
# Create a submission.tar.gz file
tar -czvf submission.tar.gz agent.py heuristics.py make_*.py imitation_agent.py model.pth lux/
```

### Debugging

1. Local visualization:
   ```bash
   luxai-s2 path/to/agentA path/to/agentB --out replay.json
   lux-ai-vis replay.json
   ```

2. Save snapshots for later analysis:
   ```bash
   # Enable snapshots by modifying the GFOOTBALL_DATA_DIR environment variable
   export GFOOTBALL_DATA_DIR=""
   ```

## Game Mechanics

### Resources
- **Wood**: Basic resource (1 fuel per unit), available from start
- **Coal**: Mid-tier resource (10 fuel per unit), requires 50 research points
- **Uranium**: Advanced resource (40 fuel per unit), requires 200 research points

### Units
- **Workers**: Can gather resources, build cities, and transfer resources
- Collection rates: 20 wood, 5 coal, or 2 uranium per turn
- Each worker can carry up to 100 resource units

### Cities
- **City Tiles**: Basic building blocks of a city
- Each city tile enables training one unit beyond the initial unit
- Cities connected by adjacent tiles form a single city with shared fuel
- Each city consumes 23 * (number of tiles) fuel per night turn

### Day/Night Cycle
- Day: 30 turns, no fuel consumption
- Night: 10 turns, cities and units consume fuel
- Units without fuel during night will die
- Cities without fuel will lose tiles

### Research
- Cities generate 1 research point per turn when researching
- 50 research points: Unlock coal harvesting
- 200 research points: Unlock uranium harvesting

## Agent Architecture

This agent implements a sophisticated decision-making system with multiple components:

### Core Components

1. **Game State Manager**: Maintains and updates the game state representation
   - Tracks resources, units, cities, and their properties
   - Calculates distances, identifies resource clusters, and analyzes opponent positions

2. **Mission System**: Assigns and manages tasks for units
   - Each unit can be assigned a mission with a target position and action
   - Missions are prioritized and updated based on changing game conditions

3. **Heuristic Engine**: Makes strategic decisions based on game state analysis
   - Identifies best resource clusters to target
   - Evaluates city building locations
   - Determines optimal unit movements

4. **Imitation Learning Module**: Uses a trained neural network to guide worker actions
   - Processes game state into a tensor representation
   - Outputs action probabilities for workers
   - Augments rule-based decisions in complex situations

### Decision Flow

1. City actions are determined first (build units, research)
2. Unit missions are assigned based on resource needs and strategic objectives
3. Unit actions are executed according to missions and tactical considerations
4. Supplementary actions handle special cases and optimizations

## Strategy Details

### Resource Management

The agent employs a cluster-based approach to resource management:
- Identifies connected groups of resources and calculates their value
- Assigns units to clusters based on size, distance, and current unit allocation
- Dynamically adjusts targeting to avoid overcrowding or ignoring valuable resources

### City Planning

Cities are built strategically to:
- Maximize resource collection efficiency
- Create defensive positions against opponent expansion
- Form connected networks to share fuel reserves
- Ensure night-time survival with adequate fuel reserves

### Night Survival

The agent implements sophisticated night survival mechanisms:
- Calculates fuel requirements for each city to survive the night
- Prioritizes fuel delivery to cities at risk
- Routes resource-laden units to cities before nightfall
- Ejects units from cities that cannot be sustained

### Research Strategy

Research priorities are adjusted based on:
- Map resource distribution
- Current game state
- Opponent research progress
- Time remaining in the game

### Unit Coordination

Units coordinate through:
- Cluster-based targeting to avoid overcrowding
- Resource transfers between units
- City building coordination
- Ejection mechanisms to spread resources efficiently

## File Structure

Detailed breakdown of the codebase:

### Core Files
- `agent.py`: Main agent entry point that handles observation and action generation
  - Processes game state updates
  - Coordinates the decision-making components
  - Returns final actions for execution

- `heuristics.py`: Contains resource targeting and strategic decision algorithms
  - `find_best_cluster()`: Identifies optimal resource targets for units
  - Distance calculations and position evaluation functions
  - Scoring mechanisms for different game scenarios

- `make_actions.py`: Implements unit and city action generation
  - `make_city_actions()`: Determines city building and research actions
  - `make_unit_missions()`: Assigns missions to units
  - `make_unit_actions()`: Converts missions to concrete game actions
  - Path planning and collision avoidance logic

- `make_annotations.py`: Handles debugging visualizations
  - Annotates game state, unit movements, and missions
  - Creates visual indicators for resource clusters and targets

- `imitation_agent.py`: ML-based decision system
  - Loads and utilizes a trained PyTorch model
  - Transforms game state into model inputs
  - Processes model outputs into concrete game actions

### Lux Engine Interface
- `lux/__init__.py`: Module exports and initialization
- `lux/game.py`: Core game state representation
  - `Game` class: Maintains the complete game state
  - `Missions` class: Tracks unit missions and targets
  - Distance and path calculations
  - Resource and city tracking

- `lux/game_objects.py`: Game entity definitions
  - `Player`, `Unit`, `City`, `CityTile` classes
  - Resource and cargo handling
  - Action generation methods

- `lux/game_map.py`: Map representation and resource handling
- `lux/game_position.py`: Position tracking and movement calculations
- `lux/constants.py` & `lux/game_constants.py`: Game rules and parameters
- `lux/annotate.py`: Visualization utilities

## Troubleshooting

### Import Error Fix

The current error:
```
ImportError: cannot import name 'Game' from 'lux.game' (/Applications/AI/Agent/LuxAI/lux/game.py)
```

This indicates that the `Game` class is either not defined in `lux/game.py` or not exported correctly. To fix:

1. Check `lux/game.py` to ensure the `Game` class is properly defined:
   ```python
   class Game:
       # Class implementation
   ```

2. Verify that `lux/__init__.py` correctly exports the `Game` class:
   ```python
   from .game import Game, Missions, Observation
   ```

3. Check for circular imports in the codebase, which might cause import errors

4. Ensure the Python environment is consistent across your development and execution environments

### Common Issues

1. **Model Loading Errors**: If `model.pth` is missing or corrupted, the imitation learning component will fail
   - Solution: Verify the model file exists and is correctly formatted

2. **Timeout Errors**: If the agent exceeds the time limit (visible in the error logs)
   - Solution: Optimize computationally expensive operations or reduce complexity

3. **Memory Issues**: If the agent uses excessive memory
   - Solution: Optimize data structures and avoid unnecessary copies of game state

4. **Logic Errors**: If the agent makes poor decisions
   - Solution: Use the annotation system to visualize decision-making and debug specific scenarios

## Performance Optimization

The agent includes several optimizations:
- Caching of distance calculations
- Matrix-based operations using NumPy for efficiency
- Targeted search algorithms to avoid exhaustive searches
- Progressive decision-making to avoid timeout issues

## Contributing

Contributions to improve the agent are welcome:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Implement your changes with appropriate tests
4. Run local matches to validate performance
5. Commit your changes (`git commit -m 'Add amazing feature'`)
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

### Development Guidelines

- Add comments to explain complex algorithms and decision logic
- Follow the existing code style and patterns
- Include annotations for debugging critical decisions
- Test against various scenarios and map types
- Document performance impacts of significant changes

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- The Lux AI Challenge organizers for creating the competition
- The Kaggle community for insights and inspiration
- Contributors to the open-source libraries used in this project