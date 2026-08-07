# DSA Graphs - Set 1

📋 Topics Included: 3

1. Graph Representation (Adjacency List)
2. Breadth First Search (BFS)
3. Depth First Search (DFS)

---

# 1. Graph Representation (Adjacency List)

## Description

A graph can be represented using an adjacency list, where each vertex stores a list of its neighboring vertices.

Example Graph:

```text
0 ---- 1
|      |
|      |
2 ---- 3
```

Adjacency List Representation:

```text
0 -> 1, 2
1 -> 0, 3
2 -> 0, 3
3 -> 1, 2
```

## C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n = 4;

    vector<vector<int>> adj(n);

    adj[0].push_back(1);
    adj[1].push_back(0);

    adj[0].push_back(2);
    adj[2].push_back(0);

    adj[1].push_back(3);
    adj[3].push_back(1);

    adj[2].push_back(3);
    adj[3].push_back(2);

    for (int i = 0; i < n; i++) {
        cout << i << " -> ";

        for (int neighbor : adj[i])
            cout << neighbor << " ";

        cout << endl;
    }

    return 0;
}
```

### Time Complexity

O(V + E)

### Space Complexity

O(V + E)
