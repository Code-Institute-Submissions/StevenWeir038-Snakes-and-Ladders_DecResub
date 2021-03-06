# Testing

Using the [flowchart](docs/wireframes/flowchart.png "Game logic flowchart") as a guide, an incremental approach was used to build the application.

## IDE tools
The problems tab beside the terminal in gitpod provided warnings for code issues.  There are a few categories. Those of primary concern were highlighted red and needed to be resolved.
This was consulted every after writing several lines of code and was especially useful for resolving syntax errors and checking for unused variables.

*Terminal output*

![problems-code-issues](docs/readme/problems-code-issues.png "problems-code-issues")

## Code Validation
Code readability and consistency was routinely checked by directly pasting into a [PEP8](http://pep8online.com/) linter.

Issues identified like those below could then be addressed using a clean as you go approach.

![pep8-check](docs/readme/pep8-check.png "pep-8-check")

## Game setup

### Casting
Care was taken when accepting inputs from the user.  These default to a *string* so required converting to an *integer* format as they were to be used in a loop to create a validated number of players.

``` python
# Immediately convert string input from user to an integer
player_count = int(input("Enter number of players between 2 and 4:\n"))
```

``` python
for p in range(1, player_count + 1):
    if p == 1:
        print("player one")  # testing
        player_list.append("P1 red")
    elif p == 2:
```
In early development an issue was found when the user didn't enter a value and just keyed *return*.

As *no* value was passed to the application it crashed as it bypassed the `validate_player_count()` function.

*Terminal output*

![no-player-input-error](docs/readme/no-player-input-error.png "no-player-input-error")

This was resolved by placing the `input()` inside a `try` statement and using truth value testing.

An added advantage to doing this was to raise/handle an error should the user enter a number as text.  E.g. *four* instead *4*. 

``` python
try:
    # code to run regardless, it may throw an exception...
    player_count = int(input(
        "Enter number of players between 2 and 4:\n"))
        raise ValueError

    if not input:
        print(f"You entered {player_count} player(s). Try again...\n")
        ...
except ValueError as e:
# except - if an exception thrown, clear terminal and restart program
    print('No value or text value submitted')
    # print(e)  # testing
    clear()    # clear terminal
    main()  # restart program
```

The associated `except` clears the terminal then restarts the program.

Note the `import` from the `os` module to build the clear_terminal function.

``` python
from os import system, name

def clear_terminal():
    """
    clear the terminal.
    """
    os.system("cls") if name == "nt" else system("clear")
```

At this stage, all the user now sees when submiting no or invalid input is the warning `No value or text value submitted`. The program resets itself after a brief period of time by employing the `sleep()` method from the imported `time` module.  This is necessary to give the users an oppurtunity to review and understand the message.

It's a confidence building experience. We don't want the application to crash caused by an error or update too quiclkly for users to get feedback.

### Validating player counts
Print statements using f strings provide human readible feedback from the terminal.  This is demonstrated when the user enters a value outside the *type* and *range* of players needed for the game.

``` python
def validate_player_count(player_count):
    """
    Check number of players supplied from game_setup() function
    is an integer >= 2 and <= 4
    """
    try:
        if player_count < 2 or player_count > 4:
            raise ValueError
    except ValueError:
        print(f"You entered {player_count} player(s). Try again...")
```

*Terminal output*

![game-setup-1-terminal](docs/readme/game-setup-1.png "game-setup-1-terminal")

### Verifying an object was created for the assigned number of players
My current understanding of Python is that it is built from list, dictionary and class structures.  As such, my challenge for this project was to:
- build a list of players based on a validated number of players entered by the user
- populate that list with predetermined player/pawn color values. (This could easily have been an inputted name)
- using dictionary comprehension, build a dictionary based of the above list.  Each key corresponds to a list value. As I wanted to follow an OOP paradigm the corresponding dictionary *values* were to be *instances* of the *Player* class.
- each instance can be accessed by their respective *key iterable*.

NB. Multiple methods and attributes can be added to the class instance in future development.  For an MVP, only the `pawn_color` and `curr_position` attributes were initially present.  These were added to at need and show the versatility of OOP.

### The **Player** class
``` python
class Player:
    """
    Player class
    """

    def __init__(self, pawn_color, curr_position=0):
        # inst properties
        self.pawn_color = pawn_color
        self.curr_square = curr_position

    # inst methods
    def location(self):
        """
        return a statement representing this object's:
        (plan is to update the VALUE ingame with dice roll or landing on a \
        snake head/ladder foot to simulate player's current position)
        """
        player_location = {f"{self.pawn_color} pawn is on square \
        {self.curr_square} "}

        return player_location
```

Each object has it's unique place in memory (proving its instance).

*Terminal output*

![verify-object-creation-terminal](docs/readme/verify-object-instance-of-a-class-creation.png "verify-object-creation-terminal")

## Game

### Considering data passing between functions
A conceptual leap I have made is to consider the structure of data that flows from one function to another.  Its must be compatible.  In this example, the game_setup() function returns a dictionary which is stored the *players* variable. By passing the players variable to the snl_game() function, we are providing the snl_game(players) function the dictionary generated by the game_setup() function.  Thus the player dictionary can now be manipulated within the snl_game() function.

``` python
players = game_setup()  # players = dict of players rtnd from game_setup()
snl_game(players)  # pass 'players' dictionary to the game
```

### Setting up an infinite loop between players
As this is a game of chance we do not know how many turns are needed for a player to win.
This requires an infinite loop which is set up using `while True:`.

NB. This isn't overly useful for debugging as you have to select *ctrl + c* simultaneously to stop.  As a computer is fast you can miss some important output on the terminal that is relevant to the debugging process.  

A solution is to comment out the infinite command using *ctrl + /* and replace with a loop that repeats several times only.

``` python
def snl_game(players):
"""
Iterate players, loop through each until win condition met
"""
# infinite loop needed to keep game live until victory condition met
# while True:
for i in range(1, 11):  # testing for 10 turns
```
*Terminal output*

![verify-forever-player-loop-terminal](docs/readme/verify-forever-player-loop.png "verify-forever-player-loop-terminal")

### Testing for a player landing on a SNAKE_HEAD or a LADDER_FOOT
If ladder and snake functionality is working correctly, then movements on the board are greater than a six as per each dice roll.  This is evidenced using terminal output.

``` python
    if new_position in SNAKE_HEAD:
        new_position = SNAKE_HEAD[new_position]
        print(f"{player_ID} landed on a SNAKE_HEAD and moves to {new_position}")
    elif new_position in LADDER_FOOT:
        new_position = LADDER_FOOT[new_position]
        print(f"{player_ID} landed on a LADDER_FOOT and moves to {new_position}")
    return new_position
```

*SNAKE_HEAD proof from terminal output*

![movement-snake-head-proof](docs/readme/movement-snake-head-proof.png "movement-snake-head-proof")

*LADDER_FOOT proof from terminal output*

![movement-ladder-foot-proof](docs/readme/movement-ladder-foot-proof.png "movement-ladder-foot-proof")

The snake and ladder functionality overrides the basic move as the code lines are after `new_position = curr_position + roll_num`.  

The current player's position value is checked against the SNAKE_HEAD or LADDER_FOOT dictionary.  If that value is equal to one already *in* the dictionary *'key'*, then the current position value becomes the *value* of the found key.

### Testing for first player reaching square 100
With the above game mechanics working, we now need to end the game when the first player sucessfully reaches square 100.
This is done by passing an attribute of the current player object to the `check_win()` function in a inner loop for each player iteration.  If the win condition is met, the application terminates after declaring a winner. 

Note there is no need to return `False` from `check_win` to keep the game running.

``` python
def check_win(player_ID, player_inst):
    if player_inst.curr_square >= 100:
        print(f"Player '{player_ID}' wins!\n")
        exit()
    return False
```

*Terminal output*

![winner](docs/readme/winner.png "winner")

### Simulating a dice roll and rolling a six for another turn
Simulating a roll fits neatly into its own `roll_dice()` function.

To get this to work the random module was imported into `run.py`. The `randint` method from this was used to return a random number from 1 to 6 to represent each side of a standard dice. This was saved into the `roll` variable.

To check if a player rolled a six, I envisaged another attribute in the Player class called `self.rolled_six = True`.
The value of the instance attribute could be changed truthy/falsy using a ternary expression taken from the `another_turn` variable.

**Unresolved Bug**

I opted to create the game without the **six roll** functionality due to a hard deadline. The code snippet has been retained on file for future use to summarise the issue and previous attempts to resolve.

A `while` loop was used as there is a chance that a six could be rolled consecutively.
The main issue was breaking out of the loop based upon the rolled_six attribute's value.
My current knowledge suggests a `while True` expresion always evaluates to true.

Review a useful article by [John Sturtz](https://realpython.com/python-while-loop/).

``` python
def roll_dice(player_inst):
    roll = random.randint(1, 6)
    # ternary expression to evaluate True or False
    another_turn = True if roll == 6 else False
    # assign bool value of another_turn variable to player_inst attribute
    player_inst.extra_roll = another_turn
    return roll
```

## Next steps
We now have a *text based simulation* of the game.  It could be useful to a developer or data scientist. The application however is intended to be **user centric**.

To help achieve this I developed a more refined wireframe.

![flowchart-1](docs/wireframes/flowchart-1.png "Game logic flowchart") 

It further conceptualised how the application should execute. It informed the developer:

- where the program should flow until *termination* 
- where to *display* information to the user
- offer the user a means to make *decisions* and branch the program
- error handle in places the user made an *input* to prevent a crash
- build functions to *process* the users choices

Note how this ties in with the legend in the diagram below.
The color palette and shapes make particular development tasks more obvious.

## User Centric Development Stage

### Error handling
Early development was useful.  Similar code constructs to capture errors can be used to handle decision areas of the project and prevent crashing. 

``` python
try:
    # code to run regardless, it may throw an exception...
    pre_game_choice = int(input(f"Select from options {Fore.RED}1{Fore.WHITE}, {Fore.GREEN}2 {Fore.WHITE}or {Fore.BLUE}3{Fore.WHITE}.\n"))
    if not input:
        raise ValueError

except ValueError as e:
    # except - if exception thrown, clear terminal and restart application
    # capture nums out of range or text input
    print(f'{Fore.RED}{Back.BLACK}Oops! Incorrect value submitted')
    sleep()
    clear_terminal()  # clear terminal
    pre_game()  # restart program
```

**Welcome Screen**

*Terminal output*

If an inputted value is not an *integer* value of *1*, *2* or *3*,  the program resets after displaying feedback after preset delay.
The *red* text color conveys a warning message to the user prior to reset.

![testing-incorrect-value-welcome](docs/readme/testing-incorrect-value-welcome.png "testing-incorrect-value-welcome")

*Terminal output*

Confirming program branching works.

![pre-game-branching-test](docs/readme/pre-game-branching-test.png "pre-game-branching-test")

**View Rules**

*Terminal output*

![testing-incorrect-value-view-rules](docs/readme/testing-incorrect-value-view-rules.png "testing-incorrect-value-view-rules")

**View Board**

*Terminal output*

![testing-incorrect-value-view-board](docs/readme/testing-incorrect-value-view-board.png "testing-incorrect-value-view-board")

**Game Setup**

*Terminal output*

![testing-incorrect-value-game-setup](docs/readme/testing-incorrect-value-game-setup.png "testing-incorrect-value-game-setup")


## Separation of concern
**View Rules**

Moved rules out of run.py into its own file to improve readability and maintainability of code.
These can be accessed as follows:

``` python
from rules import game_instructions
...

def view_rules():
    '''
    Clear terminal
    View Rules (import from rules.py)
    Back to welcome screen
    '''
    clear_terminal()
    print(game_instructions())
```

*Terminal output*

![separation-of-concern-rules](docs/readme/separation-of-concern-rules.png "separation-of-concern-rules")

### Game Board
My initial thoughts to develop the game board was to use several nested lists.
The square number landed on after completion of a player's turn was to be highlighted.

This could be achieved by targeting the relevant index (bearing in mind indexes start at 0).
Whilst doable, I considered if it was easier to target an integer rather than parse strings etc.

Also I was wished to display the board in a classical format.

*Old Board code snippet*

``` python
# create board - list of 10 nested lists.
board = []
# for i in range(0, 10):
# the quick way
# board.append(["???"]*10)
# give each square a number
# limitation in method. cannot display like proper board below
# as working within nested list structures
board.append(['????', '02', '03', '????', '05', '06', '07', '08', '????', '10'])
board.append(['11', '12', '13', '14', '15', '????', '17', '18', '19', '20'])
board.append(['????', '22', '23', '24', '25', '26', '27', '????', '29', '30'])
board.append(['31', '32', '33', '34', '35', '????', '37', '38', '39', '40'])
board.append(['41', '42', '43', '44', '45', '46', '47', '????', '49', '50'])
board.append(['????', '52', '53', '54', '55', '56', '57', '58', '59', '60'])
board.append(['61', '62', '63', '????', '65', '66', '67', '68', '69', '70'])
board.append(['????', '72', '73', '74', '75', '76', '77', '78', '79', '????'])
board.append(['81', '82', '83', '84', '85', '86', '87', '88', '89', '90'])
board.append(['91', '92', '????', '94', '????', '96', '????', '????', '99', '????'])

# stack lists on top of each other & print
for i in board:
    print(" ".join(i))  # use join method from list with one space to beautify board.
```

*Old Board on Terminal*

![old-board](docs/readme/old-board.png "old-board")

A search of [Stackoverflow](https://stackoverflow.com/a/55241525) yielded the following to achieve the classic layout:

*New Board code snippet*

``` python
# Credit to Manish V. Panchmatia(https://stackoverflow.com/a/55241525)
for i in range(99, -1, -1):
    if (i // 10) % 2 == 0:
        print("{0:4d}".format(i - 10 + 2 * (10 - (i % 10))), end=" ")
    else:
        print("{0:4d}".format(i + 1), end=" ")
    if i % 10 == 0:
        print("\r")
```

*New Board on Terminal*

![new-board-1](docs/readme/new-board-1.png "new-board-1")

First impressions, it is more `Pythonic`.

Manish's solution is excellent but it lacks an **iterable** structure to represent board squares.
I approximated his layout and incorporated a list structure with the following:

``` python
# build list of 100 items and convert from integer to string
    board = []
    row = []
    for square in range(100, 0, -1):
        row.append(str(square).zfill(3))
        # build 1 row of 10 squares at a time
        # in inner loop, use modulo to find first number divisible by 10 = 0
        if (square-1) % 10 == 0:
            board.append(row)
            # clearout row list to ready for next loop
            row = []
    # after 10 lists built, reverse order of every even row in inner loop
    for column in range(10):  # 10 cols on board as 100 / 10 = 10
        # inner loop reverses order of list
        # to approximate classic board layout
        if column % 2:
            board[column].reverse()
    # for a neat terminal output
    for square in board:
        print(" | ".join(square))
```

*Final Board on Terminal*

![final-board](docs/readme/final-board.png "final-board")

**NB. At this point, I also played around with creating a board class**.

An issue was that player moves were marked on the board.  As the game progressed, previous moves marked with a `XX` obscured the square numbers as the grid was being updated.

I when wrong here by drawing a board which saved/included a players previous position.  Really I should have drawn the same generic board and used a separate `method` within the Board class to identify a players position.  Lesson learned.

Prior to this revelation I did the following:

To display a clean board with only the players latest position, I used two functions with every player turn.
1. Draw a new board
2. Loop through the displayed board showing player position.

It works by accepting the arguments of the current player position and the `draw_board()` method into the `turn_board()` function.

*Building a board to display to terminal*

``` python
# display player position on a board in the terminal
turn_board(new_position, draw_board())
```

*Board displayed on win code snippet 1*

``` python
# first check if player is >= square 100 to display flag on square with string '100'
# requires the integer of the position value passed into turn_board() function
if position >= 100:
board[0][0] = " ????"
```

*Final Board 1 on Terminal - displaying victory*

Note that I bypassed testing for a `string` value of `"100"` as the player position may have been *more than* an *integer* of `100`.
The flag emoji would therefore not been drawn into the nested index at `[0][0]` due to a Falsy evaluation.

![testing-board-display-on-win](docs/readme/testing-board-display-on-win.png "testing-board-display-on-win")

After a few false starts and further research, I found the best way to pinpoint a list item's index based upon its value was to use `enumerate`.

When looping through a list to check for matching values, the counter value in enumerate can be inserted into a variable which holds the index if the value we are searching.  In this case.  Do something to a list item if `str_pos` == `board_xy`.


![testing-board-display-on-move](docs/readme/testing-board-display-on-move.png "testing-board-display-on-move")

The code can be condensed to the following list comprehension to achieve the same result.

``` python
board_xy = [" ????" for x, row in enumerate(board) for y, col in enumerate(row) if col == str_pos]
```

I haven't quite solved this refactor yet.

**Revisiting the Board class**

I struggled with understanding classes early on in the project so researched them whilst I developing the application with a more familiar procedural methodology due to percieved time constraints.
Before submission, some time did remain to refactor the code more inline with an OOP paradigm.

Standalone terminal board formatting and position updating functions were moved into the Board class to become its methods.

A new instance of `Board` was create for each new player turn.

The `turn_board` method searched and replaced the item in the instantiated `Board` list whose value matched the passed argument.

The printboard method formatted the instantiated `Board` for a neat terminal display.  Displaying the raw list directly would appear cumbersome.

``` python
b = Board()
b.turn_board(new_position)
b.printboard()
```

## Player class revisited & Turn incrementation
Due to the random nature of the game it can run for some time.  It would be useful to give the player a sense of how long they have been playing.

An extra attribute was added to the Player class to store the incrementation value which was updated from within `snl_game` on each iteration.

*Number of turns* code snippet*

``` python
# Added to Player class
self.num_turns = 0

# snl_game()

# increment count
player_inst.num_turns += 1
turns = player_inst.num_turns
turn_msg = f"\n{player_id} TURN {turns}\n"
print(turn_msg.upper())
```

*Terminal Output showing incrementation works*

![turn-increment](docs/readme/turn-increment.png "turn-increment")

## A note to the reader
Thankyou for making it this far.  I acknowledge this lengthly testing subsection may deviate from a standard readme.

However, as this was an educational project, I felt it was benefical to explain how my understanding of Python has evolved in a short period of time.  It has been an enjoyable exercise which has helped me realise that the more I learn the less I know.

[Return to README.md](README.md)