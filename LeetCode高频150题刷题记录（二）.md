# LeetCode高频150题刷题记录（二）

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

