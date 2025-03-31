# **Disjoint Set Union**
- *Disjoint Set Union (DSU), also known as Union-Find, is a data structure that efficiently handles dynamic connectivity queries in `O(α(n))` time using path compression and union by rank, where `α(n)` is the inverse Ackermann function*

# **DSU Functions**
|               **Function**               |                                       **Description**                                       |
|------------------------------------------|---------------------------------------------------------------------------------------------|
|`find(u)` | *Returns the representative leader of the set that contains the element `u` ([**Path Compression Optimization**](https://cp-algorithms.com/data_structures/disjoint_set_union.html#path-compression-optimization))* |
| `merge(u, v, w)` | *Merges the sets containing elements `u` and `v`, considering the edge weight `w`* |
| `same_group(u, v)` | *Determines whether elements `u` and `v` belong to the same connected component* |
| `is_leader(u)` | *Checks whether `u` is the leader (representative) of its set, meaning it is the root element of its component* |
| `node_count(u)` | *Returns the total number of nodes in the connected component containing `u`* |
| `edge_count(u)` | *Returns the total number of edges in the connected component containing `u`* |
| `min_node(u)` | *Returns the smallest node in the connected component containing `u`* |
| `max_node(u)` | *Returns the largest node in the connected component containing `u`* |
| `total_components()` | *Returns the total number of disjoint sets (connected components) currently present in the data structure* |
| `min_weight(u)` | *Retrieves the smallest edge weight among all edges within the set containing `u`* |
| `max_weight(u)` | *Retrieves the largest edge weight among all edges within the set containing `u`* |

# **DSU Implementation**
```cpp
template <typename T> struct DSU {

    private: int components;
    private: vector<T> parent, vertices, edges, minimums, maximums, minimum_weight, maximum_weight;

    public:
    DSU(int n) {
        components = n;
        parent = vertices = edges = minimums = maximums = minimum_weight = maximum_weight = vector<T>(n + 1);
        iota(parent.begin() + 1, parent.end(), 1);
        iota(minimums.begin() + 1, minimums.end(), 1);
        iota(maximums.begin() + 1, maximums.end(), 1);
        fill(vertices.begin() + 1, vertices.end(), 1);
        fill(edges.begin() + 1, edges.end(), 0);
        fill(minimum_weight.begin() + 1, minimum_weight.end(), numeric_limits<T>::max());
        fill(maximum_weight.begin() + 1, maximum_weight.end(), numeric_limits<T>::min());
    }

    T find(T u) {
        return parent[u] == u ? u : parent[u] = find(parent[u]);
    }

    void merge(T u, T v, T w = static_cast<T>(0)) {
        u = find(u);
        v = find(v);
        if(vertices[u] < vertices[v]) {
            swap(u, v);
        }
        if(u == v) {
            edges[u]++;
        } else {
            components--;
            parent[v] = u;
            vertices[u] += vertices[v];
            edges[u] += edges[v] + 1;
            minimums[u] = min(minimums[u], minimums[v]);
            maximums[u] = max(maximums[u], maximums[v]);
        }
        minimum_weight[u] = min({minimum_weight[u], minimum_weight[v], w});
        maximum_weight[u] = max({maximum_weight[u], maximum_weight[v], w});
    }

    bool same_group(T u, T v) {
        return find(u) == find(v);
    }

    bool is_leader(T u) {
        return parent[u] == u;
    }

    int node_count(T u) {
        return vertices[find(u)];
    }

    int edge_count(T u) {
        return edges[find(u)];
    }

    T min_node(T u) {
        return minimums[find(u)];
    }

    T max_node(T u) {
        return maximums[find(u)];
    }

    int total_components() {
        return components;
    }

    T min_weight(T u) {
        return minimum_weight[find(u)];
    }

    T max_weight(T u) {
        return maximum_weight[find(u)];
    }

};
```

# **DSU Problems**
| **ID** |             **Problem Name**             |             **Platform**             |             **Description**             |
|--------|------------------------------------------|--------------------------------------|-----------------------------------------|
| **01** | [**1971. Find if Path Exists in Graph**](https://leetcode.com/problems/find-if-path-exists-in-graph) | <img src="https://leetcode.com/favicon.ico" width="20" height="20"> | *`same_group(u, v)`* |
| **02** | [**D1. Mocha and Diana (Easy Version)**](https://codeforces.com/contest/1559/problem/D1) | <img src="https://codeforces.org/s/0/favicon-96x96.png" width="20" height="20"> | *`same_group(u)`* |
| **03** | [**D. Cow and Snacks**](https://codeforces.com/contest/1209/problem/D) | <img src="https://codeforces.org/s/0/favicon-96x96.png" width="20" height="20"> | *`same_group(u)`* |
| **04** | [**685. Redundant Connection II**](https://leetcode.com/problems/redundant-connection-ii) | <img src="https://leetcode.com/favicon.ico" width="20" height="20"> | *`same_group(u, v)`* |
| **05** | [**B2. Books Exchange (hard version)**](https://codeforces.com/contest/1249/problem/B2) | <img src="https://codeforces.org/s/0/favicon-96x96.png" width="20" height="20"> | *`node_count(u)`* |
| **06** | [**2316. Count Unreachable Pairs of Nodes in an Undirected Graph**](https://leetcode.com/problems/count-unreachable-pairs-of-nodes-in-an-undirected-graph) | <img src="https://leetcode.com/favicon.ico" width="20" height="20"> | *`node_count(u)`* |
| **07** | [**D - Unicyclic Components**](https://atcoder.jp/contests/abc292/tasks/abc292_d) | <img src="https://i.namu.wiki/i/oloBJdRd29lBIF-mdv1FjWucpE3tGPhudDBTvOBChAT3A5w9zDUYg51mvn6NNOwoHJZIwxkVyzeXQMhtLAcQOQ.webp" width="20" height="20"> | *`is_leader(u), node_count(u), edge_count(u)`* |
| **08** | [**2685. Count the Number of Complete Components**](https://leetcode.com/problems/count-the-number-of-complete-components) | <img src="https://leetcode.com/favicon.ico" width="20" height="20"> | *This problem is based on the concept of checking whether a component is a complete graph, see [**Complete Graph**](https://en.wikipedia.org/wiki/Complete_graph)* `is_leader(u), node_count(u), edge_count(u)` |
| **09** | [**B. Disjoint Sets Union 2**](https://codeforces.com/edu/course/2/lesson/7/1/practice/contest/289390/problem/B) | <img src="https://codeforces.org/s/0/favicon-96x96.png" width="20" height="20"> | *`min_node(u), max_node(u), node_count(u)`* |
| **10** | [**547. Number of Provinces**](https://leetcode.com/problems/number-of-provinces) | <img src="https://leetcode.com/favicon.ico" width="20" height="20"> | *`total_components()`* |
| **11** | [**1319. Number of Operations to Make Network Connected**](https://leetcode.com/problems/number-of-operations-to-make-network-connected) | <img src="https://leetcode.com/favicon.ico" width="20" height="20"> | *`total_components()`* |
| **12** | [**3493. Properties Graph**](https://leetcode.com/problems/properties-graph) | <img src="https://leetcode.com/favicon.ico" width="20" height="20"> | *`total_components()`* |
| **13** | [**2492. Minimum Score of a Path Between Two Cities**](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities) | <img src="https://leetcode.com/favicon.ico" width="20" height="20"> | *`min_weight(u)`* |
| **14** | [**D. Vessels**](https://codeforces.com/contest/371/problem/D) | <img src="https://codeforces.org/s/0/favicon-96x96.png" width="20" height="20"> | *`custom_find()`* |