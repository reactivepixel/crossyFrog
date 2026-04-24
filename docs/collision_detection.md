# Collision Detection in Crossy Frog (C++ Implementation)

This documentation describes the collision detection system for Crossy Frog using C++ with Axis-Aligned Bounding Boxes (AABB).

## AABB Concept

Axis-Aligned Bounding Boxes (AABB) are a fundamental technique in 2D collision detection where each object is represented by an axis-aligned rectangle. Two rectangles collide if their projections on both the x and y axes overlap.

## C++ Implementation

```cpp
// Core AABB functions for crossyFrog game engine

struct Rectangle {
    int x, y;
    int width, height;
};

bool containsPoint(int x, int y, const Rectangle& rect) {
  return (x >= rect.x && x <= rect.x + rect.width &&
          y >= rect.y && y <= rect.y + rect.height);
}

bool aabbCollision(const Rectangle& player, const Rectangle& obstacle) {
  // Check both axes for overlap
  bool xOverlap = containsPoint(player.x, obstacle.y, {player.x, player.width, obstacle.y});
  bool yOverlap = containsPoint(obstacle.x, player.y, {obstacle.x + obstacle.width, player.y, obstacle.y, player.height});

  return xOverlap && yOverlap;
}
```

## Key Features

- **Simple implementation**: Pure C++ with no external dependencies
- **Efficient**: Constant-time O(1) collision checks
- **Precise**: Exact AABB intersection detection
- **Composable**: Works well with physics systems
- **Maintainable**: Easy to debug and extend

## Optimization Considerations

- **Early exit**: Check cheaper axis first (commonly x-axis)
- **Bounding box updates**: Reuse rectangle structs instead of creating new ones
- **Collision response**: Implement separation algorithm for smooth interaction
- **Multiple objects**: Chain collision checks efficiently with early termination

## Usage in Game Engine

This AABB system is used throughout crossyFrog to detect collisions between:

1. Player character and obstacles
2. Players with each other (in multi-character scenarios)
3. Power-ups with physical entities
4. Environmental hazards

For complete implementation details, see `src/collision_detection.cpp`.

## Technical Details

- **Header file**: collision_detection.h contains function declarations
- **Source file**: collision_detection.cpp implements the logic
- **Game loop integration**: Collision detection runs at fixed frame rate
- **Frame skipping**: Prevents excessive checks during paused frames