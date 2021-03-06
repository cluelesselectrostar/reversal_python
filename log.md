<h1>Development Notes ⚪️⚫️</h1>

- [Conventions](#conventions)
- [Logs](#logs)
  - [Sep 29, 2020 Use helper functions for player-to-plaeyr version](#sep-29-2020-use-helper-functions-for-player-to-plaeyr-version)
  - [Sep 27, 2020 Refactor game into an OOP (Object oriented programming) version](#sep-27-2020-refactor-game-into-an-oop-object-oriented-programming-version)
  - [Sep 15, 2020 Somewhat decent AI](#sep-15-2020-somewhat-decent-ai)
  - [Sep 14, 2020 Created log](#sep-14-2020-created-log)
  - [Sep 13, 2020 Implement dumb AI](#sep-13-2020-implement-dumb-ai)
  - [Sep 6, 2020 Overhauled Interface](#sep-6-2020-overhauled-interface)
  - [Sep 5, 2020 More deubbing, creating an interface that shows all available moves](#sep-5-2020-more-deubbing-creating-an-interface-that-shows-all-available-moves)
  - [Sep 3, 2020 Blit messages within the game, attempt to refactor the `is_reversible()` and `drop_piece()` methods](#sep-3-2020-blit-messages-within-the-game-attempt-to-refactor-the-is_reversible-and-drop_piece-methods)
  - [Aug 25, 2020 GUI version of the game](#aug-25-2020-gui-version-of-the-game)
  - [Aug 24, 2020 Fixed basic functionality, created test note book, added documentation](#aug-24-2020-fixed-basic-functionality-created-test-note-book-added-documentation)
  - [Aug 23, 2020 First Commit](#aug-23-2020-first-commit)

This is a log of the the development of my reversal/ orthello game. I created this on 14th September, so all logs for any commits before 14/9 are written with respect to the titles I have given to each commit.

# Conventions

- Rather than mentioning "the player" and "the opponent", I like to refer to them as player "A" and "B" respectively for simplicity.
- Within the game, players are represented by the colors:
  - Player 1: Yellow or Black 
  - Player 2: Red or White
- For clarity, I have omitted the input parameters for the methods mentioned below.
- The board is represented by a numpy array of dimension 8x8. For instance, (3,4) refers to the location at the 4th row, in the 5th column.

# Logs

## Sep 29, 2020 Use helper functions for player-to-plaeyr version

Now both the player-to-player and player-to-AI version of the game utilises the same `Reversal` class (and the inherited `Reversal_AI` class), which means that they utilise the same members and methods. This would allow for more streamlined development. It has also alloed me to move some flags into the object itself, thus reducing the comoplexity and length of the main files.

## Sep 27, 2020 Refactor game into an OOP (Object oriented programming) version

A few days ago I read about a package called `py2app` that allows you to create standalone applications with python, which would allow me to conveniently distribute the game as a desktop app. That sounds very neat indeed and I imagine that in the very near future would come in handy.

However, before I create this deliverable, I kind of want to make sure that the player can access both the player-to-player mode as well as the player-to-AI mode within a single package. Currently, all development has went into the AI version, which means that all the methods written within the player-to-player version are outdated.

Going forward, it is in my best interest to seperate my functions into a seperate file that are accessible by multiple game modes, hence object oriented programming. There are also other subtle differences, like the players are "PLAYER vs AI" rather than "Player 1 vs Player 2". I've slightly updated the file structure:

- [reversal_helpers.py](reversal_helpers.py) now holds all the methods necessary for the game, in a class unimaginatively called Reversal.
- [reversal_ai.py](reversal_ai.py) is written to use these methods.
- [reversal_ai_nonOOP.py](reversal_ai_nonOOP.py) is the previous version of the game, which does not involve OOP.
- I will update [reversal_gui_mac.py](reversal_gui_mac.py) and [reversal.py](reversal.py) later on.

One minor (major) problem right now is that the minimax algorithm runs painfully slow. I'm not quite sure why that's the case, but I imagine that's to do with the inefficiencies in constructing and copying multiple instances of an object repetitively. That is what I have to work on right now.

(Turns out I made a pretty studpi mistake of calling `pygame.init()` every single time I initialised a new board during the minimax algorithm!)

## Sep 15, 2020 Somewhat decent AI

I have previously watched a tutorial on implementing AI for a connect4 game, which uses a decision rule called the "minimax algorithm" for determining the best move. There's quite a bit of math and game theory related stuff involved, but in short, it is based on a weighted score-based system such that it maximises the player's own gains and minimises its losses; this is iteratively applied to consecutive moves for both the player and its opponent. For more details, check out the [wikipedia page](https://en.wikipedia.org/wiki/Minimax) (check out the psuedocode as well!). I wondered how well this would be for reversi/ orthello, so without looking into other orthello projects, I went straight to implementation after watching the connect4 AI tutorial.

I continued working in [reversal_ai.py](reversal_ai.py), where I created several new methods, including `score_position()`, `pick_best_move()` and `minimax()`. Here's a short description for each method:

- `score_position()` assigns a score for the location specified. Unlike a connect4 game where you can immediately identify windows of 3/4 in a row, for orthello you can't really do that. Currently, I consider advantage points (borders and corners) as well as number of flipped pieces, though the flipped number plays a much smaller role that the aforementioned advantage points (it's just there to add a small offset). It also consider advantage points of the opponent in the next turn. I suspect this may be slightly messing the minimax algorithm.
- `pick_best_move()` is a method that is no longer used. It is just used for testing `score-position()`, so it just selects the best location based on scores on the current board only.
- `minimax()` is where I implement the minimax algorithm. It recursively calls itself, and calls on `score_position()` when the depth reaches zero. One thing is I don't consider the terminal nodes mentioned in the psuedocode - I'm not quite sure how winning moves applies to a game like Orthello.

 > Random thought: maybe I should also consider the scenario where one's own position could be immediately reversed by the opponent.

As of now, the AI isn't really super smart, but I would say that if I do make one dumb move, playing with the AI is quite hard.

Other changes:

- More bug fixes
- Updated README for new interface and new functions.
- Slight update to blitting of special messages.

## Sep 14, 2020 Created log

- Made this [log](log.md), which is mostly me looking through the commits and trying to recall what I've done.
- Is this reverse engineering?

## Sep 13, 2020 Implement dumb AI

- Created an AI version of the game in [reversal_ai.py](reversal_ai.py). AI is now player 2 for the game.
- AI only makes random moves (based on the list of available locations.)

## Sep 6, 2020 Overhauled Interface

- Overhauled the interface, with the following changes made:
  - The board is now green, with white gridlines.
  - The players are now represented with black and white pieces rather than red and yellow pieces
  - There is a wooden background behind the board. The statistics, such as turn, total pieces, number of pieces for each player, number of flipped pieces, are blit on the right hand side.

## Sep 5, 2020 More deubbing, creating an interface that shows all available moves

- Did more testing and debugging to fix bugs (most of which are related to iterations within the `orthello` method)
- Created an interface that shows all the legal moves/locations a player can make. This involves actually saving another board (another numpy array) during the game, and using that board to store the available moves. The functions are `create_avaiBoard()`, `availoc()`, `draw_avaiBoard(available_board, turn)`
  - `create_avaiBoard()` creates the board for storing available moves. Intialised with the moves for available for player 1.
  - `availoc()` returns a list of available locations for a player, and drops the available move pieces to the `available_board`.
  - `draw_avaiBoard()` displays the available moves, shown as hollow circles within the pygame window.

## Sep 3, 2020 Blit messages within the game, attempt to refactor the `is_reversible()` and `drop_piece()` methods

- Created a new GUI version called [reversal_gui_mac.py](reversal_gui_mac.py), which blit the messages directly within the game instead. By this time I have abandoned development on the file [reversal_gui.py](scrap/reversal_gui.py)
- Attempt to refactor the `is_reversible()` and `drop_piece()` methods. I combined them into one method called `orthello()`, where the last input parameter determines whether `is_reversible()`(FALSE) or `drop_piece()`(TRUE) is performed.
- More bug fixes

## Aug 25, 2020 GUI version of the game

- Created a GUI version of this game in [reversal_gui.py](scrap/reversal_gui.py). I used the `messagebox` package from `tkinter` to display messages for errors such as invalid locations, or when a player cannot move. Interestingly, the package worked well on a Windows PC but never worked for a Mac (it always crashed). It was apparent that I should blit the messages directly within the game instead.
- Updated [README.md](README.md) to reflect the GUI version of the game.
- More bug fixes

## Aug 24, 2020 Fixed basic functionality, created test note book, added documentation

- Created a function called `is_reversible()`, which is based on `drop_piece()`. The only difference is that it returns a boolean true immediately when it finds that dropping a piece is possible.
- Created a main loop for the entire game, which allows me to play the game via the command line interface. The main things within the while loop are as follows:
  - Ask for player input
  - Check whether the location of the dropped piece is valid/legal. If so, drop the piece
  - Check for end game
  - Move on to the next player
- Amended issues regarding iterations within `is_reversible()` and `drop_piece()` methods. I realised that the iterations must start from the location nearest to the dropped piece. For example, when a piece is dropped at (3,4):
  - check left: (3,3), (3,2), (3,1)......
  - check right: (3,5), (3,6), (3,7)
  - check up: (2,4), (1,4), (0,4)
  - check down: (4,4), (5,4), (6,4).......
  - check positive diagonal to the right: (2,5), (1,6), (0,7)
  - check positive diagonal to the left: (4,3), (5,2), (6,1), (7,0)
  - etc.
- Created a jupyter notebook [reversal_test.ipynb](reversal_test.ipynb) for testing purposes, where I played with myself :( and recorded the results. The results are rendered using the `matplotlib` package.
- Added documentation, which is the [README.md](README.md) that you have probably seen before coming here.

## Aug 23, 2020 First Commit

- Development of the program started a coupple days ago. It has been decided early on that the board would be represented by a numpy array of dimension 8x8.
- Completed most methods required for a command line version of the game, including `create_board()`, `drop_piece()`, `is_valid_location()`, `print_board()` and `end_game()`
  - `create_board()` creates the game board, and initialises it with 4 pieces in the centre.
  - `is_valid_location()` merely checks whether or not the location is empty
  - `end_game()` merely checks whether or not the board is full (no zeroes within the array)
  - `drop_piece()` checks locations from every direction of the piece (up, down, left, right, diagonals etc.) and finds the nearest piece of the player, say player A. Iteration for each axis continues when the location is occupied by B, and terminated when a board location is empty, A's piece is found, or when the iteration reaches the borders of the board.
  - The other methods are mostly self-explanatory based on their naming.
- Main loop not completed.
