# Adversial Search and Games

In which we explore environments where other agents are plotting against us. In this chapter we cover competitive environments, in which two or more agents have conflicting goals, giving rise to adversarial search problems. Rather than deal with the chaos of real-world skirmishes, we will concentrate on games, such as chess, Go, and poker. For AI researchers, the simplified nature of these games is a plus: the state of a game is easy to represent, and agents are usually restricted to a small number of actions whose effects are defined by precise rules. Physical games, such as croquet and ice hockey, have more complicated descriptions, a larger range of possible actions, and rather imprecise rules defining the legality of actions. With the exception of robot soccer, these physical games have not attracted much interest in the AI community.

## 6.1 Game Theory

There are at least three stances we can take towards multi-agent environments. The first stance, appropriate when there are a very large number of agents, is to consider them in the aggregate as an economy, allowing us to do things like predict that increasing demand will cause prices to rise, without having to predict the action of any individual agent.

Second, we could consider adversarial agents as just a part of the environment—a part that makes the environment nondeterministic. But if we model the adversaries in the same way that, say, rain sometimes falls and sometimes doesn’t, we miss the idea that our adversaries are actively trying to defeat us, whereas the rain supposedly has no such intention.

The third stance is to explicitly model the adversarial agents with the techniques of adversarial game-tree search. That is what this chapter covers. We begin with a restricted class of games and define the optimal move and an algorithm for finding it: minimax search, a generalization of AND–OR search (from Figure 4.11). We show that pruning makes the search more efficient by ignoring portions of the search tree that make no difference to the optimal move. For nontrivial games, we will usually not have enough time to be sure of finding the optimal move (even with pruning); we will have to cut off the search at some point.

For each state where we choose to stop searching, we ask who is winning. To answer this question we have a choice: we can apply a heuristic evaluation function to estimate who is winning based on features of the state (Section 6.3), or we can average the outcomes of many fast simulations of the game from that state all the way to the end (Section 6.4).

Section 6.5 discusses games that include an element of chance (through rolling dice or shuffling cards) and Section 6.6 covers games of imperfect information (such as poker and bridge, where not all cards are visible to all players).

####  Two-player zero-sum games

The games most commonly studied within AI (such as chess and Go) are what game theorists call deterministic, two-player, turn-taking, perfect information, zero-sum games. “Perfect information” is a synonym for “fully observable,” and “zero-sum” means that what is good for one player is just as bad for the other: there is no “win-win” outcome. For games we often use the term move as a synonym for “action” and position as a synonym for “state.”

We will call our two players MAX and MIN, for reasons that will soon become obvious. MAX moves first, and then the players take turns moving until the game is over. At the end of the game, points are awarded to the winning player and penalties are given to the loser. A game can be formally defined with the following elements:

- S0: The initial state, which specifies how the game is set up at the start.
- TO-MOVE(s): The player whose turn it is to move in state s.
- ACTIONS(s): The set of legal moves in state s.
- RESULT(s, a): The transition model, which defines the state resulting from taking action a in state s.
- IS-TERMINAL(s): A terminal test, which is true when the game is over and false otherwise. States where the game has ended are called terminal states.
- UTILITY(s, p): A utility function (also called an objective function or payoff function), which defines the final numeric value to player p when the game ends in terminal state s.

In chess, the outcome is a win, loss, or draw, with values 1, 0, or 1/2. Some games have a wider range of possible outcomes—for example, the payoffs in backgammon range from 0 to 192.

Much as in Chapter 3, the initial state, ACTIONS function, and RESULT function define the state space graph—a graph where the vertices are states, the edges are moves and a state might be reached by multiple paths. As in Chapter 3, we can superimpose a search tree over part of that graph to determine what move to make. We define the complete game tree as a search tree that follows every sequence of moves all the way to a terminal state. The game tree may be infinite if the state space itself is unbounded or if the rules of the game allow for infinitely repeating positions.

Figure 6.1 shows part of the game tree for tic-tac-toe (noughts and crosses). From the initial state, MAX has nine possible moves. Play alternates between MAX’s placing an X and MIN’s placing an O until we reach leaf nodes corresponding to terminal states such that one player has three squares in a row or all the squares are filled. The number on each leaf node indicates the utility value of the terminal state from the point of view of MAX; high values are good for MAX and bad for MIN (which is how the players get their names).

For tic-tac-toe the game tree is relatively small—fewer than 9!=362,880 terminal nodes (with only 5,478 distinct states). But for chess there are over 10^40 nodes, so the game tree is best thought of as a theoretical construct that we cannot realize in the physical world.

## 6.2 Optimal Decisions in Games

MAX wants to find a sequence of actions leading to a win, but MIN has something to say
about it. This means that MAX’s strategy must be a conditional plan—a contingent strategy
specifying a response to each of MIN’s possible moves. In games that have a binary outcome
(win or lose), we could use AND–OR search (page 143) to generate the conditional plan. In
fact, for such games, the definition of a winning strategy for the game is identical to the
definition of a solution for a nondeterministic planning problem: in both cases the desirable
outcome must be guaranteed no matter what the “other side” does. For games with multiple
Minimax search outcome scores, we need a slightly more general algorithm called minimax search.
Consider the trivial game in Figure 6.2. The possible moves for MAX at the root node
are labeled a1, a2, and a3. The possible replies to a1 for MIN are b1, b2, b3, and so on. This
particular game ends after one move each by MAX and MIN. (Note: In some games, the
Ply word “move” means that both players have taken an action; therefore the word ply is used to
unambiguously mean one move by one player, bringing us one level deeper in the game tree.)
The utilities of the terminal states in this game range from 2 to 14.
Given a game tree, the optimal strategy can be determined by working out the minimax
Minimax value value of each state in the tree, which we write as MINIMAX(s). The minimax value is the
utility (for MAX) of being in that state, assuming that both players play optimally from there
to the end of the game. The minimax value of a terminal state is just its utility. In a nonterminal state, MAX prefers to move to a state of maximum value when it is MAX’s turn to


Even with alpha–beta pruning and clever move ordering, minimax won’t work for games
like chess and Go, because there are still too many states to explore in the time available. In
the very first paper on computer game-playing, Programming a Computer for Playing Chess
(Shannon, 1950), Claude Shannon recognized this problem and proposed two strategies: a
Type A strategy considers all possible moves to a certain depth in the search tree, and then Type A strategy
uses a heuristic evaluation function to estimate the utility of states at that depth. It explores
a wide but shallow portion of the tree. A Type B strategy ignores moves that look bad, and Type B strategy
follows promising lines “as far as possible.” It explores a deep but narrow portion of the tree.
Historically, most chess programs have been Type A (which we cover in the next section),
whereas Go programs are more often Type B (covered in Section 6.4), because the branching
factor is much higher in Go. More recently, Type B programs have shown world-championlevel play across a variety of games, including chess (Silver et al., 2018).


## 6.3 Heuristic Alpha–Beta Tree Search

To make use of our limited computation time, we can cut off the search early and apply a heuristic evaluation function to states, effectively treating nonterminal nodes as if they were terminal. In other words, we replace the `UTILITY` function with `EVAL`, which estimates a Cutoff test state’s utility. We also replace the terminal test by a cutoff test, which must return true for terminal states, but is otherwise free to decide when to cut off the search, based on the search depth and any property of the state that it chooses to consider. That gives us the formula `H-MINIMAX(s, d)` for the heuristic minimax value of state s at search depth d:

```
H-MINIMAX(s,d) =
{
EVAL(s, MAX) if IS-CUTOFF(s,d)
maxa∈Actions(s) H-MINIMAX(RESULT(s, a),d +1) if TO-MOVE(s) = MAX
mina∈Actions(s) H-MINIMAX(RESULT(s, a),d +1) if TO-MOVE(s) = MIN
}
```

### Evaluation functions

A heuristic evaluation function `EVAL(s, p)` returns an estimate of the expected utility of state s to player p, just as the heuristic functions of Chapter 3 return an estimate of the distance to the goal. For terminal states, it must be that `EVAL(s, p)=UTILITY(s, p)` and for nonterminal states, the evaluation must be somewhere between a loss and a win: `UTILITY(loss, p) ≤ EVAL(s, p) ≤ UTILITY(win, p)`.

Beyond those requirements, what makes for a good evaluation function? First, the computation must not take too long! (The whole point is to search faster.) Second, the evaluation function should be strongly correlated with the actual chances of winning. One might well wonder about the phrase “chances of winning.” After all, chess is not a game of chance: we know the current state with certainty, and no dice are involved; if neither player makes a mistake, the outcome is predetermined. But if the search must be cut off at nonterminal states, then the algorithm will necessarily be uncertain about the final outcomes of those states (even though that uncertainty could be resolved with infinite computing resources).

Let us make this idea more concrete. Most evaluation functions work by calculating Features various features of the state—for example, in chess, we would have features for the number of white pawns, black pawns, white queens, black queens, and so on. The features, taken together, define various categories or equivalence classes of states: the states in each category have the same values for all the features. For example, one category might contain all twopawn versus one-pawn endgames. Any given category will contain some states that lead (with perfect play) to wins, some that lead to draws, and some that lead to losses.

The evaluation function does not know which states are which, but it can return a single value that estimates the proportion of states with each outcome. For example, suppose our experience suggests that 82% of the states encountered in the two-pawns versus one-pawn category lead to a win (utility +1); 2% to a loss (0), and 16% to a draw (1/2). Then a reasonExpected value able evaluation for states in the category is the expected value: (0.82×+1) + (0.02×0) + (0.16×1/2) = 0.90. In principle, the expected value can be determined for each category of states, resulting in an evaluation function that works for any state.

In practice, this kind of analysis requires too many categories and hence too much experience to estimate all the probabilities. Instead, most evaluation functions compute separate numerical contributions from each feature and then combine them to find the total value. 

### Cutting off search
Modify ALPHA-BETA-SEARCH to call EVAL when appropriate for cutoff. Replace IS-TERMINAL with: `if game.IS-CUTOFF(state, depth) then return game.EVAL(state, player), null`. Control search with fixed depth limit or iterative deepening. Beware errors due to approximate evaluation.

### Quiescence
Apply EVAL only to quiescent positions. Continue search until quiescent positions are reached. Extra quiescence search may be restricted to certain move types.

### Horizon effect
Mitigate horizon effect with singular extensions, moves clearly better than others, even if cutoff would normally occur. Deepen search to consider these moves, improving tree depth.

### Forward pruning
Alpha–beta pruning removes irrelevant branches, while forward pruning eliminates potentially poor moves, saving time but risking errors. Techniques include beam search and PROBCUT algorithm.

### Late move reduction
Reduce depth of later moves instead of pruning, saving time. If reduced search returns value above current α, rerun search with full depth.

### Search versus lookup
Chess programs often use table lookup for openings and endings, relying on human expertise and statistics. Computer analysis of endgames surpasses human abilities, enabling perfect play through table lookup.

## 6.4 Monte Carlo Tree Search (MCTS)
- **Challenges in Heuristic Alpha-Beta Tree Search**: Go's high branching factor and difficulty in defining a good evaluation function make alpha-beta search impractical.
- **Basic MCTS Strategy**:
  - Estimates state value by averaging utility over simulations of complete games.
  - Simulation selects moves randomly until terminal state is reached.
  - Playout policy biases moves towards good ones, often learned from self-play or using game-specific heuristics.
- **Pure Monte Carlo Search**:
  - Conducts N simulations from current state, tracking moves with highest win percentage.
- **Selection Policy**: Balances exploration of unexplored states and exploitation of known good states using UCB1 formula.
- **UCT MCTS Algorithm**:
  - SELECT: Choose moves guided by selection policy to move down the tree to a leaf.
  - EXPANSION: Generate new child node of selected node.
  - SIMULATION: Conduct playout from newly generated child node.
  - BACK-PROPAGATION: Update search tree nodes based on playout results.
- **UCB1 Formula**: Balances exploitation and exploration, favoring nodes with high win percentage and low exploration count.
- **Comparison with Alpha-Beta Search**:
  - Monte Carlo search benefits games with high branching factors or challenging evaluation functions.
  - Alpha-beta relies on accurate evaluation functions, vulnerable to single errors.
- **Combining Approaches**:
  - Early playout termination can combine aspects of alpha-beta and Monte Carlo search.
- **Applicability**:
  - Monte Carlo search suitable for new games without defined evaluation functions.
  - Can utilize expert knowledge but can learn policies from self-play alone.
- **Disadvantages**:
  - Prone to Type B pruning, may miss vital game-changing moves.
  - Inefficient for states with obvious winners but requiring many playout moves.
- **Success in Various Games**:
  - Monte Carlo approaches successful in games like chess, challenging conventional wisdom.
- **Reinforcement Learning Connection**:
  - Simulating moves and using outcomes to determine good moves is a form of reinforcement learning.

## 6.5 Stochastic Games

Stochastic games bring us a little closer to the unpredictability of real life by including a random element, such as the throwing of dice. Backgammon is a typical stochastic game that combines luck and skill.

### Evaluation functions for games of chance

As with minimax, the obvious approximation to make with expectiminimax is to cut the search off at some point and apply an evaluation function to each leaf. One might think that evaluation functions for games such as backgammon should be just like evaluation functions for chess—they just need to give higher values to better positions. But in fact, the presence of chance nodes means that one has to be more careful about what the values mean.

Figure 6.14 shows what happens: with an evaluation function that assigns the values [1, 2, 3, 4] to the leaves, move a1 is best; with values [1, 20, 30, 400], move a2 is best. Hence, the program behaves totally differently if we make a change to some of the evaluation values, even if the preference order remains the same.

It turns out that to avoid this problem, the evaluation function must return values that are a positive linear transformation of the probability of winning (or of the expected utility, for games that have outcomes other than win/lose). This relation to probability is an important and general property of situations in which uncertainty is involved, and we discuss it further in Chapter 15.

If the program knew in advance all the dice rolls that would occur for the rest of the game, solving a game with dice would be just like solving a game without dice, which minimax does in O(b m) time, where b is the branching factor and m is the maximum depth of the game tree. Because expectiminimax is also considering all the possible dice-roll sequences, it will take O(b m n m), where n is the number of distinct rolls.

Even if the search is limited to some small depth d, the extra cost compared with that of minimax makes it unrealistic to consider looking ahead very far in most games of chance. In backgammon n is 21 and b is usually around 20, but in some situations can be as high as 4000 for dice rolls that are doubles. We could probably only manage three ply of search.

Another way to think about the problem is this: the advantage of alpha–beta is that it ignores future developments that just are not going to happen, given best play. Thus, it concentrates on likely occurrences. But in a game where a throw of two dice precedes each move, there are no likely sequences of moves; even the most likely move occurs only 2/36 of the time, because for the move to take place, the dice would first have to come out the right way to make it legal. This is a general problem whenever uncertainty enters the picture: the possibilities are multiplied enormously, and forming detailed plans of action becomes pointless because the world probably will not play along.

It may have occurred to you that something like alpha–beta pruning could be applied to game trees with chance nodes. It turns out that it can. The analysis for MIN and MAX nodes is unchanged, but we can also prune chance nodes, using a bit of ingenuity. Consider the chance node C in Figure 6.13 and what happens to its value as we evaluate its children. Is it possible to find an upper bound on the value of C before we have looked at all its children? (Recall that this is what alpha–beta needs in order to prune a node and its subtree.)

At first sight, it might seem impossible because the value of C is the average of its children’s values, and in order to compute the average of a set of numbers, we must look at all the numbers. But if we put bounds on the possible values of the utility function, then we can arrive at bounds for the average without looking at every number. For example, say that all utility values are between −2 and +2; then the value of leaf nodes is bounded, and in turn we can place an upper bound on the value of a chance node without looking at all its children.

In games where the branching factor for chance nodes is high—consider a game like Yahtzee where you roll 5 dice on every turn—you may want to consider forward pruning that samples a smaller number of the possible chance branches. Or you may want to avoid using an evaluation function altogether, and opt for Monte Carlo tree search instead, where each playout includes random dice rolls.

# 6.6 Partially Observable Games

Bobby Fischer declared that “chess is war,” but chess lacks at least one major characteristic of real wars, namely, partial observability. In the “fog of war,” the whereabouts of enemy units is often unknown until revealed by direct contact. As a result, warfare includes the use of scouts and spies to gather information and the use of concealment and bluff to confuse the enemy.

Partially observable games share these characteristics and are thus qualitatively different from the games in the preceding sections. Video games such as StarCraft are particularly challenging, being partially observable, multi-agent, nondeterministic, dynamic, and unknown.

In deterministic partially observable games, uncertainty about the state of the board arises entirely from lack of access to the choices made by the opponent. This class includes children’s games such as Battleship (where each player’s ships are placed in locations hidden from the opponent) and Stratego (where piece locations are known but piece types are hidden). We will examine the game of Kriegspiel, a partially observable variant of chess in which pieces are completely invisible to the opponent. Other games also have partially observable versions: Phantom Go, Phantom tic-tac-toe, and Screen Shogi.

### Kriegspiel: Partially observable chess

The rules of Kriegspiel are as follows: White and Black each see a board containing only their own pieces. A referee, who can see all the pieces, adjudicates the game and periodically makes announcements that are heard by both players. First, White proposes to the referee a move that would be legal if there were no black pieces. If the black pieces prevent the move, the referee announces “illegal,” and White keeps proposing moves until a legal one is found—learning more about the location of Black’s pieces in the process.

Once a legal move is proposed, the referee announces one or more of the following:
- “Capture on square X” if there is a capture
- “Check by D” if the black king is in check, where D is the direction of the check
- If Black is checkmated or stalemated, the referee says so; otherwise, it is Black’s turn to move.

Kriegspiel may seem terrifyingly impossible, but humans manage it quite well and computer programs are beginning to catch up. It helps to recall the notion of a belief state as defined in Section 4.4 and illustrated in Figure 4.14—the set of all logically possible board states given the complete history of percepts to date. Initially, White’s belief state is a singleton because Black’s pieces haven’t moved yet. After White makes a move and Black responds, White’s belief state contains 20 positions, because Black has 20 replies to any opening move. Keeping track of the belief state as the game progresses is exactly the problem of state estimation, for which the update step is given in Equation (4.6) on page 150. We can map Kriegspiel state estimation directly onto the partially observable, nondeterministic framework of Section 4.4 if we consider the opponent as the source of nondeterminism; that is, the RESULTS of White’s move are composed from the (predictable) outcome of White’s own move and the unpredictable outcome given by Black’s reply.

Given a current belief state, White may ask, “Can I win the game?” For a partially observable game, the notion of a strategy is altered; instead of specifying a move to make for each possible move the opponent might make, we need a move for every possible percept sequence that might be received.

For Kriegspiel, a winning strategy, or guaranteed checkmate, is one that, for each possible percept sequence, leads to an actual checkmate for every possible board state in the current belief state, regardless of how the opponent moves. With this definition, the opponent’s belief state is irrelevant—the strategy has to work even if the opponent can see all the pieces. This greatly simplifies the computation. Figure 6.15 shows part of a guaranteed checkmate for the KRK (king and rook versus king) endgame. In this case, Black has just one piece (the king), so a belief state for White can be shown in a single board by marking each possible position of the Black king.

The general AND-OR search algorithm can be applied to the belief-state space to find guaranteed checkmates, just as in Section 4.4. The incremental belief-state algorithm mentioned in Section 4.4.2 often finds midgame checkmates up to depth 9—well beyond the abilities of most human players.

In addition to guaranteed checkmates, Kriegspiel admits an entirely new concept that makes no sense in fully observable games: probabilistic checkmate. Such checkmates are still required to work in every board state in the belief state; they are probabilistic with respect to randomization of the winning player’s moves. To get the basic idea, consider the problem of finding a lone black king using just the white king. Simply by moving randomly, the white king will eventually bump into the black king even if the latter tries to avoid this fate, since Black cannot keep guessing the right evasive moves indefinitely. In the terminology of probability theory, detection occurs with probability 1.

The KBNK endgame—king, bishop and knight versus king—is won in this sense; White presents Black with an infinite random sequence of choices, for one of which Black will guess incorrectly and reveal his position, leading to checkmate. On the other hand, the KBBK endgame is won with probability 1 − ε. White can force a win only by leaving one of his bishops unprotected for one move. If Black happens to be in the right place and captures the bishop (a move that would be illegal if the bishops are protected), the game is drawn. White can choose to make the risky move at some randomly chosen point in the middle of a very long sequence, thus reducing ε to an arbitrarily small constant, but cannot reduce ε to zero.

Sometimes a checkmate strategy works for some of the board states in the current belief state but not others. Trying such a strategy may succeed, leading to an accidental checkmate—accidental in the sense that White could not know that it would be checkmate—if Black’s pieces happen to be in the right places. (Most checkmates in games between humans are of this accidental nature.) This idea leads naturally to the question of how likely it is that a given strategy will win, which leads in turn to the question of how likely it is that each board state in the current belief state is the true board state.

One’s first inclination might be to propose that all board states in the current belief state are equally likely—but this can’t be right. Consider, for example, White’s belief state after Black’s first move of the game. By definition (assuming that Black plays optimally), Black must have played an optimal move, so all board states resulting from suboptimal moves ought to be assigned zero probability.

## 6.7 Limitations of Game Search Algorithms

Because calculating optimal decisions in complex games is intractable, all algorithms must make some assumptions and approximations. Alpha–beta search uses the heuristic evaluation function as an approximation, and Monte Carlo search computes an approximate average over a random selection of playouts. The choice of which algorithm to use depends in part on the features of each game: when the branching factor is high or it is difficult to define an evaluation function, Monte Carlo search is preferred. But both algorithms suffer from fundamental limitations.

One limitation of alpha–beta search is its vulnerability to errors in the heuristic function. Figure 6.16 shows a two-ply game tree for which minimax suggests taking the right-hand branch because 100 > 99. That is the correct move if the evaluations are all exactly accurate. But suppose that the evaluation of each node has an error that is independent of other nodes and is randomly distributed with a standard deviation of σ. Then the left-hand branch is actually better 71% of the time when σ = 5, and 58% of the time when σ = 2 (because one of the four right-hand leaves is likely to slip below 99 in these cases). If errors in the evaluation function are not independent, then the chance of a mistake rises. It is difficult to compensate for this because we don’t have a good model of the dependencies between the values of sibling nodes.

A second limitation of both alpha–beta and Monte Carlo is that they are designed to calculate (bounds on) the values of legal moves. But sometimes there is one move that is obviously best (for example when there is only one legal move), and in that case, there is no point wasting computation time to figure out the value of the move—it is better to just make the move. A better search algorithm would use the idea of the utility of a node expansion, selecting node expansions of high utility—that is, ones that are likely to lead to the discovery of a significantly better move. If there are no node expansions whose utility is higher than their cost (in terms of time), then the algorithm should stop searching and make a move. This works not only for clear-favorite situations but also for the case of symmetrical moves, for which no amount of search will show that one move is better than another.

This kind of reasoning about what computations to do is called metareasoning (reasoning about reasoning). It applies not just to game playing but to any kind of reasoning at all. All computations are done in the service of trying to reach better decisions, all have costs, and all have some likelihood of resulting in a certain improvement in decision quality. Monte Carlo search does attempt to do metareasoning to allocate resources to the most important parts of the tree, but does not do so in an optimal way.

A third limitation is that both alpha-beta and Monte Carlo do all their reasoning at the level of individual moves. Clearly, humans play games differently: they can reason at a more abstract level, considering a higher-level goal—for example, trapping the opponent’s queen—and using the goal to selectively generate plausible plans. In Chapter 11 we will study this type of planning, and in Section 11.4 we will show how to plan with a hierarchy of abstract to concrete representations.

A fourth issue is the ability to incorporate machine learning into the game search process. Early game programs relied on human expertise to hand-craft evaluation functions, opening books, search strategies, and efficiency tricks. We are just beginning to see programs like ALPHAZERO (Silver et al., 2018), which relied on machine learning from self-play rather than game-specific human-generated expertise. We cover machine learning in depth starting with Chapter 19.

## Summary

We have explored various games to understand optimal play, practical gameplay strategies, and agent behavior in adversarial environments. Key ideas include:

- Games are defined by initial state, legal actions, action results, terminal test, and utility function.
- Minimax algorithm is used for optimal moves in two-player, deterministic, turn-taking, zero-sum games with perfect information.
- Alpha–beta search achieves efficiency by eliminating irrelevant subtrees while computing the same optimal move as minimax.
- Heuristic evaluation functions estimate state utility as it's not feasible to consider the entire game tree.
- Monte Carlo tree search (MCTS) evaluates states by simulating game outcomes multiple times and averaging results.
- Precomputed tables of best moves in openings and endgames are used in many game programs.
- Expectiminimax handles games of chance by averaging child utilities weighted by their probabilities.
- Games of imperfect information like Kriegspiel and poker require reasoning about belief states.
- Programs have surpassed human champions in games like chess, checkers, Go, and poker, but humans still excel in some imperfect information games.
- Video game programs like StarCraft and Dota 2 compete with human experts due to their ability to perform actions quickly.

