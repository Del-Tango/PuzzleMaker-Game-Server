# Puzzle Maker Gaming Framework

## Overview

Puzzle Maker is an interactive gaming framework designed to create and manage text-based puzzle games through a structured chapter-section-segment system. It provides a comprehensive environment for building interactive narratives with multiple answer types, behavior controls, and game flow management.

## Features

- Hierarchical Game Structure: Chapters → Sections → Segments organization
- Multiple Answer Types: Single/multi-mode, set membership validation
- Flexible Behavior Control: Continue, repeat, restart, game-over actions
- Real-time Game Server: Network-enabled puzzle distribution
- Extensible Architecture: Easy puzzle creation and modification
- Comprehensive Logging: Detailed game session tracking
- Interactive CLI: User-friendly menu system

## Quick Start

#### Installation
```bash
git clone <repository-url>
cd puzzle-maker
```

#### Basic Usage
```bash
# Start in client mode (play puzzles)
./puzzle-maker client-mode

# Start in server mode (host puzzles)
./puzzle-maker server-mode

# Start with interactive menu system
./puzzle-maker server-cli
```

#### Game Structure

Puzzle Maker organizes games in a hierarchical structure:
```text
PuzzleDirectory/
├── game-banner          # Initial display
├── game-over           # Game end screen
├── Chapter1/
│   ├── Section1/
│   │   ├── Segment1.seg
│   │   └── Segment2.seg
│   └── Section2/
└── Chapter2/
```

### Segment File Format

Segment files (.seg) define individual puzzle levels with instructions and content:
```bash
# ANSWER_TYPE: single-mode
# EXPECTED: feel no pain
# BEHAVIOUR_OK: continue Hell yeah!
# BEHAVIOUR_NOK: continue-indexed That's not it and I'll remember what you did.
# NOK_LIMIT: 3

Ride on a train, dance in the rain or feel no pain?
```

### Answer Types

#### Single Mode

- Description: Exact match of single character, word, or number
- Expected Format: <word>

**Example**: *EXPECTED: dragon*

#### Multi Mode

- Description: Exact match of multiple characters, words, or numbers
- Expected Format: <multi word sentence>

**Example**: *EXPECTED: points of authority*

#### Single In

- Description: Single item from user must be found in set
- Expected Format: <word,word,word>

**Example**: *EXPECTED: red,blue,green*

#### Multi In

- Description: Multiple items from user must be found in set
- Expected Format: <word,word,word>

**Example**: *EXPECTED: apple,banana,orange*

### Behavior Controls

#### OK Behaviors (Correct Answers)

- continue - Proceed to next segment
- repeat - Repeat current segment
- restart - Restart from game banner
- game-over - End game immediately

#### NOK Behaviors (Incorrect Answers)

- continue-indexed - Continue counting failure
- continue-unindexed - Continue dismissing failure
- repeat-indexed - Repeat counting failure
- repeat-unindexed - Repeat dismissing failure
- restart - Restart from beginning
- game-over - End game immediately

#### Failure Limits

- NOK_LIMIT: 3 - Game over after 3 incorrect attempts

## Architecture

### Core Components
```text
src/
├── pm-actions.sh       # User interaction handlers
├── pm-computers.sh     # Answer validation logic
├── pm-fetchers.sh      # Data retrieval functions
├── pm-handlers.sh      # Game flow management
├── pm-processors.sh    # Behavior processing
├── pm-setters.sh       # Configuration management
└── puzzle-maker.sh     # Main controller
```

### Menu System

- Main Controller: Game server management
- Log Viewer: Session log inspection
- Control Panel: Configuration settings

### Configuration

Default configuration in conf/puzzle-maker.conf:

```bash
# Game Settings
PM_DEFAULT=(
['puzzle-dir']="$PM_DIRECTORY/puzzle"
['puzzle-path']="${PM_DEFAULT['puzzle-dir']}/DefaultPuzzle"
['puzzle-order']='sorted'      # sorted | scrambled
['puzzle-port']=8080
)

# Behavior Codes
PM_BEHAVIOURS_OK=(
['continue']=100
['repeat']=101
['restart']=102
['game-over']=103
)

PM_BEHAVIOURS_NOK=(
['continue-indexed']=104
['continue-unindexed']=105
['repeat-indexed']=106
['repeat-unindexed']=107
)
```

### API Reference

#### Core Game Functions

- handle_puzzle_segment() - Process individual puzzle segments
- handle_puzzle_section() - Manage section progression
- handle_puzzle_chapter() - Coordinate chapter flow
- handle_puzzle_playthrough() - Main game loop

#### Validation Functions

- compute_puzzle_segment_single_mode() - Single answer validation
- compute_puzzle_segment_multi_mode() - Multiple answer validation
- compute_puzzle_segment_single_in() - Set membership validation
- compute_puzzle_segment_multi_in() - Multiple set validation

#### Server Functions

- action_start_puzzle_server() - Launch game server
- fetch_start_puzzle_server_command() - Generate server command

**Examples:**

Simple Puzzle Segment

```bash
# ANSWER_TYPE: single-mode
# EXPECTED: linux
# BEHAVIOUR_OK: continue Correct!
# BEHAVIOUR_NOK: repeat-indexed Try again
# NOK_LIMIT: 3

What is the name of the penguin mascot for the Linux operating system?
```

Advanced Puzzle with Set Validation
```bash
# ANSWER_TYPE: multi-in
# EXPECTED: red,blue,green,yellow
# BEHAVIOUR_OK: continue Excellent!
# BEHAVIOUR_NOK: continue-unindexed Not quite right
# NOK_LIMIT: 5

Name at least two primary colors from the RGB color model:
```

## Server Mode

Puzzle Maker can operate as a network server:
```bash
# Start puzzle server on port 8080
./puzzle-maker server-mode

# Clients can connect via netcat
nc localhost 8080
```

The server handles multiple client connections and manages game sessions independently.

### Creating Custom Puzzles

1. Create Puzzle Directory
```bash
mkdir -p puzzle/MyPuzzle/Chapter1/Section1
```

2. Add Game Files

```bash
# game-banner
echo "Welcome to My Puzzle Game!" > puzzle/MyPuzzle/game-banner

# game-over
echo "Thanks for playing!" > puzzle/MyPuzzle/game-over
```

3. Create Puzzle Segments

```bash
cat > puzzle/MyPuzzle/Chapter1/Section1/Segment1.seg << 'EOF'
# ANSWER_TYPE: single-mode
# EXPECTED: 42
# BEHAVIOUR_OK: continue Correct!
# BEHAVIOUR_NOK: repeat Try again

What is the answer to life, the universe, and everything?
EOF
```

## Testing

Run Default Puzzle
```bash
./puzzle-maker client-mode
```

Test Server Functionality
```bash
./puzzle-maker server-mode
# In another terminal:
nc localhost 8080
```

### Debug Mode

Enable debug logging in configuration for detailed game flow analysis.

#### Dependencies

- Core Utilities: bash, sed, awk, grep
- Network Tools: ncat, curl, ping
- Text Processing: figlet, column
- System Tools: hostname

Install dependencies automatically:

```bash
# From control panel menu
Install-Dependencies
```

Regards, the Alveare Solutions #!/Society
