# COMP1110 Assignment 2

## Academic Honesty and Integrity

Honesty and integrity are of utmost importance. These goals are *not* at odds
with being resourceful and working collaboratively. You *should* be
resourceful, you should collaborate within your team, and you should discuss
the assignment and other aspects of the course with others taking the class.
However, *you must never misrepresent the work of others as your own*. If you
have taken ideas from elsewhere or used code sourced from elsewhere, you must
say so with *utmost clarity*. At each stage of the assignment you will be asked
to submit a statement of originality, either as a group or as individuals. This
statement is the place for you to declare which ideas or code contained in your
submission were sourced from elsewhere.

Please read the ANU's [official position](http://academichonesty.anu.edu.au/)
on academic honesty. If you have any questions, please ask me.

Carefully review the statements of originality in the [admin](admin) folder
which you must complete at each stage.  Edit the relevant statement and update
it as you complete each stage of the assignment, ensuring that when you
complete each stage, a truthful statement is committed and pushed to your repo.

## Purpose

In this assignment you will *work as a group* to master a number of major
themes of this course, including software design and implementation, group
work, using development tools such as Git and IntelliJ, and using JavaFX to
build a user interface.  **Above all, this assignment will emphasize group
work**; while you will receive an individual mark for your work based on your
contributions to the assignment, **you can only succeed if all members
contribute to your group's success**.

## Assignment Deliverables

The assignment is worth 30% of your total assessment, and it will be
marked out of 30.  So each mark in the assignment corresponds to a
mark in your final assessment for the course.  Note that for some
stages of the assignment, you will get a _group_ mark, and for others
you will be _individually_ marked.  The mark breakdown and the due
dates are described on the
[deliverables](https://cs.anu.edu.au/courses/comp1110/assessments/deliverables/)
page.

Your tutor will mark your work via GitLab, so it is essential that you carefully follow instructions for setting up and maintaining your group repository.
You will be marked according to whatever is committed to your repository at the time of the deadline.
You will be assessed on how effectively you use Git as a development tool.

## Problem Description

Your task is to implement in Java, using JavaFX, a board game called the
[Catan Dice Game](https://www.catan.com/dice-game).
Important: The Catan Dice Game comes with two variants, called "Island One"
and "Island Two": **you will implement the Island One game**.
You can find the game rules, and a PDF of the score sheet (game map) on the
web page linked above.
You can also find several video tutorials explaining how to play the game
on-line, for example <https://www.youtube.com/watch?v=DNZ4tXhnBFA>.

Catan Dice Game is a spin-off (one of many) of the "Settlers of Catan" game
series. There are even several Catan dice games (for example,
"Catan Dice Game XXL"). If you search for information or answers about the
game on-line, make sure what you read actually refers to the
**Catan Dice Game**, not one of the many other games in the series.

## Game Overview

Catan Dice can be played by one or more players. Each player has their own
score sheet (map), and players take turns rolling dice to obtain resources
and using those resources to build structures on their map. All things built
contribute to a player's score, and the player with the highest score at the
end of the game is the winner. (If playing solo, you simply attempt to
maximise your score.)

An overview of the game rules is given below. Use this, in addition to the
resources linked above. If anything is unclear, please consult Piazza for
clarification.

### The Game Board

![Island One score sheet](assets/island-one-score-sheet-annotated.png)

The game board (or map) has four types of buildable structures: Roads
(rectangles, on the edges between the hexagonal tiles), Settlements
(the small house-like shapes at tile corners), Cities (the larger house
shapes, also at tile corners) and Knights (the figures near the centre
of each tile). Initially, only one road (the filled one, with an arrow on)
is built; all other structures are unbuilt. As a player builds structures,
they mark them off on their map by filling them in.
All buildable structures on the map have a number on them, which is their
points value. All roads are worth 1 point, while settlements, cities and
knights have varied points values.

### Resources

There is six different types of resources: Ore, Grain, Wool (represented
by a sheep), Timber, Bricks and Gold. Each structure has a build cost, which
is the resources a player must have available to build it. For example,
building a Road consumes 1 Brick and 1 Timber. The build costs are summarised
at the top of the score card. Gold is not used to build anything, but can
be traded for other resources (see "Trades and Swaps" below).

### Player Turn

On their turn, a player first rolls the dice (there are six of them). Each
die is marked with the six different resources in the game.

After rolling, the player may select a number of dice to re-roll, and after
the first re-roll may again select a number of dice and roll a third time.

Next, the player can trade and/or swap resources (using Knights), and use
resources to build. A player can build more than one structure on the same
turn, as long as they have enough resources to build all of them. Building
actions are done in sequence, so that conditions required to build a
structure (see "Building Constraints" below) do not necessarily have to
be satisfied at the start of the turn. For example, a player may build a
Road to reach a Settlement, and then build that Settlement, on the same
turn. Also note that building and swapping can be interleaved: a Knight
can be used to swap a resource on the same turn that the knight was built
(but only after it has been built, of course).

When a player cannot, or chooses not to, take any more actions, they sum up the points values
of all structures built during the turn and add that to their running score.
Resources remaining at the end of the turn are lost (i.e., there is no
accumulation of resources between turns).
Important rule: If a player does not build any structure during a turn,
they get a penalty of -2 points for that turn, added to their running
score.

### Building Constraints

In addition to having the resources available, there are certain constraints
on the order in which structures can be built:

*   Roads, Settlements and Cities must form a connected network, starting
    from the initial Road.

*   Settlements, Cities and Knights must be built in order of increasing
    points value. For example, a player must have built both the 7-point and
    12-point Cities before they can build the 20-point City.

### Trades and Swaps

A player can change their resources in two ways: trading Gold for other
resources, at a rate of 2:1, or swapping resources using Knights.

To trade, the player simply exchanges two Gold for one resource of their
choice.

A Knight, once built, can be used once per game to swap a resource. The
Knights numbered 1 through 5 each allow the player to swap one available
resource of their choice for Ore, Grain, Wool, Timber and Bricks,
respectively. (This is shown by a resource icon below each Knight on the
game map.) Knight number 6 is wildcard: it can be used to swap one
available resource of the player's choice for one of any other resources
(including Gold).

### Game End

The game ends after all players have had 15 turns. The winner is the player
with the highest score.

## Encoding the Game State

The following text encoding is used by the `CatanDice` class to interface
with the tests we provide for the various tasks.
You are strongly encouraged to use your own internal representation of the
game, and convert to and from the text encoding as required to fulfill the
tasks. The text representation is *neither robust nor convenient* to work
with, hence why you should use your own representation.

We encourage you to create your own tests. Your tests can interface
directly with your code. Do not edit the supplied tests, instead
add new files.

We will use different encodings to represent different aspects of the game:

*   A _board state_ is a string that encodes the state of one player's map.
*   A _resource state_ encodes the resources available to a player.
*   An _action_ string specifies a player action (a build, trade or swap);
    an array of action strings specifies a sequence of actions. This
    encoding will be used to test the validity of action sequences, and
    for you to output multi-action plans for a player.

A complete state of the game is made up of:

*   The number of players.
*   The board state of each player.
*   Whose turn it currently is (i.e., the current player's index)
*   The resource state of the current player.
*   How many turns have been completed.
*   The accumulated score of each player.

We do not define a string representation for the complete game state,
since none of the test/task methods need it. Each of the pre-defined
methods in the `CatanDice` class will take as arguments suitable
representations of the aspects of the game that are needed for the task.

### Board State

The board state is made up of the state of the buildable structures on the
map. Roads, Settlements and Cities have only two states: unbuilt or built.
Because each Knight can be used for a resource swap only once during a game,
Knights have three states: unbuilt, built and unused, or built and used.
Since the default (starting) state of all structures is to be unbuilt, we
will not represent unbuilt states explicitly. The board state encoding
is a comma-separated list of structures that have been built.

The string encoding of a single structure consists of a letter indicating
its type (and, in the case of Knights, state) followed by a number that
identifies which one it is. The letters and their meanings are:

*   `R`: a Road
*   `S`: a Settlement
*   `C`: a City
*   `J`: an unused Knight (these are called "resource Joker" in the game
    rule book).
*   `K`: a used Knight.

Roads are numbered as shown in the image below:

![Island One map with Road numbering](assets/island-one-with-numbering.png)

Settlements, Cities and Knights are identified by their points value.

For example, `"R0,S3,R2,K1,J2"` specifies a state in which Roads 0 and 2,
the 3-point Settlement, and the 1-point and 2-point Knights are built,
and the 1-point Knight has been used but the 2-point Knight is still
unused.

### Resource State

A resource state is an array of six integers, representing the quantity
available of each of the six resources. The order of the resources is
(0) Ore, (1) Grain, (2) Wool, (3) Timber, (4) Bricks and (5) Gold.
For example, the array `{ 1, 0, 1, 2, 0, 2 }` indicates that the player
has 1 Ore, 1 Wool, 2 Timber and 2 Gold available. Note that the total
quantity of resources can vary.

### Actions

An action string consists of an action keyword, followed by one or more
arguments, each separated by a single space (` `). The format of the
arguments depends on the action keyword. The three possible actions are:

*   `"build"`: followed by a single structure identifier, as described
    in the definition of the board state above. For example, `"build R2"`
    means build Road 2, `"build J1"` means build the 1-point Knight (we
    use `J` instead of `K` since a Knight is always built in its unused
    state).

*   `"trade"`: followed by a single digit `0`-`5` specifying which
    resource to trade for. (Since the payment for a trade is always
    2 Gold, we do not have to including it in the action specification.)
    For example, `"trade 4"` means to trade 2 Gold for 1 Brick.

*   `"swap"`: followed by two digits `0`-`5` specifying the resource
    swapped out and the one it is exchanged for. For example, `"swap 1 4"`
    means exchanging 1 Grain for 1 Brick.

Note that the swap action is not unambiguous: In the example above, if
the player has both the 5-point (Brick) and 6-point (wildcard) Knights
available (built and unused), the swap can be performed using either of
the two. However, since it is always better to use a resource-specific
Knight (keeping the wildcard unused gives more future options), we can
assume that swaps will be executed using resource-specific Knights first,
and the wildcard Knight only when necessary.

An action sequence is represented by an array of action strings.

## Task list

*   **Task 1: Setup and position constructors**

	Fork the assignment 2 repo, clone it, explore the README.

	More detailed instructions on this task [are under the course deliverables](https://cs.anu.edu.au/courses/comp1110/assessments/deliverables/#D2A).

	- [ ] **One group member** should use GitLab to fork the assignment from the class account.  See [lab one](https://gitlab.cecs.anu.edu.au/comp1110/comp1110-labs/blob/master/src/comp1110/lab1/README.md) if you've forgotten how to do this.
	- [ ] Establish your group name (your tutor will assign it to you) and [add it to your gitlab project description](https://cs.anu.edu.au/courses/comp1110/assessments/deliverables/#D2A).
	- [ ] Add all group members as *maintainers* of the project.
	- [ ] Verify your repo project description and membership with your tutor.
	- [ ] Edit, commit, and push the `admin/members.yml` file.
	- [ ] Each group member should clone the repo.
	- [ ] Set up the repo for [upstream pulls](https://gitlab.cecs.anu.edu.au/comp1110/comp1110-labs/blob/master/src/comp1110/lab4/README.md#gitlab-upstream-pulls-for-assignment).
	- [ ] Exchange contact information among group members.
	- [ ] Set a regular meeting time.

*   **Task 2: Design**

	Make an initial *design* for your code, including skeleton code.

	- [ ] Read the description for [D2B](https://cs.anu.edu.au/courses/comp1110/assessments/deliverables/#D2B)
	- [ ] Create a sketch of your design and save it as a pdf admin/B-design.pdf, commit, and push.  You may use any tool you like to create the sketch (including a photo of a paper sketch).  What is important is that file is a pdf file, and it is named correctly.
	- [ ] Create a design **skeleton**.   This means creating classes, methods, and java doc, **without writing any functional code.**  This skeleton should convey the overall approach you think you will take without actually providing a working solution (which is why you will not write any working code).
	- [ ] Make sure to update your originality and contribution statements in `admin/B-originality.yml` and `admin/B-contribution.yml`.

*   **Task 3: Check Board State is Well Formed**

	Check that the string encoding of a board state is well formed.

	Hint: You can use the `String.split` method to transform the input string
	into an array of Strings (one per structure that is mentioned).

	- [ ] Implement the `isBoardStateWellFormed(String[] board_state)` method
in the `CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution
statements, commit and push, and close this issue.

*   **Task 4: Check Action is Well Formed**

	Check that the string encoding of a player action is well formed.

	- [ ] Implement the `isActionWellFormed(String action)` method in the
`CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution
statements, commit and push, and close this issue.


*   **Task 5: Visualise Game States**

	Complete the `Viewer` class so that it displays the board state specified in
	the string input via the text box. Note that your display does not have to
	look like the original board game: it must only show the state of the board
	in a clear and easy-to-understand manner.
	You may want to look at the code for the `Game` class of assignment one to
	get some ideas.

	- [ ] Show the map structures, and their relationships (e.g., how Roads
connect).
	- [ ] Show the state of each structure (unbuilt or built, and unused or used
for Knights).
	- [ ] Implement the `displayState()` method of the `Viewer` class so that it
displays an entire state visually, according to the string input via the text
box.
	- [ ] When done, make sure to update your originality and contribution
statements, commit and push, and close this issue.


*   **Task 6: Roll dice**

	Implement dice rolling.

	Hint: The `java.util.random` package provides functions for generating
	(pseudo-)random numbers.

	- [ ] Implement the `rollDice(int n_dice, int[] resource_state)` method in the `CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.


*   **Task 7: Check Resource Availability**

	Check if a given structure can be built with available resources.

	- [ ] Implement the `checkResources(String structure, int[] resource_state)` method in the `CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.


*   **Task 8: Check Build Constraints**

	Check if given structure is a possible next build, given a board state.

	- [ ] Implement the `checkBuildConstraints(String structure, String board_state)` method in the `CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.

*   **Task 9: Check Action Validity**

	Check if a given action can be performed in the given board and resource state.

	- [ ] Implement the `canDoAction(String action, String board_state, int[] resource_state)` method in the `CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.


*   **Task 10: Implement a playable game for one player.**

	The game GUI must fit in a 1200x700 window, and must be built with JavaFX 17.
	Apart from that, the design of the GUI is up to you.

	- [ ] Extend the `Game` class in `src/gui` to a playable single-player game.
	- [ ] The game GUI should be intuitive and easy to use. You can assume that the player is familiar with the concepts and rules of CatanDice (you do not need to document them) but if you expect the player will need some instructions on how to use your GUI to play, you may write those in the "Player instructions" section of the file `admin/F-features.md`.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.

*   **Task 11: Check Action Sequence Validity**

	Check if a sequence of actions can be executed, starting from the given board and resource state.

	- [ ] Implement the `canDoSequence(String[] actions, String board_state, int[] resource_state)` method in the `CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.

*   **Task 12: Check Resource Availability, with Trades and Swaps**

	Check if a given structure can be built with available resources, considering possible trades and swaps.

	- [ ] Implement the `checkResourcesWithTradeAndSwap(String structure, int[] String board_state, resource_state)` method in the `CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.


*   **Task 13: Find path to target structure.**

	Find the path of roads that must be built to reach a specified structure in a given board state.

	- [ ] Implement the `pathTo(String structure, String board_state)` method in the `CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.

*   **Task 14: Generate Build Plans**

	Generate an action sequence to build a specified structure, starting from the given board and resource state.

	- [ ] Implement the `buildPlan(String target_structure, int[] resource_state, String board_state)` method in the `CatanDice` class.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.


*   **Task 15: AI player**

	Implement an AI player.

	An AI player has to make two kinds of decisions: what dice to re-roll,
	and what actions to take after rolling is done.

	The `buildPlan` method can be used as the basis for a simple AI player:
	iterate over unbuilt structures, find which can be built, and select the
	highest scoring. Repeat until nothing more can be done.

	- [ ] Extend your game implementation to include a computer opponent. The game should still be playable by a single human player. It is up to you how you represent the board state of and actions taken by the AI player in the game GUI.
	- [ ] When done, make sure to update your originality and contribution statements, commit and push, and close this issue.


## Evaluation Criteria

It is essential that you refer to the
[deliverables page](https://cs.anu.edu.au/courses/comp1110/assessments/deliverables/)
to check that you understand each of the deadlines and what is
required.  Your assignment will be marked via tests run through git's
continuous integration (CI) framework, so all submittable materials
will need to be in git and in the *correct* locations, as prescribed
by the
[deliverables page](https://cs.anu.edu.au/courses/comp1110/assessments/deliverables/).

**The mark breakdown is described on the
[deliverables](https://cs.anu.edu.au/courses/comp1110/assessments/deliverables/)
page.**

### Part One

In the first part of the assignment you will:
* Set up your assignment (Task #1).
* Create a design skeleton (Task #2).
* Implement parts of the testing interface to the game (Tasks #3 and #4).
* Implement a simple viewer that allows you to visualize game states (Task #5).
* Implement basic parts of the game logic (Tasks #6, #7 and #8).

An indicative grade level for each task for the [completion of part one](https://cs.anu.edu.au/courses/comp1110/assessments/deliverables/#D2C) is as follows:

**Pass**
* Tasks #1, #2, #3 and #4.

**Credit**
* Task #5 *(in addition to all tasks required for Pass)*

**Distinction**
* Task #6, #7 and #8. *(in addition to all tasks required for Credit)*

### Part Two

Create a fully working game, using JavaFX to implement a playable
graphical version of the game in a 1200x700 window.

Notice that aside from the window size, the details of exactly how the
game looks etc, are **intentionally** left up to you. You may choose to
closely follow the look of the original board game, or you may choose to
present the game in totally different manner.

The only **firm** requirements are that:

* Your game must run using Java 17 and JavaFX 17.
* Your implementation must respect the specification of the game rules
  given here.
* Your game must be easy to play.
* Your game must runs in a 1200x700 window.
* Your game must be executable on a standard lab machine from a jar
  file called `game.jar`,

Your game must successfully run from `game.jar` from within another
user's (i.e.  your tutor's) account on a standard lab machine. In
other words, your game must not depend on any features not self-contained
within that jar file and the Java 17 runtime.

An indicative grade level for each task for the [completion of part
two](https://cs.anu.edu.au/courses/comp1110/assessments/deliverables/#D2F)
is as follows:

**Pass**
* Correctly implements all of the <b>Part One</b> criteria.
* Appropriate use of git (as demonstrated by the history of your repo).
* Completion of Task #10.
* Executable on a standard lab computer from a runnable jar file,
  game.jar, which resides in the root level of your group repo.

**Credit**
* _All of the Pass-level criteria, plus the following..._
* Tasks #9 and #11.

**Distinction**
* _All of the Credit-level criteria, plus the following..._
* Tasks #12 and #13.

**High Distinction**
* _All of the Distinction-level criteria, plus the following..._
* Tasks #14 and #15.
