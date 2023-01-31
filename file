

from colorama import Fore, Style
from random import choice
from math import inf
import time
import os


left_down_angle = '\u2514'
right_down_angle = '\u2518'
right_up_angle = '\u2510'
left_up_angle = '\u250C'

middle_junction = '\u253C'
top_junction = '\u252C'
bottom_junction = '\u2534'
right_junction = '\u2524'
left_junction = '\u251C'


bar = Style.BRIGHT + Fore.RED + '\u2502' + Fore.RESET + Style.RESET_ALL
dash = '\u2500'

first_line = Style.BRIGHT + Fore.RED + left_up_angle + dash + dash + dash + top_junction + dash + \
    dash + dash + top_junction + dash + dash + dash + \
    right_up_angle + Fore.RESET + Style.RESET_ALL
middle_line = Style.BRIGHT + Fore.RED + left_junction + dash + dash + dash + middle_junction + dash + \
    dash + dash + middle_junction + dash + dash + dash + \
    right_junction + Fore.RESET + Style.RESET_ALL
last_line = Style.BRIGHT + Fore.RED + left_down_angle + dash + dash + dash + bottom_junction + dash + \
    dash + dash + bottom_junction + dash + dash + dash + \
    right_down_angle + Fore.RESET + Style.RESET_ALL

XPLAYER = 1
OPLAYER = -1
EMPTY = 0

board = [[EMPTY, EMPTY, EMPTY],
         [EMPTY, EMPTY, EMPTY],
         [EMPTY, EMPTY, EMPTY]]



# Showing board def like rows the it shows the bar and it goes until end
def showboard(array):
    print(first_line)
    for a in range(len(array)):
        for i in array[a]:
            if i == EMPTY:
                print(bar, ' ', end=' ')
            else:
                if i == 1:
                    print(bar, "X", end=' ')
                else:
                    print(bar, "O", end=' ')
        print(bar)
        if a == 2:
            print(last_line)
        else:
            print(middle_line)

# Clearing Board using putting EMPTY in all x's & y's


def clearBoard(brd):
    for x, row in enumerate(brd):
        for y, col in enumerate(row):
            brd[x][y] = EMPTY

# This def checks if any Player wins or not using checking states


def winningPlayer(brd, player):
    winningStates = [[brd[0][0], brd[0][1], brd[0][2]],
                     [brd[1][0], brd[1][1], brd[1][2]],
                     [brd[2][0], brd[2][1], brd[2][2]],
                     [brd[0][0], brd[1][0], brd[2][0]],
                     [brd[0][1], brd[1][1], brd[2][1]],
                     [brd[0][2], brd[1][2], brd[2][2]],
                     [brd[0][0], brd[1][1], brd[2][2]],
                     [brd[0][2], brd[1][1], brd[2][0]]]

    if [player, player, player] in winningStates:
        return True

    return False

# This def returns the winner to winning position


def gameWon(brd):
    return winningPlayer(brd, XPLAYER) or winningPlayer(brd, OPLAYER)

# This def shows the winner or tie position to User with timeout time


def printResult(brd):
    if winningPlayer(brd, XPLAYER):
        print(Fore.GREEN + "😍 X won!" + '\n')
        time.sleep(2)

    elif winningPlayer(brd, OPLAYER):
        print(Fore.GREEN + "😍 O won!" + '\n')
        time.sleep(2)

    else:
        print(Fore.YELLOW + "😔 Tie No winner!" + '\n')
        time.sleep(2)

# This def checks if the requested cell is empty or not
# First it makes cells empty then enumerate adds a counter to an iterable and returns it in a form of enumerating object.


def emptyCells(brd):
    emptyC = []
    for x, row in enumerate(brd):
        for y, col in enumerate(row):
            if brd[x][y] == EMPTY:
                emptyC.append([x, y])

    return emptyC

# This def checks if the lenth of cells is full or not


def boardFull(brd):
    if len(emptyCells(brd)) == 0:
        return True
    return False

# This def sets entered user position in its cell


def setposition(brd, x, y, player):
    brd[x][y] = player

# This def defines the all moves available to use and convers to array to be entered & (try) wants the user to enter next move the it checks emptycells if its full or not , at the end it sets the position & shows scoreboard & prints alpha & beta , and if all was wrong it wants the user to enter correct number


def playerMove(brd):
    e = True
    moves = {1: [0, 0], 2: [0, 1], 3: [0, 2],
             4: [1, 0], 5: [1, 1], 6: [1, 2],
             7: [2, 0], 8: [2, 1], 9: [2, 2]}
    while e:
        try:
            move = int(input(
                Fore.CYAN + "\n ┌─[Insert Your position : ]\n └──╼(1-9)>>> " + Fore.YELLOW + ""))
            if not move in range(1, 10):
                print(Fore.RED + "Info:: " + Fore.YELLOW + "Invalid position!!")
            elif not (moves[move] in emptyCells(brd)):
                print(Fore.RED + "Info:: " + Fore.YELLOW + "Invalid position!!")
            else:
                os.system("cls")
                setposition(brd, moves[move][0], moves[move][1], XPLAYER)
                showboard(brd)
                print("Alpha = -infinity")
                print("Beta = infinity")
                e = False
        except (KeyError, ValueError):
            print(Fore.RED + "Info:: " + Fore.YELLOW + "Please Enter a Number!!")

# This def set the scores for if Xplayer or OPlayer won the gamed with values of 1 , -1


def getScore(brd):
    if winningPlayer(brd, XPLAYER):
        return 1

    elif winningPlayer(brd, OPLAYER):
        return -1

    else:
        return 0

# This def is the MiniMax Algorithm


def MiniMax(brd, depth, player):
    row = -1
    col = -1
    best = 0

    if player == XPLAYER:
        best = -inf
    else:
        best = inf

     # if we were in root position or anyone won the game it will show us the winner & row & collumn

    if depth == 0 or gameWon(brd):
        return [row, col, getScore(brd)]

    else:
        # if none it will run with setting position in [0][1] the it rerun MiniMax with 1 lower depth
        for cell in emptyCells(brd):
            setposition(brd, cell[0], cell[1], player)
            score = MiniMax(brd, depth - 1, -player)
            if player == XPLAYER:

                # Here if score was higher than best => best will be same as score & we go to next collumn

                if score[2] > best:
                    best = score[2]
                    row = cell[0]
                    col = cell[1]

                # Here if score was lower than best => best will be same as score & we go to next collumn
            else:
                if score[2] < best:
                    best = score[2]
                    row = cell[0]
                    col = cell[1]

            # Here it will put brd[0][1] = EMPTY

            setposition(brd, cell[0], cell[1], EMPTY)

        if player == XPLAYER:
            return [row, col]

        else:
            return [row, col]

# This def is the MiniMax Alpha-Beta Purning Algorithm


def MiniMax_AB(brd, depth, alpha, beta, player):
    row = -1
    col = -1
    alpha_cut = 0
    beta_cut = 0

    # if we were in root position or anyone won the game it will show us the winner & row & collumn
    if depth == 0 or gameWon(brd):
        return [row, col, getScore(brd)]

    # if none it will run with setting position in [0][1] the it rerun MiniMax_AB with 1 lower depth
    else:
        for cell in emptyCells(brd):
            setposition(brd, cell[0], cell[1], player)
            score = MiniMax_AB(brd, depth - 1, alpha, beta, -player)
            if player == XPLAYER:

                # Here if score was higher than alpha => alpha will be same as score & we go to next collumn

                if score[2] > alpha:
                    alpha = score[2]
                    row = cell[0]
                    col = cell[1]

            else:

                # Here if score was lower than beta => beta will be same as score & we go to next collumn

                if score[2] < beta:
                    beta = score[2]
                    row = cell[0]
                    col = cell[1]

            # Here it will put brd[0][1] = EMPTY & we will not cut this node

            setposition(brd, cell[0], cell[1], EMPTY)
            alpha_cut_check = False

            # But here if the alpha >= beta we will cut & we will rename our alpha,beta's

            if alpha >= beta:
                alpha_cut = alpha
                beta_cut = beta
                alpha_cut_check = True
                break

        if player == XPLAYER:
            return [row, col, alpha, beta, alpha_cut_check, alpha_cut, beta_cut]

        else:
            return [row, col, beta, alpha, alpha_cut_check, alpha_cut, beta_cut]


# Here we will use AI and choice library to choose a place set it , if all of our cells were full. (First Player)

def AIMoveMiniMax(brd):
    if len(emptyCells(brd)) == 9:
        x = choice([0, 1, 2])
        y = choice([0, 1, 2])
        setposition(brd, x, y, OPLAYER)
        showboard(brd)

    # If not full we will run untill its over

    else:
        result = MiniMax_AB(brd, len(emptyCells(brd)), -inf, inf, OPLAYER)
        setposition(brd, result[0], result[1], OPLAYER)
        showboard(brd)

# Here we will use AI and choice library to choose a place set it , if all of our cells were full in Alpha-Beta Purning. (First Player)


def AIMoveAB(brd):
    if len(emptyCells(brd)) == 9:
        x = choice([0, 1, 2])
        y = choice([0, 1, 2])
        setposition(brd, x, y, OPLAYER)
        showboard(brd)
        print("Alpha = -infinity")
        print("Beta = infinity")

    # If not full we will run untill its over with fulling empty cells & it counts alpha,beta's cut

    else:
        result = MiniMax_AB(brd, len(emptyCells(brd)), -inf, inf, OPLAYER)
        setposition(brd, result[0], result[1], OPLAYER)
        showboard(brd)
        print("Alpha =", result[3])
        print("Beta =", result[2])
        if result[4]:
            print("Alpha cut ", end="")
            print("( Alpha_cut =", result[5], end="")
            print(", Beta_cut =", result[6], ")")

# Here we will use AI and choice library to choose a place set it , if all of our cells were full in Alpha-Beta Purning but with one difference.


def AIMoveABForSimple(brd):
    if len(emptyCells(brd)) == 9:
        x = choice([0, 1, 2])
        y = choice([0, 1, 2])
        setposition(brd, x, y, OPLAYER)
        showboard(brd)
        print("Alpha = -infinity")
        print("Beta = infinity")

    # If not full we will run untill its over with final cell & it counts alpha,beta's cut
    else:
        result = MiniMax_AB(brd, 2, -inf, inf, OPLAYER)
        setposition(brd, result[0], result[1], OPLAYER)
        showboard(brd)
        print("Alpha =", result[3])
        print("Beta =", result[2])
        if result[4]:
            print("Alpha cut ", end="")
            print("( Alpha_cut =", result[5], end="")
            print(", Beta_cut =", result[6], ")")

# Here we will use AI and choice library to choose a place set it , if all of our cells were full in Alpha-Beta Purning. (First Player)


def AI2MoveAB(brd):
    if len(emptyCells(brd)) == 9:
        x = choice([0, 1, 2])
        y = choice([0, 1, 2])
        setposition(brd, x, y, XPLAYER)
        showboard(brd)
        print("Alpha = -infinity")
        print("Beta = infinity")

    # If not full we will run untill its over with fulling empty cells & it counts alpha,beta's cut

    else:
        result = MiniMax_AB(brd, len(emptyCells(brd)), -inf, inf, XPLAYER)
        setposition(brd, result[0], result[1], XPLAYER)
        showboard(brd)
        print("Alpha =", result[2])
        print("Beta =", result[3])
        if result[4]:
            print("Alpha cut ", end="")
            print("( Alpha_cut =", result[5], end="")
            print(", Beta_cut =", result[6], ")")


# If the user chooses AI vs Player , this will run & it wants to choose (AB vs AB) or (simple vs AB). it makes moves & sends each move for its def

def bot():
    print("MiniMax vs MiniMax_AB or MiniMax_AB vs MiniMax_AB? (1/2)")
    mode = int(input())
    if mode == 1:
        currentPlayer = XPLAYER
        clearBoard(board)

        while not (boardFull(board) or gameWon(board)):
            makeMoveForMiniMax(board, currentPlayer, 2)
            currentPlayer *= -1

        printResult(board)
    else:
        currentPlayer = XPLAYER
        clearBoard(board)

        while not (boardFull(board) or gameWon(board)):
            makeMove(board, currentPlayer, 2)
            currentPlayer *= -1

        printResult(board)

# MiniMax_AB vs MiniMax_AB Mode


def makeMove(brd, player, mode):
    if mode == 1:
        if player == XPLAYER:
            playerMove(brd)

        else:
            AIMoveAB(brd)
    else:
        if player == OPLAYER:
            AIMoveAB(brd)
        else:
            AI2MoveAB(brd)

# If the user enters mode 2 , this will makes (player vs AI) & If ther user enters 2 , this will makes (AI vs AI)


def makeMoveForMiniMax(brd, player, mode):
    if mode == 1:
        if player == XPLAYER:
            playerMove(brd)

        else:
            AIMoveMiniMax(brd)
    else:
        if player == OPLAYER:
            AIMoveMiniMax(brd)
        else:
            AI2MoveAB(brd)

# Easy Move


def makeMoveForSimple(brd, player, mode):
    if mode == 1:
        if player == XPLAYER:
            playerMove(brd)

        else:
            AIMoveABForSimple(brd)




# In here User enters the diffculty between Hard or Easy & User enters to be first or second...


def Pvbot():
    os.system("cls")
    mode = int(input(
        Fore.CYAN + "\n ┌─[Select diffculty , Easy or hard?]\n └──╼(1 or 2)>>> " + Fore.YELLOW + ""))
    if mode == 1:
        while True:
            try:
                os.system("cls")
                order = int(input(
                    Fore.CYAN + "\n ┌─[Do you want to play First or Second?]\n └──╼(1 or 2)>>> " + Fore.YELLOW + ""))
                if not (order == 1 or order == 2):
                    os.system("cls")
                    print(Fore.RED + "Info:: " +
                          Fore.YELLOW + "Please pick 1 or 2")
                    time.sleep(1)
                    Pvbot()
                else:
                    break

            # If not number entered
            except (KeyError, ValueError):
                print(Fore.RED + "Info:: " + Fore.YELLOW +
                      "Please Enter a Number!!")
                time.sleep(1)
                Pvbot()
        # Clear the board
        clearBoard(board)
        if order == 2:
            currentPlayer = OPLAYER
        else:
            currentPlayer = XPLAYER

        while not (boardFull(board) or gameWon(board)):
            makeMoveForSimple(board, currentPlayer, 1)
            currentPlayer *= -1

        # Final Result
        printResult(board)
    else:
        while True:
            try:
                os.system("cls")
                order = int(input(
                    Fore.CYAN + "\n ┌─[Do you want to play First or Second?]\n └──╼(1 or 2)>>> " + Fore.YELLOW + ""))
                if not (order == 1 or order == 2):
                    os.system("cls")
                    print(Fore.RED + "Info:: " +
                          Fore.YELLOW + "Please pick 1 or 2")
                    time.sleep(1)
                    Pvbot()
                else:
                    break
            # If wrong number entered
            except (KeyError, ValueError):
                print(Fore.RED + "Info:: " + Fore.YELLOW +
                      "Please Enter a Number!!")
                time.sleep(1)
                Pvbot()
        # Clear the board
        clearBoard(board)
        # If user want to be second or first
        if order == 2:
            currentPlayer = OPLAYER
        else:
            currentPlayer = XPLAYER

        while not (boardFull(board) or gameWon(board)):
            makeMove(board, currentPlayer, 1)
            currentPlayer *= -1
        # Final Result
        printResult(board)


# Run & Welcome print with Copyright & Asking the user want to play (AI vs AI) or (Player vs AI) & capturing answer


def run():
    while True:
        os.system("cls")
        print(Fore.CYAN + "           TIC" + Fore.BLUE + "-" +
              Fore.GREEN + "TAC" + Fore.BLUE + "-" + Fore.CYAN + "TOE" + "\nBy Bardia Fardar\n")
        mode = input(
            Fore.CYAN + "\n ┌─[AI vs AI or Player vs AI?]\n └──╼(1 or 2)>>> " + Fore.GREEN + "")
        if int(mode) == 1:
            bot()
        else:
            Pvbot()


# It makes the run definition to not be used by module but it uses as script
if __name__ == "__main__":
    run()

