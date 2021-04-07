# This is the Tic Tac Toe final project

import random
'''
REQUIREMENTS
This is the game for tic tac toe.
The rules:
There are two players that will play against each other
One player will "X" and the other player will be "O"
One of the players will go first. Let's say that Player 1 who is "X" gets chosen to go first. They will choose
a position on the board to put their "X". The board will then save this and it will then be Player 2's ("O") turn.
Player 2 will get to put in an "O" anywhere on the board except in the positions that are already taken. 
The game is completed once there is 3 X's in a row/column/diagonal or 3 O's in a row/column/diagonal or if all the
spaces in the board are taken. 
 
'''

'''
DESIGN
This is the design of the document
It includes many different methods within each class.
The first class is called Board. It has methods such as drawBoard, makeMove, isWinner, etc.
This class and its methods are responsible for making updates to the gameboard.
The second class is called Player. It has methods such as inputPlayerLetter, whoGoesFirst, etc.
This is and its methods are responsible for taking in user input and passing it along to the Board class in order to 
update the board and it is used to determine which player will have what letter and if the two players would like the
game to continue. 
'''

class Board:
    def __init__(self, board):
        self.board = board

    def drawBoard(self):
        # this prints out the updated board after each turn.
        # "board" is a list of 10 strings representing the board (ignore index 0)
        print('   |   |')
        print(' ' + self.board[7] + ' | ' + self.board[8] + ' | ' + self.board[9])
        print('   |   |')
        print('-----------')
        print('   |   |')
        print(' ' + self.board[4] + ' | ' + self.board[5] + ' | ' + self.board[6])
        print('   |   |')
        print('-----------')
        print('   |   |')
        print(' ' + self.board[1] + ' | ' + self.board[2] + ' | ' + self.board[3])
        print('   |   |')

    def makeMove(self, letter, move):
        # this does the actual updating to the board
        self.board[move] = letter

    def isWinner(self, le):
        # this lets us know if there is a winner
        bo = self.board
        return ((bo[7] == le and bo[8] == le and bo[9] == le) or  # across the top
                (bo[4] == le and bo[5] == le and bo[6] == le) or  # across the middle
                (bo[1] == le and bo[2] == le and bo[3] == le) or  # across the bottom
                (bo[7] == le and bo[4] == le and bo[1] == le) or  # down the left side
                (bo[8] == le and bo[5] == le and bo[2] == le) or  # down the middle
                (bo[9] == le and bo[6] == le and bo[3] == le) or  # down the right side
                (bo[7] == le and bo[5] == le and bo[3] == le) or  # diagonal
                (bo[9] == le and bo[5] == le and bo[1] == le))  # diagonal

    def getBoardCopy(self):
        dupeBoard = []
        for i in self.board:
            dupeBoard.append(i)
        return Board(dupeBoard)

    def isSpaceFree(self, move):
        # this lets us know if a space is available.
        return self.board[move] == ' '

    def isBoardFull(self):
        # this lets us know if the entire board is full or there is still some space left.
        for i in range(1, 10):
            if self.isSpaceFree(i):
                return False
        return True


class Player:
    def __init__(self, board):
        self.board = board

    def inputPlayerLetter(self):
        letter = ''
        while not (letter == 'X' or letter == 'O'):
            print('Do you want to be X or O?')
            letter = input().upper()

        # the first element in the tuple is the player 1's letter, the second is the player 2's letter.
        if letter == 'X':
            return ['X', 'O']
        else:
            return ['O', 'X']

    def whoGoesFirst(self):
        # this lets us see which player gets to go first
        if random.randint(0, 1) == 0:
            return 'Player 1'
        else:
            return 'Player 2'

    def playAgain(self):
        # this asks if the players want to play the game again
        print('Do you want to play again? (yes or no)')
        return input().lower().startswith('y')

    def getPlayerMove(self):
        # this asks for the players next move(what position they want to enter their number in) and it checks to see
        # that the position is available
        move = ' '
        while move not in '1 2 3 4 5 6 7 8 9'.split() or not self.board.isSpaceFree(int(move)):
            print('What is your next move? (1-9)')
            move = input()
        return int(move)

'''
This part finally combines all of the classes and their methods to make a smooth running program.
'''

print('Welcome to Tic Tac Toe!')

while True:
    # Initializes the board and player.
    tic_tac_toe_board = Board([' '] * 10)
    player = Player(tic_tac_toe_board)

    # Gets the players' chosen letter.
    playerLetter1, playerLetter2 = player.inputPlayerLetter()
    # Chooses which player gets to go first, randomly.

    turn = player.whoGoesFirst()
    print('The ' + turn + ' will go first.')
    gameIsPlaying = True

    while gameIsPlaying:
        if turn == 'Player 1':
            # Player 1's turn.
            tic_tac_toe_board.drawBoard()
            print('It is your turn ' + turn + ".", end=" ")
            move = player.getPlayerMove()
            tic_tac_toe_board.makeMove(playerLetter1, move)

            if tic_tac_toe_board.isWinner(playerLetter1):
                tic_tac_toe_board.drawBoard()
                print('Hooray! Player 1 has won the game!')
                gameIsPlaying = False
            else:
                if tic_tac_toe_board.isBoardFull():
                    tic_tac_toe_board.drawBoard()
                    print('The game is a tie!')
                    break
                else:
                    turn = 'Player 2'

        else:
            # Player 2's turn.
            tic_tac_toe_board.drawBoard()
            print('It is your turn ' + turn + ".", end=" ")
            move = player.getPlayerMove()
            tic_tac_toe_board.makeMove(playerLetter2, move)

            if tic_tac_toe_board.isWinner(playerLetter2):
                tic_tac_toe_board.drawBoard()
                print('Player 2 has beaten you! You lose.')
                gameIsPlaying = False
            else:
                if tic_tac_toe_board.isBoardFull():
                    tic_tac_toe_board.drawBoard()
                    print('The game is a tie!')
                    break
                else:
                    turn = 'Player 1'

    if not player.playAgain():
        break
