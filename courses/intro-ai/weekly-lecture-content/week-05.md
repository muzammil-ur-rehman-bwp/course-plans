# Week 5 — Lecture Content: Adversarial Search

## 1. Games as Search Problems
A two-player, zero-sum, perfect-information game (like Tic-Tac-Toe) can be formulated as a
search problem over a **game tree**: each node is a board state, each edge is a legal move, and
leaves are terminal states with a utility value (e.g., +1 for a MAX win, -1 for a MIN win, 0 for
a draw). Unlike Weeks 3–4, we are not searching for a path to a goal — we are searching for the
**best move**, assuming an adversary who plays optimally against us.

## 2. The Minimax Algorithm
MAX picks the action that maximizes the resulting value; MIN picks the action that minimizes it.
The value of a node is computed recursively from its children.

```python
def minimax(state, maximizing):
    if is_terminal(state):
        return utility(state)
    if maximizing:
        return max(minimax(result(state, a), False) for a in legal_moves(state))
    else:
        return min(minimax(result(state, a), True) for a in legal_moves(state))

def best_move(state):
    return max(legal_moves(state), key=lambda a: minimax(result(state, a), False))
```

## 3. Alpha-Beta Pruning
Minimax explores the entire game tree. Alpha-beta pruning carries two bounds — `alpha` (the best
value MAX can guarantee so far) and `beta` (the best value MIN can guarantee so far) — and cuts
off a branch as soon as it is proven irrelevant to the final decision, **without changing the
result**.

```python
def alphabeta(state, maximizing, alpha=float("-inf"), beta=float("inf")):
    if is_terminal(state):
        return utility(state)
    if maximizing:
        value = float("-inf")
        for a in legal_moves(state):
            value = max(value, alphabeta(result(state, a), False, alpha, beta))
            alpha = max(alpha, value)
            if alpha >= beta:
                break  # beta cutoff: MIN will never let MAX reach this branch
        return value
    else:
        value = float("inf")
        for a in legal_moves(state):
            value = min(value, alphabeta(result(state, a), True, alpha, beta))
            beta = min(beta, value)
            if alpha >= beta:
                break  # alpha cutoff
        return value
```

## 4. Tic-Tac-Toe Case Study
```python
def legal_moves(board):
    return [i for i, cell in enumerate(board) if cell == "."]

def result(board, move, player):
    new_board = list(board)
    new_board[move] = player
    return "".join(new_board)

WINS = [(0,1,2),(3,4,5),(6,7,8),(0,3,6),(1,4,7),(2,5,8),(0,4,8),(2,4,6)]

def winner(board):
    for a, b, c in WINS:
        if board[a] != "." and board[a] == board[b] == board[c]:
            return board[a]
    return None

def is_terminal(board):
    return winner(board) is not None or "." not in board

def utility(board):
    w = winner(board)
    return {"X": 1, "O": -1}.get(w, 0)
```
Tic-Tac-Toe's full game tree has at most 9! = 362,880 leaf sequences — small enough that plain
minimax solves it instantly, which is exactly why it is used here as a clean teaching example
before scaling arguments (chess has roughly 10^120 possible games) motivate pruning and
evaluation functions for larger games.

## 5. Evaluation Functions (Brief)
For games too large to search to terminal states (chess, Go), an **evaluation function**
estimates the value of a non-terminal state (e.g., material balance in chess), and search is cut
off at a fixed depth. This course does not implement a depth-limited evaluation function, but
students should recognize the idea: alpha-beta + evaluation functions + enormous compute is
exactly how Deep Blue played chess (Week 1 history).

## 6. In-Class Exercise
Trace minimax on a 3-ply subtree of Tic-Tac-Toe by hand, counting nodes visited; then trace
alpha-beta pruning on the same subtree and compare the count.
