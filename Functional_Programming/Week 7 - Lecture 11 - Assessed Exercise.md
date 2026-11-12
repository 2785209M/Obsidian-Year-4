No lecture on Thursday, no Quiz this week, and no Lab sheet either.
But we're starting the assessed exercise.

# Hackage
Haskell libraries are found on Hackage (https://hackage.haskell.org/)

# Stack
Stack is a Haskell toolchain and build system which is necessary for the assessed exercise

# Hoogle
Search Engine for Haskell functions

# Assessed Exercise
The assessed exercise will be a game of Kinekt 4.
## The Tasks
- Win Detection
	- Implement win-detection using pre-made functions
- Rendering and interaction
	- Use the GLOSS library to:
		- create a picture to represent the board
		- Render the Picture to the screen and handle user interaction
- AI Player
	- Implement an AI player using the minimax algorithm

## Handling User Interaction
```
interactIO
	:: Display
	-> Color
	-> world
	-> (world -> IO Picture)
	-> (Event -> world -> IO world)
	-> (Controller -> IO ())
	-> IO ()
```

## AI Player
We will implement an AI player using the minimax algorithm
This is an odl school symbolic AI algorithm that doesn't implement machine learning.

### Game Trees
We have many possible moves for a player to make. We can represent these possible moves by using a game tree. The favourability for a given player can be calculated using a heuristic function.

## Heuristics
We need to rank possible game states to tell us the quality of a given move.
We can do this by defining a heuristic function that assigns a score based on how "good" a board is for a given player

## The minimax algorithm
The minimax algorithm allows us to pick the best move by looking ahead a certain number of moves in the future.
Say we're picking a move for red; the root node is the initial game state, and each edge is a potential move.
When exploring the tree, you must recursively calculate the scores for each node.
- If we're looking at Red's turn, we pick the maximum score: we want to explore the best possible move for Red
-  If we're looking at Yellow's turn, we want to pick the minimum score: we want to model Yellow picking the move that does the most damage to Red
