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

The answer of course is to form breakout groups of size $k < n$. Like $k=3$. We tried. People were excited. 
The only issue was that some colleagues were never part of the same group. Others were frequently lumbed together. 

We want maximal exposure and diversity. How to maximise the number of iterations until the two people meet again?

## The solution for $k=2$

or a pairing 

|         |         |         |         |
|---------|---------|---------|---------|
| (2,1)   |         |         |         |
| (3,1)   | (3,2)   |         |         |
| (...,1) | (...,2) | (...,...)|         |
| ($n$,1) | ($n$,2) | ($n$,...)| ($n$,$n-1$)|
