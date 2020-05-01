Klondike
========

The last thing the world needs is another program to implement Solitaire, a.k.a. Klondike. 
But here it is, implemented in the q programming language. 
Why?

Q is the language of kdb+, a proprietary ultra-fast database widely used in financial technology.
It is often overlooked that q is a general-purpose programming language, free for non-commercial use.

Q programs for financial technology tend to be closely guarded and anyway of little interest to those working outside that field. 
Games and puzzles are well-known problem domains, non-trivial but small enough for study.
An implementation of Klondike is a useful case study for writing q programs. 

A project of the Kx Librarian (librarian@kx.com).


About Klondike
--------------

https://en.wikipedia.org/wiki/Klondike_(solitaire)


Run
---

```bash
$ q klondike.q
```


Usage
-----

```q
q)see g:deal[]
21 [] 3H  __ __ __ __
0
TC [] [] [] [] [] []
   2H [] [] [] [] []
      3D [] [] [] []
         JH [] [] []
            7D [] []
               9D []
                  2C
"_____________________"
"score: 0"
TC JH
9D TC
2C 3D
```

Above a new game is dealt, assigned to `g` and displayed. 

The first row of the display shows 

-   the number of cards in the stock (21)
-   the hidden card/s of the stock
-   the top card exposed on the waste (3H)
-   the four empty piles of the foundation

The second row shows the number of passes through the stock (0).

There follows the tableau and a line. Below the line, the score and the three possible moves: TC to JH, 9D to TC, and 2C to 3D.

```q
q)see move[g] `TC`JH
21 [] 3H  __ __ __ __
0
__ [] [] [] [] [] []
   2H [] [] [] [] []
      3D [] [] [] []
         JH [] [] []
         TC 7D [] []
               9D []
                  2C
"_____________________"
"score: 5"
9D TC
2C 3D
```

Above we see the game after one of the possible moves. 
As it happens, all three moves can be made independently.

```q
q)see g:g move/(`TC`JH;`9D`TC;`2C`3D)
21 [] 3H  __ __ __ __
0
__ [] [] [] [] [] []
   2H [] [] [] [] []
      3D [] [] [] []
      2C JH [] [] []
         TC 7D KC []
         9D       5D
"_____________________"
"score: 15"
KC
```

KC can be moved to the empty pile.

```q
q)see g:move[g] `KC
21 [] 3H  __ __ __ __
0
KC [] [] [] [] [] []
   2H [] [] [] [] []
      3D [] [] [] []
      2C JH [] 6H []
         TC 7D    []
         9D       5D
"_____________________"
"score: 20"
"No moves possible"
```

When an ace is exposed it can be moved to the foundation with the same syntax used above to move KC.

Time to turn cards from the stock. 
This version of Klondike turns 3 cards and allows unlimited passes through the stock. It could easily be generalised to accommodate other versions, e.g. turn 1 card, only 3 passes.

```q
q)see g:turn[g]
18 [] TS  __ __ __ __
0
KC [] [] [] [] [] []
   2H [] [] [] [] []
      3D [] [] [] []
      2C JH [] 6H []
         TC 7D    []
         9D       5D
"_____________________"
"score: 20"
"No moves possible"
```

And so on. 


Discussion
----------

The source code is short, and written for study and discussion. 
<!-- See [code.kx.com/q/learn/reading/klondike](https://code.kx.com/q/learn/reading/klondike/). -->


Agenda
------

- [x] Data object to represent game
- [x] `see` to display game state
- [x] `move` to change game state
- [x] `turn` to change game state
- [ ] Detect when game is over
- [ ] Detect when no more useful moves are possible
- [ ] Autoplay
- [ ] HTML5 GUI
- [ ] Monte-Carlo simulation to tablulate probability of winning for each game version
- [ ] Machine-learning to improve play tactics