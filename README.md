<h1>ExpNo 7 : Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game</h1> 
<h3>Name: R K JAYA KRISNAA      </h3>
<h3>Register Number : 212223223002          </h3>
<H3>Aim:</H3>
<p>
Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
</p>
<h1>GOALS of Alpha-Beta Pruning in MiniMax Search Algorithm</h1>

<h3>Improve the decision-making efficiency of the computer player by reducing the number of evaluated nodes in the game tree.</h3>
<h3>Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning and the Minimax algorithm with Python Code.</h3>
<h1>IMPLEMENTATION</h1>

The project involves developing a Tic-Tac-Toe game implementation incorporating the Alpha-Beta pruning with the Minimax algorithm. Using this algorithm, the computer player analyzes the game state, evaluates possible moves, and selects the optimal action based on the anticipated outcomes.

<h1>The Minimax algorithm</h1>

recursively evaluates all possible moves and their potential outcomes, creating a game tree.

<h1>Alpha-Beta pruning</h1>

Alpha–Beta (𝛼−𝛽) algorithm is actually an improved minimax using a heuristic. It stops evaluating a move when it makes sure that it’s worse than a previously examined move. Such moves need not to be evaluated further.

When added to a simple minimax algorithm, it gives the same output but cuts off certain branches that can’t possibly affect the final decision — dramatically improving the performance
<hr>
<h2>Sample Input and Output:</h2>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8d5e329a-9aff-41a6-bcf0-46efa10e1b92)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/438b242d-54ba-443e-b040-a936e6ae3b55)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/99a33390-fa11-4ade-a19f-e93bcd7aaec9)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/440797bd-53cb-49c1-b18d-89776864c3e7)
![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/81575a16-26b2-46f1-a8ac-27c9ed0a0fe5)

## Program : 
```PYTHON
# ExpNo 7 : Implement Alpha-beta pruning of Minimax Search Algorithm for a Simple TIC-TAC-TOE game
# Name: R K JAYA KRISNAA
# Register Number: 212223223002

import math

HUMAN = 'O'
AI = 'X'
nodes_evaluated = 0

def print_board(board):
    print()
    for i in range(0, 9, 3):
        print(f" {board[i]} | {board[i+1]} | {board[i+2]} ")
        if i < 6:
            print("---|---|---")
    print()

def check_winner(board, player):
    win_conditions = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],  # Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8],  # Columns
        [0, 4, 8], [2, 4, 6]              # Diagonals
    ]
    return any(all(board[pos] == player for pos in line) for line in win_conditions)

def is_board_full(board):
    return ' ' not in board

def score(board):
    if check_winner(board, AI):
        return 10
    elif check_winner(board, HUMAN):
        return -10
    return 0

def minimax_alpha_beta(board, depth, is_maximizing, alpha, beta):
    global nodes_evaluated
    nodes_evaluated += 1

    current_score = score(board)
    
    # Base cases: game over (win, loss, or draw)
    if current_score == 10:
        return current_score - depth  # Prefer faster wins
    if current_score == -10:
        return current_score + depth  # Delay losses
    if is_board_full(board):
        return 0

    if is_maximizing:
        best_score = -math.inf
        for i in range(9):
            if board[i] == ' ':
                board[i] = AI
                val = minimax_alpha_beta(board, depth + 1, False, alpha, beta)
                board[i] = ' '
                best_score = max(best_score, val)
                alpha = max(alpha, best_score)
                # Beta cutoff (Prune remaining branches)
                if beta <= alpha:
                    break
        return best_score
    else:
        best_score = math.inf
        for i in range(9):
            if board[i] == ' ':
                board[i] = HUMAN
                val = minimax_alpha_beta(board, depth + 1, True, alpha, beta)
                board[i] = ' '
                best_score = min(best_score, val)
                beta = min(beta, best_score)
                # Alpha cutoff (Prune remaining branches)
                if beta <= alpha:
                    break
        return best_score

def find_best_move(board):
    global nodes_evaluated
    nodes_evaluated = 0
    best_score = -math.inf
    best_move = -1

    for i in range(9):
        if board[i] == ' ':
            board[i] = AI
            move_score = minimax_alpha_beta(board, 0, False, -math.inf, math.inf)
            board[i] = ' '
            if move_score > best_score:
                best_score = move_score
                best_move = i

    print(f"Nodes evaluated with Alpha-Beta Pruning: {nodes_evaluated}")
    return best_move

def main():
    board = [' ' for _ in range(9)]
    print("Tic-Tac-Toe with Minimax + Alpha-Beta Pruning")
    print("You are 'O' and AI is 'X'. Positions are 1 to 9:")
    print(" 1 | 2 | 3 \n---|---|---\n 4 | 5 | 6 \n---|---|---\n 7 | 8 | 9 \n")

    while True:
        # Human turn
        while True:
            try:
                move = int(input("Enter your move (1-9): ")) - 1
                if 0 <= move <= 8 and board[move] == ' ':
                    board[move] = HUMAN
                    break
                else:
                    print("Invalid move. Try an empty spot between 1 and 9.")
            except ValueError:
                print("Please enter a valid number.")

        print_board(board)

        if check_winner(board, HUMAN):
            print("You win!")
            break
        if is_board_full(board):
            print("It's a Draw!")
            break

        # AI turn
        print("AI is thinking...")
        ai_move = find_best_move(board)
        board[ai_move] = AI
        print_board(board)

        if check_winner(board, AI):
            print("AI wins!")
            break
        if is_board_full(board):
            print("It's a Draw!")
            break

if __name__ == "__main__":
    main()
```
## Output : 

<img width="532" height="975" alt="image" src="https://github.com/user-attachments/assets/85b3566a-f186-445e-aa85-5a310c8a8b68" />
<img width="477" height="843" alt="image" src="https://github.com/user-attachments/assets/517ed6e7-dbea-44ed-92c6-947386032d98" />

## Result : 

Thus, the Alpha-Beta Pruning optimization of the Minimax Search Algorithm for a simple Tic-Tac-Toe game was successfully implemented, reducing the number of evaluated game-tree nodes while maintaining optimal AI decisions, and the output was verified successfully.

