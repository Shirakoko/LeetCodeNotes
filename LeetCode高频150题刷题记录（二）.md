# LeetCode高频150题刷题记录（二）

# 【二叉搜索树】

## [530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

给你一个二叉搜索树的根节点 `root` ，返回 **树中任意两不同节点值之间的最小差值** 。

差值是一个正数，其数值等于两值之差的绝对值。

**示例 1：**

```
输入：root = [4,2,6,1,3]
输出：1
```

**示例 2：**

```
输入：root = [1,0,48,null,null,12,49]
输出：1
```

> **思路：二叉搜索树的中序遍历一定是升序数组，因此最小绝对差一定在先后遍历到的两个节点之间。**

```cs
public class Solution {
    private TreeNode preNode = null; // 上一个访问的节点
    private int res = int.MaxValue; // 记录结果

    public int GetMinimumDifference(TreeNode root) {
        InorderTraverse(root);
        return res;    
    }

    // 辅助函数，中序遍历树并更新结果
    private void InorderTraverse(TreeNode root) {
        if(root == null) {
            return;
        }
        InorderTraverse(root.left);
        if(preNode != null) {
            res = Math.Min(res, Math.Abs(root.val - preNode.val));
        }
        preNode = root;
        InorderTraverse(root.right);
    }
}
```

## [230. 二叉搜索树中第 K 小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)

给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k` 小的元素（从 1 开始计数）。

**示例 1：**

```
输入：root = [3,1,4,null,2], k = 1
输出：1
```

**示例 2：**

```
输入：root = [5,3,6,2,4,null,null,1], k = 3
输出：3
```

> **思路：中序遍历并计数；每访问一个节点`k--`，k减为0说明当前访问的节点就是第`k`小的元素。**

```cs
public class Solution {
    public int KthSmallest(TreeNode root, int k) {
        // 用栈来做中序遍历
        Stack<TreeNode> stack = new Stack<TreeNode>();
        // 当前遍历到的节点
        TreeNode curNode = root; 
        // 已访问的节点个数
        int visitedCount = 0;

        while(curNode != null || stack.Count > 0) {
            while(curNode != null) {
                // 左子节点不断入栈，直到为空
                stack.Push(curNode);
                curNode = curNode.left;
            }

            curNode = stack.Pop(); // 弹栈
            k--; // 访问
            if(k == 0) {
                break;
            }
            curNode = curNode.right; // 转向右子树
        }

        return curNode.val;
    }
}
```

## [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例 1：**

```
输入：root = [2,1,3]
输出：true
```

**示例 2：**

```
输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

> **思路：中序遍历二叉树，记录上一个访问过的节点，如果上一个访问过的节点值≥当前访问的节点值，直接判定不有效。**

```csharp
public class Solution {
    private bool res = true;
    TreeNode lastNode = null; // 上个访问过的节点
    public bool IsValidBST(TreeNode root) {
        /** 前序遍历 */
        InorderTraverse(root);
        return res;
    }
    
    private void InorderTraverse(TreeNode root) {
        if(root == null) {
            return;
        }

        InorderTraverse(root.left);
        if(lastNode != null && lastNode.val >= root.val) {
            res = false;
            return;
        }
        lastNode = root;
        InorderTraverse(root.right);
    }
}
```

【图】

## [200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

**示例 1：**

```
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```

**示例 2：**

```
输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```

> **思路：遍历这个二维数组，如果当前数为1，则进入感染函数并将岛个数增加1**
>
> - **感染函数：递归式深度搜索标注，将所有相连的1都标注成2，避免遍历过程中的重复计数的情况。**

```csharp
public class Solution {
    public int NumIslands(char[][] grid) {
        int islandNum = 0;
        for(int i=0; i<grid.Length; i++) {
            for(int j=0; j<grid[0].Length; j++) {
                if(grid[i][j] == '1') {
                    islandNum ++;
                    InFect(grid, i, j);
                }
            }
        }

        return islandNum;
    }

    /** 辅助函数：把所有相连的1标注成2 */
    private void InFect(char[][] grid, int i, int j) {
        if(i<0 || i>=grid.Length || j<0 || j>=grid[0].Length) {
            // 排除超出网格范围的索引
            return;
        }
        if(grid[i][j] != '1') {
            // 不感染不为1的数
            return;
        }
        grid[i][j] = '2';
        // 递归处理四个方向的格子
        InFect(grid, i+1, j);
        InFect(grid, i-1, j);
        InFect(grid, i, j+1);
        InFect(grid, i, j-1);
    }
}
```

## [130. 被围绕的区域](https://leetcode.cn/problems/surrounded-regions/)

给你一个 `m x n` 的矩阵 `board` ，由若干字符 `'X'` 和 `'O'` 组成，**捕获** 所有 **被围绕的区域**：

- **连接：**一个单元格与水平或垂直方向上相邻的单元格连接。
- **区域：连接所有** `'O'` 的单元格来形成一个区域。
- **围绕：**如果您可以用 `'X'` 单元格 **连接这个区域**，并且区域中没有任何单元格位于 `board` 边缘，则该区域被 `'X'` 单元格围绕。

通过 **原地** 将输入矩阵中的所有 `'O'` 替换为 `'X'` 来 **捕获被围绕的区域**。你不需要返回任何值。

**示例 1：**

```
输入：board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

输出：[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
```

**示例 2：**

```
输入：board = [["X"]]

输出：[["X"]]
```

> **思路：找到所有位于边界上的 `'O'`，以及这些 `'O'` 所连接的其他 `'O'`；将它们标记为一个临时字符 `'#'`；在标记完成后，再次遍历整个矩阵，将所有未被标记的 `'O'` 替换为 `'X'`，同时将标记为 `'#'` 的字符恢复为 `'O'`。**

```cs
public class Solution {
    public void Solve(char[][] board) {
        if(board.Length == 0) {
            return;
        }

        int rows = board.Length;
        int cols = board[0].Length;

        // 把所有边上的'O'和与边相连的'O'替换为'#'
        for(int i=0; i<rows; i++) {
            if(board[i][0] == 'O') {
                Infect(board, i, 0);
            }
            if(board[i][cols - 1] == 'O') {
                Infect(board, i, cols - 1);
            }
        }

        for(int j=0; j<cols; j++) {
            if(board[0][j] == 'O') {
                Infect(board, 0, j);
            }
            if(board[rows - 1][j] == 'O') {
                Infect(board, rows - 1, j);
            }
        }

        // 遍历整个棋盘，把所有的'O'替换为'X'，把所有的'#'替换为'O'
        for(int i=0; i<rows; i++) {
            for(int j=0; j<cols; j++) {
                if(board[i][j] == 'O') {
                    board[i][j] = 'X';
                } else if(board[i][j] == '#') {
                    board[i][j] = 'O';
                }
            }
        }
    }

    /** 辅助函数，把所有与'O'相连的'O'替换为'#' */
    private void Infect(char[][] board, int i, int j) {
        if (i < 0 || i >= board.Length || j < 0 || j >= board[0].Length) {
            return;
        }

        if (board[i][j] != 'O') {
            return;
        }

        board[i][j] = '#';
        Infect(board, i - 1, j);
        Infect(board, i + 1, j);
        Infect(board, i, j - 1);
        Infect(board, i, j + 1);
    }
}
```

## [133. 克隆图](https://leetcode.cn/problems/clone-graph/)

给你无向连通图中一个节点的引用，请你返回该图的深拷贝（克隆）。

图中的每个节点都包含它的值 `val`（`int`） 和其邻居的列表（`list[Node]`）。

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

**测试用例格式：**

简单起见，每个节点的值都和它的索引相同。例如，第一个节点值为 1（`val = 1`），第二个节点值为 2（`val = 2`），以此类推。该图在测试用例中使用邻接列表表示。

**邻接列表** 是用于表示有限图的无序列表的集合。每个列表都描述了图中节点的邻居集。

给定节点将始终是图中的第一个节点（值为 1）。你必须将 **给定节点的拷贝** 作为对克隆图的引用返回。

**示例 1：**

```
输入：adjList = [[2,4],[1,3],[2,4],[1,3]]
输出：[[2,4],[1,3],[2,4],[1,3]]
解释：
图中有 4 个节点。
节点 1 的值是 1，它有两个邻居：节点 2 和 4 。
节点 2 的值是 2，它有两个邻居：节点 1 和 3 。
节点 3 的值是 3，它有两个邻居：节点 2 和 4 。
节点 4 的值是 4，它有两个邻居：节点 1 和 3 。
```

**示例 2：**

```
输入：adjList = [[]]
输出：[[]]
解释：输入包含一个空列表。该图仅仅只有一个值为 1 的节点，它没有任何邻居。
```

**示例 3：**

```
输入：adjList = []
输出：[]
解释：这个图是空的，它不含任何节点。
```

> **思路：用一个字典储存原始节点和克隆节点的映射关系，递归式深度优先搜索遍历节点，先创建节点，再连接节点：**
>
> 1. **创建节点：**
>
>    - **当遇到一个未访问过的节点时，创建一个克隆节点。**
>    - **在字典中添加原始节点和克隆节点的映射关系。**
>
> 2. **递归处理邻居：**
>
>    - **对当前节点的每个邻居递归调用 DFS。**
>    - **递归调用会返回邻居节点的克隆节点，然后将这些克隆节点添加到当前克隆节点的 `neighbors` 列表中。**
>
> 3. **返回克隆节点：**
>
>    - **如果节点已经访问过（即字典中已经存在映射），直接返回字典中存储的克隆节点。**
>
>      

```csharp
public class Solution
{
    // 记录原始节点和克隆节点之间的映射关系
    private Dictionary<Node, Node> visited = new Dictionary<Node, Node>();
    public Node CloneGraph(Node node)
    {
        if(node == null)
        {
            return null;
        }

        return DFS(node);
    }
   
    /** 辅助函数，DFS遍历图的每个节点并克隆，返回克隆后的节点 */
    private Node DFS(Node node)
    {
        if(node == null)
        {
            return null;
        }

        // 如果节点已经访问过，直接返回它的克隆节点
        if (visited.ContainsKey(node))
        {
            return visited[node];
        }
		
        // 否则创建一个克隆节点并将原始节点和克隆节点的映射关系加入字典
        Node cloneNode = new Node(node.val);
        visited.Add(node, cloneNode);
		
        // 处理克隆节点的邻居的连接关系
        foreach(var neighbor in node.neighbors)
        {
            cloneNode.neighbors.Add(DFS(neighbor));
        }

        return cloneNode;
    }
}
```

> **思路二：用一个字典储存原始节点和克隆节点的映射关系，用队列辅助迭代式广度优先搜索遍历节点。**

```cs
public class Solution
{
    // 记录原始节点和克隆节点之间的映射关系
    private Dictionary<Node, Node> visited = new Dictionary<Node, Node>();
    // 辅助队列，用于广度优先搜索
    Queue<Node> queue = new Queue<Node>();
    public Node CloneGraph(Node node)
    {
        if(node == null)
        {
            return null;
        }

        Node cloneNode = new Node(node.val); // 创建起始节点的克隆节点
        visited.Add(node, cloneNode); // 指定起始节点和克隆节点的映射关系
        queue.Enqueue(node); // 起始节点入队

        while(queue.Count > 0)
        {
            Node curNode = queue.Dequeue();
            Node cloneCurNode = visited[curNode];

            foreach(var neighbor in curNode.neighbors)
            {
                if (visited.ContainsKey(neighbor) == false)
                {
                    // 如果邻居节点没有被克隆过，需要克隆
                    Node cloneNeighbor = new Node(neighbor.val);
                    visited.Add(neighbor, cloneNeighbor);
                    // 邻居节点入队
                    queue.Enqueue(neighbor);
                }

                // 建立克隆节点的邻居的连接关系
                cloneCurNode.neighbors.Add(visited[neighbor]);
            }
        }

        return cloneNode;
    }
}
```

## [399. 除法求值](https://leetcode.cn/problems/evaluate-division/)

给你一个变量对数组 `equations` 和一个实数值数组 `values` 作为已知条件，其中 `equations[i] = [Ai, Bi]` 和 `values[i]` 共同表示等式 `Ai / Bi = values[i]` 。每个 `Ai` 或 `Bi` 是一个表示单个变量的字符串。

另有一些以数组 `queries` 表示的问题，其中 `queries[j] = [Cj, Dj]` 表示第 `j` 个问题，请你根据已知条件找出 `Cj / Dj = ?` 的结果作为答案。

返回 **所有问题的答案** 。如果存在某个无法确定的答案，则用 `-1.0` 替代这个答案。如果问题中出现了给定的已知条件中没有出现的字符串，也需要用 `-1.0` 替代这个答案。

**注意：**输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。

**注意：**未在等式列表中出现的变量是未定义的，因此无法确定它们的答案。

**示例 1：**

```
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
条件：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
结果：[6.0, 0.5, -1.0, 1.0, -1.0 ]
注意：x 是未定义的 => -1.0
```

**示例 2：**

```
输入：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
输出：[3.75000,0.40000,5.00000,0.20000]
```

**示例 3：**

```
输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
输出：[0.50000,2.00000,-1.00000,-1.00000]
```

> **思路：弗洛伊德算法。**
>
> 1. **构建图：**
>    - **将每个变量作为图的节点。**
>    - **根据 `equations` 和 `values` 构建图的边。例如，`a / b = 2.0` 表示从 `a` 到 `b` 的边权重为 `2.0`，同时从 `b` 到 `a` 的边权重为 `1 / 2.0`。**
> 2. **初始化距离矩阵：**
>    - **使用一个二维字典 `dist` 来存储任意两个节点之间的距离（即比值）。**
>    - **初始化 `dist[i][i] = 1.0`，表示每个节点到自己的距离为 `1.0`。**
>    - **将已知的边权重赋值到 `dist` 中。**
> 3. **弗洛伊德算法：**
>    - **通过三重循环遍历所有节点对 `(i, j)` 和中间节点 `k`，更新 `dist[i][j]`。**
>    - **如果存在路径 `i -> k` 和 `k -> j`，则更新 `dist[i][j]` 为 `dist[i][k] * dist[k][j]`。**
> 4. **处理查询：**
>    - **对于每个查询 `(start, end)`，检查 `dist[start][end]` 是否存在。**
>    - **如果存在，返回 `dist[start][end]`；否则返回 `-1.0`。**

```csharp
public class Solution
{
    public double[] CalcEquation(IList<IList<string>> equations, double[] values, IList<IList<string>> queries)
    {
        // Step1：根据equations和values构建图
        // graph[start][end]表示从节点start到节点end的边代价
        var graph = new Dictionary<string, Dictionary<string, double>>();
        for (int i = 0; i < equations.Count; i++)
        {
            string start = equations[i][0]; // 起点
            string end = equations[i][1]; // 终点
            double distance = values[i]; // start到end的边代价

            // 构建start->end的边，代价为distance
            if (graph.ContainsKey(start) == false)
            {
                graph.Add(start, new Dictionary<string, double>());
            }
            graph[start][end] = distance;

            // 构建end->start的边，代价为1 / distance
            if (graph.ContainsKey(end) == false)
            {
                graph.Add(end, new Dictionary<string, double>());
            }
            graph[end][start] = 1 / distance;
        }

        // Step2：初始化代价矩阵
        // dist[A][A]始终为1.0，再根据graph中的数据将已知的边代价赋值到dist中
        var dist = new Dictionary<string, Dictionary<string, double>>();
        foreach (var point in graph.Keys)
        {
            // 图中的每个点都需要一个字典存储它到相邻节点的边权重
            dist[point] = new Dictionary<string, double>();
            // 初始化自己到自己的代价为1.0
            dist[point][point] = 1.0;

            // 将point到邻居点的边代价赋值到dist中
            foreach (var neighbor in graph[point].Keys)
            {
                dist[point][neighbor] = graph[point][neighbor];
            }
        }

        // Step3：应用弗洛伊德算法收敛距离矩阵dist
        // 通过三层循环遍历所有的节点对(i, j)和中间节点k，更新dist[i][j]
        foreach (var k in graph.Keys)
        {
            foreach (var i in graph.Keys)
            {
                foreach (var j in graph.Keys)
                {
                    // 如果存在路径i->k且存在路径k->j，根据dist[i][k]+dist[k][j]是否＜dist[i][j]来更新dist[i][j]
                    if (dist[i].ContainsKey(k) && dist[k].ContainsKey(j))
                    {
                        double newDistance = dist[i][k] * dist[k][j];
                        // 如果i->j本不连通，通过k连通了，需要更新i->j的代价
                        if (dist[i].ContainsKey(j) == false)
                        {
                            dist[i][j] = newDistance;
                        }
                    }
                }
            }
        }

        // Step4：查询
        var result = new double[queries.Count];
        for (int i = 0; i < queries.Count; i++)
        {
            string start = queries[i][0];
            string end = queries[i][1];
            if (dist.ContainsKey(start) == false || dist[start].ContainsKey(end) == false)
            {
                // 如果 dist[start][end] 不存在，返回 -1.0
                result[i] = -1.0;
            }
            else
            {
                result[i] = dist[start][end];
            }
        }

        return result;
    }
}
```

## [207. 课程表](https://leetcode.cn/problems/course-schedule/)

你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 **必须** 先学习课程 `bi` 。

- 例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。

请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。
```

**示例 2：**

```
输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
输出：false
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。
```

> **思路：拓扑排序判断图中是否有环。**
>
> 1. **构建图：**
>    - **使用 `List<HashSet<int>>` 表示图的邻接表，`graph[i]` 存储节点 `i` 的所有邻居。**
>    - **使用 `int[] inDegree` 记录每个节点的入度（即有多少节点指向它）。**
> 2. **初始化图：**
>    - **为每个课程初始化一个空的邻居列表。**
> 3. **填充图和入度数组：**
>    - **遍历 `prerequisites`，将依赖关系添加到图中，并更新目标课程的入度。**
> 4. **拓扑排序（BFS）：**
>    - **将所有入度为 0 的节点加入队列。**
>    - **依次从队列中取出节点，将其邻居的入度减 1。如果邻居的入度变为 0，将其加入队列。**
>    - **记录已处理的节点数量 `count`。**
> 5. **判断结果：**
>    - **如果 `count == numCourses`，说明所有节点都被成功处理，图中没有环，可以完成所有课程。**
>    - **否则，说明图中存在环，无法完成所有课程。**

```csharp
public class Solution
{
    public bool CanFinish(int numCourses, int[][] prerequisites)
    {
        // graph是一个有向无权图，graph[p] 表示点 p 的邻居列表
        List<HashSet<int>> graph = new List<HashSet<int>>(numCourses);
        // inDegree记录每个节点的入度，int[i]表示有多少个节点指向节点i
        int[] inDegree = new int[numCourses];

        // 初始化图，构建每个节点的邻居列表
        for(int i=0; i<numCourses; i++)
        {
            graph.Add(new HashSet<int>());
        }

        // Step1：根据 prerequisites 构建 graph
        for(int i=0; i<prerequisites.Length; i++)
        {
            int preCourse = prerequisites[i][1]; // 先修课
            int course = prerequisites[i][0]; // 课程

            if(graph[preCourse].Add(course))
            {
                inDegree[course]++; // course节点的入度增加1
            }
        }

        // Step2：使用广度优先搜索进行拓扑排序判断图中是否有环
        // 用count记录不构成环的节点的个数，如果count＜numCourses，返回false
        // 用于广度优先搜索的辅助队列
        Queue<int> queue = new Queue<int>();

        // 将入度为0的点加入队列
        for(int i=0; i<numCourses; i++)
        {
            if (inDegree[i] == 0)
            {
                queue.Enqueue(i);
            }
        }

        // 记录已经完成拓扑排序的节点的个数
        int count = 0;

        // 广度遍历进行拓扑排序
        while(queue.Count > 0)
        {
            int current = queue.Dequeue(); // 当前节点已处理
            count++;

            foreach (var neighbor in graph[current])
            {
                inDegree[neighbor]--; // 邻居的入度减少1

                if (inDegree[neighbor] == 0)
                {
                    // 如果邻居的入度减为0，要加入队列
                    queue.Enqueue(neighbor);
                }
            }
        }

        return count >= numCourses;
    }
}
```

## [210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/)

现在你总共有 `numCourses` 门课需要选，记为 `0` 到 `numCourses - 1`。给你一个数组 `prerequisites` ，其中 `prerequisites[i] = [ai, bi]` ，表示在选修课程 `ai` 前 **必须** 先选修 `bi` 。

- 例如，想要学习课程 `0` ，你需要先完成课程 `1` ，我们用一个匹配来表示：`[0,1]` 。

返回你为了学完所有课程所安排的学习顺序。可能会有多个正确的顺序，你只要返回 **任意一种** 就可以了。如果不可能完成所有课程，返回 **一个空数组** 。

**示例 1：**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：[0,1]
解释：总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 [0,1] 。
```

**示例 2：**

```
输入：numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
输出：[0,2,1,3]
解释：总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。
因此，一个正确的课程顺序是 [0,1,2,3] 。另一个正确的排序是 [0,2,1,3] 。
```

**示例 3：**

```
输入：numCourses = 1, prerequisites = []
输出：[0]
```

> **思路：拓扑排序，将排序结果加入列表，最后判断列表个数是否为节点个数。**
>
> 1. **问题转化：将课程和先修关系转化为有向无权图，课程是节点，先修关系是边。**
> 2. **拓扑排序：**
>    - **统计每个节点的入度（依赖的课程数）。**
>    - **将所有入度为 0 的节点加入队列。**
>    - **使用广度优先搜索遍历图，依次将入度为 0 的节点加入结果列表，并更新邻接节点的入度。**
> 3. **结果验证：如果结果列表的长度等于课程总数，返回结果；否则返回空数组。**

```csharp
public class Solution
{
    public int[] FindOrder(int numCourses, int[][] prerequisites)
    {
        List<List<int>> graph = new List<List<int>>();
        int[] InDegree = new int[numCourses];

        for(int i=0; i<numCourses; i++)
        {
            graph.Add(new List<int>());
        }

        // Step1：根据prerequisites构建graph
        for (int i=0; i<prerequisites.Length; i++)
        {
            int preCourse = prerequisites[i][1];
            int course = prerequisites[i][0];

            graph[preCourse].Add(course);
            InDegree[course]++;
        }

        // Step2：使用广度优先搜索拓扑排序
        Queue<int> queue = new Queue<int>();
        // 将入度为0的点加入队列
        for(int i=0; i<numCourses; i++)
        {
            if (InDegree[i] == 0)
            {
                queue.Enqueue(i);
            }
        }
        
        // 拓扑排序结果列表，记录已经完成排序的点
        List<int> result = new List<int>();

        while(queue.Count > 0)
        {
            int current = queue.Dequeue();
            result.Add(current);

            foreach(var neighbor in graph[current])
            {
                InDegree[neighbor]--;
                if (InDegree[neighbor] == 0)
                {
                    queue.Enqueue(neighbor);
                }
            }
        }

        if(result.Count < numCourses)
        {
            // 说明还有点未完成排序
            return [];
        }

        return result.ToArray();
    }
}
```

# 【图的广度优先搜索】

## [909. 蛇梯棋](https://leetcode.cn/problems/snakes-and-ladders/)

给你一个大小为 `n x n` 的整数矩阵 `board` ，方格按从 `1` 到 `n2` 编号，编号遵循 转行交替方式，**从左下角开始** （即，从 `board[n - 1][0]` 开始）的每一行改变方向。

你一开始位于棋盘上的方格 `1`。每一回合，玩家需要从当前方格 `curr` 开始出发，按下述要求前进：

- 选定目标方格`next`，目标方格的编号在范围`[curr + 1, min(curr + 6, n2)]`。
  - 该选择模拟了掷 **六面体骰子** 的情景，无论棋盘大小如何，玩家最多只能有 6 个目的地。
- 传送玩家：如果目标方格 `next` 处存在蛇或梯子，那么玩家会传送到蛇或梯子的目的地。否则，玩家传送到目标方格 `next` 。 
- 当玩家到达编号 `n2` 的方格时，游戏结束。

如果 `board[r][c] != -1` ，位于 `r` 行 `c` 列的棋盘格中可能存在 “蛇” 或 “梯子”。那个蛇或梯子的目的地将会是 `board[r][c]`。编号为 `1` 和 `n2` 的方格不是任何蛇或梯子的起点。

注意，玩家在每次掷骰的前进过程中最多只能爬过蛇或梯子一次：就算目的地是另一条蛇或梯子的起点，玩家也 **不能** 继续移动。

- 举个例子，假设棋盘是 `[[-1,4],[-1,3]]` ，第一次移动，玩家的目标方格是 `2` 。那么这个玩家将会顺着梯子到达方格 `3` ，但 **不能** 顺着方格 `3` 上的梯子前往方格 `4` 。（简单来说，类似飞行棋，玩家掷出骰子点数后移动对应格数，遇到单向的路径（即梯子或蛇）可以直接跳到路径的终点，但如果多个路径首尾相连，也不能连续跳多个路径）

返回达到编号为 `n2` 的方格所需的最少掷骰次数，如果不可能，则返回 `-1`。

**示例 1：**

```
输入：board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]
输出：4
解释：
首先，从方格 1 [第 5 行，第 0 列] 开始。 
先决定移动到方格 2 ，并必须爬过梯子移动到到方格 15 。
然后决定移动到方格 17 [第 3 行，第 4 列]，必须爬过蛇到方格 13 。
接着决定移动到方格 14 ，且必须通过梯子移动到方格 35 。 
最后决定移动到方格 36 , 游戏结束。 
可以证明需要至少 4 次移动才能到达最后一个方格，所以答案是 4 。 
```

**示例 2：**

```
输入：board = [[-1,-1],[-1,3]]
输出：1
```

> **思路：广度优先搜索寻找最短路径，确保第一次到达目标节点时所用的步数是最小的。把问题抽象为一个图的最短路径问题，每个格子可以看作图中的一个节点，骰子的步数（1到6）决定了从一个节点到另一个节点的边。梯子和蛇则相当于图中的特殊边，可以直接将玩家从一个节点传送到另一个节点。**

```csharp
public class Solution
{
    public int SnakesAndLadders(int[][] board)
    {
        int n = board.Length;

        // 目标数字（最后一个数字）
        int targetNumber = n * n; 

        // 辅助队列，用于广度优先搜索
        Queue<int> queue = new Queue<int>();
        // 辅助数组，记录每个节点是否被访问过
        bool[] visited = new bool[targetNumber + 1];
        // 辅助数组，记录起点到每个节点的步数
        int[] steps = new int[targetNumber + 1];

        /** 广度优先搜索 */
        // 初始节点入队，标记访问、更新步数
        queue.Enqueue(1);
        visited[1] = true;
        steps[1] = 0;

        while (queue.Count > 0)
        {
            // 出队，判断如果到达目标节点，返回步数
            int currentNumber = queue.Dequeue();
            if (currentNumber == targetNumber)
            {
                return steps[currentNumber];
            }

            // 遍历所有可能的骰子步数（1到6）
            for (int i = 1; i <= 6; i++)
            {
                // 得到下一步的节点nextNumber
                int nextNumber = currentNumber + i;

                // 如果下一步节点超过棋盘范围，跳过
                if(nextNumber > targetNumber)
                {
                    continue;
                }

                // 根据board中梯和蛇的信息获取下一步的实际节点
                int row = GetRow(n, nextNumber);
                int col = GetCol(n, nextNumber);

                // 如果当前格子有梯子或蛇，更新nextNumber
                if (board[row][col] != -1)
                {
                    nextNumber = board[row][col]; // 到达梯或蛇的终点
                }

                // 如果下一个节点没被访问过，入队并标记访问、更新步数
                if (visited[nextNumber] == false)
                {
                    queue.Enqueue(nextNumber);
                    visited[nextNumber] = true;
                    steps[nextNumber] = steps[currentNumber] + 1;
                }
            }
        }

        // 如果无法到达终点，返回-1
        return -1;
    }

    // 辅助函数，根据数字获取行索引
    private int GetRow(int n, int number)
    {
        // 棋盘的行是从下往上数的
        return (n - 1) - (number - 1) / n;
    }

    // 辅助函数，根据数字获取列索引
    private int GetCol(int n, int number) 
    {
        int row = GetRow(n, number);
        if(row % 2 != n % 2)
        {
            return (number - 1) % n;
        }
        else
        {
            return (n - 1) - (number - 1) % n;
        }
    }
}
```

## [433. 最小基因变化](https://leetcode.cn/problems/minimum-genetic-mutation/)

基因序列可以表示为一条由 8 个字符组成的字符串，其中每个字符都是 `'A'`、`'C'`、`'G'` 和 `'T'` 之一。

假设我们需要调查从基因序列 `start` 变为 `end` 所发生的基因变化。一次基因变化就意味着这个基因序列中的一个字符发生了变化。

- 例如，`"AACCGGTT" --> "AACCGGTA"` 就是一次基因变化。

另有一个基因库 `bank` 记录了所有有效的基因变化，只有基因库中的基因才是有效的基因序列。（变化后的基因必须位于基因库 `bank` 中）

给你两个基因序列 `start` 和 `end` ，以及一个基因库 `bank` ，请你找出并返回能够使 `start` 变化为 `end` 所需的最少变化次数。如果无法完成此基因变化，返回 `-1` 。

注意：起始基因序列 `start` 默认是有效的，但是它并不一定会出现在基因库中。

**示例 1：**

```
输入：start = "AACCGGTT", end = "AACCGGTA", bank = ["AACCGGTA"]
输出：1
```

**示例 2：**

```
输入：start = "AACCGGTT", end = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]
输出：2
```

**示例 3：**

```
输入：start = "AAAAACCC", end = "AACCCCCC", bank = ["AAAACCCC","AAACCCCC","AACCCCCC"]
输出：3
```

> **思路：有向无权图的广度优先搜索，寻找最短路径。**
>
> 1. **问题建模：**
>    - **将每个基因序列看作图中的一个节点。**
>    - **如果两个基因序列之间只有一个字符不同，则在这两个节点之间建立一条边。**
>    - **问题转化为：在图中找到从 `startGene` 到 `endGene` 的最短路径。**
> 2. **图的构建：**
>    - **遍历基因库 `bank`，将每个基因序列作为节点加入图中。**
>    - **对于每一对基因序列，检查它们是否只有一个字符不同。如果是，则在它们之间建立双向边。**
>    - **将 `startGene` 也加入图中，并检查它与基因库中其他基因序列的连接关系。**
> 3. **广度优先搜索（BFS）：**
>    - **使用 BFS 从 `startGene` 开始遍历图，寻找到达 `endGene` 的最短路径。**
>    - **BFS 的特点是逐层遍历，因此第一次到达 `endGene` 时的路径长度一定是最短的。**
>    - **使用队列辅助 BFS，并用一个字典记录每个节点的访问状态和步数。**
> 4. **边界条件：**
>    - **如果 `endGene` 不在基因库 `bank` 中，直接返回 `-1`。**
>    - **如果 `startGene` 和 `endGene` 相同，直接返回 `0`。**

```csharp
public class Solution
{
    public int MinMutation(string startGene, string endGene, string[] bank)
    {
        // graph[A]：List<string>，表示A的所有邻居
        Dictionary<string, List<string>> graph = new Dictionary<string, List<string>>();

        // 把bank中的字符串当作节点加入graph
        for(int i=0; i<bank.Length; i++)
        {
            if (!graph.ContainsKey(bank[i])) // 检查是否已经存在
            {
                graph.Add(bank[i], new List<string>());
            }
        }

        // 根据bank构建graph中节点的连接关系
        for(int i=0; i<bank.Length-1; i++)
        {
            for(int j=i+1; j<bank.Length; j++)
            {
                var start = bank[i];
                var end = bank[j];
                if(CheckNotSameCharIsOne(start, end))
                {
                    // 构建双向连接关系
                    graph[start].Add(end);
                    graph[end].Add(start);
                }
            }
        }

        // 把startGene加入graph，并尝试构建与graph中已有节点的连接关系
        if(graph.ContainsKey(startGene) == false)
        {
            graph.Add(startGene, new List<string>());
            for(int i=0; i<bank.Length; i++)
            {
                if(CheckNotSameCharIsOne(startGene, bank[i]))
                {
                    // 构建双向连接关系
                    graph[startGene].Add(bank[i]);
                    graph[bank[i]].Add(startGene);
                }
            }
        }

        // 广度搜索，寻找从start到end的最短路径
        Queue<string> queue = new Queue<string>(); // 广度搜索辅助队列
        Dictionary<string, bool> visited = new Dictionary<string, bool>(); // 标记节点是否访问过，防止重复访问
        Dictionary<string, int> steps = new Dictionary<string, int>(); // 记录从起点到当前节点的最小步数

        // 起始节点入队，标记访问，记录步数为0
        queue.Enqueue(startGene);
        visited[startGene] = true;
        steps[startGene] = 0;

        while(queue.Count > 0)
        {
            string currentStr = queue.Dequeue();
            // 检查是否到达终点
            if (currentStr == endGene)
            {
                return steps[currentStr];
            }

            // 遍历当前节点的邻居
            foreach(var neighbor in graph[currentStr])
            {
                // 如果邻居没被访问过，邻居入队，标记访问，记录步数
                if (visited.ContainsKey(neighbor) == false || visited[neighbor] == false)
                {
                    queue.Enqueue(neighbor);
                    visited[neighbor] = true;
                    steps[neighbor] = steps[currentStr] + 1;
                }
            }
        }

        // 如果都遍历完了也没到达终点，返回-1
        return -1;
    }

    // 辅助函数，判断两个字符串不一样的字符个数是否为1
    private bool CheckNotSameCharIsOne(string s1, string s2)
    {
        int count = 0;
        for(int i=0; i<8; i++)
        {
            if (s1[i] != s2[i])
            {
                count++;
            }

            if(count > 1)
            {
                return false;
            }
        }

        return count == 1;
    }
}
```

## [127. 单词接龙](https://leetcode.cn/problems/word-ladder/)

字典 `wordList` 中从单词 `beginWord` 到 `endWord` 的 **转换序列** 是一个按下述规格形成的序列 `beginWord -> s1 -> s2 -> ... -> sk`：

- 每一对相邻的单词只差一个字母。
-  对于 `1 <= i <= k` 时，每个 `si` 都在 `wordList` 中。注意， `beginWord` 不需要在 `wordList` 中。
- `sk == endWord`

给你两个单词 `beginWord` 和 `endWord` 和一个字典 `wordList` ，返回 *从 `beginWord` 到 `endWord` 的 **最短转换序列** 中的 **单词数目*** 。如果不存在这样的转换序列，返回 `0` 。

**示例 1：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：5
解释：一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 返回它的长度 5。
```

**示例 2：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
输出：0
解释：endWord "cog" 不在字典中，所以无法进行转换。
```

>  **思路：和上一题几乎一样，有向无权图的广度优先搜索，寻找最短路径。**
>
> 1. **初始化图：**
>    - **遍历 `wordList`，将每个单词作为图的节点，并初始化其邻居列表。**
>    - **遍历 `wordList` 中的所有单词对，检查它们是否可以建立连接。如果可以，则在它们之间建立双向边。**
> 2. **将 `beginWord` 加入图：**
>    - **如果 `beginWord` 不在图中，则将其加入图。**
>    - **遍历 `wordList`，检查 `beginWord` 与每个单词是否可以建立连接。如果可以，则建立双向边。**
> 3. **BFS 遍历：**
>    - **将 `beginWord` 加入队列，并标记为已访问，记录步数为 1。**
>    - **从队列中取出当前节点，检查它是否为目标节点。如果是，则返回当前步数。**
>    - **遍历当前节点的所有邻居节点，将未访问的邻居节点加入队列，并更新步数。**
>    - **重复上述过程，直到队列为空或找到目标节点。**

```csharp
public class Solution
{
    public int LadderLength(string beginWord, string endWord, IList<string> wordList)
    {
        int n = wordList.Count;
        // graph[A]表示A的邻居列表
        Dictionary<string, List<string>> graph = new Dictionary<string, List<string>>();

        // 把wordList中的每个单词作为节点加入graph
        for(int i=0; i<n; i++)
        {
            if (graph.ContainsKey(wordList[i]) == false)
            {
                graph.Add(wordList[i], new List<string>());
            }
        }

        // 遍历wordList，构建节点的连接关系
        for(int i=0; i<n-1; i++)
        {
            for(int j=i+1; j<n; j++)
            {
                string start = wordList[i];
                string end = wordList[j];

                if(CheckNotSameCharIsOne(start, end))
                {
                    // 构建双向连接关系
                    graph[start].Add(end);
                    graph[end].Add(start);
                }
            }
        }

        // 把beginWord加入graph
        if(graph.ContainsKey(beginWord) == false)
        {
            graph.Add(beginWord, new List<string>());
            // 遍历wordList，检查每个节点是否能与beginWord构建连接关系
            for(int i=0; i<n; i++)
            {
                if (CheckNotSameCharIsOne(beginWord, wordList[i]))
                {
                    // 构建双向连接关系
                    graph[beginWord].Add(wordList[i]);
                    graph[wordList[i]].Add(beginWord);
                }
            }
        }

        // 广度优先搜索遍历节点
        Queue<string> queue = new Queue<string>(); // 辅助队列，用于广度优先搜索
        HashSet<string> visited = new HashSet<string>(); // 辅助哈希集合，记录节点是否被访问过，避免重复访问
        Dictionary<string, int> steps = new Dictionary<string, int>(); // 辅助字典，记录从起点到该节点的步数

        // 起始节点加入队列，标记访问并记录步数
        queue.Enqueue(beginWord);
        visited.Add(beginWord);
        steps[beginWord] = 1;

        while(queue.Count > 0)
        {
            string currentWord = queue.Dequeue();

            // 检查当前节点已经是终点
            if(currentWord == endWord)
            {
                return steps[currentWord];
            }
            
            // 遍历邻居节点，如果邻居节点没有被访问过，邻居节点入队、标记访问、记录步数
            foreach(var neighbor in graph[currentWord])
            {
                if(visited.Contains(neighbor) == false)
                {
                    queue.Enqueue(neighbor);
                    visited.Add(neighbor);
                    steps[neighbor] = steps[currentWord] + 1;
                }
            }
        }

        // 如果遍历完了还没到达终点，返回0
        return 0;
    }

    // 辅助函数，判断两个字符串不一样的字符个数是否为1
    private bool CheckNotSameCharIsOne(string s1, string s2)
    {
        int length = s1.Length;
        int count = 0;
        for(int i=0; i<length; i++)
        {
            if (s1[i] != s2[i])
            {
                count++;
            }

            if(count > 1)
            {
                return false;
            }
        }

        return count == 1;
    }
}
```

# 【字典树】

## [208. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)

**[Trie](https://baike.baidu.com/item/字典树/9825209?fr=aladdin)**（发音类似 "try"）或者说 **前缀树** 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。

请你实现 Trie 类：

- `Trie()` 初始化前缀树对象。
- `void insert(String word)` 向前缀树中插入字符串 `word` 。
- `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
- `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。

**示例：**

```
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]

解释
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True
```

> **思路：自定义数据结构`TrieNode`表示前缀树节点，用`Dictionary<char, TrieNode>`数据结构存储其子节点，用`IsEndOfWord`属性标识某节点是否是单词的末尾节点；从根节点到叶子节点的字符序列表示一个单词。**

```cs
public class TrieNode
{
    // 子节点字典
    public Dictionary<char, TrieNode> Children {get; set;}
    // 标记当前节点是否是一个单词的末尾
    public bool IsEndOfWord {get; set;}

    public TrieNode()
    {
        Children = new Dictionary<char, TrieNode>();
        IsEndOfWord = false;
    }
}

public class Trie {
    // 根节点
    private readonly TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
    
    public void Insert(string word) {
        TrieNode current = root;
        // 遍历单词中的每个字符
        foreach(char c in word)
        {
            if(!current.Children.ContainsKey(c))
            {
                // 如果当前字符不在子节点中，新建一个子节点并加入字典
                current.Children[c] = new TrieNode();
            }

            // 否则，移动current到该字符对应的子节点
            current = current.Children[c];
        }

        // 标记最后一个节点为单词末尾
        current.IsEndOfWord = true;
    }
    
    public bool Search(string word) {
        TrieNode current = root;

        // 遍历单词中的每个字符
        foreach(char c in word)
        {
            // 如果字符不在当前节点的子节点中，直接返回false
            if(!current.Children.ContainsKey(c))
            {
                return false;
            }

            // 否则， 移动current到该字符对应的子节点
            current = current.Children[c];
        }

        // 检查是否标记为单词结束
        return current.IsEndOfWord;
    }
    
    public bool StartsWith(string prefix) {
        TrieNode current = root;

        // 遍历单词中的每个字符
        foreach(char c in prefix)
        {
            // 如果字符不在当前节点的子节点中，直接返回false
            if(!current.Children.ContainsKey(c))
            {
                return false;
            }

            // 否则， 移动current到该字符对应的子节点
            current = current.Children[c];
        }

         // 只要前缀路径存在就返回true，不关心是否单词结束
        return true;
    }
}
```

## [211. 添加与搜索单词 - 数据结构设计](https://leetcode.cn/problems/design-add-and-search-words-data-structure/)

请你设计一个数据结构，支持 添加新单词 和 查找字符串是否与任何先前添加的字符串匹配 。

实现词典类 `WordDictionary` ：

- `WordDictionary()` 初始化词典对象
- `void addWord(word)` 将 `word` 添加到数据结构中，之后可以对它进行匹配
- `bool search(word)` 如果数据结构中存在字符串与 `word` 匹配，则返回 `true` ；否则，返回 `false` 。`word` 中可能包含一些 `'.'` ，每个 `.` 都可以表示任何一个字母。

**示例：**

```
输入：
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
输出：
[null,null,null,null,false,true,true,true]

解释：
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // 返回 False
wordDictionary.search("bad"); // 返回 True
wordDictionary.search(".ad"); // 返回 True
wordDictionary.search("b.."); // 返回 True
```

> **思路：用Trie前缀树实现，Search方法用深度优先搜索实现，可处理通配符的情况。**
>
> 1. **递归深度优先：从根节点开始，沿着单词字符顺序递归深入，每次处理一个字符（类似走迷宫一条路走到黑）**
> 2. **通配符处理：遇到`.`时，遍历当前节点的所有子节点（相当于同时尝试所有可能的字符路径）**
> 3. **终止条件：当递归到单词末尾时，检查当前节点是否被标记为单词结尾（IsEndOfWord）**
> 4. **回溯机制：某条路径失败时会自动回溯到上一个分叉点，继续尝试其他可能性（递归调用栈自动实现）**
> 5. **剪枝优化：遇到普通字符时，如果不存在对应子节点直接返回false，避免无效搜索**

```cs
public class TrieNode
{
    public Dictionary<char, TrieNode> Children {get; set;}
    public bool IsEndOfWord {get; set;}
    
    public TrieNode()
    {
        Children = new Dictionary<char, TrieNode>();
        IsEndOfWord = false;
    }
}

public class WordDictionary {
    private readonly TrieNode root;

    public WordDictionary() {
        root = new TrieNode();
    }
    
    public void AddWord(string word) {
        TrieNode current = root;

        foreach(char c in word)
        {
            if(!current.Children.ContainsKey(c))
            {
                current.Children[c] = new TrieNode();
            }

            current = current.Children[c];
        }

        current.IsEndOfWord = true;
    }
    
    public bool Search(string word) {
        return SearchHelper(word, 0, root);
    }

    // 辅助函数，递归处理有通配符的情况
    private bool SearchHelper(string word, int index, TrieNode node)
    {
        // 已到达单词末尾，检查是否是单词结束节点
        if(index == word.Length)
        {
            return node.IsEndOfWord;
        }

        char currentChar = word[index];

        if(currentChar == '.')
        {
            // 处理通配符需要遍历所有子节点
            foreach(var child in node.Children.Values)
            {
                if(SearchHelper(word, index + 1, child))
                {
                    // 只要有一条路径满足条件，就返回true
                    return true;
                }
            }
            return false; // 所有路径都不满足条件，就返回false
        }
        else
        {
            // 普通字符处理
            if(!node.Children.ContainsKey(currentChar))
            {
                return false;
            }
            return SearchHelper(word, index + 1, node.Children[currentChar]);
        }
    }
}
```

## [212. 单词搜索 II](https://leetcode.cn/problems/word-search-ii/)

给定一个 `m x n` 二维字符网格 `board` 和一个单词（字符串）列表 `words`， *返回所有二维网格上的单词* 。

单词必须按照字母顺序，通过 **相邻的单元格** 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

**示例 1：**

```
输入：board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
输出：["eat","oath"]
```

**示例 2：**

```
输入：board = [["a","b"],["c","d"]], words = ["abcb"]
输出：[]
```

思路一（超时）：辅助函数`SeachHelper`以某个格子为起点，递归搜索单词是否存在；辅助函数`CheckWordExist`遍历每个可能的起点，搜索单词在网格中是否存在；时间复杂度是`O(k * m * n * 4^L)`，L为单词平均长度。

```cs
public class Solution {
    public IList<string> FindWords(char[][] board, string[] words) {
        int m = board.Length;
        int n = board[0].Length;

        IList<string> result = new List<string>();

        foreach(string word in words) {
            // 遍历words中的每个单词，检查字典中是否存在
            if(CheckWordExist(m, n, board, word)) {
                result.Add(word);
            }
        }

        return result;
    }

    // 辅助函数，查询某个单词是否存在
    private bool CheckWordExist(int m, int n, char[][] board, string word)
    {
        for(int i=0; i<m; i++)
        {
            for(int j=0; j<n; j++)
            {
                // 遍历所有可能的起点
                if(SearchHelper(m, n, board, i, j, word, 0)) {
                    return true;
                }
            }
        }

        return false;
    }

    // 辅助函数，指定某个起点，递归查询某个单词是否存在
    private bool SearchHelper(int m, int n, char[][] board, int i, int j, string word, int index) {
        if(index == word.Length) {
            // 全部匹配
            return true;
        }

        if(i<0 || i >= m || j < 0 || j >= n) {
            // 超出网格范围
            return false;
        }

        if(board[i][j] == word[index])
        {
            // 临时标记为已访问（避免重复使用）
            char temp = board[i][j];
            board[i][j] = '#';

            // 遍历四个方向，递归匹配下一个字符，只要有一条路走通，就返回true
            bool found = 
                SearchHelper(m, n, board, i + 1, j, word, index + 1) || // 向右
                SearchHelper(m, n, board, i - 1, j, word, index + 1) || // 向左
                SearchHelper(m, n, board, i, j + 1, word, index + 1) || // 向上
                SearchHelper(m, n, board, i, j - 1, word, index + 1);   // 向下

            // 恢复字符
            board[i][j] = temp;

            return found;
                
        }
        return false;
    }
}
```

> **思路二：在思路一的基础上，用Trie前缀树来预处理 `words`，这样可以在 `board` 上进行一次深度优先搜索，同时匹配多个单词，而不是对每个单词单独搜索。这种方法的时间复杂度可以优化到 `O(m * n * 4^L)`。**
>
> 1. **构建 Trie 树：将所有 `words` 预处理存入 Trie 树，方便高效匹配。**
> 2. **DFS 搜索：遍历 `board` 的每个单元格，从 Trie 树的根节点开始匹配。**
> 3. **剪枝优化：如果当前字符不在 Trie 中，直接跳过；如果匹配到完整单词，加入结果并标记避免重复。**
> 4. **回溯恢复：临时标记已访问的字符为 `'#'`，递归后恢复原值，确保不影响其他路径搜索。**

```csharp
public class Solution {
    public IList<string> FindWords(char[][] board, string[] words) {
        int m = board.Length;
        int n = board[0].Length;

        TrieNode root = BuildTrie(words); // 1. 构建Trie树
        IList<string> result = new List<string>();

        for(int i=0; i<m; i++) {
            for(int j=0; j<n; j++) {
                SearchHelper(m, n, board, i, j, root, result); // 2. 从每个单元格开始DFS
            }
        }
        return result;
    }

    private void SearchHelper(int m, int n, char[][] board, int i, int j, TrieNode node, IList<string> result) {
        if(i<0 || i >= m || j < 0 || j >= n) return; // 越界检查

        char c = board[i][j];
        if(c == '#' || !node.Children.ContainsKey(c)) return; // 3. 剪枝：字符不匹配或已访问

        node = node.Children[c]; // 移动到Trie的子节点
        if(node.Word != null) {
            result.Add(node.Word); // 4. 找到完整单词
            node.Word = null; // 避免重复添加
        }

        board[i][j] = '#'; // 标记为已访问
        // 5. 向四个方向DFS
        SearchHelper(m, n, board, i+1, j, node, result);
        SearchHelper(m, n, board, i-1, j, node, result);
        SearchHelper(m, n, board, i, j+1, node, result);
        SearchHelper(m, n, board, i, j-1, node, result);
        board[i][j] = c; // 6. 回溯恢复字符
    }

    private class TrieNode {
        public Dictionary<char, TrieNode> Children { get; } = new Dictionary<char, TrieNode>();
        public string Word {get; set;} // 存储叶子节点对应的完整单词
    }

    private TrieNode BuildTrie(string[] words) {
        TrieNode root = new TrieNode();
        foreach(string word in words) {
            TrieNode current = root;
            foreach(char c in word) {
                if(!current.Children.ContainsKey(c)) {
                    current.Children[c] = new TrieNode(); // 7. 构建Trie的路径
                }
                current = current.Children[c];
            }
            current.Word = word; // 8. 在叶子节点记录单词
        }
        return root;
    }
}
```

# 【回溯】

## [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例 2：**

```
输入：digits = ""
输出：[]
```

**示例 3：**

```
输入：digits = "2"
输出：["a","b","c"]
```

> **思路一：深度优先搜索+回溯。递归探索，递归函数中记录当前长度，递归终止条件是当前长度等于输入的数字长度；循环遍历当前数字对应的各个字母，在循环中调用递归函数，每次递归返回后都要移除最后一个字母并尝试其他可能。**

```cs
public class Solution
{
    public Dictionary<char, char[]> numToLetters = new Dictionary<char, char[]>()
    {
        {'2', new char[] {'a', 'b', 'c'} },
        {'3', new char[] {'d', 'e', 'f'} },
        {'4', new char[] {'g', 'h', 'i'} },
        {'5', new char[] {'j', 'k', 'l'} },
        {'6', new char[] {'m', 'n', 'o'} },
        {'7', new char[] {'p', 'q', 'r', 's'} },
        {'8', new char[] {'t', 'u', 'v'} },
        {'9', new char[] {'w', 'x', 'y', 'z'} }
    };

    public IList<string> LetterCombinations(string digits)
    {
        IList<string> result = new List<string>();
        if(string.IsNullOrEmpty(digits))
        {
            return result;
        }

        Backtrack(digits, 0, new StringBuilder(), result);
        return result;
    }

    // 辅助函数，回溯
    private void Backtrack(string digits, int index, StringBuilder current, IList<string> result)
    {
        // 终止条件：当前字符串长度 == 数字长度
        if(index == digits.Length)
        {
            result.Add(current.ToString());
            return;
        }

        char digit = digits[index];
        if(numToLetters.ContainsKey(digit) == false)
        {
            return;
        }

        foreach(char c in numToLetters[digit])
        {
            // 遍历当前数字对应的所有字母
            current.Append(c);
            Backtrack(digits, index + 1, current, result); // 递归
            current.Remove(current.Length - 1, 1); // 删除最后一个字符（回溯）
        }
    }
}
```

> **思路二：广度优先搜索。用队列辅助，初始化加入一个空字符串到队列；遍历每个数字，可得到该数字对应的字母列表，依次取出队列中的组合，遍历字母列表中的每个字母，追加新字母后重新入队；当处理完所有数字时，队列中的组合即为结果。**

```csharp
public class Solution
{
    public Dictionary<char, char[]> numToLetters = new Dictionary<char, char[]>()
    {
        {'2', new char[] {'a', 'b', 'c'} },
        {'3', new char[] {'d', 'e', 'f'} },
        {'4', new char[] {'g', 'h', 'i'} },
        {'5', new char[] {'j', 'k', 'l'} },
        {'6', new char[] {'m', 'n', 'o'} },
        {'7', new char[] {'p', 'q', 'r', 's'} },
        {'8', new char[] {'t', 'u', 'v'} },
        {'9', new char[] {'w', 'x', 'y', 'z'} }
    };

    public IList<string> LetterCombinations(string digits)
    {
        IList<string> result = new List<string>();
        if(string.IsNullOrEmpty(digits))
        {
            return result;
        }

        Queue<string> queue = new Queue<string>(); // 辅助队列，用于广度优先搜索
        queue.Enqueue(""); // 初始放入一个空字符串

        foreach(char digit in digits)
        {
            if(numToLetters.ContainsKey(digit) == false)
            {
                continue;
            }

            char[] letters = numToLetters[digit]; // 当前数字对应的字母
            int currentSize = queue.Count; // 当前队列的元素个数
            for(int i=0; i<currentSize; i++)
            {
                string current = queue.Dequeue(); // 取出当前组合
                foreach(char c in letters)
                {
                    // 遍历当前数字对应的每个字母，追加新字母再放入队列
                    queue.Enqueue(current + c);
                }
            }
        }

        // 队列里的所有字符串就是最终结果
        while (queue.Count > 0)
        {
            result.Add(queue.Dequeue());
        }

        return result;
    }
}
```

## [77. 组合](https://leetcode.cn/problems/combinations/)

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

**示例 1：**

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**示例 2：**

```
输入：n = 1, k = 1
输出：[[1]]
```

> **思路一：深度优先搜索+回溯。**

```csharp
public class Solution
{
    public IList<IList<int>> Combine(int n, int k)
    {
        IList<IList<int>> result = new List<IList<int>>();
        Backtrack(n, k, 1, new List<int>(), result);
        return result;
    }

    private void Backtrack(int n, int k, int start, List<int> current, IList<IList<int>> result)
    {
        // 回溯终止条件：当前字符串长度 == k
        if(current.Count == k)
        {
            result.Add(new List<int>(current)); // 需要存current的副本到结果，而非引用
            return;
        }

        // 遍历所有可选字符（≥start的字符）
        for(int num = start; num <= n; num++)
        {
            current.Add(num); // 选择num
            Backtrack(n, k, num + 1, current, result); // 递归下一层
            current.RemoveAt(current.Count - 1); // 删除current中的最后一个字符（num），回溯
        }
    }
}
```

> **剪枝优化：如果剩余数字不足以填满 `k` 个，提前终止递归。**

```
for (int i = start; i <= n - (k - current.Count) + 1; i++) {
    current.Add(i);
    Backtrack(n, k, i + 1, current, result);
    current.RemoveAt(current.Count - 1);
}
```

> **思路二：广度优先搜索。**

```cs
public class Solution
{
    public IList<IList<int>> Combine(int n, int k)
    {
        IList<IList<int>> result = new List<IList<int>>();
        Queue<List<int>> queue = new Queue<List<int>>(); // 辅助队列，用于深度优先搜索
        queue.Enqueue(new List<int>());

        while(queue.Count > 0)
        {
            List<int> current = queue.Dequeue();
            if(current.Count == k)
            {
                // 如果当前取出的数字列表长度 == 目标长度k，直接加入结果并跳过后面步骤
                result.Add(current);
                continue;
            }

            // 根据当前取出的数字列表，得到最小的下一个数字
            int startNum = current.Count == 0 ? 1 : current.Last() + 1;
            for(int num = startNum; num <= n; num ++)
            {
                // 遍历下一个数字的所有可能，添加到当前数字列表末尾，重新加入队列
                List<int> newNumList = new List<int>(current);
                newNumList.Add(num);
                queue.Enqueue(newNumList);
            }
        }

        return result;
    }
}
```

## [46. 全排列](https://leetcode.cn/problems/permutations/)

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

> **思路一：深度优先搜索+回溯。**

```csharp
public class Solution {
    public IList<IList<int>> Permute(int[] nums) {
        IList<IList<int>> result = new List<IList<int>>();
        Backtrack(nums, 0, new List<int>(), result);
        return result;
    }

    private void Backtrack(int[] nums, int index, List<int> current, IList<IList<int>> result) {
        if(index == nums.Length) {
            result.Add(new List<int>(current));
            return;
        }

        foreach(int num in nums) {
            if(current.Contains(num) == false) {
                // 遍历nums中剩余的数字
                current.Add(num); // 添加到current中
                Backtrack(nums, index + 1, current, result); // 递归调用
                current.RemoveAt(current.Count - 1);
            }
        }
    }
}
```

> **思路二：广度优先搜索。**

```cs
public class Solution {
    public IList<IList<int>> Permute(int[] nums) {
        IList<IList<int>> result = new List<IList<int>>();
        Queue<List<int>> queue = new Queue<List<int>>();
        queue.Enqueue(new List<int>());

        while(queue.Count > 0)
        {
            List<int> current = queue.Dequeue();

            // 如果当前列表的长度 == 输入的nums长度，直接加入结果并跳过后面步骤
            if(current.Count == nums.Length) {
                result.Add(current);
                continue;
            }

            foreach(int num in nums) {
                // 遍历nums中每个每个元素
                if(current.Contains(num) == false) {
                    // 如果当前列表current中不含有，则加到后面并重新入队
                    List<int> newNumList = new List<int>(current);
                    newNumList.Add(num);
                    queue.Enqueue(newNumList);
                }
            }
        }

        return result;
    }
}
```

## [39. 组合总和](https://leetcode.cn/problems/combination-sum/)

给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例 2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

**示例 3：**

```
输入: candidates = [2], target = 1
输出: []
```

> **思路一：深度优先搜索+回溯。为了避免重复，递归函数中传入`index`作为遍历的起始下标。**

```cs
public class Solution {
    public IList<IList<int>> CombinationSum(int[] candidates, int target) {
        IList<IList<int>> result = new List<IList<int>>();
        Backtrack(candidates, target, 0, new List<int>(), result);
        return result;
    }

    // 辅助函数，回溯
    private void Backtrack(int[] candidates, int target, int index, List<int> current, IList<IList<int>> result) {
        // 回溯退出条件
        if(target < 0) {
            return;
        }
        if(target == 0) {
            // 刚好凑到目标和，current加入结果
            result.Add(new List<int>(current));
            return;
        }

        for (int i = index; i < candidates.Length; i++) {
            if(candidates[i] <= target) {
                // 遍历candidates中每个数字，如果可以用来凑数（比target小）就加入current
                current.Add(candidates[i]);
                Backtrack(candidates, target - candidates[i], i, current, result); // 递归调用
                current.RemoveAt(current.Count - 1); // 删除最后一个元素，回溯
            }
        }
    }
}
```

> **思路二：广度优先搜索。**

```cs
public class Solution {
    public IList<IList<int>> CombinationSum(int[] candidates, int target) {
        IList<IList<int>> result = new List<IList<int>>();
        Queue<(List<int>, int, int)> queue = new Queue<(List<int>, int, int)>();
        queue.Enqueue((new List<int>(), 0, 0)); // (当前组合, 当前总和, 起始下标)

        while(queue.Count > 0) {
            var (current, sum, start) = queue.Dequeue();

            if(sum > target) {
                continue;
            }
            if(sum == target) {
                result.Add(current);
                continue;
            }

            for (int i = start; i < candidates.Length; i++) {
                if(candidates[i] <= target - sum) {
                    // 剪枝：只有 sum + num <= target 才入队
                    List<int> newNumList = new List<int>(current);
                    newNumList.Add(candidates[i]);
                    queue.Enqueue((newNumList, sum + candidates[i], i)); // 限制下一次从 i 开始选，避免重复组合
                }
            }
        }

        return result;
    }
}
```

## [52. N 皇后 II](https://leetcode.cn/problems/n-queens-ii/)

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n × n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回 **n 皇后问题** 不同的解决方案的数量。

**示例 1：**

```
输入：n = 4
输出：2
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例 2：**

```
输入：n = 1
输出：1
```

> **思路：深度优先搜索+回溯。回溯递归函数中传入已安排的行数，以及用于记录列和两个对角线占用情况的数据结构。**

```cs
public class Solution {
    public int TotalNQueens(int n) {
        int result = 0;
        Backtrack(n, 0, new bool[n], new List<int>(), new List<int>(), ref result);
        return result;
    }

    // row-上个放置的皇后所在的行索引
    // rows-行的占用情况；cols-列的占用情况
    // diag1-主对角线的占用情况；diag2-副对角线的占用情况
    private void Backtrack(int n, int row, bool[] cols, List<int> diag1, List<int> diag2, ref int result) {
        if(row == n) {
            result ++;
            return;
        }

        for(int i=0; i<n; i++) {
            int d1 = row - i; // 对角线1的标识
            int d2 = row + i; // 对角线2的标识

            if(cols[i] == false && !diag1.Contains(d1) && !diag2.Contains(d2)) {
                // 标记列和主、副对角线为占用
                cols[i] = true;
                diag1.Add(d1);
                diag2.Add(d2);
                Backtrack(n, row + 1, cols, diag1, diag2, ref result); // 递归调用
                // 回溯复原，探索其他可能
                cols[i] = false;
                diag1.Remove(d1);
                diag2.Remove(d2);
            }
        }
    }
}
```

## [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/)

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

**示例 1：**

```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

**示例 2：**

```
输入：n = 1
输出：["()"]
```

> **思路：递归回溯。**
>
> 1. **回溯：递归尝试所有可能的括号组合。**
> 2. **约束条件：**
>    - **左括号数 ≤ `n`（保证数量足够）。**
>    - **右括号数 ≤ 左括号数（保证有效性）。**
> 3. **终止条件：当组合长度达到 `2n` 时，保存结果。**

```csharp
public class Solution {
    public IList<string> GenerateParenthesis(int n) {
        IList<string> result = new List<string>();
        Backtrack(n, 0, 0, new StringBuilder(), result);
        return result;
    }

    private void Backtrack(int n, int leftCount, int rightCount, StringBuilder current, IList<string> result) {
        if (current.Length == 2 * n) {
            // 如果当前组合长度已达2n（即左右括号各n个），说明已生成一个有效组合
            result.Add(current.ToString());
            return;
        }

        // 添加左括号的条件：左括号数量未达到n（即还可以添加左括号）
        if (leftCount < n) {
            current.Append('(');
            Backtrack(n, leftCount + 1, rightCount, current, result);
            current.Remove(current.Length - 1, 1);
        }

        // 尝试添加右括号的条件：右括号数量必须小于左括号数量（保证有效性）
        if (rightCount < leftCount) {
            current.Append(')');
            Backtrack(n, leftCount, rightCount + 1, current, result);
            current.Remove(current.Length - 1, 1);
        }
    }
}
```

## [79. 单词搜索](https://leetcode.cn/problems/word-search/)

给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

**示例 1：**

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

**示例 2：**

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
输出：false
```

> **思路一：深度优先搜索+回溯。**
>
> 1. **遍历起点：**
>    - **遍历网格中每个单元格作为可能的起点，只有当单元格字符与单词首字符匹配时才进行搜索**
> 2. **递归搜索：**
>    - **从当前单元格向四个方向（上、下、左、右）递归搜索**
>    - **使用回溯法标记已访问的单元格（临时修改为'*'）**
>    - **搜索完成后恢复单元格原始值**
> 3. **终止条件：**
>    - **成功条件：已匹配完整个单词（index == word.Length）**
>    - **失败条件：越界或字符不匹配**

```cs
public class Solution {
    public bool Exist(char[][] board, string word) {
        int m = board.Length;
        int n = board[0].Length;

        // 遍历每个可能的起点
        for(int i=0; i<m; i++) {
            for(int j=0; j<n; j++) {
                if (Backtrack(board, word, 0, i, j)) {
                    // 只要找到一条路，就算成功
                    return true;
                }
            }
        }

        return false;
    }

    // 用于递归回溯的辅助函数，index-要匹配的字符、(row, col)-匹配位置
    private bool Backtrack(char[][] board, string word, int index, int row, int col) {
        if(index == word.Length) {
            return true;
        }

        if(row < 0 || row >= board.Length || col < 0 || col >= board[0].Length) {
            // 不处理越界坐标
            return false;
        }

        if(board[row][col] != word[index]) {
            // 不匹配（包含已被'*'标记）
            return false;
        }

        char temp = board[row][col];
        board[row][col] = '*'; // 标记

        // 递归查找四个方向，只要有一个方向匹配，就算找到
        bool found = Backtrack(board, word, index+1, row-1, col)  // 左
                   ||Backtrack(board, word, index+1, row+1, col)  // 右
                   ||Backtrack(board, word, index+1, row, col-1)  // 上
                   ||Backtrack(board, word, index+1, row, col+1); // 下
        
        // 回溯，恢复字符
        board[row][col] = temp;

        return found;
    }
}
```

> **思路二：广度优先搜索（不是最优方案）。**
>
> 1. **遍历起点：**
>    - **遍历所有可能的起点，只考虑与单词首字符匹配的起点**
> 2. **队列初始化：**
>    - **使用队列存储搜索状态（位置坐标、已匹配字符索引、访问标记）**
>    - **每个状态携带独立的访问标记数组**
> 3. **广度优先扩展：**
>    - **从队列取出当前状态**
>    - **向四个方向扩展新状态**
>    - **确保新位置在网格内且未被访问过**
>    - **字符匹配时才加入队列**
> 4. **终止条件：**
>    - **成功条件：已匹配完整个单词**
>    - **失败条件：队列耗尽仍未找到**

```csharp
public class Solution {
    public bool Exist(char[][] board, string word) {
        int m = board.Length;
        int n = board[0].Length;

        // 遍历每个可能的起点
        for(int i=0; i<m; i++) {
            for(int j=0; j<n; j++) {
                if(board[i][j] == word[0] && BFS(board, word, i, j)) {
                    return true;
                }
            }
        }

        return false;
    }

    // 辅助函数，用于广度优先搜索
    private bool BFS(char[][] board, string word, int row, int col) {
        int m = board.Length, n = board[0].Length;
        // 行、列、上个匹配的字符的索引、标记坐标是否被访问的二维数组
        var queue = new Queue<(int, int, int, bool[,])>();
        // 标记坐标是否被访问
        var initialVisited = new bool[m, n];
        initialVisited[row, col] = true;
        
        queue.Enqueue((row, col, 0, initialVisited));

        // 四个方向的向量
        int[][] directions = new int[][] { 
            new int[] { -1, 0 }, // 上
            new int[] { 1, 0 },  // 下
            new int[] { 0, -1 }, // 左
            new int[] { 0, 1 }   // 右
        };

        while(queue.Count > 0) {
            var (i, j, index, visited) = queue.Dequeue();
            if(index == word.Length - 1) {
                return true;
            }

            // 遍历四个方向
            foreach (var dir in directions) {
                int newRow = i + dir[0];
                int newCol = j + dir[1];

                if(newRow < 0 || newRow >= m || newCol < 0 || newCol >= n) {
                    // 跳过不合法的坐标
                    continue;
                }

                if(visited[newRow, newCol] == false && board[newRow][newCol] == word[index+1]) {
                    // 为每个新状态创建独立的visited数组
                    var newVisited = (bool[,])visited.Clone();
                    newVisited[newRow, newCol] = true;
                    queue.Enqueue((newRow, newCol, index+1, newVisited));
                }
            }
        }

        return false;
    }
}
```

# 【分治】

## [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 平衡 二叉搜索树。

**示例 1：**

```
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案。
```

**示例 2：**

```
输入：nums = [1,3]
输出：[3,1]
解释：[1,null,3] 和 [3,1] 都是高度平衡二叉搜索树。
```

> **思路：分治+递归。递归函数中记录处理的数的下标范围，每次对范围做二分。**

```cs
public class Solution {
    public TreeNode SortedArrayToBST(int[] nums) {
        return Divide(nums, 0, nums.Length - 1);
    }

    private TreeNode Divide(int[] nums, int start, int end) {
        if(start > end) {
            return null;
        }

        int midIndex = (start + end) / 2; // 中点索引，作为根节点
        TreeNode root = new TreeNode(nums[midIndex]);
        root.left = Divide(nums, start, midIndex - 1);
        root.right = Divide(nums, midIndex + 1, end);

        return root;
    }
}
```

## [148. 排序链表](https://leetcode.cn/problems/sort-list/)

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。

**示例 1：**

```
输入：head = [4,2,1,3]
输出：[1,2,3,4]
```

**示例 2：**

```
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
```

**示例 3：**

```
输入：head = []
输出：[]
```

> **思路：先用快慢指针找到链表的中间节点，断开链表为两个子链表，对每个子链表分别递归调用排序得到排序后的链表，再使用【合并两个有序列表】方法合并两个排序后的子链表。**

```csharp
public class Solution {
    public ListNode SortList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        
        // 此时链表中至少有2个节点
        // 找到链表的中间节点
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // 分割链表，此时slow所在的索引是 length-1/2
        ListNode list2 = slow.next;
        slow.next = null; // 断开

        // 递归排序两个子链表
        ListNode sortedList1 = SortList(head);
        ListNode sortedList2 = SortList(list2);

        return MergeTwoLists(sortedList1, sortedList2); // 合并两个有序链表
    }

    // 合并两个有序链表
    private ListNode MergeTwoLists(ListNode list1, ListNode list2) {
        if(list1 == null) {
            return list2;
        }
        if(list2 == null) {
            return list1;
        }
        if(list1.val < list2.val) {
            list1.next = MergeTwoLists(list1.next, list2);
            return list1;
        } else {
            list2.next = MergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```

## [427. 建立四叉树](https://leetcode.cn/problems/construct-quad-tree/)

给你一个 `n * n` 矩阵 `grid` ，矩阵由若干 `0` 和 `1` 组成。请你用四叉树表示该矩阵 `grid` 。

你需要返回能表示矩阵 `grid` 的 四叉树 的根结点。

四叉树数据结构中，每个内部节点只有四个子节点。此外，每个节点都有两个属性：

- `val`：储存叶子结点所代表的区域的值。1 对应 **True**，0 对应 **False**。注意，当 `isLeaf` 为 **False** 时，你可以把 **True** 或者 **False** 赋值给节点，两种值都会被判题机制 **接受** 。
- `isLeaf`: 当这个节点是一个叶子结点时为 **True**，如果它有 4 个子节点则为 **False** 。

```
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
```

我们可以按以下步骤为二维区域构建四叉树：

1. 如果当前网格的值相同（即，全为 `0` 或者全为 `1`），将 `isLeaf` 设为 True ，将 `val` 设为网格相应的值，并将四个子节点都设为 Null 然后停止。
2. 如果当前网格的值不同，将 `isLeaf` 设为 False， 将 `val` 设为任意值，然后如下图所示，将当前网格划分为四个子网格。
3. 使用适当的子网格递归每个子节点。

**四叉树格式：**

你不需要阅读本节来解决这个问题。只有当你想了解输出格式时才会这样做。输出为使用层序遍历后四叉树的序列化形式，其中 `null` 表示路径终止符，其下面不存在节点。

它与二叉树的序列化非常相似。唯一的区别是节点以列表形式表示 `[isLeaf, val]` 。

如果 `isLeaf` 或者 `val` 的值为 True ，则表示它在列表 `[isLeaf, val]` 中的值为 **1** ；如果 `isLeaf` 或者 `val` 的值为 False ，则表示值为 **0** 。

**示例 1：**

```
输入：grid = [[0,1],[1,0]]
输出：[[0,1],[1,0],[1,1],[1,1],[1,0]]
解释：此示例的解释如下：
请注意，在下面四叉树的图示中，0 表示 false，1 表示 True 。
```

**示例 2：**

```
输入：grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
输出：[[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
解释：网格中的所有值都不相同。我们将网格划分为四个子网格。
topLeft，bottomLeft 和 bottomRight 均具有相同的值。
topRight 具有不同的值，因此我们将其再分为 4 个子网格，这样每个子网格都具有相同的值。
解释如下图所示：
```

> **思路：分治+递归。在递归函数中记录处理区域的行起始索引、行结束索引、列起始索引、列结束索引。**
>
> 1. **递归终止条件：当处理区域缩小到单个单元格时，直接创建节点并设置为叶子节点。**
> 2. **区域划分和递归处理：将当前区域划分为四个子区域，对每个子区域递归构建四叉树。**
> 3. **合并判断：如果四个子节点都是叶子节点且值相同，则合并为一个新的叶子节点。**

```cs
public class Solution {
    public Node Construct(int[][] grid) {
        int n = grid.Length;
        return GetQuadTree(grid, 0, n-1, 0, n-1);
    }

    public Node GetQuadTree(int [][] grid, int rowStart, int rowEnd, int colStart, int colEnd) {
        if (rowStart == rowEnd && colStart == colEnd) {
            // 返回叶子节点
            return new Node(grid[rowStart][colStart] == 1, true);
        }

        int rowMid = (rowStart + rowEnd) / 2;
        int colMid = (colStart + colEnd) / 2;

        Node root = new Node(false, false);
        root.topLeft = GetQuadTree(grid, rowStart, rowMid, colStart, colMid);
        root.topRight = GetQuadTree(grid, rowStart, rowMid, colMid+1, colEnd);
        root.bottomLeft = GetQuadTree(grid, rowMid+1, rowEnd, colStart, colMid);
        root.bottomRight = GetQuadTree(grid, rowMid+1, rowEnd, colMid+1, colEnd);

        // 如果四个子节点都是叶子节点且值相同，合并到一个新节点（设置为叶子节点）
        if(root.topLeft.isLeaf && root.topRight.isLeaf && root.bottomLeft.isLeaf && root.bottomRight.isLeaf
            && root.topLeft.val == root.topRight.val
            && root.topLeft.val == root.bottomLeft.val
            && root.topLeft.val == root.bottomRight.val) {
            return new Node(root.topLeft.val, true);
        }

        return root;
    }
}
```

## [23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入：lists = []
输出：[]
```

**示例 3：**

```
输入：lists = [[]]
输出：[]
```

> **思路：将k个链表不断二分递归拆解成两两合并的子问题，再通过合并两个有序链表的操作自底向上逐层合并，最终得到完整的有序链表，时间复杂度为O(Nlogk)。**

```cs
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
public class Solution {
    public ListNode MergeKLists(ListNode[] lists) {
        int count = lists.Length;
        if(count == 0) {
            return null;
        }

        return MergeKListsHelper(lists, 0, count-1);
    }

    // 辅助函数：递归地将链表数组分成两半，分别合并
    private ListNode MergeKListsHelper(ListNode[] lists, int start, int end) {
        if(start == end) {
            return lists[start];
        }

        int mid = start + (end - start) / 2;
        ListNode leftList = MergeKListsHelper(lists, start, mid);
        ListNode rightList = MergeKListsHelper(lists, mid + 1, end);

        return MergeTwoLists(leftList, rightList);
    }

    // 辅助函数：合并两个有序链表
    private ListNode MergeTwoLists(ListNode list1, ListNode list2) {
        if(list1 == null) {
            return list2;
        }
        if(list2 == null) {
            return list1;
        }
        if(list1.val < list2.val) {
            list1.next = MergeTwoLists(list1.next, list2);
            return list1;
        } else {
            list2.next = MergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```

# 【Kadane算法】

## [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组**是数组中的一个连续部分。

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

> **思路：Kadane算法。**
>
> 1. **初始化：`maxCurrent` 和 `maxGlobal` 设为数组第一个元素。**
> 2. **遍历数组：从第二个元素开始，计算以当前元素结尾的最大子数组和（`maxCurrent`），并更新全局最大值（`maxGlobal`）。**
> 3. **返回结果：最终 `maxGlobal` 就是整个数组的最大子数组和。**

```cs
public class Solution {
    public int MaxSubArray(int[] nums) {
        int count= nums.Length;

        int maxCurrent = nums[0]; // 当前元素结尾的最大子数组和
        int maxGlobal = nums[0]; // 全局最大子数组和

        for(int i=1; i<count; i++) {
            maxCurrent = Math.Max(nums[i], maxCurrent + nums[i]); // 决定要不要带上前面积累的和
            maxGlobal = Math.Max(maxGlobal, maxCurrent); // 更新全局最大子数组和
        }

        return maxGlobal;
    }
}
```

## [918. 环形子数组的最大和](https://leetcode.cn/problems/maximum-sum-circular-subarray/)

给定一个长度为 `n` 的**环形整数数组** `nums` ，返回 *`nums` 的非空 **子数组** 的最大可能和* 。

**环形数组** 意味着数组的末端将会与开头相连呈环状。形式上， `nums[i]` 的下一个元素是 `nums[(i + 1) % n]` ， `nums[i]` 的前一个元素是 `nums[(i - 1 + n) % n]` 。

**子数组** 最多只能包含固定缓冲区 `nums` 中的每个元素一次。形式上，对于子数组 `nums[i], nums[i + 1], ..., nums[j]` ，不存在 `i <= k1, k2 <= j` 其中 `k1 % n == k2 % n` 。

**示例 1：**

```
输入：nums = [1,-2,3,-2]
输出：3
解释：从子数组 [3] 得到最大和 3
```

**示例 2：**

```
输入：nums = [5,-3,5]
输出：10
解释：从子数组 [5,5] 得到最大和 5 + 5 = 10
```

**示例 3：**

```
输入：nums = [3,-2,2,-3]
输出：3
解释：从子数组 [3] 和 [3,-2,2] 都可以得到最大和 3
```

> **思路一：暴力解法（会超时），在每个起始位置都做一次Kadane算法求最大子数组和。**

```cs
public class Solution {
    public int MaxSubarraySumCircular(int[] nums) {
        int result = int.MinValue;
        // 对每个位置求最大子数组和
        for(int i=0; i<nums.Length; i++) {
            result = Math.Max(result, MaxSubarraySum(nums, i));
        }
        return result;
    }

    private int MaxSubarraySum(int[] nums, int startPos) {
        int n = nums.Length;
        int maxCurrent = nums[startPos];
        int maxGlobal = nums[startPos];
        int pos = startPos; // 记录当前索引

        for (int i = 1; i < n; i++) {
            pos = (pos + 1) % n; // 下一个元素的位置
            maxCurrent = Math.Max(nums[pos], maxCurrent + nums[pos]);
            maxGlobal = Math.Max(maxGlobal, maxCurrent);
        }
        return maxGlobal;
    }
}
```

> **思路二：取反。环形情况的最大子数组和可能跨越数组的首尾，比如 `[5, -3, 5]` 的最大子数组可以是 `[5, 5]`（首尾相连），这种情况相当于数组总和减去中间的最小子数组和 `[-3]`。**

```cs
public class Solution {
    public int MaxSubarraySumCircular(int[] nums) {
        int total = nums[0]; // 记录数组总和
        int maxCurrent = nums[0];
        int maxGlobal = nums[0]; // 最大子数组和
        int minCurrent = nums[0];
        int minGlobal = nums[0]; // 最小子数组和

        for(int i=1; i<nums.Length; i++) {
            maxCurrent = Math.Max(nums[i], maxCurrent + nums[i]);
            maxGlobal = Math.Max(maxGlobal, maxCurrent);

            minCurrent = Math.Min(nums[i], minCurrent + nums[i]);
            minGlobal = Math.Min(minGlobal, minCurrent);

            total += nums[i];
        }

        if(maxGlobal < 0) {
            return maxGlobal;
        }

        return Math.Max(maxGlobal, total - minGlobal);
    }
}
```

# 【二分查找】

## [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

**示例 1:**

```
输入: nums = [1,3,5,6], target = 5
输出: 2
```

**示例 2:**

```
输入: nums = [1,3,5,6], target = 2
输出: 1
```

**示例 3:**

```
输入: nums = [1,3,5,6], target = 7
输出: 4
```

> **思路一：二分查找，时间复杂度为`O(log n)`**。

```cs
public class Solution {
    public int SearchInsert(int[] nums, int target) {
        return SearchInserHelper(nums, target, 0, nums.Length - 1);
    }

    private int SearchInserHelper(int[] nums, int target, int start, int end) {
        if(start == end) {
            if(nums[start] >= target) {
                return start;
            } else {
                return start + 1;
            }
        }
        int mid = start + (end - start) / 2;
        if(target > nums[mid]) {
            return SearchInserHelper(nums, target, mid+1, end);
        } else {
            return SearchInserHelper(nums, target, start, mid);
        }
    }
}
```

> **思路二：遍历数组，记录比`target`小的最后一个元素的下标，时间复杂度为`O(n)`。**

```cs
public class Solution {
    public int SearchInsert(int[] nums, int target) {
        int smallerIndex = -1;
        for(int i=0; i<nums.Length; i++) {
            if(nums[i] < target) {
                smallerIndex = i;
            } else if(nums[i] == target) {
                return i;
            }
        }

        return smallerIndex + 1;
    }
}
```

## [74. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

给你一个满足下述两条属性的 `m x n` 整数矩阵：

- 每行中的整数从左到右按非严格递增顺序排列。
- 每行的第一个整数大于前一行的最后一个整数。

给你一个整数 `target` ，如果 `target` 在矩阵中，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
输出：true
```

**示例 2：**

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
输出：false
```

> **思路一：行列遍历。**

```cs
public class Solution {
    public bool SearchMatrix(int[][] matrix, int target) {
        int m = matrix.Length;
        int n = matrix[0].Length;

        for(int row=0; row<m; row++) {
            // 遍历每一行
            if(target >= matrix[row][0] && target <= matrix[row][n - 1]) {
                // 如果target在该行的范围内，遍历列查找
                for(int col=0; col<n; col++) {
                    if(target == matrix[row][col]) {
                        return true;
                    }
                }
                return false;
            }
        }

        return false;
    }
}
```

> **思路二：二分递归查找，注意下标和行列索引的转换。**

```cs
public class Solution {
    public bool SearchMatrix(int[][] matrix, int target) {
        int m = matrix.Length;
        int n = matrix[0].Length;

        return SearchMatrixHelper(matrix, target, m, n, 0, m*n - 1);
    }

    private bool SearchMatrixHelper(int[][] matrix, int target, int m, int n, int start, int end) {
        if (start > end) {  // 确保递归终止
            return false;
        }
        int midIndex = start + (end - start) / 2;
        int midNum = matrix[midIndex / n][midIndex % n];

        if(target == midNum) {
            return true;
        }
        if(target < midNum) {
            return SearchMatrixHelper(matrix, target, m, n, start, midIndex-1);
        } else {
            return SearchMatrixHelper(matrix, target, m, n, midIndex+1, end);
        }
    }
}
```

## [162. 寻找峰值](https://leetcode.cn/problems/find-peak-element/)

峰值元素是指其值严格大于左右相邻值的元素。

给你一个整数数组 `nums`，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 **任何一个峰值** 所在位置即可。

你可以假设 `nums[-1] = nums[n] = -∞` 。

你必须实现时间复杂度为 `O(log n)` 的算法来解决此问题。 

**示例 1：**

```
输入：nums = [1,2,3,1]
输出：2
解释：3 是峰值元素，你的函数应该返回其索引 2。
```

**示例 2：**

```
输入：nums = [1,2,1,3,5,6,4]
输出：1 或 5 
解释：你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。
```

> **思路：用二分法和局部单调性逐步缩小查找范围。因为左右两侧都是负无穷，不可能一直上坡和下坡，因此：**
>
> 1. **若`nums[mid] > nums[mid + 1]`，则`[0, mid]`一定有峰值**
> 2. **若`nums[mid] <= nums[mid + 1]`，则`[mid+1, n)`一定有峰值**

```cs
public class Solution {
    public int FindPeakElement(int[] nums) {
        int left = 0;
        int right = nums.Length - 1;
        while(left < right) {
            int mid = (left + right) / 2;
            if(nums[mid] > nums[mid + 1]) {
                // mid为下坡路，有可能是山峰，或山峰的右侧，right左移缩小范围
                right = mid;
            } else {
                // 反之说明mid为上坡路，在山峰的左侧，left右移缩小范围
                left = mid + 1;
            }
        }

        return left;
    }
}
```

## [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3：**

```
输入：nums = [1], target = 0
输出：-1
```

> **思路一：暴力循环查找。**

```cs
public class Solution {
    public int Search(int[] nums, int target) {
        int result = -1;
        for(int i=0; i<nums.Length; i++) {
            if(nums[i] == target) {
                result = i; 
                break;
            }
        }

        return result;
    }
}
```

> **思路二：二分查找，找到有序区间，根据target在不在有序区间内，左移左边界或右移有边界，逐步缩小区间。**

```cs
public class Solution {
    public int Search(int[] nums, int target) {
        int left = 0;
        int right = nums.Length - 1;
        while(left <= right) {
            int mid = (left + right) / 2;
            if(nums[mid] == target) {
                return mid;
            }

            if(nums[left] <= nums[mid]) {
                // 说明[left, mid]一定有序
                if(nums[left] <= target && target < nums[mid]) {
                    // target在[nums[left], nums[mid])范围内，right左移
                    right = mid - 1;
                } else {
                    // target不在范围内，left右移
                    left = mid + 1;
                }
            } else {
                // 说明[mid, right]一定有序
                if(nums[mid] < target && target <= nums[right]) {
                    // target在(nums[mid], nums[right]]范围内，left右移
                    left = mid + 1;
                } else {
                    // target不在范围内，right左移
                    right = mid - 1;
                }
            }
        }

        return -1;
    }
}
```

## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

> **思路一：暴力循环查找。**

```cs
public class Solution {
    public int[] SearchRange(int[] nums, int target) {
        int left = -1; int right = -1;
        for(int i=0; i<nums.Length; i++) {
            if(nums[i] == target) {
                if(left == -1) {
                    left = i;
                }
                right = i;
            }
        }

        return new int[]{left, right};
    }
}
```

> **思路二：使用两次二分查找分别确定目标的左边界和右边界。以查找左边界为例，二分值`nums[mid]`有三种情况：**
>
> - **`nums[mid] < target`**
>   **目标值在右半部分，调整 `left = mid + 1`。**
> - **`nums[mid] > target`**
>   **目标值在左半部分，调整 `right = mid - 1`。**
> - **`nums[mid] == target`**
>   **记录当前位置 `res = mid`，并继续向左搜索（`right = mid - 1`），确保找到最左边的目标值。**

```cs
public class Solution {
    public int[] SearchRange(int[] nums, int target) {
        int leftBound = FindLeftBound(nums, target);
        int rightBound = FindRightBound(nums, target);
        return new int[]{leftBound, rightBound};
    }

    // 辅助函数：找到左边界
    private int FindLeftBound(int[] nums, int target) {
        int res = -1;
        int left = 0; int right = nums.Length - 1;
        while(left <= right) {
            int mid = (left + right) / 2;
            if(nums[mid] < target) {
                left = mid + 1;
            } else if(nums[mid] > target) {
                right = mid - 1;
            } else {
                res = mid; // nums[mid] == target
                right = mid - 1; // right左移：1.继续向左查找；2.防止死循环
            }
        }

        return res;
    }

    // 辅助函数：找到右边界
    private int FindRightBound(int[] nums, int target) {
        int res = -1;
        int left = 0; int right = nums.Length - 1;
        while(left <= right) {
            int mid = (left + right) / 2;
            if(nums[mid] < target) {
                left = mid + 1;
            } else if(nums[mid] > target) {
                right = mid - 1;
            } else {
                res = mid; // nums[mid] == target
                left = mid + 1; // left右移：1.继续向右查找；2.防止死循环
            }
        }

        return res;
    }
}
```

## [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,2]`
- 若旋转 `7` 次，则可以得到 `[0,1,2,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个元素值 **互不相同** 的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2]
输出：0
解释：原数组为 [0,1,2,4,5,6,7] ，旋转 4 次得到输入数组。
```

**示例 3：**

```
输入：nums = [11,13,15,17]
输出：11
解释：原数组为 [11,13,15,17] ，旋转 4 次得到输入数组。
```

> **思路：旋转数组的特点是，旋转点左侧的元素都大于旋转点右侧的元素，可以用二分法找到旋转点，每次比较`nums[mid]和nums[right]`，存在两种情况：**
>
> 1. **nums[mid] > nums[right]，说明旋转点在(mid, right]区间内，调整left为mid+1**
> 2. **nums[mid]<=nums[right]，说明旋转点在[left, mid]区间，调整right=mid**
>
> **终止条件：当left == right时，left即为旋转点。**

```cs
public class Solution {
        public int FindMin(int[] nums) {
        int left = 0, right = nums.Length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1; // 旋转点在右侧
            } else {
                right = mid;    // 旋转点在左侧或当前mid
            }
        }
        return nums[left]; // left == right，指向最小值
    }
}
```

## [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

算法的时间复杂度应该为 `O(log (m+n))` 。

**示例 1：**

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

**示例 2：**

```
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

> **思路：每次递归中，比较两个数组的第 `index/2` 个元素（基于下标），元素较小的那个数组的前 `index/2` 个元素不可能包含第 `index` 个的元素，因此可以排除这部分元素。**
>
> **主方法：如果总长度为奇数，直接返回第k小的数（k为总长度的一半加1）；如果总长度为偶数，返回中间两个数的平均值。**
>
> **辅助方法：**
>
> - **递归处理：首先确保第一个数组是较短的数组。如果其中一个数组长度为0，直接返回另一个数组的第k小的数。如果`index`为0，返回两个数组当前起始位置的最小值。**
> - **比较和排除：比较两个数组的第`index/2`，较小的那个数组的前`index/2`个元素可以排除，继续在剩下的部分中递归查找第`index- index/2`小的数。**

```cs
public class Solution {
    public double FindMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.Length;
        int n = nums2.Length;
        int total = m + n;
        if(total % 2 == 1) {
            // 如果总长度是奇数，直接返回中间的那个数
            return (double)FindIndexInSortedArrays(nums1, 0, m, nums2, 0, n, total/2);
        } else {
            int left = FindIndexInSortedArrays(nums1, 0, m, nums2, 0, n, total/2 - 1);
            int right = FindIndexInSortedArrays(nums1, 0, m, nums2, 0, n, total/2);
            return (left+right)/2.0;
        }
    }

    // 辅助函数，找到两个数组合并后的升序数组中指定下标的数
    private int FindIndexInSortedArrays(int[] nums1, int start1, int len1, int[] nums2, int start2, int len2, int index) {
        if(len1 > len2) {
            // 确保nums1是较短的数组
            return FindIndexInSortedArrays(nums2, start2, len2, nums1, start1, len1, index);
        }

        if(len1 == 0) {
            // 如果nums1已经为空，直接在nums2中返回第index个元素
            return nums2[start2 + index];
        }

        if(index == 0) {
            // 如果index为0，直接返回两个数组第一个元素中更小的
            return Math.Min(nums1[start1], nums2[start2]);
        }

        // 计算两个数组的中间位置下标
        int mid1 = start1 + Math.Min(len1, (index+1) / 2) - 1;
        int mid2 = start2 + Math.Min(len2, (index+1) / 2) - 1;

        if(nums1[mid1] > nums2[mid2]) {
            // 排除nums2的前index/2个元素，继续在剩余部分中寻找第index - (mid2 - start2 + 1)小的元素
            return FindIndexInSortedArrays(
                nums1, start1, len1, 
                nums2, mid2 + 1, len2 - (mid2 - start2 + 1), 
                index - (mid2 - start2 + 1));
        } else {
            // 排除nums1的前index/2个元素，继续在剩余部分中寻找第index - (mid1 - start1 + 1)小的元素
            return FindIndexInSortedArrays(
                nums1, mid1 + 1, len1 - (mid1 - start1 + 1), 
                nums2, start2, len2, 
                index - (mid1 - start1 + 1));
        }
    }
}
```

# 【堆】

## [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

给定整数数组 `nums` 和整数 `k`，请返回数组中第 `**k**` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例 1:**

```
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4
```

> **思路一：堆排序，自下而上建大根堆，循环弹出堆顶元素`k-1`次并调整堆，最后得到的堆顶元素就是第`k`大元素。**

```cs
public class Solution {
    public int FindKthLargest(int[] nums, int k) {
        int heapSize = nums.Length;

        // 自下而上建堆
        for(int i = heapSize/2 - 1; i>=0; i--) {
            // 从最后一个非叶子节点开始，对每个节点进行下沉操作
            SiftDown(nums, heapSize, i);
        }

        // 弹出堆顶元素k-1次，剩下的堆顶就是第k大的元素
        for(int i=0; i<k-1; i++) {
            // 交换堆顶元素和最后一个元素，并减少堆的大小，以模拟弹出操作
            Swap(nums, 0, heapSize - 1);
            heapSize --;

            // 堆顶元素下沉，重新变成大根堆
            SiftDown(nums, heapSize, 0);
        }

        return nums[0];
    }

    // 辅助函数：大顶堆下沉操作，O(logN)，len-数组长度，index-目标节点索引
    private void SiftDown(int[] arr, int len, int index) {
        int lagest = index; // index和左右子树中最大的那个数
        int left = 2*index+1;
        int right = 2*index+2;

        if(left < len && arr[left] > arr[lagest]) {
            lagest = left;
        }

        if(right < len && arr[right] > arr[lagest]) {
            lagest = right;
        }

        if(lagest != index) {
            // 如果lagest是左子树或右子数，当前节点需要和该子节点交换
            Swap(arr, index, lagest);
            // 递归，继续下沉
            SiftDown(arr, len, lagest);
        }
    }

    // 辅助函数：交换数组中两个元素
    private void Swap(int[]arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

> **思路二：快速排序。**
>
> 1. **分区：选择一个基准值（pivot），将数组分为两部分：**
>    - - **左边 ≥ pivot**
>      - **右边 < pivot**
> 2. **递归选择**
>    - **如果基准值的索引正好是 `k-1`，直接返回。**
>    - **如果基准值索引 < `k-1`，在右半部分继续查找，否则在左半部分查找。**

```cs
public class Solution {
    public int FindKthLargest(int[] nums, int k) {
        int left = 0;
        int right = nums.Length - 1;
        while(true) {
            int pivotIndex = Partition(nums, left, right);
            if(pivotIndex == k - 1) {
                return nums[pivotIndex];
            }
            if(pivotIndex < k - 1) {
                // 在右半部分继续寻找
                left = pivotIndex + 1;
            } else {
                // 在左半部分继续寻找
                right = pivotIndex - 1;
            }
        }
    }

    // 辅助函数：分区，并返回基准值pivot的索引
    private int Partition(int nums, int left, int right) {
        int pivot = nums[right]; // 选择最右边元素作为基准值
        int i = left;
        for(int j = left; j<right; j++) {
            if(nums[j] >= pivot) {
                // 将≥pivot的元素移动到左侧
                Swap(nums, i, j);
                i++;
            }
        }

        Swap(nums, i, right); // 将pivot的值放到正确位置i
        return i;
    }

    // 辅助函数：交换数组中两个元素
    private void Swap(int[]arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```













## [295. 数据流的中位数](https://leetcode.cn/problems/find-median-from-data-stream/)

**中位数**是有序整数列表中的中间值。如果列表的大小是偶数，则没有中间值，中位数是两个中间值的平均值。

- 例如 `arr = [2,3,4]` 的中位数是 `3` 。
- 例如 `arr = [2,3]` 的中位数是 `(2 + 3) / 2 = 2.5` 。

实现 MedianFinder 类:

- `MedianFinder() `初始化 `MedianFinder` 对象。
- `void addNum(int num)` 将数据流中的整数 `num` 添加到数据结构中。
- `double findMedian()` 返回到目前为止所有元素的中位数。与实际答案相差 `10-5` 以内的答案将被接受。

**示例 1：**

```
输入
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
输出
[null, null, null, 1.5, null, 2.0]

解释
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // 返回 1.5 ((1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

> **思路一：用`List.Sort()`杀死比赛，但是会超时。**

```cs
public class MedianFinder {
    List<int> nums;

    public MedianFinder() {
        nums = new List<int>();
    }
    
    public void AddNum(int num) {
        nums.Add(num);
        nums.Sort(); // 升序排序
    }
    
    public double FindMedian() {
        int count = nums.Count;
        if(count % 2 == 1) {
            // 奇数个元素
            return (double)nums[count/2];
        } else {
            // 偶数个元素
            return (nums[count/2] + nums[count/2 - 1])/2.0;
        }
    }
}
```

> **思路二：双堆法。**
>
> 1. **`maxHeap`（大顶堆）：存储 较小的一半数，堆顶是最大值。**
> 2. **`minHeap`（小顶堆）：存储 较大的一半数，堆顶是最小值。**
> 3. **保证 `maxHeap` 的大小 等于或比 `minHeap` 大 1（这样中位数可以直接从堆顶获取）。**
> 4. **插入新数时，动态调整两个堆的平衡。**

```cs
public class MedianFinder {
    PriorityQueue<int, int> maxHeap; // 大顶堆（储存较小的一半）
    PriorityQueue<int, int> minHeap; // 小顶堆（储存较大的一半）

    public MedianFinder() {
        maxHeap = new PriorityQueue<int, int>(Comparer<int>.Create((x, y) => y.CompareTo(x))); // 堆顶元素最大，需要降序排序
        minHeap = new PriorityQueue<int, int>();
    }
    
    public void AddNum(int num) {
        if(maxHeap.Count == 0 || num <= maxHeap.Peek()) {
            // 如果大顶堆为空，或添加的数比中位数小，加入较小的一半
            maxHeap.Enqueue(num, num);
        } else {
            // 否则加入最大的一半
            minHeap.Enqueue(num, num);
        }

        // 平衡maxHeap和minHeap的个数，使得maxHeap的大小等于或比minHeap大1
        if(maxHeap.Count < minHeap.Count) {
            // maxHeap过小，要从minHeap移一个元素过来
            int minHeapNum = minHeap.Dequeue();
            maxHeap.Enqueue(minHeapNum, minHeapNum);
        } else if(maxHeap.Count > minHeap.Count + 1) {
            // maxHeap过大，要移出一个元素到minHeap
            int maxHeapNum = maxHeap.Dequeue();
            minHeap.Enqueue(maxHeapNum, maxHeapNum);
        }
    }
    
    public double FindMedian() {
        if(maxHeap.Count > minHeap.Count) {
            // 奇数个元素
            return (double)maxHeap.Peek();
        } else {
            // 偶数个元素
            return (maxHeap.Peek() + minHeap.Peek())/2.0;
        }
    }
}
```

# 【位运算】

## [67. 二进制求和](https://leetcode.cn/problems/add-binary/)

给你两个二进制字符串 `a` 和 `b` ，以二进制字符串的形式返回它们的和。

**示例 1：**

```
输入:a = "11", b = "1"
输出："100"
```

**示例 2：**

```
输入：a = "1010", b = "1011"
输出："10101"
```

> **思路一：内置API，但是字符串长度过长时会报错：Value was either too large or too small for a UInt32。**

```cs
public class Solution {
    public string AddBinary(string a, string b) {
        // 将二进制字符串转换为十进制整数
        int num1 = Convert.ToInt32(a, 2);
        int num2 = Convert.ToInt32(b, 2);

        // 相加并转换回二进制字符串
        return Convert.ToString(num1 + num2, 2);
    }
}
```

> **思路二：字符串处理，需要考虑进位。**

```cs
public class Solution {
    public string AddBinary(string a, string b) {
        StringBuilder result = new StringBuilder();
        int i = a.Length - 1;
        int j = b.Length - 1;
        int carry = 0;

        while (i >= 0 || j >= 0 || carry > 0)
        {
            int digit1 = (i >= 0) ? a[i] - '0' : 0; // 第一个数
            int digit2 = (j >= 0) ? b[j] - '0' : 0; // 第二个数
            i--; j--;

            int sum = digit1 + digit2+carry;
            result.Insert(0, sum % 2); // 奇数插入1，偶数插入0
            carry = sum / 2; // 如果sum >= 2，则进位
        }

        return result.ToString();
    }
}
```

## [190. 颠倒二进制位](https://leetcode.cn/problems/reverse-bits/)

颠倒给定的 32 位无符号整数的二进制位。

**提示：**

- 请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
- 在 Java 中，编译器使用二进制补码记法来表示有符号整数。因此，在 **示例 2** 中，输入表示有符号整数 `-3`，输出表示有符号整数 `-1073741825`。

**示例 1：**

```
输入：n = 00000010100101000001111010011100
输出：964176192 (00111001011110000010100101000000)
解释：输入的二进制串 00000010100101000001111010011100 表示无符号整数 43261596，
     因此返回 964176192，其二进制表示形式为 00111001011110000010100101000000。
```

**示例 2：**

```
输入：n = 11111111111111111111111111111101
输出：3221225471 (10111111111111111111111111111111)
解释：输入的二进制串 11111111111111111111111111111101 表示无符号整数 4294967293，
     因此返回 3221225471 其二进制表示形式为 10111111111111111111111111111111 。
```

> **思路：循环32次，每次将`res` 左移一位，取出 `n` 的最低位（`n & 1`），并将其加到 `res` 的最低位，将 `n` 右移一位。**

```csharp
public class Solution {
    public uint reverseBits(uint n) {
        uint res = 0;
        for(int i=0; i<32; i++) {
            // 循环32次
            res = (res << 1) | (n & 1); // 将res左移一位与n的最低位相加
            n = n >> 1; // n右移一位
        }

        return res;
    }
}
```

## [191. 位1的个数](https://leetcode.cn/problems/number-of-1-bits/)

给定一个正整数 `n`，编写一个函数，获取一个正整数的二进制形式并返回其二进制表达式中 设置位 的个数（也被称为汉明重量）。

**示例 1：**

```
输入：n = 11
输出：3
解释：输入的二进制串 1011 中，共有 3 个设置位。
```

**示例 2：**

```
输入：n = 128
输出：1
解释：输入的二进制串 10000000 中，共有 1 个设置位。
```

**示例 3：**

```
输入：n = 2147483645
输出：30
解释：输入的二进制串 1111111111111111111111111111101 中，共有 30 个设置位。
```

>  **思路一：内置API杀死比赛。**

```cs
public class Solution {
    public int HammingWeight(int n) {
        uint un = (uint)n;
        return BitOperations.PopCount(un);
    }
}
```

> **思路二：每次判断最后1位是否为1，然后右移1位。**

```cs
public class Solution {
    public int HammingWeight(int n) {
        int count = 0;
        while(n != 0) {
            count += (n & 1); // 检查最后一位是否为1，若是，则加入计数
            n = n >> 1; // n右移一位
        }

        return count;
    }
}
```

> **思路三：每次用`n = (n - 1) & n`去除最右边的1（`n-1`会把`n`的最右边的1变成0，并且该位右边的所有`0`都会变成`1`）。**

```cs
public class Solution {
    public int HammingWeight(int n) {
        int count = 0;
        while(n != 0) {
            n = (n - 1) & n;
            count ++;
        }

        return count;
    }
}
```

## [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

**示例 1 ：**

```
输入：nums = [2,2,1]

输出：1
```

**示例 2 ：**

```
输入：nums = [4,1,2,1,2]

输出：4
```

**示例 3 ：**

```
输入：nums = [1]

输出：1
```

> **思路：利用异或运算的性质，出现两次的数字在异或后会被抵消，只出现一次的数字会保留下来。**
>
> 1. **任何数和0异或都是它本身：`a ^ 0 = a`**
> 2. **任何数和自身异或都是0：`a ^ a = 0`**
> 3. **满足交换律和结合律：`a ^ b ^ a = (a ^ a) ^ b = 0 ^ b = b`**

```cs
public class Solution {
    public int SingleNumber(int[] nums) {
        int result = 0;
        foreach(int num in nums) {
            result = result ^ num;
        }
        return result;
    }
}
```

## [137. 只出现一次的数字 II](https://leetcode.cn/problems/single-number-ii/)

给你一个整数数组 `nums` ，除某个元素仅出现 **一次** 外，其余每个元素都恰出现 **三次 。**请你找出并返回那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法且使用常数级空间来解决此问题。

**示例 1：**

```
输入：nums = [2,2,3,2]
输出：3
```

**示例 2：**

```
输入：nums = [0,1,0,1,0,1,99]
输出：99
```

> **思路：统计所有数字的每一位上1出现的总次数，对于出现三次的数字，每位上的1会出现3次，对于只出现一次的数字，每位上的1只会出现1次或0次，因此每位上的1出现的次数对3取模就得到只出现1次的数字在该位的值。**

```cs
public class Solution {
    public int SingleNumber(int[] nums) {
        int result = 0;
        for(int i=0; i<32; i++) {
            // 遍历int32位中的每一位

            // 遍历所有数字，统计该位中1出现的次数
            int sum = 0; 
            foreach(int num in nums) {
                sum += (num >> i) & 1;
            }

            // 对3取模得到只出现一次的数字在该位的值，赋在result的对应位上
            if(sum % 3 != 0) {
                result = result | (1<<i);
            }
        }

        return result;
    }
}
```

## [201. 数字范围按位与](https://leetcode.cn/problems/bitwise-and-of-numbers-range/)

给你两个整数 `left` 和 `right` ，表示区间 `[left, right]` ，返回此区间内所有数字 **按位与** 的结果（包含 `left` 、`right` 端点）。

**示例 1：**

```
输入：left = 5, right = 7
输出：4
```

**示例 2：**

```
输入：left = 0, right = 0
输出：0
```

**示例 3：**

```
输入：left = 1, right = 2147483647
输出：0
```

> **思路一：从`m`到`n`的所有数字的按位与结果等于 `m` 和 `n` 的公共前缀后面补零，逐步右移 `m` 和 `n`，直到它们相等，记录右移的次数；然后左移相同的次数，得到公共前缀。**

```cs
public class Solution {
    public int RangeBitwiseAnd(int left, int right) {
        int shift = 0; // 记录右移的位数

        // left和right不断右移，直到相等
        while(left != right) {
            left = left >> 1;
            right = right >> 1;
            shift ++;
        }

        // 最后left（right）是原来left和right的公共前缀，再左移shift位得到按位与结果
        return left << shift;
    }
}
```

> **思路二：Brian Kernighan算法，每次对`n`和 `number−1` 之间进行按位与运算后，`number`中最右边的1会被抹去变成0，因此可以不断清除`n`最右边的 1，直到它小于或等于`m`，最终得到的`n`就是公共前缀后面补0。**

```cs
public class Solution {
    public int RangeBitwiseAnd(int left, int right) {
        while(left < right) {
            right = right & (right - 1);
        }

        return right;
    }
}
```

## [9. 回文数](https://leetcode.cn/problems/palindrome-number/)

给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

- 例如，`121` 是回文，而 `123` 不是。

**示例 1：**

```
输入：x = 121
输出：true
```

**示例 2：**

```
输入：x = -121
输出：false
解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例 3：**

```
输入：x = 10
输出：false
解释：从右向左读, 为 01 。因此它不是一个回文数。
```

> **思路一：转换成字符串，变成判断是否是回文字符串的问题。**

```cs
public class Solution {
    public bool IsPalindrome(int x) {
        if (x < 0) {
            return false;
        }
        else if (x < 10) {
            return true;
        }

        string str = x.ToString();
        int left = 0; int right = str.Length - 1;
        while(left < right) {
            if (str[left] != str[right]) {
                return false;
            }
            left++; right--;
        }

        return true;
    }
}
```

> **思路二：反转数字，反转后的结果与原数字比较是否相等。**

```cs
public class Solution {
    public bool IsPalindrome(int x) {
        if (x < 0) {
            return false;
        }
        else if (x < 10) {
            return true;
        }

        int originalNum = x;
        int reversedNum = 0;
        while(x > 0) {
            reversedNum = reversedNum * 10 + x % 10; // 得到最后一位，赋给reversedNum
            x = x / 10; // 丢弃最后一位
        }

        return reversedNum == originalNum;
    }
}
```

## [66. 加一](https://leetcode.cn/problems/plus-one/)

给定一个由 **整数** 组成的 **非空** 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

**示例 1：**

```
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
```

**示例 2：**

```
输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
```

**示例 3：**

```
输入：digits = [9]
输出：[1,0]
解释：输入数组表示数字 9。
加 1 得到了 9 + 1 = 10。
因此，结果应该是 [1,0]。
```

> **思路一（会溢出）：数组转换成整型，+1后再转换回数组。**

```cs
public class Solution {
    public int[] PlusOne(int[] digits) {
        int num = 0;
        foreach(int digit in digits) {
            num = num * 10 + digit;
        }

        num = num + 1;
        List<int> result = new List<int>();

        while(num > 0) {
            result.Insert(0, num % 10);
            num = num / 10;
        }

        return result.ToArray();
    }
}
```

> **思路二：直接操作数组，模拟手工加法过程。从数组的最后一位开始遍历，如果当前位小于 9，直接加 1 并返回结果；如果当前位是 9，则将其置为 0，并继续向高位进位；如果所有位都是 9（例如 `[9,9,9]`），则加 1 后会多出一位（`[1,0,0,0]`），创建一个长度增加 1 的新数组，并将最高位设为 1。**

```cs
public class Solution {
    public int[] PlusOne(int[] digits) {
        for(int i = digits.Length - 1; i >= 0; i--) {
            if(digits[i] < 9) {
                // 如果当前位小于 9，直接加 1 并返回结果
                digits[i] ++;
                return digits;
            }

            digits[i] = 0; // 进位
        }

        // 如果所有的位都为9，需要增加一位为1xxxx...
        int[] newDigits = new int[digits.Length + 1];
        newDigits[0] = 1;
        return newDigits;
    }
}
```

## [172. 阶乘后的零](https://leetcode.cn/problems/factorial-trailing-zeroes/)

给定一个整数 `n` ，返回 `n!` 结果中尾随零的数量。

提示 `n! = n * (n - 1) * (n - 2) * ... * 3 * 2 * 1`

**示例 1：**

```
输入：n = 3
输出：0
解释：3! = 6 ，不含尾随 0
```

**示例 2：**

```
输入：n = 5
输出：1
解释：5! = 120 ，有一个尾随 0
```

**示例 3：**

```
输入：n = 0
输出：0
```

> **思路：末尾有多少个零取决于这个数能被 10 整除多少次，而 10 可以分解为 2 × 5。因此，`n!` 末尾的零的数量是由 `n!` 的质因数分解中 2 和 5 的个数决定的。由于在阶乘中，2 的因子比 5 的因子多（偶数比 5 的倍数更频繁出现），所以末尾零的数量由 `n!` 中 5 的因子的个数决定。**
>
> 1. **遍历 `n` 的每个可能的 5 的幂次（5, 25, 125, ...）。**
> 2. **将 `n` 除以这些幂次并累加商，得到 5 的因子总数。**

```cs
public class Solution {
    public int TrailingZeroes(int n) {
        int count = 0;
        while (n > 0) {
            n /= 5;
            count += n;
        }
        return count;
    }
}
```

## [69. x 的平方根 ](https://leetcode.cn/problems/sqrtx/)

给你一个非负整数 `x` ，计算并返回 `x` 的 **算术平方根** 。

由于返回类型是整数，结果只保留 **整数部分** ，小数部分将被 **舍去 。**

**注意：**不允许使用任何内置指数函数和算符，例如 `pow(x, 0.5)` 或者 `x ** 0.5` 。

**示例 1：**

```
输入：x = 4
输出：2
```

**示例 2：**

```
输入：x = 8
输出：2
解释：8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。
```

> **思路一：线性搜索。**

```cs
public class Solution {
    public int MySqrt(int x) {
        if (x == 0) return 0;
        int result = 0;
        for(int i = 1; i <= x / i; i++) { // 避免溢出
            result = i;
        }

        return result;
    }
}
```

> **思路二：二分搜索。**

```cs
public class Solution {
    public int MySqrt(int x) {
        if (x == 0) return 0;
        
        int result = 0;
        int left = 1; int right = x;
        while(left <= right) {
            int mid = (left + right) / 2;
            if(mid <= x / mid) { // 防止溢出
                result = mid;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return result;
    }
}
```

## [50. Pow(x, n)](https://leetcode.cn/problems/powx-n/)

实现 pow(x, n)，即计算 `x` 的整数 `n` 次幂函数（即，`xn` ）。

**示例 1：**

```
输入：x = 2.00000, n = 10
输出：1024.00000
```

**示例 2：**

```
输入：x = 2.10000, n = 3
输出：9.26100
```

**示例 3：**

```
输入：x = 2.00000, n = -2
输出：0.25000
解释：2-2 = 1/22 = 1/4 = 0.25
```

> **思路：二分递归法，将 `x^n` 分解为 `x^(n/2) * x^(n/2)`（如果 `n` 是偶数），或者 `x^(n/2) * x^(n/2) * x`（如果 `n` 是奇数）。**

```cs
public class Solution {
    public double MyPow(double x, int n) {
        if(n == 0) {
            return 1.0;
        }

        if(n < 0) {
            // 处理 n = -2147483648 的情况，避免溢出
            if(n == int.MinValue) {
                return 1.0 / (MyPow(x, -(n + 1)) * x);
            }
            // 转换成正数幂再取1/n
            return 1.0 / MyPow(x, -n);
        }

        double half = MyPow(x, n / 2);
        if(n % 2 == 1) {
            // 奇数，要多乘一个x
            return half * half * x;
        } else {
            // 偶数
            return half * half;
        }
    }
}
```

