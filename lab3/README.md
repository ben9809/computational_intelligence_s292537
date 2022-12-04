## LAB 03

# NIM

### Task 3.1

My evolving strategy consist of creating a genome with size equal to the nim_size and for each gene choose a random number between 1 and the nim row at that specific position. The goal of the GA algorithm is to find the minimum number of element to subtract for each row so that the game takes more time to be completed so if that could give a chance of winning.

The fitness function of the genome mesures the ratio of victories over the total number of matches and if the player uses our strategy this measure should be 0.

The stategy used in the evaluation function by the other player is the nim-sum, considered the most optimal strategy.

Hyper parameter have been tuned but the nim-sum strategy has resulted unbeatable.
