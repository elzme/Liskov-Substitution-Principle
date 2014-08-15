Liskov Substitution Principle
============================


The basic premise of the Liskov Substitution Principle is that if we
have an object ```S```,
which is a subtype of ```T``` then we should be able to substitute any
object of type ```T```
with ```S```, and still maintain the properties of our program. In other
words, if we have
several objects that are subtypes of ```T```, they should all be
interchangable without
effecting how the program operates.

Let's look at a Tic Tac Toe program, in which a human can play against
several different
computers, and each computer uses a different strategy when choosing a
move: an unbeatable
strategy, an easy strategy, etc. If we consider each player whether it
be a computer or a human, as a *subtype* of
```Player```, then each player should be interchangable with one
another. So, as we can see below
in our ```Game``` module, regardless of which player's turn it is, we
can simply call ```get_move``` on it.

```
defmodule Game
  def game_loop do
    player.get_move
  end
end
```

Creating our program in this manner allows our ```gamp_loop``` to switch
back and forth between the ```HumanPlayer``` and the
```ComputerPlayer``` and get that player's move, without having to tell
our ```Game``` module which player's turn it is. Furthermore,
if we decided to create a new unbeatable strategy for the computer to
use, we could replace the ```Computer Player``` with
```UnbeatableComputerPlayer```, and the transistion will be seamless to
the
```Game```. All it
knows is that two ```Player``` objects will be there. This allows for
flexibility in our system in that we're able to add as many new
types of
```Players``` as we want without ever having to touch ```Game```. So, by
making sure that we adhere to the Liskov Substitution Principle, we are
also adhering
to the Open-Closed Principle at the same time - we're closing our system
off to
modifiying ```Game```, but opening it to the possibility of extending
the
functionality by
adding new player strategies.

To further understand this principle and it's
benefits, let's see what this ```game_loop``` would look like if it
violated the Liskov Substitution Principle:

```
defmodule Game
  def game_loop do
    cond do
      player.type == :human_player ->
        get_human_player_move
      player.type == :unbeatable_computer ->
        get_unbeatable_computer_player_move
      player.type == :easy_computer_player ->
        get_easy_computer_player_move
    end
  end
end
```

In this case, the players (```HumanPlayer```, ```UnbeatablePlayer```,
and ```EasyComputerPlayer```) are **not** interchangable.
Our ```game_loop``` has to be aware of every type of player that we
currently have, and will need to be changed for every new type of player
that we choose to add going forward. As we can also see in the example above, the Liskov
Substitution Principle and the Open-Closed Principle are closely linked, and this example
violates both of them. In the implementation of ```game_loop``` above
we are opening our function up to needing revision, each time a new
type of computer strategy is created (when we really want to be closing
this part
of our application off to change).

The Liskov Substitution Principle is essential for code flexibility and
reusability. As we can see in the first implementation
of ```Game``` we're easily able reuse this module in any sort program
that requires a game loop. And when we use this flexible module, we can
create any number of player types to use interchangably.

