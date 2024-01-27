# Optimally break a large standup in small groups

## A broken standup

Standups can be useful. A brief daily meeting where team members provide a concise update on their work since the last standup, highlighting achievements, potential roadblocks, and upcoming tasks.
If you work on multiple teams, your work has few dependencies on your colleagues and project-leads confuse process with progress, standups can be a cancer. 

I was part of a growing team, from $n = 3$ to $n = 10$. Most workstreams did not overlap. But for 15 min each day, we went through a ritualistic reading of our daily todos. 
No context. No information transfer. No questions. Only the breath of a 90 second on-screen timer in their necks.

When we were 3 people still made sense. You had 5 min per person. You could check-in with each other. This is what mattered to the team-lead. 
Keeping a sense of social accountability, knowing what everyone is upto and feeling the temperature. It just did not scale well beyond a three people team. 

## Keeping in touch and sanity

For a team of $n$ members and only weak technical overlap, we seek a standup format that
1. supports valuable exchange of information,
2. strengthens the sense of belonging and,
3. scales with $n$.

The answer of course is to form breakout groups of size $k < n$. Like $k=3$. Breakout groups are run in parallel. It's more personal. You can ask questions. You can pickup people where they are. 

We tried. The team was happy. Only one issue remained. Some colleagues were never part of the same group. Others were frequently lumbed together. 

We want maximal exposure and diversity. How to maximise the number of iterations until the two people meet again?

## The solution for $k=2$

For $n$ team members and $k=2$, we list the unique pairs. The diagonal is irrelevant because people are not paired with themselves. We keep only the lower triangular matrix because there is no order within a pair:

|         |         |         |         |
|---------|---------|---------|---------|
| (2,1)   |         |         |         |
| (3,1)   | (3,2)   |         |         |
| (...,1) | (...,2) | (...,...)|         |
| ($n$,1) | ($n$,2) | ($n$,...)| ($n,n-1$)|

An example of a standup would be every 2nd entry in the 1-off-diagonal: $(2,1),(4,3),(6,5),\dots$. We see that each standup has $n/2$ pairs (assuming $n$ is even). Assuming such a sequence exists, there are $n-1$ standups before pairs are repeated. For example, for the first person the standups $n-1$ standups would include the following pairs $(2,1),(3,1),(4,1),\dots,(n,1))$. How do we select $n-1$ standups of $n/2$ pairs, such that all $n(n-1)/2$ pairs occur exactly once?

The chess player Richard Schurig had, according to Wikipedia, the same problem. He wanted that over the course of the tournament each player was paired with all others. The scheduling algorithm goes under the name [Round-Robin Tournament]([url](https://en.wikipedia.org/wiki/Round-robin_tournament)). 

The following figure shows a visualisation of pairs for $n=10$. The initial pairing is a cross-diagonal in the upper triangular matrix (in dark blue). With each iteration the diagonal shifts by two. Iterations go (over the rainbow) from blue to red. Each iteration a point hits the bottom of the grid, i.e., the y-coordinate becomes zero. This is when we re-introduce them in the lower triangular matrix. Since we do not care about the ordering of the pair, these points can be brought back into the upper triangle by interchanging x and y coordinates, shown in shaded colors. Note how the re-introduced points are shifted by 1 to avoid leaving have of the grid empty and double-covering the other half. The axis that defies this rule is the zero-axis. Here, a single points simply travels down from (1,10) all the way to (1,2). 

The final panel shows all points together. It looks messy. But you can see the diagonals traveling across the grid.

![round_robin_tournament_visual](https://github.com/Jegmi/jegmi.github.io/assets/3259580/0110972f-328c-4427-9cb1-32982315c8c2)

## Further thoughts

Standups need not be broken. However, it is an open question how to generalise the Round-Robin tournament to triplets $k=3$. If you know the answer, please reach out! 
