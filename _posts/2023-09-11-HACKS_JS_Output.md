---
toc: false
comments: true
layout: post
title: JS Output Hack
type: hacks
courses: { compsci: {week: 3} }
---

## Sample
<!-- Head contains information to Support the Document -->
<head>
    <!-- load jQuery and DataTables output style and scripts -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.4/css/jquery.dataTables.min.css">
    <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>var define = null;</script>
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.13.4/js/jquery.dataTables.min.js"></script>
</head>

<!-- Body contains the contents of the Document -->
<body>
    <table id="xdemo" class="table">
        <thead>
            <tr>
                <th>Make</th>
                <th>Model</th>
                <th>Year</th>
                <th>Color</th>
                <th>Price</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Ford</td>
                <td>Mustang</td>
                <td>2022</td>
                <td>Red</td>
                <td>$35,000</td>
            </tr>
            <tr>
                <td>Toyota</td>
                <td>Camry</td>
                <td>2022</td>
                <td>Silver</td>
                <td>$25,000</td>
            </tr>
            <tr>
                <td>Tesla</td>
                <td>Model S</td>
                <td>2022</td>
                <td>White</td>
                <td>$80,000</td>
            </tr>
            <tr>
                <td>Cadillac</td>
                <td>Broughan</td>
                <td>1969</td>
                <td>Black</td>
                <td>$10,000</td>
            </tr>
            <tr>
                <td>Ford</td>
                <td>F-350</td>
                <td>1997</td>
                <td>Green</td>
                <td>$15,000</td>
            </tr>
            <tr>
                <td>Ford</td>
                <td>Excursion</td>
                <td>2003</td>
                <td>Green</td>
                <td>$25,000</td>
            </tr>
            <tr>
                <td>Ford</td>
                <td>Ranger</td>
                <td>2012</td>
                <td>Red</td>
                <td>$8,000</td>
            </tr>
            <tr>
                <td>Kuboto</td>
                <td>L3301 Tractor</td>
                <td>2015</td>
                <td>Orange</td>
                <td>$12,000</td>
            </tr>
            <tr>
                <td>Ford</td>
                <td>Fusion Energi</td>
                <td>2015</td>
                <td>Guard</td>
                <td>$25,000</td>
            </tr>
            <tr>
                <td>Acura</td>
                <td>XL</td>
                <td>2006</td>
                <td>Grey</td>
                <td>$10,000</td>
            </tr>
            <tr>
                <td>Ford</td>
                <td>F150 Lightning</td>
                <td>2024</td>
                <td>Guard</td>
                <td>$70,000</td>
            </tr>
        </tbody>
    </table>
</body>

<!-- Script is used to embed executable code -->
<script>
    $("#xdemo").DataTable();
</script>

## Algorithms
<html lang="en">
<head>
    <!-- Load jQuery and DataTables output style and scripts -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.4/css/jquery.dataTables.min.css">
    <script type="text/javascript" language="javascript" src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script type="text/javascript" language="javascript" src="https://cdn.datatables.net/1.13.4/js/jquery.dataTables.min.js"></script>
</head>
<body>
    <table id="algorithm-table" class="table">
        <thead>
            <tr>
                <th>Algorithm</th>
                <th>Description</th>
                <th>Sample C++ Code</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Bubble Sort</td>
                <td>A simple sorting algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order.</td>
                <td>
                    <pre>
void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
            }
        }
    }
}
                    </pre>
                </td>
            </tr>
            <tr>
                <td>Quick Sort</td>
                <td>An efficient sorting algorithm that uses a divide-and-conquer strategy to sort an array.</td>
                <td>
                    <pre>
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pivot = partition(arr, low, high);
        quickSort(arr, low, pivot - 1);
        quickSort(arr, pivot + 1, high);
    }
}

int partition(int arr[], int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}
                    </pre>
                </td>
            </tr>
            <tr>
                <td>Breadth-First Search (BFS)</td>
                <td>A graph traversal algorithm that explores all the vertices of a graph in breadth-first order.</td>
                <td>
                    <pre>
void bfs(int start, vector&lt;int&gt; adj[], vector&lt;bool&gt; &amp;visited) {
    queue&lt;int&gt; q;
    visited[start] = true;
    q.push(start);
    
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        cout &lt;&lt; "Visiting node " &lt;&lt; u &lt;&lt; endl;
        
        for (int v : adj[u]) {
            if (!visited[v]) {
                visited[v] = true;
                q.push(v);
            }
        }
    }
}
                    </pre>
                </td>
            </tr>
            <tr>
                <td>Disjoint Set Union (DSU)</td>
                <td>A data structure and algorithm for efficiently maintaining and querying disjoint sets.</td>
                <td>
                    <pre>
int parent[MAX];
int setSize[MAX];

// DSU initialization
void makeSet(int v) {
    parent[v] = v;
    setSize[v] = 1;
}

// DSU find operation with path compression
int findSet(int v) {
    if (v == parent[v])
        return v;
    return parent[v] = findSet(parent[v]);
}

// DSU union operation by size
void unionSets(int a, int b) {
    a = findSet(a);
    b = findSet(b);
    if (a != b) {
        if (setSize[a] < setSize[b])
            swap(a, b);
        parent[b] = a;
        setSize[a] += setSize[b];
    }
}
                    </pre>
                </td>
            </tr>
            <tr>
                <td>Dijkstra's Algorithm</td>
                <td>An algorithm for finding the shortest path between nodes in a graph with non-negative edge weights.</td>
                <td>
                    <pre>
vector&lt;pair&lt;int, int&gt;&gt; adj[MAX]; // {neighbor, weight}

vector&lt;int&gt; dijkstra(int start, int n) {
    vector&lt;int&gt; dist(n, INT_MAX);
    dist[start] = 0;
    priority_queue&lt;pair&lt;int, int&gt;, vector&lt;pair&lt;int, int&gt;&gt;, greater&lt;pair&lt;int, int&gt;&gt;&gt; pq;
    pq.push({0, start});
    
    while (!pq.empty()) {
        int u = pq.top().second;
        int w = pq.top().first;
        pq.pop();
        
        if (w > dist[u])
            continue;
        
        for (auto neighbor : adj[u]) {
            int v = neighbor.first;
            int weight = neighbor.second;
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }
    
    return dist;
}
                    </pre>
                </td>
            </tr>
            <tr>
                <td>Binary Lifting</td>
                <td>An algorithm for efficiently calculating LCA (Lowest Common Ancestor) and other tree-related queries.</td>
                <td>
                    <pre>
const int MAX = 10005;
const int LOG2MAX = 20;

int parent[MAX][LOG2MAX]; // parent[i][j] stores the 2^j-th ancestor of node i
int depth[MAX]; // depth[i] stores the depth of node i in the tree

// Binary Lifting preprocessing
void binaryLifting(int u, int p, int d) {
    parent[u][0] = p;
    depth[u] = d;
    for (int i = 1; i < LOG2MAX; i++) {
        if (parent[u][i - 1] != -1) {
            parent[u][i] = parent[parent[u][i - 1]][i - 1];
        } else {
            parent[u][i] = -1;
        }
    }
    for (int v : adj[u]) {
        if (v != p) {
            binaryLifting(v, u, d + 1);
        }
    }
}

// Finding LCA using Binary Lifting
int findLCA(int u, int v) {
    if (depth[u] < depth[v]) {
        swap(u, v);
    }
    for (int i = LOG2MAX - 1; i >= 0; i--) {
        if (depth[u] - (1 << i) >= depth[v]) {
            u = parent[u][i];
        }
    }
    if (u == v) {
        return u;
    }
    for (int i = LOG2MAX - 1; i >= 0; i--) {
        if (parent[u][i] != parent[v][i]) {
            u = parent[u][i];
            v = parent[v][i];
        }
    }
    return parent[u][0];
}
                    </pre>
                </td>
            </tr>
            <tr>
                <td>Prefix Sums</td>
                <td>An algorithm for efficiently calculating cumulative sums in an array.</td>
                <td>
                    <pre>
vector&lt;int&gt; calculatePrefixSums(vector&lt;int&gt; &amp;arr) {
    int n = arr.size();
    vector&lt;int&gt; prefixSums(n);
    prefixSums[0] = arr[0];
    
    for (int i = 1; i < n; i++) {
        prefixSums[i] = prefixSums[i - 1] + arr[i];
    }
    
    return prefixSums;
}
                    </pre>
                </td>
            </tr>
            <tr>
                <td>Dynamic Programming (DP)</td>
                <td>A technique for solving problems by breaking them down into smaller subproblems and caching their solutions.</td>
                <td>
                    <pre>
vector&lt;int&gt; memo(MAX, -1);

int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }
    if (memo[n] != -1) {
        return memo[n];
    }
    memo[n] = fibonacci(n - 1) + fibonacci(n - 2);
    return memo[n];
}
                    </pre>
                </td>
            </tr>
            <!-- Add more algorithms and code samples as needed -->
        </tbody>
    </table>

    <script>
        // Initialize DataTable
        $(document).ready(function() {
            $("#algorithm-table").DataTable();
        });
    </script>
</body>
</html>

## Add/Remove Rows
<!DOCTYPE html>
<html>
<head>
    <title>jQuery Table Demo</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
</head>
<body>
    <h1>Table with jQuery</h1>
    <table id="myTable" border="1">
        <thead>
            <tr>
                <th>Name</th>
                <th>Age</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>John</td>
                <td>30</td>
            </tr>
            <tr>
                <td>Alice</td>
                <td>25</td>
            </tr>
        </tbody>
    </table>

    <button id="addRow">Add Row</button>
    <button id="removeRow">Remove Row</button>

    <script>
        // Add a new row to the table
        $("#addRow").click(function() {
            var name = prompt("Enter Name:");
            var age = prompt("Enter Age:");
            if (name && age) {
                var newRow = $("<tr><td>" + name + "</td><td>" + age + "</td></tr>");
                $("#myTable tbody").append(newRow);
            }
        });

        // Remove the last row from the table
        $("#removeRow").click(function() {
            $("#myTable tbody tr:last").remove();
        });
    </script>
</body>
</html>
