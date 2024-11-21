import random

class Player:
    def __init__(self, name, marker):
        self.name = name
        self.marker = marker

    def make_move(self, board):
        pass

class HumanPlayer(Player):
    def make_move(self, board):
        while True:
            try:
                move = int(input(f"{self.name}, enter your move (1-9): ")) - 1
                if move not in range(9) or board[move] != ' ':
                    print("Invalid move. Try again.")
                else:
                    return move
            except ValueError:
                print("Invalid input. Enter a number between 1 and 9.")

class ComputerPlayer(Player):
    def make_move(self, board):
        print(f"{self.name} is making a move...")
        return random.choice([i for i, cell in enumerate(board) if cell == ' '])

class Game:
    def __init__(self):
        self.board = [' '] * 9
        self.players = []
        self.current_turn = None

    def display_board(self):
        print("\n")
        print(f" {self.board[0]} | {self.board[1]} | {self.board[2]} ")
        print("---+---+---")
        print(f" {self.board[3]} | {self.board[4]} | {self.board[5]} ")
        print("---+---+---")
        print(f" {self.board[6]} | {self.board[7]} | {self.board[8]} ")
        print("\n")

    def display_positions(self):
        print("Here are the positions on the board:")
        print("\n")
        print("  1 |  2 |  3 ")
        print("---+---+---")
        print("  4 |  5 |  6 ")
        print("---+---+---")
        print("  7 |  8 |  9 ")
        print("\n")

    def check_winner(self, marker):
        win_combinations = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],  # rows
            [0, 3, 6], [1, 4, 7], [2, 5, 8],  # columns
            [0, 4, 8], [2, 4, 6]              # diagonals
        ]
        return any(all(self.board[i] == marker for i in combo) for combo in win_combinations)

    def is_draw(self):
        return all(cell != ' ' for cell in self.board)

    def switch_turn(self):
        self.current_turn = self.players[1] if self.current_turn == self.players[0] else self.players[0]

    def play(self):
        print("Welcome to Tic Tac Toe!")
        self.display_positions()

        # Create players
        player_name = input("Enter your name: ")
        player_marker = input("Choose your marker (X or O): ").upper()
        while player_marker not in ['X', 'O']:
            player_marker = input("Invalid choice. Please choose X or O: ").upper()

        human = HumanPlayer(player_name, player_marker)
        computer = ComputerPlayer("Computer", 'O' if player_marker == 'X' else 'X')
        self.players = [human, computer]

        # Randomly decide who goes first
        self.current_turn = random.choice(self.players)
        print(f"{self.current_turn.name} will go first!")

        # Game loop
        while True:
            self.display_board()
            print(f"{self.current_turn.name}'s turn ({self.current_turn.marker}):")
            move = self.current_turn.make_move(self.board)
            self.board[move] = self.current_turn.marker

            if self.check_winner(self.current_turn.marker):
                self.display_board()
                print(f"Congratulations! {self.current_turn.name} wins!")
                break

            if self.is_draw():
                self.display_board()
                print("It's a draw!")
                break

            self.switch_turn()

# Run the game
if __name__ == "__main__":
    game = Game()
    game.play()
