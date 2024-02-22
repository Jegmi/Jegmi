# The Round-Robin tournament for three-player games

A trade-off in organisations is between group size and information exchange. Large orgs, like global companies, can benefit from specialsed workers but often suffer from slow and leaky processes to exchange information between teams, departments and across hierachies. Small orgs, like startups, can benefit from quick information exchange, but the total amount of expertise is limited due to the small number of brains involved. This raises the question of how to best exchange information for a given size.

In companies, many ways exist to exchange information: emails, documents, coffee chats, company-wide meetings, to name a few. To make the problem tractable, let us focus on only one type of exchange, often used in feedback sessions or for problem-solving: the 1on1 or four-eye conversation. Allowing only for 1on1, full information exchange within a group means that each person should have a 1on1 with each other member. The solution to this problem come from a somewhat unexpected place: chess. 

The chess player Richard Schurig invented the [Round-Robin Tournament]([url](https://en.wikipedia.org/wiki/Round-robin_tournament)), which we will understand in detail later. For now, let us appreciate that the Round-Robin Tournament has applications beyond chess. I found a curious application as co-organizer of kissing workshop at [Nowhere](www.goingnowhere.org), a festival. We used the Round-Robin Tournament so that participant could explore the kissing techniques of all other participants without running into repeatitions.

This blog-post is motivated by yet another motivation. In my last job, I was part of a growing team. To stay in-tune with each other, we had daily 15-min checkins. This worked when we were a team of three. But as we grew to 10, the checkins had degraded to a context-free, ritualistic reading of our daily todos with the breath of a 90 second on-screen timer in our necks. Organising the checkins as parallel 1on1s via a Round-robin tournament would certainly have improved our lot. With one 1on1 per day, you would have checked in with the remaining 9 colleagues within a 9 day-window. But we felt that 3-person meetings were just as useful as 1on1, with the additional benefit that we would need fewer days to see all colleagues.

## The Round-Robin Tournament visualised

Using the language of games, as the Round-Rubin Tournament originates from chess, we assume an even number of $n$ players and 1on1 games. We then list all matches in a table. The diagonal is irrelevant because people are not matched with themselves. We keep only the lower triangular matrix because there is no order within a match:

|         |         |         |         |
|---------|---------|---------|---------|
| (2,1)   |         |         |         |
| (3,1)   | (3,2)   |         |         |
| (...,1) | (...,2) | (...,...)|         |
| ($n$,1) | ($n$,2) | ($n$,...)| ($n,n-1$)|

Assuming one match per player per day, the first day of the tournament could be organised as the 2nd entry in the 1-off-diagonal: $(2,1),(4,3),(6,5),\dots$. We see that each day has $n/2$ matches (assuming $n$ is even). There are $n-1$ days before matches are repeated. For example, for the first player could go through the following $n-1$ matches $(2,1),(3,1),(4,1),\dots,(n,1))$. How do we select $n-1$ days of $n/2$ matches, such that all $n(n-1)/2$ matches occur exactly once?

The following figure shows a visualisation of tournament for $n=10$. The initial match-up is a cross-diagonal in the upper triangular matrix (in dark blue). With each iteration the diagonal shifts by two. Iterations go (over the rainbow) from blue to red. Each iteration a point hits the bottom of the grid, i.e., the y-coordinate becomes zero. This is when we re-introduce them in the lower triangular matrix. Since we do not care about the ordering of the pair, these points can be brought back into the upper triangle by interchanging x and y coordinates, shown in shaded colors. Note how the re-introduced points are shifted by 1 to avoid leaving have of the grid empty and double-covering the other half. The axis that defies this rule is the zero-axis. Here, a single points simply travels down from (1,10) all the way to (1,2). 

The final panel shows all points together. It looks messy. But you can see the diagonals traveling across the grid.

![round_robin_tournament_visual](https://github.com/Jegmi/jegmi.github.io/assets/3259580/0110972f-328c-4427-9cb1-32982315c8c2)

## Round-Robin Tournament with three-player games

Now, we consider a modification of the Round-Rubin Tournament. Instead of 1on1 matches, we consider three-player matches. The goal is to organise the matches such that players face each other exactly once and have zero wait times. If on the first day of the tournament we schedule a match between the first three players $(1,2,3)$, the remaining tournament days should include the match $(1,4,5)$ but not the match $(1,2,4)$. 

Under these constraints, many group sizes $n$ do not have a solution. Consider $n=4$. If we schedule $(1,2,3)$ on day one, we already have a wait day for player 4. Even if we did not care about wait times, we still run into the issue that all players need to be matched with player $4$ in a three-player game during the rest of the tournament. Yet, we already ran out of permisslbe options $x$ to complete the match up (1,4, x) because the first three players were already matched on day one. This is why we need additional assumption about the group size $n$. (Remember that the Round-Rubin Tournament required the assumption of an even number of players too)

So far, I have only found solutions for the first three powers of three: $n=3$, $n=9$ and $n=27$. Since $n=3$ is trivial, let us focus on the later two.

### Solution for $n=9$:

A three-player Round-Rubin Tournament requires 4 days for $n=9$. For instance, player 1 will be matched up with 2 other players per day. After 4 days, they will have been matched with the remaining players $2$ to $9$.

With each player being represented by a number, we visualise the solution as masks on a telefone pad:

|         |         |         |
|---------|---------|---------|
| 1 | 2 | 3 |
| 4 | 5 | 6 |
| 7 | 8 | 9 |

On day 1, we organise three matches A, B and C as horizontals:

|         |         |         |
|---------|---------|---------|
| A | A | A |
| B | B | B |
| C | C | C |

On day 2, we organise matches as verticals:

|         |         |         |
|---------|---------|---------|
| A | B | C |
| A | B | C |
| A | B | C |

Note that the overlap between matches on the first and second day is only one players because horizontals and verticals overlap only in one location in the grid. At this point, each players has been matched with their own horizontal and vertical. Player 1, for example, need to be matched with the bottom-right 2x2 matrix in the coming two days. We achieve this by using (wrapped) diagonals on day 3:

|         |         |         |
|---------|---------|---------|
| A | B | C |
| B | C | A |
| C | A | B |

On day 4, the final day, we organise diagonals again change their orientation by 90 degrees:

|         |         |         |
|---------|---------|---------|
| A | B | C |
| C | A | B |
| B | C | A |

We can see that each player was matched up exactly once with all others via three-player games. The short-hands for these masks will be -,|,/,\.

### Solution for $n=27$:

The tournament takes 13 days because each player will each day face two of the remaining 26 oppoonents. To find a viable tournament schedule, we picture the telefon-dial from $n=9$ stacked three times. The result is a 3x3x3 cube, which we call 3-cube for short. Our strategy is to first exploit the three spatial orientations to reduce the the cube to a stack of 3x3 dials, 3-squares for short. We already know how to solve 3-squares from the previous section. In a second step, we come up with matches that use the full 3-dimensional structure, meaning that the three players do not share any 3-square. Let's start.

**Scheduling based on 3-squares**
The first 4 days, we apply the masks from above to each of the 3x3 slices: -,|,/,\

The next 3 days, we turn our perspective on the cube by 90 degress to the right in the x-y plane. This gives us 3 novel 3x3 telefone dial pads (with different player numbers). Again, we apply the masks from above. There is a catch, however. The vertical mask is redundant because the columns were not affected by a turn in the x-y plane. The masks used are: -,/,\

The next 2 days, we turn our perspective on the cube by another 90 degrees in the x-z plane. If we turned in the x-y plane we would return to initial perspective modulo a mirror. At this point, all spatial dimensions have been used for match-ups via columns (vertical or horizontals). We use the masks: /,\.

**Scheduling based for the center of the cube**
With 9 days of the unique matches scheduled, we covered each way to decompose the 3-cube into 3-squares. The remaining 4 days, we find match ups that go beyond 3-squares but use locations from all over the 3-cube instead. Each match must contain three players from different 3-squares of the cube, no matter from which perspective we used for slicing.

To have a starting point, we recognise that the central location in the cube is unique. It shares a 3-square with all other locations except for the 8 corners of the cube. The only solution are the 4 cube-diagonals. To see this, take any corner location and central location in the cube. To find the final player in the match, we recognise that the corner location shares slices with 6 out of the 7 remaining corners. Therefore, only the opposite corner location is permissible in the match.

**Scheduling based for corners and faces of the 3-cube**

With the center location out of the way, each corner locations already participates in one match (represented by the cube-diagonal). Each corner location therefore needs another 3 matches. Taking any corner location and removing the 3-squares, which the corner location has taken part in, we obtain a 2-cube (with the original corner location sitting outside of 2-cube). In this 2-cube, we must find 3 matches by choosing two locations within it. The key insight is that the two locations cannot come from any shared 3-square (or 2-square for that matter) because 3-squares were covered in the first 9 days of the tournament. Thus, the only permissible locations lay on the diagonals of the 2-cube. One diagonal was already for scheduling 4 days based on diagonals crossing the central location. This leaves exactly 3 diagonals in the 2-cube which we can use for scheduling.

At this point we are almost done. To see why, let us show that matches based on the 3 diagonals in the 2-cubes cover matches for all remaining players. Which players remain? The ones that are either represented by a face of the 3-cube (like the number 5 in phone dial) and the edges of the 3-cube (for example players 2, 4, 6 and 8). There are 6 faces and 12 edges.

The 3 diagonals of a 2-cube always match one face and one edge. We can visualise this by sitting on any corner of the 3-cube and mentally constructing a line from one of three edges connected to that corner to the opposite face connected to the same corner. Since a face of the 3-cube is connected to 4 unique corners, it will automatically be part of 4 days of unique matches.

**Scheduling based on the remaining edges**

Each of the 12 edges, however, is only connected to 2 corners. This supplies it with 2 days of matches but another 2 days must be scheduled. Since all corners, faces and the center location are already fully accounted for, two remaining match ups per edge must be found among other edges.

Taking any edge, we reminder ourselves with which of the other 11 edges it has already been paired: all edges on the same slice (e.g. for player 2, it was paired with 4, 6 and 8 already) for all possible slices. Thus there are only 4 edges permissible which are indeed not easy to visualise. I like to think of them as the two diagonals on the two 2-squares which which do not share any slice with the original edge location. Since we can only take one location per 2-square with the original edge, there are exactly 2 match-ups. Furthermore, each edge will participate twice in such 2-square. Thus all edges can be matched up within 2 days of the tournament.

There seems to be a strong analogy between finding the three player match-ups and computing the determined of a 3-dimensional tensor. This might be the topic of another blog post. We close out by printing the tournament schedule and a discussion.

## Discussion

The successful examples for $n=9$ and $n=27$ suggest that a three-player Round-Rubin Tournament works for any groups size that is a power of three. The construction of a solution for $n=27$ was tedious. Thus a different approach is needed to explore general powers of $3$. An interesting theoretical question is if a solution for general $k$-player Round-Rubin Tournaments exists. Although $k < 6$ is most interesting for organisations.

The constraint that group sizes should be either $n=9$ or $n=27$ seems overly restrictive. What if a group has $n=23$? For the original application of running team checkins, a pragmatic solution is introducing four virtual players for a total of $n=27$. Depending on the application, one needs to find rules for the cases that $1$ or $2$ virtual players are paired with real players, e.g. merging these smaller groups at the expense of including repeats among the match-ups.
