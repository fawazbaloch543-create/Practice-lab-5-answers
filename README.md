# Practice-lab-5-answers

import math

board = [" "] * 9

def display_board():
    print()
    for i in range(0, 9, 3):
        print(" | ".join(board[i:i+3]))
        if i < 6:
            print("--+---+--")
    print()

def check_winner(player):
    wins = [
        (0, 1, 2), (3, 4, 5), (6, 7, 8),
        (0, 3, 6), (1, 4, 7), (2, 5, 8),
        (0, 4, 8), (2, 4, 6)
    ]

    for a, b, c in wins:
        if board[a] == board[b] == board[c] == player:
            return True
    return False

def is_draw():
    return " " not in board

def minimax(is_maximizing, alpha, beta):
    if check_winner("O"):
        return 1
    if check_winner("X"):
        return -1
    if is_draw():
        return 0

    if is_maximizing:
        best_score = -math.inf

        for i in range(9):
            if board[i] == " ":
                board[i] = "O"
                score = minimax(False, alpha, beta)
                board[i] = " "

                best_score = max(best_score, score)
                alpha = max(alpha, best_score)

                if beta <= alpha:
                    break

        return best_score

    else:
        best_score = math.inf

        for i in range(9):
            if board[i] == " ":
                board[i] = "X"
                score = minimax(True, alpha, beta)
                board[i] = " "

                best_score = min(best_score, score)
                beta = min(beta, best_score)

                if beta <= alpha:
                    break

        return best_score

def ai_move():
    best_score = -math.inf
    best_move = None

    for i in range(9):
        if board[i] == " ":
            board[i] = "O"
            score = minimax(False, -math.inf, math.inf)
            board[i] = " "

            if score > best_score:
                best_score = score
                best_move = i

    if best_move is not None:
        board[best_move] = "O"

def play_game():
    board[:] = [" "] * 9

    print("TIC-TAC-TOE")
    print("You are X and AI is O.")
    print("Choose positions from 1 to 9.")

    while True:
        print("\nPositions:")
        print("1 | 2 | 3")
        print("--+---+--")
        print("4 | 5 | 6")
        print("--+---+--")
        print("7 | 8 | 9")

        display_board()

        try:
            move = int(input("Enter your move (1-9): ")) - 1
        except ValueError:
            print("Enter a valid number.")
            continue

        if move < 0 or move > 8 or board[move] != " ":
            print("Invalid move! Try again.")
            continue

        board[move] = "X"

        if check_winner("X"):
            display_board()
            print("You win!")
            break

        if is_draw():
            display_board()
            print("Game is a draw!")
            break

        ai_move()
        print("AI has played.")

        if check_winner("O"):
            display_board()
            print("AI wins!")
            break

        if is_draw():
            display_board()
            print("Game is a draw!")
            break

play_game()


# OUTPUT:

# TIC-TAC-TOE
# You are X and AI is O.
# Choose positions from 1 to 9.

# Positions:
# 1 | 2 | 3
# --+---+--
# 4 | 5 | 6
# --+---+--
# 7 | 8 | 9

# Enter your move (1-9): 5
# AI has played.

# Enter your move (1-9): 1
# AI has played.

# Enter your move (1-9): 9
# Invalid move! Try again.

# Enter your move (1-9): 7
# AI has played.

# AI wins!
