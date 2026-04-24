Movement System Options (Design Exploration)

=============Option 1: Top-Down Grid-Based (Classic Frogger/Crossy Road)=============

*Description
-Player moves on a 2D grid from a top-down perspective
-Each input = one tile movement
-Camera is fixed above

*Pros
-Very simple to implement
-Clean collision logic
-Matches assignment expectations closely
-Easy to debug and test
-Works perfectly with scoring (forward = progress)

*Cons
-Less visually dynamic
-Limited depth in movement expression

*Best For
-Fast implementation
-Reliable team integration
-“Safe” choice

=============Option 2: Side View Grid-Based (2.5D Frogger Variant)=============

*Description
-Still grid-based, but viewed from the side
-Movement feels more like:
-Forward = into screen (lane change)
-Left/Right = horizontal

*Pros
-More visually interesting than top-down
-Still keeps grid simplicity
-Easier to add animation polish

*Cons
-Slightly harder to conceptualize grid
-Camera + depth can confuse collision logic

*Best For
-Teams wanting aesthetic upgrade without complexity explosion

=============Option 3: Isometric / Free Movement (Advanced)=============

*Description
-Angled camera (isometric)
-Movement can be:
-Grid-based OR
-Fully analog (free movement)

*Pros
-Looks AAA compared to others
-More immersive
-Opens design space

*Cons 
-Hardest to implement
-Collision becomes more complex
-Not ideal for a short sprint

*Best For
-Only if your team wants to push beyond scope

=============Option 4: Direction + Action Movement (Input Variant)=============

*Description

Instead of instant movement:

1. Player chooses direction
2. Player presses confirm/move button

Like:

Turn → Commit → Move
Example:
-Arrow key = face direction
-Space = move

*Pros
-More deliberate gameplay
-Adds strategy
-Cleaner separation of:
-Facing
-Movement

*Cons
-Slower pacing
-Less “arcade feel”
-Might not match Frogger-style expectations

==============================================================================
Design system supporting multiple movement types:

enum class MovementMode
{
    Grid_TopDown,
    Grid_SideView,
    Free_Isometric
};

enum class InputMode
{
    InstantMove,
    DirectionThenMove
};

The movement system is designed to support multiple gameplay styles,
including top-down grid-based movement, side-view grid movement, and
isometric free movement. While the primary implementation will focus on a top-
down grid-based system for reliability and simplicity, the architecture allows for
extensibility through configurable movement and input modes. Additionally, an
alternative input system separating directional input from movement execution
is proposed to support more deliberate gameplay interactions.

==============================================================================

Core Movement Feature Responsibilities

1. Movement Rules

You define how the player is allowed to move.

This includes:

enum class MovementMode
{
    TopDownGrid,
    SideViewGrid,
    IsometricFree
};

Your system should document/support:

Top-down grid movement
Side-view grid movement
Isometric/free movement
Direction-then-move controls
Instant movement controls

Your team may only build one version, but your documentation shows the feature can scale.

2. Input Handling

You own how player input becomes movement.

Keyboard examples:

enum class Direction
{
    Up,
    Down,
    Left,
    Right,
    None
};

Supported input options:

Arrow Keys
WASD
Space / Enter for confirm movement
Optional controller support:
D-pad
Left stick
A / X button for confirm

3. Control Modes

This is where your movement options connect to accessibility.

Mode A: Instant Movement

Player presses a direction and immediately moves.

Example:

W / Up Arrow = move forward one tile

Best for:

Classic Crossy Road / Frogger feel
Fast arcade gameplay
Mode B: Direction Then Move

Player points/faces first, then presses a button to move.

Example:

Arrow Key = face direction
Space = move

Best for:

Players who need more time
Reduced input stress
More deliberate gameplay

This is a strong accessibility option.

4. Movement Validation

Before moving, your system checks:

Is the target tile inside the grid?
Is the tile blocked?
Is the player already moving?
Is the player allowed to move right now?

Example:

bool CanMoveTo(int x, int y);

This gives collision and obstacle teammates a clean integration point.

5. Player State

You track basic movement-related player state:

struct PlayerMovementState
{
    int gridX;
    int gridY;

    Direction facingDirection;
    MovementMode movementMode;

    bool isMoving;
    bool inputEnabled;
};

This supports both normal movement and accessibility modes.

6. Accessibility Options

Your feature can support:

Control Remapping

Players can change movement keys.

Example:

Move Up: W or Up Arrow
Move Down: S or Down Arrow
Confirm Move: Space
Hold vs Tap Movement

Options:

Tap to move one tile
Hold to repeat movement

This helps different player preferences.

Input Delay / Repeat Rate

Players can adjust how quickly held input repeats.

Example:

Repeat Delay: 0.25 seconds
Repeat Delay: 0.50 seconds
Repeat Delay: 0.75 seconds

This helps avoid accidental rapid movement.

Direction Preview

For direction-then-move mode, show where the player will move before committing.

Example:

Player faces right → target tile highlights → player presses Space

This improves clarity.

Pause Input During Movement

Prevents accidental double moves.

if (player.isMoving)
{
    return;
}

The Core Movement System is responsible for translating player input into
movement behavior across multiple supported movement styles. This includes
top-down grid movement, side-view grid movement, isometric/free movement
concepts, and direction-then-move controls. The system manages player
position, facing direction, input state, movement validation, and accessibility-
related control options such as keyboard remapping, tap/hold behavior, input
repeat delay, and optional controller support. By separating movement logic
from scoring, collision, and economy systems, this feature provides a flexible
foundation that other gameplay systems can safely integrate with.

==============================================================================

System Architecture

Game Project
│
├── App Layer
│   ├── Main Loop
│   ├── Input Collection
│   └── Rendering / UI Calls
│
├── Simulation Layer
│   ├── Player
│   ├── GridSystem
│   ├── MovementSystem
│   ├── CollisionSystem      ← teammate-owned, placeholder interface
│   ├── ScoreSystem          ← teammate-owned, placeholder interface
│   ├── EconomySystem        ← teammate-owned, placeholder interface
│   └── AudioFeedbackSystem  ← teammate-owned/optional, placeholder interface
│
└── Core Layer
    ├── Vector2Int
    ├── Direction
    ├── MovementMode
    ├── InputMode
    ├── TileType
    └── GameEvents

==============================================================================

Core responsibility

Player
GridSystem
MovementSystem
Input-to-Movement mapping
Movement settings/accessibility options
Shared enums and basic data structs
