# Minimal Size Chess State (MSCS) v1

MSCS is a way to store any legal state of chess wich can occur during a game as a 192-bit representation, or when encoded in base 64, as 3 characters.

This project will, along with an explanation of how the algorithm functions, implement a chess game which will update in real time what the state is acording to the algortihm and a converter which can take chess moves from a game and convert them to a state showing the final state of the board, take a MSCS-code and create the accompaniyng board.

## Encoding

The endoing works by representing all places on the board as based on bits on a table (as can be seen [below](#the-pieces))

### The leading bits

The first ten bits are the only ones not representing a place on the board. The 1st represents if it's white or blacks turn to move (or if it's mate which side lost) and the 2nd which king is first on the board. The following 4 bits (2 for white and black respectivly) represent on what sides castling is possible, 1 for left and 1 for right. The next bit says if a pawn was moved 2 squares and the final 3 bits says which column that pawn is in (this is to allow en passant).

### The board

The board is read from the bottom left reading to the top of each column (1 though 8) and then the remaining columns to the right (A though H).

### The pieces

Where `x` represents the color bit

192-bit implementation
```
0     - Empty space (Min 32 on the board, or ½)
1000x - Pawn (Max 16 on the board, or ¼)
1001  - King (Max 2 on the board)
1010x  - Queen (Max 18 on the board)
1011x  - Bishop (Max 20 on the board)
110x  - Knight (Max 20 on the board)
111x  - Rook (Max 20 on the board)
```
In the worst case (i.e. the case where the most bits possible are used) we have 32 empty squares (1-bit), 16 pawns (5-bit), 2 kings (4-bit), 2 queens (5-bit), 4 bishops (5-bit), 4 knights (4-bit) and 4 rooks (4-bit). This uses a total of 182 bits leaving 10 bits which can be used as leading bits.

## Issues

Currently there are inefficencies. The "pawn has moved two spaces"-bit is not needed and could be determined entirely based on the 3 following bits. Currently we allow pawns on rows 1 and 8 where they can never stand, a separate table for those rows could save some room. The ultimate goal would be to reach a 160-bit state, although how fesible that is in reality is still uncertain.
