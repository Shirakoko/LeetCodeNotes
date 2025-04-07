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

