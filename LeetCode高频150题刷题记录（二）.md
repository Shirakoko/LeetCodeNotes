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

