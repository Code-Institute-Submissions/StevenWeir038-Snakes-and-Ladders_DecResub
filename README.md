# **Snakes and Ladders**
![Image of game board](docs/readme/game-image.png "Image of game board") 

Welcome to **Snakes and Ladders**, a classic boardgame enjoyed by children (and adults) throughout the world.

It's a game of simple logic and chance making it a suitable project to learn Python. 
I do hope you enjoy this take on the original!

## Wireframes
The game's logic was mapped out before coding began using a free version of [Lucidchart](https://www.lucidchart.com/pages/).

This web based platform is intuitive to use due to it's drag, drop and snap capabilities.

![Mockup](docs/wireframes/flowchart.png "Game logic flowchart") 

## Future Adaptations
Three additional game rules can be applied for extra complexity.

1. Each time a player throws a 6, they are entitled to roll the dice and move again.
2. If a player's pawn lands on a square occupied by an opponents pawn, that pawn is removed from the board and they must start again. 
3. An exact throw is required to reach square 100.  If the throw exceeds 100 the player must move backwards. Watch out for the snakes!

## Features
### Browser View

![Splash](docs/readme/live-deployment/splash.png "Splash") 

The game was developed at 1920 X 1080 pixels.  No real responsiveness has been built into the browser due to the backend nature of the project.  That being said, I used *Dev Tools*  `F12` in *Google Chrome* to test and position the terminal element to `width:115%` and `height:85vh` within the css inline code in `layout.html`.  This ideally positioned the svg background against the terminal.

Despite the frontend being a secondary concern I wanted to engage the user by:

1. providing an image that conveys that the game is Snakes and Ladders.
2. giving the `button` element in the webpage a bespoke title referencing the classic `CLICKME` convention.  Since we are programming and snakes do what they do, `bite` becomes `byte`.

*(I know, I hope my code is better than my humor!)*

### Splash Screen
It is customary to give a game a start screen before entering it menus.  This was incorporated due to user expectations during the testing phase..

![Splash-One](docs/readme/live-deployment/splash-one.png "Splash-One") 

### Main Menu
Menu options are color coded and given intuitive titles for the user.

![Main Menu](docs/readme/live-deployment/main-menu.png "Main Menu")

### Game Rules
The game rules can be accessed from the main menu.

![Rules](docs/readme/live-deployment/rules.png "Rules")

### View Board
The user can view the game board from the main menu for an appreciation of how the game board displays at the end of each turn.

![Menu Board](docs/readme/live-deployment/menu-board.png "Menu Board")


### Play Game (setup)
The user can select a validated number of players for the game between 2 and 4 with full error handling capabilities.

![Play Game](docs/readme/live-deployment/play-game.png "Play Game")


### Quit Application
The user is asked if they want to quit the application.  If *yes*, the application closes down. If *no*, the main menu is displayed.
Built in error handling accepts all case variations for `Y`es/`N`o.  An invalid input asks the user to retry. 

![Quit App](docs/readme/live-deployment/quit-app.png "Quit App")

### Game
In game, a player can land on a normal square.

![Game Move Standard](docs/readme/live-deployment/game-move-standard.png "Game Move Standard")

Land on a snake and move to the bottom of that the snake.

![Game Move Snake](docs/readme/live-deployment/game-move-snake.png "Game Move Snake")

Land on a ladder and move to the top of that ladder.

![Game Move Ladder](docs/readme/live-deployment/game-move-ladder.png "Game Move Ladder")

Reach or move past 100 therefore win the game.
If they win they are asked to return to the main menu.

![Game Move Victory](docs/readme/live-deployment/game-move-victory.png "Game Move Victory")

An addendum was made to the game following user feedback.  Users didn't know for how long the game was running and this was confusing/frustrating.  A counter was added and displayed for each turn to facilitate their requests.

A few formatting tweaks were also added on request.

![turn-addendum-user-feedback](docs/readme/turn-addendum-user-feedback.png "turn-addendum-user-feedback")


## Deployment
The live application can be viewed from this [link](https://snakes-and-ladders-sw.herokuapp.com/).

Go to [DEPLOYMENT.md](DEPLOYMENT.md) for detailed instructions to deploy application to Heroku.

## Testing & Debugging
Extensive testing has been undertaken to prevent program crashes or unintended actions.
Go to [TESTING.md](TESTING.md) to view known bugs and fixes.

### PEP8 Validation
Code in `run.py` successfully meets pep8 standards using the linked [validation](http://pep8online.com/) tool.

![pep8-validation-pass](docs/readme/pep8-validation-pass.png "pep8-validation-pass")

## Technologies Used
Flowcharts created with [Lucidchart](https://www.lucidchart.com/pages/).

Web deployment using [Heroku](https://www.heroku.com/about).

SVG background in browser generated using [Convertio](https://convertio.co/) and edited using [Boxy](https://boxy-svg.com/).  Both were free to use.
(Note, this was nothing more than a personal/fun touch but I hope it reinforces to the user as to what game they are playing)

Python version 3.8

Additional Python libraries used:
- **os** to clear terminal window & center display
- **time** to produce time delays to user inputs
- **random** to simulate dice roll
- **colorama** to beautify display
- **termcolor** to beautify title

## Media and Content
### Credits
Thank you to my mentor [Tim Nelson](https://tim.2bn.dev/) for his candor.  Fantastic as always.

Beyond the Code Institute LMS a few key sources cemented my understanding of how to combine working with loops, dictionaries and classes. In particular, accessing their attributes and using them within loops, lists and dictionary comprehensions.

- [Abarneret](https://stackoverflow.com/a/17662224)
- [Jobel](https://stackoverflow.com/a/41720350)
- [James Gallagher](https://careerkarma.com/blog/python-convert-list-to-dictionary/)
- [schneebuzz](https://stackoverflow.com/a/59999615)

To repeat the same iteration of a loop depending on a condition.
- [David Heffernan](https://stackoverflow.com/a/7293992)

Error handling for empty and non integer values at the same time.
- [Joshua Burns](https://stackoverflow.com/a/4994509)

To replace all occurrences of an element in a nested list
- [Indhumathy Chelliah](https://betterprogramming.pub/10-important-tips-for-using-nested-lists-in-python-38ceca68be35)

To clear the terminal window.
- [poke](https://stackoverflow.com/a/2084628)

To center content on the terminal window
- [Joe Iddon](https://stackoverflow.com/a/52138950)

To display a more traditional game board layout.
- [Manish V. Panchmatia](https://stackoverflow.com/a/55241525)

And to provide the board's text/alignment some uniformity.
- [yucer](https://stackoverflow.com/q/40999973)

How to be PEP8 compliant when working with long *f-strings*
- [Danny Bullis](https://stackoverflow.com/a/69908278)

Inspiration to use a SVG background in the browser.
- [Matt Bodden](https://github.com/MattBCoding). A fellow student at the [Code Institute](https://codeinstitute.net/).  We have never met though I appreciate the quality of his content.

### Content
Emojis from Emojipedia.
- [Balloon](https://emojipedia.org/balloon/)
- [Chequered Flag](https://emojipedia.org/chequered-flag/)
- [Snake](https://emojipedia.org/snake/)
- [Paperclip](https://emojipedia.org/linked-paperclips/) (analogue was used as ladder unavailable)

[Board image](https://www.istockphoto.com/vector/snakes-and-ladders-black-and-white-gm1066160462-285104267 "Board image") courtesy of iStock.

Browser background from [Wallpaper Cave](https://wallpapercave.com/w/wp9142232). This was converted to an *svg* format to enable browser display.

[SNAKE_HEAD and LADDER_FOOT dictionaries ](docs/readme/own-gameboard.png "Own Gameboard") based of a game purchased from [Ambassador Games](http://www.ambassadorgames.com/craftsman-deluxe-game-house.htm).
