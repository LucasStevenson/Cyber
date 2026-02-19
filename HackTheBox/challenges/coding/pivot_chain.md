# Pivot Chain

> Difficulty: Easy

## Description

After successfully leveraging a weak PIN system to access a critical internal server, you've infiltrated CygnusCorp's network.  
Your next objective is clear: pivot laterally toward the Core Administration Server, CygnusCorp’s most sensitive asset. 

The network is heavily monitored by advanced detection systems.
Each pivot path between hosts carries a certain detection risk, based on factors such as traffic visibility, authentication requirements, and known blue team monitoring points.

Your task is to carefully plan your lateral movement to avoid detection.
Use the network map to identify the safest path — the sequence of pivots with the lowest cumulative detection risk — to reach the Core Administration Server from your current compromised host.

Input Format:
* two integers N (number of hosts) and M (number of paths)
* The starting compromised host and the target Core Administration Server.
* A list of M pivot paths between hosts (unidirectional), each with an associated detection risk.

Output Format:
* Print a single integer representing the lowest cumulative detection risk to travel from the starting host to the Core Administration Server. 

5 <= N <= 150000

6 <= M <= 10^6

```
Example Input:
5 6 host_1 host_5
host_1 host_2 7
host_2 host_3 6
host_3 host_4 6
host_4 host_5 7
host_5 host_3 11
host_1 host_4 20

Expected output:
26

Output Explanation:
There are 5 hosts and 6 paths between them.
The starting compromised host is host_1 and the target Core Administration Server is host_5.
The optimal route is the following:
(origin -> destination (risk, total cumulative risk))
host_1 -> host_2 (7, total=7)
host_2 -> host_3 (6, total=13)
host_3 -> host_4 (6, total=19)
host_4 -> host_5 (7, total=26)
```

## Solution

This is clearly a shortest-path graph problem.

At first I thought a simple BFS should be good enough for this, but I later realized I didn't read the problem constraints closely enough. BFS doesn't work because the graph can get pretty big and our code needs to run in a timely manner.

This was my BFS implementation

```py
# BFS IMPLEMENTATION
# DOES NOT WORK FOR THIS CHALLENGE
# GRAPH GETS TOO BIG
from collections import defaultdict

n, m, src, dst = input().split()

G = defaultdict(list) # { host_n: (host_m, cost) }

for _ in range(int(m)):
    a, b, cost_str = input().split()
    G[a].append((b, int(cost_str)))

Q = [(src,0)]
ans = float("inf")
while Q:
    host, cost_so_far = Q.pop()
    for child_host, cost in G[host]:
        updated_cost = cost_so_far + cost
        if child_host == dst:
            ans = min(updated_cost, ans)
        else:
            Q.append((child_host, updated_cost))

print(ans)
```

Let's use dijkstra's algorithm to speed things up since (due to the priority queue) it always processes nodes in order of increasing distance from the source. This means that once we hit the desired destination, we know it's the minimum distance/cost and we can stop

```py
from collections import defaultdict
import heapq, sys 

n, m, src, dst = sys.stdin.readline().strip().split()

G = defaultdict(list) # { host_n: (host_m, cost) }

for _ in range(int(m)):
    a, b, cost_str = sys.stdin.readline().strip().split()
    G[a].append((b, int(cost_str)))

PQ = [(0,src)]
g_costs = {src: 0}

while PQ:
    cost_so_far, host = heapq.heappop(PQ)
    if host == dst:
        print(cost_so_far)
        break
    if cost_so_far > g_costs.get(host, float("inf")):
        continue
    for child_host, cost in G[host]:
        new_cost = cost_so_far + cost
        if child_host not in g_costs or (new_cost < g_costs[child_host]):
            g_costs[child_host] = new_cost
            heapq.heappush(PQ, (new_cost, child_host))
```

> My solution originally wasnt passing all the tests ("Time Limit Exceeded") **SOLELY** because I used `input()` instead of `sys.stdin.readline()` to read in the input......
>
> On the bright side, at least I learned that `input()` is much slower than using `sys`. I didn't know that until today

## Flag

`HTB{st34lthy_p1v0t1ng_compl3t3}`
