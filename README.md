# tic-tac-toe-minimax
 Tic-Tac-Toe game developed by Bardia Fardar

# Introduction
To solve games using AI, we will introduce the concept of a game tree followed by mini-max algorithm. The different states of the game are represented by nodes in the game tree.
In the game tree, the nodes are sorted in levels that correspond to each player's turns in the game so that the “root” node of the tree  is the beginning position in the game. In tic-tac-toe, this would be the empty grid with no X`s or O`s played yet. Under root node, on the second level, there are the possible states that can result from the first player’s moves, be it X or O. We call these nodes the “children” of the root node. Each node on the second level, would further have as its children nodes the states that can be reached from it by the opposing player's moves. This is continued, level by level, until reaching state where the game is over.

# What is Mini-max?
Minimax is a decision rule used in artificial intelligence, decision theory, game theory, statistics, and philosophy for minimizing the possible loss for a worst case (maximum loss) scenario. When dealing with gains, it is referred to as "maximin" – to maximize the minimum gain.

# How does it works?
In game theory, minimax is a decision rule used to minimize the worst-case potential loss; in other words, a player considers all of the best opponent responses to his strategies, and selects the strategy such that the opponent's best strategy gives a payoff as large as possible.


# Understanding the Algorithm

```python

def MiniMaxAB(brd, depth, alpha, beta, player):
    row = -1
    collumn = -1
    alpha_cut = 0
    beta_cut = 0

    if depth == 0 or gameWon(brd):
        return [row, collumn, getScore(brd)]

    else:
        for cell in emptyCells(brd):
            setposition(brd, cell[0], cell[1], player)
            score = MiniMaxAB(brd, depth - 1, alpha, beta, -player)
            if player == XPLAYER:
                if score[2] > alpha:
                    alpha = score[2]
                    row = cell[0]
                    collumn = cell[1]

            else:
                if score[2] < beta:
                    beta = score[2]
                    row = cell[0]
                    collumn = cell[1]

            setposition(brd, cell[0], cell[1], EMPTY)
            alpha_cut_check = False

            if alpha >= beta:
                alpha_cut = alpha
                beta_cut = beta
                alpha_cut_check = True
                break

        if player == XPLAYER:
            return [row, collumn, alpha, beta, alpha_cut_check, alpha_cut, beta_cut]

        else:
            return [row, collumn, beta, alpha, alpha_cut_check, alpha_cut, beta_cut]

```

Now we'll see each part of this pseudocode with Python implementation. The Python implementation is available at this repository. First of all, consider it:
> ┌───┬───┬───┐
> │ O │ X │ O │
> ├───┼───┼───┤
> │ X │   │   │
> ├───┼───┼───┤
> │   │   │   │
> └───┴───┴───┘

> alpha = -inf
> beta = inf

>  ┌─[Insert Your position : ]
   └──╼(1-9)>>>

The Alpha may be X or O and the Beta may be O or X, whatever. The board is 3x3.

```python
def MiniMaxAB(brd, depth, alpha, beta, player):
```
* **brd**: the current board in tic-tac-toe (node)
* **depth**: index of the node in the game tree
* **alpha**: maximum
* **beta**: minimum
* **player**: may be a *X* player or *O* player

```python
if alpha >= beta:
    alpha_cut = alpha
    beta_cut = beta
    alpha_cut_check = True
    break
```

Both players start with your worst score. If player is Alpha, its score is -infinity. Else if player is Beta, its score is +infinity. 

The best move on the board is [-1, -1] (row and column) for all.

```python
if depth == 0 or gameWon(brd):
    return [row, col, getScore(brd)]
```

If the depth is == 0, then the board hasn't new empty cells to play. Or, if a player wins, then the game ended for MAX or MIN. So the score for that state will be returned.

* If Alpha won: return +1
* If Beta won: return -1
* Else: return 0 (Draw)

Now we'll see the main part of this code that contains recursion.

```python
def emptyCells(brd):
    emptyC = []
    for x, row in enumerate(brd):
        for y, col in enumerate(row):
            if brd[x][y] == EMPTY:
                emptyC.append([x, y])

    return emptyC
```

For each valid moves (empty cells):
* **x**: receives cell row index
* **y**: receives cell column index
* **brd[x][y]**: it's like board[available_row][available_col] receives MAX or MIN player
* **score = minimax(brd, depth - 1, -player)**:
  * brd: is the current board in recursion;
  * depth -1: index of the next state;
  * -player: if a player is MAX (+1) will be MIN (-1) and vice versa.

The move (+1 or -1) on the board is undo and the row, column are collected.

The next step is compare the score with best.

```python
if player == XPLAYER:
	if score[2] > best[2]:
		best = score[2]
        row = cell[0]
        col = cell[1]
else:
	if score[2] < best[2]:
		best = score[2]
        row = cell[0]
        col = cell[1]
```

For XPLAYER player, a bigger score will be received. For a YPlayer, a lower score will be received.

# Evaluation Function

An evaluation function, also known as a heuristic evaluation function or static evaluation function, is a function used by game-playing programs to estimate the value or goodness of a position in the minimax and related algorithms. The evaluation function is typically designed to prioritize speed over accuracy; the function looks only at the current position and does not explore possible moves (therefore static).

