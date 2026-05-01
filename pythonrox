#!/usr/bin/env python3
# Tic-Tac-Toe with optional single-player AI (minimax)

import math

def print_board(b):
    print()
    for i in range(3):
        row = [b[i*3 + j] if b[i*3 + j] != ' ' else str(i*3 + j + 1) for j in range(3)]
        print(" " + " | ".join(row))
        if i < 2:
            print("---+---+---")
    print()

def check_win(b, p):
    wins = [(0,1,2),(3,4,5),(6,7,8),
            (0,3,6),(1,4,7),(2,5,8),
            (0,4,8),(2,4,6)]
    return any(b[a] == b[b1] == b[c] == p for a,b1,c in wins)

def board_full(b):
    return all(cell != " " for cell in b)

def available_moves(b):
    return [i for i,cell in enumerate(b) if cell == " "]

def get_move_human(b, player):
    while True:
        try:
            choice = input(f"Player {player} - choose cell (1-9): ").strip()
            idx = int(choice) - 1
            if idx < 0 or idx > 8:
                print("Enter a number from 1 to 9.")
            elif b[idx] != " ":
                print("Cell already taken. Choose another.")
            else:
                return idx
        except ValueError:
            print("Please enter a number (1-9).")

def minimax(b, depth, is_maximizing, ai_player, human_player):
    if check_win(b, ai_player):
        return 10 - depth
    if check_win(b, human_player):
        return depth - 10
    if board_full(b):
        return 0

    if is_maximizing:
        best = -math.inf
        for mv in available_moves(b):
            b[mv] = ai_player
            score = minimax(b, depth+1, False, ai_player, human_player)
            b[mv] = " "
            best = max(best, score)
        return best
    else:
        best = math.inf
        for mv in available_moves(b):
            b[mv] = human_player
            score = minimax(b, depth+1, True, ai_player, human_player)
            b[mv] = " "
            best = min(best, score)
        return best

def best_move(b, ai_player, human_player):
    best_score = -math.inf
    move = None
    for mv in available_moves(b):
        b[mv] = ai_player
        score = minimax(b, 0, False, ai_player, human_player)
        b[mv] = " "
        if score > best_score:
            best_score = score
            move = mv
    return move

def choose_option(prompt, options):
    opt = None
    options_str = "/".join(options)
    while opt not in options:
        opt = input(f"{prompt} ({options_str}): ").strip().lower()
    return opt

def main():
    print("Tic-Tac-Toe")
    mode = choose_option("Choose mode: single or two player", ["single", "two"])
    while True:
        board = [" "] * 9
        if mode == "single":
            # Let human choose symbol
            human_symbol = choose_option("Do you want to be X or O?", ["x", "o"]).upper()
            ai_symbol = "O" if human_symbol == "X" else "X"
            turn_choice = choose_option("Who goes first? human or computer", ["human", "computer"])
            current = human_symbol if turn_choice == "human" else ai_symbol
            print_board(board)
            while True:
                if current == human_symbol:
                    mv = get_move_human(board, human_symbol)
                    board[mv] = human_symbol
                else:
                    print("Computer is thinking...")
                    mv = best_move(board, ai_symbol, human_symbol)
                    if mv is None:
                        mv = available_moves(board)[0]
                    board[mv] = ai_symbol
                    print(f"Computer plays {mv+1}")
                print_board(board)
                if check_win(board, current):
                    if current == human_symbol:
                        print("You win! Congratulations!")
                    else:
                        print("Computer wins. Better luck next time.")
                    break
                if board_full(board):
                    print("It's a draw.")
                    break
                current = ai_symbol if current == human_symbol else human_symbol
        else:
            # Two-player mode
            current = "X"
            print_board([str(i+1) for i in range(9)])
            while True:
                print_board(board)
                mv = get_move_human(board, current)
                board[mv] = current
                if check_win(board, current):
                    print_board(board)
                    print(f"Player {current} wins!")
                    break
                if board_full(board):
                    print_board(board)
                    print("It's a draw.")
                    break
                current = "O" if current == "X" else "X"

        replay = choose_option("Play again? y or n", ["y", "n"])
        if replay != "y":
            print("Thanks for playing!")
            break
        mode = choose_option("Choose mode: single or two player", ["single", "two"])

if __name__ == "__main__":
    main()
