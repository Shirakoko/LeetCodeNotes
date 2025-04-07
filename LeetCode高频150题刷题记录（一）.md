# LeetCode经典150题做题笔记（一）

# 【数组/字符串】

## [88.合并两个有需数组](https://leetcode.cn/problems/merge-sorted-array?envType=study-plan-v2&envId=top-interview-150)

给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

**示例 2：**

```
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
```

**示例 3：**

```
输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。
```

> **思路：双指针，倒序填充第一个数组。**

```C#
public class Solution {
    public void Merge(int[] nums1, int m, int[] nums2, int n) {
        int p1 = m-1; // 数组1的指针
        int p2 = n-1; // 数组2的指针

        int current = m+n-1; // 整个数组的指针

        while(p1>=0 && p2>=0)
        {
            if(nums1[p1] > nums2[p2])
            {
                nums1[current] = nums1[p1];
                p1--;
            }
            else
            {
                nums1[current] = nums2[p2];
                p2--;
            }
            current--;
        }

        while(p2>=0)
        {
            nums1[current] = nums2[p2];
            p2--;
            current--;
        }
    }
}
```

## [27.移除元素](https://leetcode.cn/problems/remove-element?envType=study-plan-v2&envId=top-interview-150)

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素。元素的顺序可能发生改变。然后返回 `nums` 中与 `val` 不同的元素的数量。

假设 `nums` 中不等于 `val` 的元素数量为 `k`，要通过此题，您需要执行以下操作：

- 更改 `nums` 数组，使 `nums` 的前 `k` 个元素包含不等于 `val` 的元素。`nums` 的其余元素和 `nums` 的大小并不重要。
- 返回 `k`。

**用户评测：**

评测机将使用以下代码测试您的解决方案：

```
int[] nums = [...]; // 输入数组
int val = ...; // 要移除的值
int[] expectedNums = [...]; // 长度正确的预期答案。
                            // 它以不等于 val 的值排序。

int k = removeElement(nums, val); // 调用你的实现

assert k == expectedNums.length;
sort(nums, 0, k); // 排序 nums 的前 k 个元素
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有的断言都通过，你的解决方案将会 **通过**。

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2,_,_]
解释：你的函数函数应该返回 k = 2, 并且 nums 中的前两个元素均为 2。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3,_,_,_]
解释：你的函数应该返回 k = 5，并且 nums 中的前五个元素为 0,0,1,3,4。
注意这五个元素可以任意顺序返回。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

> **思路：指针移动，跳过需要剔除的元素。**

```C#
public class Solution {
    public int RemoveElement(int[] nums, int val) {
        int current = 0; // 指针
        for(int i=0; i<nums.Length; i++)
        {
            if(nums[i] != val)
            {
                // 跳过需要剔除的元素
                nums[current] = nums[i];
                current++;
            }
        }
        return current;
    }
}
```

## [26.删除数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array?envType=study-plan-v2&envId=top-interview-150)

给你一个 **非严格递增排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

> **思路：指针移动，跳过重复元素。**

```c#
public class Solution {
    public int RemoveDuplicates(int[] nums) {
        int current = 0; // 新数组指针
        for(int i=0; i<nums.Length; i++)
        {
            if(nums[i] != nums[current])
            {
                // 修改后一位元素为nums[i]
                current++; 
                nums[current] = nums[i];
            }
        }
        return current+1;
    }
}
```

## [80.删除有序数组中的重复项Ⅱ](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii?envType=study-plan-v2&envId=top-interview-150)

给你一个有序数组 `nums` ，请你原地删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在原地并在使用 O(1) 额外空间的条件下完成。

**说明：**

为什么返回数值是整数，但输出的答案是数组呢？

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**示例 1：**

```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3。 不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,1,2,3,3]
输出：7, nums = [0,0,1,1,2,3,3]
解释：函数应返回新长度 length = 7, 并且原数组的前七个元素被修改为 0, 0, 1, 1, 2, 3, 3。不需要考虑数组中超出新长度后面的元素。
```

> **思路：滑动窗口，右指针每次移动1直到数组末尾，左指针遇到不重复数字时才移动。**

```c#
public class Solution {
    public static int LENGTH = 2;
    public int RemoveDuplicates(int[] nums) {
        if (nums.Length <= 2) {
            return nums.Length;
        }
        int left = 0; // 滑动窗口左指针
        int right = LENGTH; // 滑动窗口右指针
        for(; right<nums.Length; right++)
        {
            if(nums[left] != nums[right])
            {
                // 遇到不重复数字时左指针才移动
                nums[left+LENGTH] = nums[right];
                left++;
            }
        }

        return left+LENGTH;
    }
}
```

## [169.多数元素](https://leetcode.cn/problems/majority-element?envType=study-plan-v2&envId=top-interview-150)

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1：**

```
输入：nums = [3,2,3]
输出：3
```

**示例 2：**

```
输入：nums = [2,2,1,1,1,2,2]
输出：2
```

> **思路：摩尔投票算法**
>
> ​	**候选元素：假设当前元素是多数元素。**
>
> ​	**计数：遍历数组，如果当前元素与候选元素相同，则计数加 1；否则计数减 1。**
>
> ​	**重新选择候选元素：如果计数减到 0，则重新选择当前元素作为候选元素。**
>
> ​	**最终验证：遍历结束后，候选元素就是多数元素。**

```c#
public class Solution {
    public int MajorityElement(int[] nums) {
        int candidate = nums[0]; // 多数元素候选人
        int count = 0; // 投票计数

        for(int i=0; i<nums.Length; i++)
        {
            if(count == 0)
            {
                // 计数归零时重新选候选元素
                candidate = nums[i];
            }
            
            if(nums[i] == candidate)
            {
                count ++;
            }
            else
            {
                count --;
            }
        }
        return candidate;
    }
}
```

## [189.轮转数组](https://leetcode.cn/problems/rotate-array?envType=study-plan-v2&envId=top-interview-150)

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

**示例 1:**

```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
```

**示例 2:**

```
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
```

> **思路：三次翻转数组。**
>
> ​	**第一次：翻转整个数组。**
>
> ​	**第二次：翻转0到k-1。**
>
> ​	**第三次：翻转k到length-1。**

```c#
public class Solution {
    public void Rotate(int[] nums, int k) {
        k = k % nums.Length;
        reverse(nums, 0, nums.Length-1);
        reverse(nums, 0, k-1);
        reverse(nums, k, nums.Length-1);
    }

    public void reverse(int[] array, int left, int right)
    {
        for(;left<right; left++, right--)
        {
            int temp = array[left];
            array[left] = array[right];
            array[right] = temp;
        }
    }
}
```

## [121.买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock?envType=study-plan-v2&envId=top-interview-150)

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

> **思路：动态规划，每天动态更新最大利润和最低价格。**

```c#
public class Solution {
    public int MaxProfit(int[] prices) {
        if(prices.Length == 0) {
            return 0;
        }

        int maxProfit = 0; // 最大利润，初始化为0
        int minPrice = prices[0]; // 最低价格，初始化为第0天的价格

        for(int i=0; i<prices.Length; i++)
        {
            minPrice = Math.Min(minPrice, prices[i]);
            maxProfit = Math.Max(maxProfit, prices[i] - minPrice);
        }

        return maxProfit;
    }
}
```

## [122.买卖股票的最佳时机Ⅱ](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii?envType=study-plan-v2&envId=top-interview-150)

给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。

返回 *你能获得的 **最大** 利润* 。

**示例 1：**

```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4。
随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3。
最大总利润为 4 + 3 = 7 。
```

**示例 2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4。
最大总利润为 4 。
```

**示例 3：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 交易无法获得正利润，所以不参与交易可以获得最大利润，最大利润为 0。
```

> **思路一：动态规划，由于每天的操作（买入、卖出或不操作）会影响未来的利润，需要记录每天的状态，以便做出最优决策。**
>
> **为了描述每天的状态，定义：**
>
> ​	**`dp[i][0]`：第 `i` 天结束时不持有股票的最大利润。**
>
> ​	**`dp[i][1]`：第 `i` 天结束时持有股票的最大利润。**
>
> **找到 `dp[i][0]` 和 `dp[i][1]` 之间的关系：**
>
> ​	**`dp[i][0]`：第 `i` 天不持有股票**
>
> ​		**可能是前一天也不持有股票，即 `dp[i-1][0]`。**
>
> ​		**也可能是前一天持有股票，今天卖出，即 `dp[i-1][1] + prices[i]`。**
>
> ​	**因此：**
> $$
> dp[i][0]=max⁡(dp[i−1][0],dp[i−1][1]+prices[i])
> $$
> 
>
> ​	**`dp[i][1]`：第 `i` 天持有股票**
>
> ​		**可能是前一天也持有股票，即 `dp[i-1][1]`。**
>
> ​		**也可能是前一天不持有股票，今天买入，即 `dp[i-1][0] - prices[i]`。**
>
> ​	**因此：**
> $$
> dp[i][1]=max⁡(dp[i−1][1],dp[i−1][0]−prices[i])
> $$
> **第 0 天（`i = 0`）：**
>
> ​	**如果不持有股票，利润为 0，即 `dp[0][0] = 0`。**
>
> ​	**如果持有股票，利润为 `-prices[0]`（因为买入股票花费了 `prices[0]`），即 `dp[0][1] = -prices[0]`。**
>
> **从第 1 天开始遍历 `prices` 数组，更新 `dp[i][0]` 和 `dp[i][1]`。**
>
> **最终的最大利润是 `dp[n-1][0]`，因为最后一天不持有股票才能获得最大利润。**

```c#
public class Solution {
    public int MaxProfit(int[] prices) {
        if(prices.Length <= 1) {
            return 0;
        }

        // 设 dp[i][0] 表示第 i 天结束时不持有股票的最大利润
        // 设 dp[i][1] 表示第 i 天结束时持有股票的最大利润。
        int[,] dp = new int[prices.Length,2];

        dp[0,0] = 0; // 第0天不持有股票的最大利润
        dp[0,1] = -prices[0]; // 第0天持有股票的最大利润

        // 状态转移
        for(int i=1; i<prices.Length; i++)
        {
            // 第i天不持有股票可能是：1.前一天也不持有；2.前一天持有今天卖出
            dp[i, 0] = Math.Max(dp[i-1, 0], dp[i-1, 1] + prices[i]);
            // 第i天持有股票可能是：1.前一天不持有今天买入；2.前一天持有
            dp[i, 1] = Math.Max(dp[i-1, 0] - prices[i], dp[i-1, 1]);
        }

        return dp[prices.Length-1, 0];
    }
}
```

> **思路二：贪心算法，把所有的涨幅都赚到，所有的跌幅都躲过。**

```c#
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        for (int i = 1; i < prices.length; i++) {
        	if (prices[i] > prices[i - 1]) {
        		maxProfit += prices[i] - prices[i - 1];
			}
        }
        return maxProfit;
    }
}
```

## [55.跳跃游戏](https://leetcode.cn/problems/jump-game?envType=study-plan-v2&envId=top-interview-150)

给你一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

**示例 2：**

```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

> **思路：动态更新每一位置的最大可达处索引，在遍历时检查是否已到达终点，以及是否永远不能到达终点。**

```c#
public class Solution {
    public bool CanJump(int[] nums) {
        if (nums.Length <= 1) {
            return true;
        }
        int destination = nums.Length - 1; // 终点处索引
        int maxReachIndex = 0; // 每一个位置的最大可达处索引
    
        for(int i=0; i<=maxReachIndex; i++) {
            // 动态更新maxReachIndex
            maxReachIndex = Math.Max(maxReachIndex, i+nums[i]);
            // 检查是否已经能到终点
            if (maxReachIndex >= destination) {
                return true;
            }
            // 如果当前能够到达的最远位置不超过当前位置，无法继续跳跃
            if (maxReachIndex <= i) {
                return false;
            }
        }

       return false;
   }
}
```

> **整理优化：**

```c#
public class Solution {
    public bool CanJump(int[] nums) {
        int n = nums.Length;
        int maxReachIndex = 0;
        for (int i = 0; i < n && maxReachIndex < n - 1; i++) {
            if (maxReachIndex < i) {
                return false;
            }
            maxReachIndex = Math.Max(maxReachIndex, i + nums[i]);
        }
        return true;
    }
}
```

## [45.跳跃游戏Ⅱ](https://leetcode.cn/problems/jump-game-ii?envType=study-plan-v2&envId=top-interview-150)

给定一个长度为 `n` 的**0 索引**整数数组 `nums`。初始位置为 `nums[0]`。

每个元素 `nums[i]` 表示从索引 `i` 向后跳转的最大长度。换句话说，如果你在 `nums[i]` 处，你可以跳转到任意 `nums[i + j]` 处:

- `0 <= j <= nums[i]` 
- `i + j < n`

返回到达 `nums[n - 1]` 的最小跳跃次数。生成的测试用例可以到达 `nums[n - 1]`。

**示例 1:**

```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**示例 2:**

```
输入: nums = [2,3,0,1,4]
输出: 2
```

> **思路一：贪心算法，反向查找上一个跳跃点，每次选择距离当前跳跃点最远的跳跃点**

```c#
public class Solution {
    public int Jump(int[] nums) {
        if(nums.Length <= 1) {
            return 0;
        }

        /* 贪心算法：反向查找跳跃点 */
        int position = nums.Length - 1; // 跳跃点，初始化为终点位置
        int steps = 0; // 跳跃步数
        while(position > 0) {
            // 找到距离当前跳跃点最远的跳跃点
            for(int i=0; i<position; i++) {
                if(i+nums[i] >= position) {
                    // 设置该点为下一个跳跃点
                    position = i;
                    steps++;
                    break;
                }
            }
        }
        return steps;
    }
}
```

> **思路二：贪心算法，正向查找下一个跳跃点，每次选择距离起点最远的跳跃点**

```c#
public class Solution {
    public int Jump(int[] nums) {
        if(nums.Length <= 1) {
            return 0;
        }

        /* 贪心算法：正向查找跳跃点 */
        int destination = nums.Length - 1; // 终点处索引
        int position = 0; // 跳跃点，初始化为起点位置
        int maxPosition = 0; // 每一个位置的最大可达处索引
        int steps = 0;
        for (int i=0; i<destination; i++) {
            maxPosition = Math.Max(maxPosition, i+nums[i]); // 每个位置能到达的最远位置
            if (i == position) {
                // 遍历到上个跳跃点能到达的最远位置时，更新跳跃点和步数
                position = maxPosition;
                steps++;
            }
        }
        return steps;
    }
}
```

> **思路三：动态规划**
>
> ​	**定义状态：设 `dp[i]` 表示从起点到位置 `i` 的最小跳跃次数。**
>
> ​	**状态转移方程：对于每个位置 `i`，遍历从 `0` 到 `i-1` 的所有位置 `j`：如果从位置 `j` 可以跳到位置 `i`（即 `j + nums[j] >= i`），则更新 `dp[i]`：**
> $$
> dp[i]=min⁡(dp[i],dp[j]+1)
> $$
> ​	**初始化数组：`dp[0] = 0`，因为起点不需要跳跃；其他位置的 `dp[i]` 初始化为一个较大的值（如 `int.MaxValue`），表示暂时无法到达。**
>
> ​	**遍历数组：遍历数组中的每个位置 `i`，从 `1` 到 `n-1`；对于每个位置 `i`，遍历从 `0` 到 `i-1` 的所有位置 `j`，更新 `dp[i]`。**
>
> ​	**返回结果：最终 `dp[n-1]` 就是从起点到终点的最小跳跃次数。**

```C#
public class Solution {
    public int Jump(int[] nums) {
        int n = nums.Length;
        if (n <= 1) {
            return 0; // 如果数组长度为 0 或 1，不需要跳跃
        }

        int[] dp = new int[n]; // dp[i] 表示到达位置 i 的最小跳跃次数
        for (int i = 1; i < n; i++) {
            dp[i] = int.MaxValue; // 初始化为最大值，表示暂时无法到达
        }

        dp[0] = 0; // 起点不需要跳跃

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                // 如果从位置 j 可以跳到位置 i
                if (j + nums[j] >= i) {
                    // 更新 dp[i]
                    dp[i] = Math.Min(dp[i], dp[j] + 1);
                }
            }
        }

        return dp[n - 1]; // 返回终点的最小跳跃次数
    }
}
```

## [274.H指数](https://leetcode.cn/problems/h-index?envType=study-plan-v2&envId=top-interview-150)

给你一个整数数组 `citations` ，其中 `citations[i]` 表示研究者的第 `i` 篇论文被引用的次数。计算并返回该研究者的 **`h` 指数**。

根据维基百科上 [h 指数的定义](https://baike.baidu.com/item/h-index/3991452?fr=aladdin)：`h` 代表“高引用次数” ，一名科研人员的 `h` **指数** 是指他（她）至少发表了 `h` 篇论文，并且 **至少** 有 `h` 篇论文被引用次数大于等于 `h` 。如果 `h` 有多种可能的值，**`h` 指数** 是其中最大的那个。

**示例 1：**

```
输入：citations = [3,0,6,1,5]
输出：3 
解释：给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3。
```

**示例 2：**

```
输入：citations = [1,3,1]
输出：1
```

> **思路一：先初始化H指数最高为文章篇数，依次递减，找到符合条件的H指数。**

```c#
public class Solution {
    public int HIndex(int[] citations) {
        if (citations.Length == 0) {
            return 0;
        }

        int hIndex = citations.Length; // 初始化H指数为文章篇数，从大往小找符合要求的H指数
        while (hIndex >= 0) {
            int count = 0;
            // 统计在当前H指数下符合条件的文章篇数
            for(int i=0; i<citations.Length; i++) { 
                if (citations[i] >= hIndex) {
                    count ++;
                }
            }
            if (count < hIndex) {
                // 若H指数不符合要求，就递减并再次进入循环
                hIndex--;
            } else {
                // 符合要求，就退出循环
                break;
            }
        }
        return hIndex;
    }
}
```

> **思路二：排序，遍历排序后的数组，找到第一个满足 `citations[i] < i + 1` 的位置，`i` 就是H指数。**

```c#
public class Solution {
    public int HIndex(int[] citations) {
        if (citations.Length == 0) {
            return 0;
        }

        // 对引用次数进行降序排序
        Array.Sort(citations);
        Array.Reverse(citations);

        int hIndex = 0;
        for (int i = 0; i < citations.Length; i++) {
            if (citations[i] >= i + 1) {
                hIndex = i + 1;
            } else {
                break;
            }
        }

        return hIndex;
    }
}
```

> **思路三：先排序，再二分查找。**

```c#
public class Solution {
    public int HIndex(int[] citations) {
        if (citations.Length == 0) {
            return 0;
        }

        // 对引用次数进行升序排序
        Array.Sort(citations);

        int n = citations.Length;
        int left = 0;
        int right = n;

        while(left <= right) {
            // 中间索引作为当前的H值
            int mid = left + (right - left)/2;
            if(mid == 0 || citations[n - mid] >= mid) {
                left ++; // H指数可能更大
            } else {
                right --; // H指数可能更小
            }
        }

        return right; // 返回最大的H值
    }
}
```

## [380. O(1) 时间插入、删除和获取随机元素](https://leetcode.cn/problems/insert-delete-getrandom-o1/)

实现`RandomizedSet` 类：

- `RandomizedSet()` 初始化 `RandomizedSet` 对象
- `bool insert(int val)` 当元素 `val` 不存在时，向集合中插入该项，并返回 `true` ；否则，返回 `false` 。
- `bool remove(int val)` 当元素 `val` 存在时，从集合中移除该项，并返回 `true` ；否则，返回 `false` 。
- `int getRandom()` 随机返回现有集合中的一项（测试用例保证调用此方法时集合中至少存在一个元素）。每个元素应该有 **相同的概率** 被返回。

你必须实现类的所有函数，并满足每个函数的 **平均** 时间复杂度为 `O(1)` 。

**示例：**

```
输入
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
输出
[null, true, false, true, 2, true, false, 2]

解释
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // 向集合中插入 1 。返回 true 表示 1 被成功地插入。
randomizedSet.remove(2); // 返回 false ，表示集合中不存在 2 。
randomizedSet.insert(2); // 向集合中插入 2 。返回 true 。集合现在包含 [1,2] 。
randomizedSet.getRandom(); // getRandom 应随机返回 1 或 2 。
randomizedSet.remove(1); // 从集合中移除 1 ，返回 true 。集合现在包含 [2] 。
randomizedSet.insert(2); // 2 已在集合中，所以返回 false 。
randomizedSet.getRandom(); // 由于 2 是集合中唯一的数字，getRandom 总是返回 2 。
```

> **思路：变长数组（动态数组）和哈希表的组合；变长数组：支持 O(1) 时间的随机访问，哈希表：支持 O(1) 时间的插入和删除。**
>
> ​	**插入：**
>
> ​		**将元素添加到数组末尾（O(1)）**
>
> ​		**在哈希表中记录元素的索引（O(1)）**
>
> ​	**删除：**
>
> ​		**通过哈希表找到元素在数组中的索引（O(1)）**
>
> ​		**将数组末尾的元素移动到被删除元素的位置（O(1)）**
>
> ​		**更新哈希表中末尾元素的索引（O(1)）**
>
> ​		**删除数组末尾的元素（O(1)）**
>
> ​		**删除哈希表中的元素（0(1)）**
>
> ​	**获取随机元素：**
>
> ​		**生成一个随机索引，从数组中返回对应元素（O(1)）。**

```c#
public class RandomizedSet {
    private List<int> list; // 变长数组，存储元素
    private Dictionary<int, int> dict; // 哈希表，记录元素到索引的映射
    private Random random; // 随机数生成器

    public RandomizedSet() {
        list = new List<int>();
        dict = new Dictionary<int, int>();
        random = new Random();
    }

    // 插入元素
    public bool Insert(int val) {
        if (dict.ContainsKey(val)) {
            return false; // 元素已存在
        }

        list.Add(val); // 添加到数组末尾
        dict[val] = list.Count - 1; // 记录元素索引
        return true;
    }

    // 删除元素
    public bool Remove(int val) {
        if (!dict.ContainsKey(val)) {
            return false; // 元素不存在
        }

        int index = dict[val]; // 获取元素索引
        int lastElement = list[list.Count - 1]; // 获取数组末尾元素

        // 将末尾元素移动到被删除元素的位置
        list[index] = lastElement;
        dict[lastElement] = index; // 更新末尾元素的索引

        // 删除末尾元素
        list.RemoveAt(list.Count - 1);
        dict.Remove(val); // 从哈希表中删除元素

        return true;
    }

    // 获取随机元素
    public int GetRandom() {
        int randomIndex = random.Next(list.Count); // 生成随机索引
        return list[randomIndex]; // 返回随机元素
    }
}
```

## [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

给你一个整数数组 `nums`，返回 数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内。

请 **不要使用除法，**且在 `O(n)` 时间复杂度内完成此题。

**示例 1:**

```
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```

**示例 2:**

```
输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

> **思路：前缀积乘后缀积，跳过自己**

```C#
public class Solution {
    public int[] ProductExceptSelf(int[] nums) {
        /** 前缀积*后缀积 */
        int n = nums.Length;

        int pre = 1; // 前缀积
        int suf = 1; // 后缀积

        int[] answer = new int[n];

        for(int i=0; i<n; i++)
        {
            answer[i] = pre;
            pre *= nums[i];
        }

        for(int j=n-1; j>=0; j--)
        {
            answer[j] *= suf;
            suf *= nums[j];
        }

        return answer;
    }
}
```

## [134. 加油站](https://leetcode.cn/problems/gas-station/)

在一条环路上有 `n` 个加油站，其中第 `i` 个加油站有汽油 `gas[i]` 升。

你有一辆油箱容量无限的的汽车，从第 `i` 个加油站开往第 `i+1` 个加油站需要消耗汽油 `cost[i]` 升。你从其中的一个加油站出发，开始时油箱为空。

给定两个整数数组 `gas` 和 `cost` ，如果你可以按顺序绕环路行驶一周，则返回出发时加油站的编号，否则返回 `-1` 。如果存在解，则 **保证** 它是 **唯一** 的。

**示例 1:**

```
输入: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
输出: 3
解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
```

**示例 2:**

```
输入: gas = [2,3,4], cost = [3,4,3]
输出: -1
解释:
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。
```

> **思路：贪心算法**
>
> ​	**如果总耗油量＞总加油量，一定不能绕一周，直接返回-1**
>
> ​	**如果[start, end)区间内剩余的汽油不足以到达end，说明在(start, end)区间内的任何一点出发都不能到达，下一个出发点是end+1**
>
> ​	**如果选择某个点作为start能到达n-1，则一定能绕一周，因为前面已经判断过总耗油量≤总加油量**

## [135. 分发糖果](https://leetcode.cn/problems/candy/)

`n` 个孩子站成一排。给你一个整数数组 `ratings` 表示每个孩子的评分。

你需要按照以下要求，给这些孩子分发糖果：

- 每个孩子至少分配到 `1` 个糖果。
- 相邻两个孩子评分更高的孩子会获得更多的糖果。

请你给每个孩子分发糖果，计算并返回需要准备的 **最少糖果数目** 。

**示例 1：**

```
输入：ratings = [1,0,2]
输出：5
解释：你可以分别给第一个、第二个、第三个孩子分发 2、1、2 颗糖果。
```

**示例 2：**

```
输入：ratings = [1,2,2]
输出：4
解释：你可以分别给第一个、第二个、第三个孩子分发 1、2、1 颗糖果。
     第三个孩子只得到 1 颗糖果，这满足题面中的两个条件。
```

> **思路：双向遍历；从左到右遍历，如果当前孩子的评分比左边的高，则当前孩子的糖果数比左边的多 1；从右到左遍历，如果当前孩子的评分比右边的高，则当前孩子的糖果数比右边多1；使用 `Math.Max` 确保不会覆盖从左到右遍历时已经分配的糖果数。**

```csharp
public class Solution {
    public int Candy(int[] ratings) {
        /** 双向遍历 */
        int n = ratings.Length;
        if (n == 1) {
            return 1;
        }

        int[] candy = new int[n]; // 每个小孩的糖果数组
        Array.Fill(candy, 1);

        // 从左到右遍历，确保右边评分更高的孩子获得更多糖果
        for(int i=1; i<n; i++) {
            if(ratings[i] > ratings[i-1]) {
                candy[i] = candy[i-1] + 1;
            }
        }

        // 从右到左遍历，确保坐标评分更高的孩子获得更多的糖果
        for(int i=n-2; i>=0; i--) {
            if(ratings[i] > ratings[i+1]) {
                candy[i] = Math.Max(candy[i], candy[i+1] + 1);
            }
        }

        int sum=0;
        for(int i=0; i<n; i++) {
            sum += candy[i];
        }

        return sum;
    }
}
```

## [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

**示例 2：**

```
输入：height = [4,2,0,3,2,5]
输出：9
```

> **思路一：暴力解法（会超时）。**

```c#
public class Solution {
    public int Trap(int[] height) {
        int n = height.Length;
        // 1个柱子和2个柱子不能装水
        if(n <= 2) {
            return 0;
        }

        /**暴力解法：求每个柱子上面的雨水是多少格*/
        int volume = 0;
        for(int i=1; i<n-1; i++) {
            int leftMax = 0;
            int rightMax = 0;
            GetLeftMaxAndRightMax(height, i, ref leftMax, ref rightMax);

            // 短板效应，得到左右最高柱子中相对矮的柱子的高度
            int minHeight = Math.Min(leftMax, rightMax);
            if (minHeight > height[i]) {
                volume += minHeight - height[i];
            }
        }

        return volume;
    }

    // 用一次遍历得到某个柱子左边和右边的最高柱子
    private void GetLeftMaxAndRightMax(int[] height, int pos, ref int leftMax, ref int rightMax) {
        int lMax = 0;
        int rMax = 0;
        for(int i=0; i<height.Length; i++) {
            if(i<pos)
            {
                // 左边
                if(height[i] > lMax) {
                    lMax = height[i];
                }
            }
            else if(i > pos)
            {
                // 右边
                if (height[i] > rMax) {
                    rMax = height[i];
                }
            }
        }
        leftMax = lMax;
        rightMax = rMax;
    }
}
```

> **思路二：单调栈+横向统计。**

```C#
public class Solution {
    public int Trap(int[] height) {
        int n = height.Length;
        if(n<=2) {
            return 0;
        }
        
        int volume = 0;
        Stack<int> stack = new Stack<int>();

        for(int i=0; i<n; i++)
        {
            while(stack.Count>0 && height[stack.Peek()] < height[i]) {
                int bottomIndex = stack.Pop(); // 弹出栈顶，作为凹槽的底部
                if(stack.Count == 0) {
                    break; // 如果栈为空，说明左边没有柱子，无法形成凹槽
                }
                int leftIndex = stack.Peek(); // 左边界是新的栈顶
                int H = Math.Min(height[i], height[leftIndex]) - height[bottomIndex]; // 高度
                int W = i - leftIndex - 1; // 宽度 = 右边界 - 左边界 - 1
                volume += H * W;
            }
            stack.Push(i);
        }
        return volume;
    }
}
```

> **思路三：动态规划+纵向统计，分别维护1.当前柱子左侧的最大高度数组和2.当前柱子右侧的最大高度数组；每一位置能接到的雨水根据短板效应决定。**

```c#
public class Solution {
    public int Trap(int[] height) {
        int n = height.Length;
        if(n<=2) {
            return 0;
        }
        
        int[] leftMax = new int[n]; // 当前柱子左侧的最大高度（包含自己）
        int[] rightMax = new int[n];  // 当前柱子右侧的最大高度（包含自己）

        // 遍历，计算每个柱子左侧的最大高度
        leftMax[0] = height[0];
        for(int i=1; i<n; i++) {
            leftMax[i] = Math.Max(leftMax[i-1], height[i]);
        }
        // 遍历，计算每个柱子右侧的最大高度
        rightMax[n-1] = height[n-1];
        for(int i=n-2; i>=0; i--) {
            rightMax[i] = Math.Max(rightMax[i+1], height[i]);
        }

        int volume = 0;
        for(int i=0; i<n; i++) {
            // 当前柱子上方的雨水量 = 左右最大高度的较小值 - 当前柱子的高度
            volume += Math.Min(leftMax[i], rightMax[i]) - height[i];
        }
        return volume;
    }
}
```

> **思路四：双指针法+纵向统计，用两个指针遍历数组的每个元素，再用两个变量分别维护左指针左侧的最大高度和右指针右侧的最大高度；每一位置能接到的雨水根据短板效应决定。**

```c#
public class Solution {
    public int Trap(int[] height) {
        int n = height.Length;
        if(n<=2) {
            return 0;
        }
        
        int lp = 0; // 左指针
        int rp = n-1; // 右指针

        int leftMax = height[lp]; // 左指针左侧的最大高度
        int rightMax = height[rp]; // 右指针右侧的最大高度

        int volume = 0;
        while(lp <rp)
        {
            if(height[lp] < height[rp])
            {
                lp ++;
                leftMax = Math.Max(leftMax, height[lp]);
                volume += leftMax - height[lp];
            }
            else
            {
                rp --;
                rightMax = Math.Max(rightMax, height[rp]);
                volume += rightMax - height[rp];
            }
        }
        return volume;
    }
}
```

## [13. 罗马数字转整数](https://leetcode.cn/problems/roman-to-integer/)

罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 `2` 写做 `II` ，即为两个并列的 1 。`12` 写做 `XII` ，即为 `X` + `II` 。 `27` 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。

> **思路：通常情况下，罗马数字中小的数字在右边；需要表示4或9时才出现小的数字在左边的情况；可以遍历除最后一个数字之外的所有数字，如果当前数字比下一个小就减去，否则就加上。**

```c#
public class Solution {
    public int RomanToInt(string s) {
        int total= 0;
        int n = s.Length;

        for(int i=0; i<n-1; i++) {
            int curNumber = GetNumber(s[i]);
            int nextNumber = GetNumber(s[i+1]);
            if (curNumber <nextNumber) {
                total -= curNumber;
            } else {
                total += curNumber;
            }
        }
        total += GetNumber(s[n-1]);
        return total;
    }

    public int GetNumber(char s) {
        switch(s)
        {
            case 'I':
                return 1;
            case 'V':
                return 5;
            case 'X':
                return 10;
            case 'L':
                return 50;
            case 'C':
                return 100;
            case 'D':
                return 500;
            case 'M':
                return 1000;
            default:
                return 0;
        }
    }
}
```

## [12. 整数转罗马数字](https://leetcode.cn/problems/integer-to-roman/)

七个不同的符号代表罗马数字，其值如下：

| 符号 | 值   |
| ---- | ---- |
| I    | 1    |
| V    | 5    |
| X    | 10   |
| L    | 50   |
| C    | 100  |
| D    | 500  |
| M    | 1000 |

罗马数字是通过添加从最高到最低的小数位值的转换而形成的。将小数位值转换为罗马数字有以下规则：

- 如果该值不是以 4 或 9 开头，请选择可以从输入中减去的最大值的符号，将该符号附加到结果，减去其值，然后将其余部分转换为罗马数字。
- 如果该值以 4 或 9 开头，使用 **减法形式**，表示从以下符号中减去一个符号，例如 4 是 5 (`V`) 减 1 (`I`): `IV` ，9 是 10 (`X`) 减 1 (`I`)：`IX`。仅使用以下减法形式：4 (`IV`)，9 (`IX`)，40 (`XL`)，90 (`XC`)，400 (`CD`) 和 900 (`CM`)。
- 只有 10 的次方（`I`, `X`, `C`, `M`）最多可以连续附加 3 次以代表 10 的倍数。你不能多次附加 5 (`V`)，50 (`L`) 或 500 (`D`)。如果需要将符号附加4次，请使用 **减法形式**。

给定一个整数，将其转换为罗马数字。

> **思路：罗列出所有的特征数和对应的特征字母，从大到小遍历特征数，用循环递减法，依次拼接字符串。**

```c#
public class Solution {
    public string IntToRoman(int num) {
        int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        string[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

        StringBuilder res = new StringBuilder();
        for(int i=0; i<values.Length; i++)
        {
            while(num >= values[i])
            {
                num -= values[i];
                res.Append(symbols[i]);
            }
        }

        return res.ToString();
    }
}
```

## [58. 最后一个单词的长度](https://leetcode.cn/problems/length-of-last-word/)

给你一个字符串 `s`，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中 **最后一个** 单词的长度。

**单词** 是指仅由字母组成、不包含任何空格字符的最大子字符串。

**示例 1：**

```
输入：s = "Hello World"
输出：5
解释：最后一个单词是“World”，长度为 5。
```

**示例 2：**

```
输入：s = "   fly me   to   the moon  "
输出：4
解释：最后一个单词是“moon”，长度为 4。
```

**示例 3：**

```
输入：s = "luffy is still joyboy"
输出：6
解释：最后一个单词是长度为 6 的“joyboy”。
```

> **思路：反向遍历。**

```c#
public class Solution {
    public int LengthOfLastWord(string s) {
        /** 不使用Trim和Split */
        int length = s.Length;
        int count = 0;
        // 最后一个非空格字符的位置
        int pos = length - 1;

        // 跳过末尾的空格
        while(s[pos] == ' ' && pos>=0) {
            pos--;
        }

        // 从最后一个非空格字符开始算
        for(int i=pos; i>=0; i--) {
            if(s[i] == ' ') {
                break;
            }
            count ++;
        }

        return count;
    }
}
```

## [14. 最长公共前缀](https://leetcode.cn/problems/longest-common-prefix/)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1：**

```
输入：strs = ["flower","flow","flight"]
输出："fl"
```

**示例 2：**

```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

> **思路一：纵向扫描，从第一个位置开始，比较每个字符串的相同位置的字符是否相同。**

```c#
public string LongestCommonPrefix(string[] strs) {
    if (strs == null || strs.Length == 0)
        return "";

    for (int i = 0; i < strs[0].Length; i++) {  // 以第一个字符串为基准
        char c = strs[0][i];  // 当前字符
        for (int j = 1; j < strs.Length; j++) {  // 遍历其他字符串
            if (i >= strs[j].Length || strs[j][i] != c)  // 如果字符不匹配或长度不足
                return strs[0].Substring(0, i);  // 返回当前前缀
        }
    }
    return strs[0];  // 如果所有字符都匹配，返回第一个字符串
}
```

> **思路二：横向扫描，依次比较每个字符串，逐步缩小公共前缀的范围。**

```C#
public class Solution {
    public string LongestCommonPrefix(string[] strs) {
        if (strs == null || strs.Length == 0) {
            return "";
        }

        string prefix = strs[0];  // 假设第一个字符串是最长公共前缀
        int count = strs.Length;

        for (int i = 1; i < count; i++) {
            prefix = LongestCommonPrefix(prefix, strs[i]);  // 更新 prefix
            if (prefix.Length == 0) {  // 如果 prefix 为空，直接返回
                break;
            }
        }

        return prefix;
    }

    // 辅助函数：计算两个字符串的公共前缀
    private string LongestCommonPrefix(string str1, string str2) {
        int length = Math.Min(str1.Length, str2.Length);  // 取较短字符串的长度
        int index = 0;

        // 逐个字符比较
        while (index < length && str1[index] == str2[index]) {
            index++;
        }

        return str1.Substring(0, index);  // 返回公共前缀
    }
}
```

> **思路三：二分查找，将最长公共前缀的长度作为搜索范围，通过二分查找逐步缩小范围。**

```C#
public class Solution {
    public string LongestCommonPrefix(string[] strs) {
        if (strs == null || strs.Length == 0)
            return "";

        int minLen = strs.Min(s => s.Length);  // 找到最短字符串的长度
        int low = 0, high = minLen;

        while (low <= high) {
            int mid = (low + high) / 2;
            if (IsCommonPrefix(strs, mid))
                low = mid + 1;  // 尝试更长的前缀
            else
                high = mid - 1;  // 尝试更短的前缀
        }

        return strs[0].Substring(0, high);
    }

    private bool IsCommonPrefix(string[] strs, int len) {
        string prefix = strs[0].Substring(0, len);  // 取第一个字符串的前 len 个字符
        for (int i = 1; i < strs.Length; i++) {
            if (!strs[i].StartsWith(prefix))
                return false;
        }
        return true;
    }
}
```

## [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

给你一个字符串 `s` ，请你反转字符串中 **单词** 的顺序。

**单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。

返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果字符串。

**注意：**输入字符串 `s`中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

**示例 1：**

```
输入：s = "the sky is blue"
输出："blue is sky the"
```

**示例 2：**

```
输入：s = "  hello world  "
输出："world hello"
解释：反转后的字符串中不能存在前导空格和尾随空格。
```

**示例 3：**

```Csharp
输入：s = "a good   example"
输出："example good a"
解释：如果两个单词间有多余的空格，反转后的字符串需要将单词间的空格减少到仅有一个。
```

> **思路一：调库。**

```C#
public class Solution {
    public string ReverseWords(string s) {
        return string.Join(' ', s.Split(' ').Where(str => str != "").Reverse());
    }
}
```

> **思路二：1.移除首尾空格和中间的多余空格，使字符串变成标准的用空格分隔开的一些单词；2.反转整个字符串；3.以空格为分隔符，反转每个单词。**

```csharp
public class Solution {
    public string ReverseWords(string s) {
        /** 1.移除首尾空格和中间的多余空格 */
        /** 2.反转整个字符串 */
        /** 3.反转每个单词 */
        StringBuilder res = TrimSpaces(s);
        ReverseString(res, 0, res.Length - 1);
        ReverseEachWord(res);

        return res.ToString();
    }
    /** 辅助函数1.移除空格 */
    public StringBuilder TrimSpaces(string s)
    {
        int left = 0; int right = s.Length - 1; // 左右指针
        // 去掉开头的空格
        while(left<=right && s[left] == ' ') {
            left ++;
        }

        // 去掉末尾的空格
        while(left <= right && s[right] == ' ') {
            right --;
        }

        // 去掉中间的多余空格
        // 遇到字符就加入StringBuilder，遇到空格就看StringBuilder中上一个字符是否是空格
        StringBuilder res = new StringBuilder();
        while(left <= right) {
            char c = s[left];
            if(c != ' ') {
                res.Append(c);
            } else if(res.Length > 0 && res[res.Length - 1] != ' ') {
                res.Append(c);
            }

            left++;
        }

        return res;
    }

    /** 辅助函数2.指定索引反转字符串*/
    public void ReverseString(StringBuilder str, int left, int right) {
        while(left < right) {
            char tmp = str[left];
            str[left] = str[right];
            str[right] = tmp;
            left ++; right --;
        }
    }

    /** 辅助函数3.给定一个标准的用空格分隔的单词串，反转每个单词*/
    public void ReverseEachWord(StringBuilder str) {
        int n = str.Length;
        int start = 0; int end = 0; // 单词的开头和结尾指针

        while(start < n) {
            // 找到单词的末尾
            while(end < n && str[end] != ' ') {
                end++;
            }

            // 反转单词
            ReverseString(str, start, end - 1);
            // 更新start和end，寻找下一个单词
            start = end + 1; end++;
        }
    }
}
```

## [6. Z 字形变换](https://leetcode.cn/problems/zigzag-conversion/)

将一个给定字符串 `s` 根据给定的行数 `numRows` ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"PAYPALISHIRING"` 行数为 `3` 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"PAHNAPLSIIGYIR"`。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```

**示例 1：**

```
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```

**示例 2：**

```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```

**示例 3：**

```
输入：s = "A", numRows = 1
输出："A"
```

> **思路一：1.声明一个长度为numRows的字符串数组，用numRows个字符串表示每一行的字符；2.用求余数的方法确定s中每个字符在输出字符串中的位置，拼接到对应的字符串数组中；3.把字符串数组拼接成最终输出的字符串。**

```csharp
public class Solution {
    public string Convert(string s, int numRows) {
        if(numRows == 1) {
            return s;
        }
        
        int length = s.Length;
        int group = 2*numRows - 2; // 一组的个数
        StringBuilder[] lines = new StringBuilder[numRows]; // 每一行的字符

        for(int i=0; i<numRows; i++) {
            lines[i] = new StringBuilder();
        }

        for(int i=0; i<length; i++) {
            int remain = i % group; // 求余
            if(remain <= numRows - 1) {
                // 竖着的
                lines[remain].Append(s[i]);
            } else {
                // 斜着的
                lines[group - remain].Append(s[i]);
            }
        }

        StringBuilder res = new StringBuilder();
        for(int i=0; i<numRows; i++) {
            res.Append(lines[i].ToString());
        }
        return res.ToString();
    }
}
```

> **思路二：Flag法，通过一个 方向标志（`flag`）来控制字符的分配方向（向下或向上），从而模拟 Z 字形排列的过程。**

```CSharp
public class Solution {
    public string Convert(string s, int numRows) {
        if(numRows == 1) {
            return s;
        }
        
        StringBuilder[] lines = new StringBuilder[numRows]; // 每一行的字符
        for(int i=0; i<numRows; i++) {
            lines[i] = new StringBuilder();
        }

        int currentRow = 0; // 当前行
        bool goingDown = false; // 是否向下走的标识

        foreach(char c in s) {
            lines[currentRow].Append(c);

            // 如果到达第一行或最后一行，改变方向
            if(currentRow == 0 || currentRow == numRows - 1) {
                goingDown = !goingDown;
            }

            // 根据是否向下走，更新currentRow
            currentRow += goingDown ? 1 : -1;
        }

        StringBuilder res = new StringBuilder();
        for(int i=0; i<numRows; i++) {
            res.Append(lines[i].ToString());
        }
        return res.ToString();
    }
}
```

## [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

**示例 1：**

```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

**示例 2：**

```
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```

> **思路一：暴力匹配**
>
> - **用两个指针分别遍历 `haystack` 和 `needle`。**
> - **如果字符匹配，指针一起移动；如果不匹配，`haystack` 指针回退到上一次匹配起点的下一个位置，`needle` 指针归零。**
> - **如果 `needle` 指针到达末尾，说明匹配成功，返回起始位置。**

```c#
public class Solution {
    public int StrStr(string haystack, string needle) {
        int hLength = haystack.Length;
        int nLength = needle.Length;

        if (hLength < nLength) return -1; // needle比haystack长，直接返回-1

        int pH = 0; // hayStack指针
        int pN = 0; // needle指针

        while (pH < hLength && pN < nLength) {
            if (haystack[pH] != needle[pN]) {
                pH = pH - pN + 1; // hayStack指针回退到上一次匹配起点的下一个位置
                pN = 0; // needle指针归零
            } else {
                pH++; pN++;// 匹配成功，指针一起移动
            }

            if (pN == nLength) return pH - nLength; // 匹配成功，返回起始位置
        }

        return -1; // 未找到匹配
    }
}
```

> **思路二：KMP算法**
>
> - **构建部分匹配表（next 数组）：**
>  - **`next[j]` 表示子字符串 `needle` 的前 `j` 个字符中，最长相同前缀和后缀的长度。**
>   
>  - **例如，子字符串 `"ababc"` 的 `next` 数组为 `[-1, 0, 0, 1, 2，0]`。**
>   - **通过双指针 `i` 和 `j` 计算 `next` 数组：**
>     - **如果 `p[i] == p[j]`，则 `next[j+1] = i+1`。**
>     - **如果 `p[i] != p[j]`，则回退 `i` 到 `next[i]`，直到匹配或 `i = -1`。**
>   
>- **匹配过程：**
> 
>  - **主字符串和子字符串从左到右逐个字符匹配。**
> 
>  - **如果字符匹配，两个指针一起右移。**
> 
>  - **如果字符不匹配，子字符串的指针根据 `next` 数组回退到某个位置，主字符串的指针不需要回退。**

```c#
public class Solution {
    public int StrStr(string haystack, string needle) {
        int hLength = haystack.Length;
        int nLength = needle.Length;

        if(nLength == 0) {
            return 0;
        }
        if(hLength < nLength) {
            return -1;
        }

        int[] next = GetNext(needle); // 生成next数组

        int pH = 0, pN = 0; // heyStack和needle的指针

        while(pH < hLength && pN < nLength) {
            if(pN == -1 || haystack[pH] == needle[pN]) {
                pH++; pN++; // 匹配成功，指针一起移动
            } else {
                pN = next[pN]; // 不匹配时，子串回退到next[pN]，主串不回退
            }

            if(pN == nLength) {
                return pH - nLength;
            }
        }

        return -1;
    }

    /** 辅助函数：生成Next数组 */
    private int[] GetNext(string p) {
        int n = p.Length;
        int[] next = new int[n+1];
        next[0] = -1;
        int i = -1, j = 0; // i表示前缀指针，j表示后缀指针

        while(j<n) {
            if(i == -1 || p[i] == p[j]) {
                i++; j++;
                next[j] = i; // 更新Next数组
            } else {
                i = next[i]; // 回退前缀指针
            }
        }

        return next;
    }
}
```

## [68. 文本左右对齐](https://leetcode.cn/problems/text-justification/)

给定一个单词数组 `words` 和一个长度 `maxWidth` ，重新排版单词，使其成为每行恰好有 `maxWidth` 个字符，且左右两端对齐的文本。

你应该使用 “**贪心算法**” 来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格 `' '` 填充，使得每行恰好有 *maxWidth* 个字符。

要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。

文本的最后一行应为左对齐，且单词之间不插入**额外的**空格。

**注意:**

- 单词是指由非空格字符组成的字符序列。
- 每个单词的长度大于 0，小于等于 *maxWidth*。
- 输入单词数组 `words` 至少包含一个单词。

> **思路：贪心模拟**
>
> 1. **确定每行的单词范围：**
>    - **使用 `left` 和 `right` 指针确定当前行可以容纳的单词范围。**
>    - **通过累加单词长度和至少的空格数（`right - left`）来判断是否超出 `maxWidth`。**
> 2. **处理最后一行：**
>    - **如果 `right` 遍历到最后一个单词，将剩余单词左对齐，并在行末填充空格。**
> 3. **处理单单词行：**
>    - **如果当前行只有一个单词，直接左对齐并在行末填充空格。**
> 4. **处理多单词行：**
>    - **计算平均空格数 `avgSpaces` 和多余的空格数 `extraSpaces`。**
>    - **使用 `Join` 函数拼接单词，并根据 `extraSpaces` 分配多余的空格。**

```c#
public class Solution {
    public IList<string> FullJustify(string[] words, int maxWidth) {
        IList<string> res = new List<string>();
        int right = 0; // 当前行的最后一个单词在words中的索引
        int n = words.Length; // 单词总个数

        while (true) {
            int left = right; // 当前行的第一个单词在words中的索引
            int sumLen = 0; // 当前行不含空格的单词长度

            // 确定当前行可以放多少个单词
            while (right < n && sumLen + words[right].Length + (right - left) <= maxWidth) {
                sumLen += words[right].Length;
                right++;
            }

            // 如果已经遍历完所有单词，返回结果
            if (right == n) {
                string lastLine = string.Join(" ", words, left, right - left);
                lastLine = lastLine.PadRight(maxWidth); // 在行末填充空格
                res.Add(lastLine);
                return res;
            }

            int numWords = right - left;
            int numSpaces = maxWidth - sumLen;

            // 当前行只有一个单词，左对齐并在行末填充空格
            if (numWords == 1) {
                string line = words[left].PadRight(maxWidth);
                res.Add(line);
                continue;
            }

            // 当前行有多个单词
            int avgSpaces = numSpaces / (numWords - 1); // 每个单词之间的平均空格数
            int extraSpaces = numSpaces % (numWords - 1); // 多余的空格数

            StringBuilder lineBuilder = new StringBuilder();
            for (int i = left; i < right; i++) {
                lineBuilder.Append(words[i]);

                // 如果不是最后一个单词，根据i是否属于前extraSpaces个单词决定添加的空格个数
                if (i < right - 1) {
                    int spaces = avgSpaces + (i < left + extraSpaces ? 1 : 0);
                    lineBuilder.Append(new string(' ', spaces));
                }
            }

            res.Add(lineBuilder.ToString());
        }
    }
}
```

# 【双指针】

## [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 **回文串** 。

字母和数字都属于字母数字字符。

给你一个字符串 `s`，如果它是 **回文串** ，返回 `true` ；否则，返回 `false` 。

> **思路一：双指针法**

```C#
public class Solution {
    public bool IsPalindrome(string s) {
        // 预处理字符串
        StringBuilder sb = new StringBuilder();
        foreach (char c in s) {
            if (char.IsLetter(c)) {
                // 如果是字母，就把小写字母加入预处理字符串
                sb.Append(char.ToLower(c));
            } else if (char.IsDigit(c)) {
                // 如果是数字，就把数字加入预处理字符串
                sb.Append(c);
            }
        }

        // 判断预处理字符串是不是回文串
        int left = 0; // 左指针
        int right = sb.Length - 1; // 右指针
        while (left < right) {
            if (sb[left] != sb[right]) {
                return false; // 如果字符不相等，返回 false
            }
            left++; // 左指针右移
            right--; // 右指针左移
        }

        return true; // 如果所有字符都相等，返回 true
    }
}
```

> **思路二：反转字符串，判断反转后的字符串和原字符串是否相等**

```
public class Solution {
    public bool IsPalindrome(string s) {
        // 预处理字符串
        StringBuilder sb = new StringBuilder();
        foreach (char c in s) {
            if (char.IsLetter(c)) {
                // 如果是字母，就把小写字母加入预处理字符串
                sb.Append(char.ToLower(c));
            } else if (char.IsDigit(c)) {
                // 如果是数字，就把数字加入预处理字符串
                sb.Append(c);
            }
        }

        // 将预处理后的字符串反转
        string processed = sb.ToString();
        string reversed = new string(processed.Reverse().ToArray());

        // 判断反转后的字符串是否与原字符串相等
        return processed == reversed;
    }
}
```

## [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)

给定字符串 **s** 和 **t** ，判断 **s** 是否为 **t** 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

**示例 1：**

```
输入：s = "abc", t = "ahbgdc"
输出：true
```

**示例 2：**

```
输入：s = "axc", t = "ahbgdc"
输出：false
```

> **思路一：双指针模拟法**

```csharp
public class Solution {
    public bool IsSubsequence(string s, string t) {
        int sLength = s.Length;
        int tLength = t.Length;
        if(sLength == 0) {
            return true;
        }
        if(sLength > tLength) {
            return false;
        }

        int sP = 0; // s的指针
        int tP = 0; // t的指针
        while(sP < sLength && tP < tLength) {
            if(s[sP] == t[tP]) {
                // 如果字符相同，两个指针一起移动
                sP++; tP++;
            } else {
                // 如果不同，则只有t的指针移动
                tP ++;
            }

            // 检查s的指针是否到末尾
            if(sP == sLength) {
                return true;
            }
        }

        return false;
    }
}
```

> **思路二：动态规划**
>
> 1. **定义状态：**
>    - **`dp[i][j]` 表示 `s` 的前 `i` 个字符是否是 `t` 的前 `j` 个字符的子序列。**
> 2. **状态转移方程：**
>    - **如果 `s[i-1] == t[j-1]`，则 `dp[i][j] = dp[i-1][j-1]`。**
>    - **如果 `s[i-1] != t[j-1]`，则 `dp[i][j] = dp[i][j-1]`。（即忽略 `t` 的第 `j` 个字符）**
> 3. **初始化：**
>    - **`dp[0][j] = True`，因为空字符串是任何字符串的子序列。**
>    - **`dp[i][0] = False`（`i > 0`），因为非空字符串不可能是空字符串的子序列。**
> 4. **最终结果：**
>    - **`dp[len(s)][len(t)]` 即为所求。**

```csharp
public class Solution {
    public bool IsSubsequence(string s, string t) {
        int sLength = s.Length;
        int tLength = t.Length;
        
        bool[,] dp = new bool[sLength+1, tLength+1];

        // 空字符串是任何字符串的子序列
        for(int i=0; i<=tLength; i++) {
            dp[0, i] = true;
        }

        // 填充dp数组
        for(int i=1; i<=sLength; i++) {
            for(int j=1; j<=tLength; j++) {
                if(s[i-1] == t[j-1]) {
                    dp[i, j] = dp[i-1, j-1];
                } else {
                    dp[i, j] = dp[i, j-1];
                }
            }
        }

        return dp[sLength, tLength];
    }
}
```

## [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

给你一个下标从 **1** 开始的整数数组 `numbers` ，该数组已按 **非递减顺序排列** ，请你从数组中找出满足相加之和等于目标数 `target` 的两个数。如果设这两个数分别是 `numbers[index1]` 和 `numbers[index2]` ，则 `1 <= index1 < index2 <= numbers.length` 。

以长度为 2 的整数数组 `[index1, index2]` 的形式返回这两个整数的下标 `index1` 和 `index2`。

你可以假设每个输入 **只对应唯一的答案** ，而且你 **不可以** 重复使用相同的元素。

你所设计的解决方案必须只使用常量级的额外空间。

**示例 1：**

```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

**示例 2：**

```
输入：numbers = [2,3,4], target = 6
输出：[1,3]
解释：2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。
```

**示例 3：**

```
输入：numbers = [-1,0], target = -1
输出：[1,2]
解释：-1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

> **思路一：两个for循环暴力匹配**

```csharp
public class Solution {
    public int[] TwoSum(int[] numbers, int target) {
        int n = numbers.Length;
        int[] res = new int[2];
        for(int i=0; i<n-1; i++) {
            for(int j=i+1; j<n; j++) {
                if(numbers[i] + numbers[j] == target) {
                    res[0] = i+1; res[1] = j+1;
                    return res;
                }
            }
        }

        return res;
    }
}
```

> **思路二：区间分割**
>
> - **遍历数组，找到 `mid`，使得 `2 * numbers[mid] <= target`。**
> - **这意味着 第一个数一定在`mid` 左边（含`mid`），第二个数一定在`mid`右边（含mid）。**
> - **可以将搜索范围缩小到 `mid` 的左侧和右侧。**

```csharp
public class Solution {
    public int[] TwoSum(int[] numbers, int target) {
        int n = numbers.Length;
        int mid = 0; // 区间分割点

        // 找到 mid，使得 2 * numbers[mid] <= target
        for (int i = 0; i < n; i++) {
            if (2 * numbers[i] <= target) {
                mid = i;
            } else {
                break;
            }
        }

        int left = mid; // 左指针从 mid 开始
        int right = mid; // 右指针从 mid 开始
        int[] res = new int[2]; // 结果数组

        // 双指针搜索
        while (left >= 0 && right <= n - 1) {
            int sum = numbers[left] + numbers[right];
            if (sum < target) {
                right++; // 和太小，右指针右移
            } else if (sum > target) {
                left--; // 和太大，左指针左移
            } else {
                if (left != right) {
                    // 找到答案，返回下标（从 1 开始）
                    res[0] = left + 1; res[1] = right + 1;
                    return res;
                } else {
                    left--; // 避免使用同一个元素
                }
            }
        }

        return res; // 如果没有找到，返回空数组
    }
}
```

> **思路三：O(n)时间复杂度的双指针，本质是缩减搜索空间，将数组平铺展开成二维，发现搜索空间是一个倒三角，每次排除一行或一列，最多经过 *n* 步以后，就能排除所有的搜索空间。**

```csharp
public class Solution {
    public int[] TwoSum(int[] numbers, int target) {
        /** 双指针法 */
        int left = 0; int right = numbers.Length - 1;
        int[] res = new int[2];
        while(left < right) {
            int sum = numbers[left] + numbers[right];
            if(sum < target) {
                left ++;
            } else if(sum > target) {
                right --;
            } else {
                res[0] = left + 1; res[1] = right + 1;
                return res;
            }
        }

        return res;
    }
}
```

## [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

**示例 1：**

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

> **思路：经典双指针，每次移动短板的指针，因为left++和right--都是为了尝试取到更多的水，如果短板不动的话，取到的水永远不会比上次多。**

```csharp
public class Solution {
    public int MaxArea(int[] height) {
        int lp = 0; // 左指针
        int rp = height.Length - 1; // 右指针
        int maxVolume = 0;

        while(lp < rp) {
            // 计算当前容积
            int curVolume = Math.Min(height[lp], height[rp]) * (rp - lp);
            // 更新容积
            maxVolume = Math.Max(maxVolume, curVolume);
            // 因为高度受限于数字较小的指针，因此需要移动数字较小的指针
            if(height[lp] < height[rp]) {
                lp ++;
            } else {
                rp --;
            }
        }

        return maxVolume;
    }
}
```

## [15. 三数之和](https://leetcode.cn/problems/3sum/)

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

思路：先排序，后用双指针

- 外层循环的`i`确定三数中的第一个数，需要去除重复

- 左指针`lp`和右指针`rp`在`i`的右侧寻找，在找到符合条件的三元组后也需要去除重复

```csharp
public class Solution {
    public IList<IList<int>> ThreeSum(int[] nums) {
        Array.Sort(nums); // 先对数组进行排序
        int n = nums.Length;
        int lp = 0; // 左指针
        int rp = nums.Length - 1; // 右指针
        IList<IList<int>> res = new List<IList<int>>(); // 结果

        // i是三数中的第一个数的索引，剩下两个数的索引是lp和rp，在i的右侧找到
        for(int i=0; i<n-2; i++) {
            if(nums[i] > 0) {
                // 有序数组，最小的数都大于0了，三数之和肯定大于0，直接退出
                break;
            }
            if(i > 0 && nums[i] == nums[i-1]) {
                // 如果当前元素和前一个元素相同，说明已经处理过这个值
                continue;
            }
            lp = i + 1; rp = n - 1;
            while(lp < rp) {
                int sum = nums[i] + nums[lp] + nums[rp];
                if(sum > 0) {
                    // 和过大了，右指针递减
                    rp --;
                } else if(sum < 0) {
                    // 和过小了，左指针递增
                    lp ++;

                } else {
                    // 和刚好为0，加入结果
                    res.Add(new List<int>{nums[i], nums[lp], nums[rp]});
                    // 跳过重复的元素
                    while(lp < rp && nums[lp] == nums[lp + 1]) {
                        lp ++;
                    }
                    while(lp < rp && nums[rp] == nums[rp - 1]) {
                        rp --;
                    }
                    lp ++; rp --;
                }
            }
        }

        return res;
    }
}
```

# 【滑动窗口】

## [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

给定一个含有 `n` 个正整数的数组和一个正整数 `target` **。**

找出该数组中满足其总和大于等于 `target` 的长度最小的 **子数组**`[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度**。**如果不存在符合条件的子数组，返回 `0` 。

**示例 1：**

```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

**示例 2：**

```
输入：target = 4, nums = [1,4,4]
输出：1
```

**示例 3：**

```
输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```

> **思路：滑动窗口**
>
> 1. **滑动窗口：用两个指针 `lp`（左）和 `rp`（右）表示窗口的左右边界。**
> 2. **窗口扩展：`rp` 向右移动，累加元素到 `sum`。**
> 3. **窗口收缩：当 `sum >= target` 时，记录窗口长度 `rp - lp + 1`，并尝试收缩窗口（移动 `lp` 向右）。**
> 4. **更新最小长度：每次满足条件时，更新最小长度 `minLen`。**
> 5. **返回结果：若找到满足条件的子数组，返回 `minLen`；否则返回 0。**

```csharp
public class Solution {
    public int MinSubArrayLen(int target, int[] nums) {
        int lp = 0; int rp = 0; // 滑动窗口的左右指针
        int n = nums.Length;
        int sum = 0;
        int minLen = int.MaxValue;

        while(rp < n) {
            sum += nums[rp]; // 扩展窗口
            while(sum >= target && lp <= rp) {
                minLen = Math.Min(minLen, rp - lp + 1); // 更新最小长度
                sum -= nums[lp]; // 收缩窗口
                lp++;
            }
            rp++;
        }

        return minLen == int.MaxValue ? 0 : minLen;
    }
}
```

## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长 子串** 的长度。

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

> **思路：**
>
> 1. **滑动窗口：用两个指针 `lp`（左）和 `rp`（右）表示窗口的左右边界。**
> 2. **哈希集合：用 `HashSet` 存储当前窗口中的字符，确保字符唯一。**
> 3. **窗口扩展：`rp` 向右移动，尝试将字符加入集合。**
> 4. **窗口收缩：如果当前字符已存在于集合中，移动 `lp` 向右，并从集合中移除字符，直到当前字符可以加入集合。**
> 5. **更新最大长度：每次窗口扩展后，计算当前窗口长度 `rp - lp`，并更新 `maxLen`。**

```csharp
public class Solution {
    public int LengthOfLongestSubstring(string s) {
        int lp = 0, rp = 0, maxLen = 0; // 左右指针和最大长度
        HashSet<char> set = new HashSet<char>(); // 存储当前窗口的字符

        while (rp < s.Length) {
            while (set.Contains(s[rp])) { // 如果字符重复，收缩窗口
                set.Remove(s[lp]);
                lp++;
            }
            set.Add(s[rp]); // 扩展窗口
            rp++;
            maxLen = Math.Max(maxLen, rp - lp); // 更新最大长度
        }

        return maxLen;
    }
}
```

## [30. 串联所有单词的子串](https://leetcode.cn/problems/substring-with-concatenation-of-all-words/)

给定一个字符串 `s` 和一个字符串数组 `words`**。** `words` 中所有字符串 **长度相同**。

 `s` 中的 **串联子串** 是指一个包含 `words` 中所有字符串以任意顺序排列连接起来的子串。

- 例如，如果 `words = ["ab","cd","ef"]`， 那么 `"abcdef"`， `"abefcd"`，`"cdabef"`， `"cdefab"`，`"efabcd"`， 和 `"efcdab"` 都是串联子串。 `"acdbef"` 不是串联子串，因为他不是任何 `words` 排列的连接。

返回所有串联子串在 `s` 中的开始索引。你可以以 **任意顺序** 返回答案。

**示例 1：**

```
输入：s = "barfoothefoobarman", words = ["foo","bar"]
输出：[0,9]
解释：因为 words.length == 2 同时 words[i].length == 3，连接的子字符串的长度必须为 6。
子串 "barfoo" 开始位置是 0。它是 words 中以 ["bar","foo"] 顺序排列的连接。
子串 "foobar" 开始位置是 9。它是 words 中以 ["foo","bar"] 顺序排列的连接。
输出顺序无关紧要。返回 [9,0] 也是可以的。
```

**示例 2：**

```
输入：s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
输出：[]
解释：因为 words.length == 4 并且 words[i].length == 4，所以串联子串的长度必须为 16。
s 中没有子串长度为 16 并且等于 words 的任何顺序排列的连接。
所以我们返回一个空数组。
```

**示例 3：**

```
输入：s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
输出：[6,9,12]
解释：因为 words.length == 3 并且 words[i].length == 3，所以串联子串的长度必须为 9。
子串 "foobarthe" 开始位置是 6。它是 words 中以 ["foo","bar","the"] 顺序排列的连接。
子串 "barthefoo" 开始位置是 9。它是 words 中以 ["bar","the","foo"] 顺序排列的连接。
子串 "thefoobar" 开始位置是 12。它是 words 中以 ["the","foo","bar"] 顺序排列的连接。
```

> **思路：滑动窗口，使用滑动窗口在`s`中寻找长度为`totalLen`的子串，对于每个窗口，检查是否包含words中所有的单词，且出现个数是否对得上；因此需要预先用哈希表`wordCounts`记录`words`中每个单词的出现次数；对于每个窗口用另一个哈希表`curCounts`记录窗口中各单词的出现次数，并与`wordCounts`比较。**
>
> 1. **外层循环:**
>    - **外层循环从 `0` 到 `wordLen - 1`，确保滑动窗口覆盖所有可能的起始位置。**
>    - **例如，如果 `wordLen = 3`，外层循环会分别从 `0`、`1`、`2` 开始滑动窗口：**
>      - **从 `0` 开始：检查子串 `0, 3, 6, ...`**
>      - **从 `1` 开始：检查子串 `1, 4, 7, ...`**
>      - **从 `2` 开始：检查子串 `2, 5, 8, ...`**
> 2. **滑动窗口:**
>    - **内层循环移动右指针，逐步扩大窗口。**
>    - **如果当前单词在 `words` 中，更新当前窗口的单词统计。**
>    - **如果当前单词出现次数超过 `words` 中的次数，移动左指针缩小窗口。**
> 3. **记录结果:**
>    - **如果当前窗口包含 `words` 中的所有单词，记录起始索引。**
> 4. **重置窗口:**
>    - **如果当前单词不在 `words` 中，重置窗口。**

```csharp
public class Solution {
    public IList<int> FindSubstring(string s, string[] words) {
        List<int> result = new List<int>();

        int wordLen = words[0].Length; // 每个单词的长度
        int totalLen = wordLen * words.Length; // 目标子串的总长度
        Dictionary<string, int> wordCounts = new Dictionary<string, int>();

        // 统计 words 中每个单词的出现次数
        foreach (var word in words) {
            if (wordCounts.ContainsKey(word)) {
                wordCounts[word]++;
            } else {
                wordCounts[word] = 1;
            }
        }

        // 遍历所有起始位置的偏移
        for(int i=0; i<wordLen; i++) {
            int left = i;
            Dictionary<string, int> curCounts = new Dictionary<string, int>();
            int count = 0; // 匹配的单词个数

            for(int right = i; right <= s.Length - wordLen; right += wordLen) {
                string word = s.Substring(right, wordLen); // 取一个单词

                if(wordCounts.ContainsKey(word)) {
                    // 如果字典中有该单词，更新当前窗口的单词统计
                    if(curCounts.ContainsKey(word)) {
                        curCounts[word] ++;
                    } else {
                        curCounts[word] = 1;
                    }

                    // 匹配的单词个数增加
                    count ++;
                    // 如果当前单词出现次数超过 words 中的次数，移动左指针
                    while(curCounts[word] > wordCounts[word]) {
                        string leftWord = s.Substring(left, wordLen);
                        curCounts[leftWord] --;
                        count --;
                        left += wordLen;
                    }

                    // 检查匹配的单词个数是否与words中的单词个数相同
                    if(count == words.Length) {
                        result.Add(left);
                        string leftWord = s.Substring(left, wordLen);
                        curCounts[leftWord] --;
                        count --;
                        left += wordLen;
                    }

                } else {
                    // 如果字典中没有该单词，重置窗口
                    curCounts.Clear();
                    count = 0;
                    left = right + wordLen; // 左指针移到下一个可能的子串起始处
                }
            }
        }
        
        return result;
    }
}
```

# 【矩阵】

## [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

**注意：**

- 对于 `t` 中重复字符，我们寻找的子字符串中该字符数量必须不少于 `t` 中该字符数量。
- 如果 `s` 中存在这样的子串，我们保证它是唯一的答案。 

**示例 1：**

```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。
```

**示例 2：**

```
输入：s = "a", t = "a"
输出："a"
解释：整个字符串 s 是最小覆盖子串。
```

**示例 3:**

```
输入: s = "a", t = "aa"
输出: ""
解释: t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。
```

> **思路：滑动窗口**
>
> 1. **统计字符频率：**
>    - **首先统计字符串 `t` 中每个字符的出现次数，存储在字典 `tCountDict` 中。**
> 2. **初始化滑动窗口：**
>    - **使用两个指针 `left` 和 `right` 来表示滑动窗口的左右边界。**
>    - **使用字典 `countDict` 来记录当前窗口中每个字符的出现次数。**
>    - **使用变量 `count` 来记录当前窗口中已经匹配了 `t` 中字符的个数。**
> 3. **扩展窗口：**
>    - **移动右指针 `right`，将字符加入窗口，并更新 `countDict` 和 `count`。**
>    - **如果当前字符是 `t` 中的字符，并且 `countDict` 中该字符的个数等于 `tCountDict` 中的个数，说明该字符已经匹配完成，`count` 加 1。**
> 4. **收缩窗口：**
>    - **当 `count` 等于 `tCountDict` 的大小时，说明当前窗口已经包含了 `t` 中的所有字符。**
>    - **此时尝试移动左指针 `left`，缩小窗口，同时更新 `countDict` 和 `count`。**
>    - **如果缩小窗口后，某个字符的个数不再满足 `tCountDict` 中的要求，则 `count` 减 1。**
> 5. **更新最小窗口：**
>    - **在每次找到满足条件的窗口时，更新最小窗口的长度和起始位置。**

```csharp
public class Solution {
    public string MinWindow(string s, string t) {
        int sLen = s.Length; 
        int tLen = t.Length;
        if (sLen < tLen) {
            return "";
        }

        // 统计字符串 t 中每个字符的出现次数
        Dictionary<char, int> tCountDict = new Dictionary<char, int>();
        foreach (char c in t) {
            if (tCountDict.ContainsKey(c)) {
                tCountDict[c]++;
            } else {
                tCountDict[c] = 1;
            }
        }

        int left = 0; // 滑动窗口的左指针
        int right = 0; // 滑动窗口的右指针
        int count = 0; // 匹配的字符个数
        int minLen = int.MaxValue; // 最小窗口长度
        int minStart = 0; // 最小窗口的起始位置
        Dictionary<char, int> countDict = new Dictionary<char, int>(); // 当前窗口中字符的统计

        while (right < sLen) {
            char curChar = s[right];
            if (tCountDict.ContainsKey(curChar)) {
                // 更新当前窗口中字符的统计
                if (countDict.ContainsKey(curChar)) {
                    countDict[curChar]++;
                } else {
                    countDict[curChar] = 1;
                }

                // 如果当前字符的个数满足 t 中的要求，增加匹配的字符个数
                if (countDict[curChar] == tCountDict[curChar]) {
                    count++;
                }
            }

            // 当窗口满足条件时，尝试收缩左侧
            while (count == tCountDict.Count) {
                // 更新最小窗口
                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    minStart = left;
                }

                char leftChar = s[left];
                if (tCountDict.ContainsKey(leftChar)) {
                    countDict[leftChar]--;
                    // 如果窗口中某字符的个数不再满足要求，减少匹配的字符个数
                    if (countDict[leftChar] < tCountDict[leftChar]) {
                        count--;
                    }
                }
                left++;
            }
            right++;
        }

        // 返回最小窗口对应的子串
        return minLen == int.MaxValue ? "" : s.Substring(minStart, minLen);
    }
}
```

## [36. 有效的数独](https://leetcode.cn/problems/valid-sudoku/)

请你判断一个 `9 x 9` 的数独是否有效。只需要 **根据以下规则** ，验证已经填入的数字是否有效即可。

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。

**注意：**

- 一个有效的数独（部分已被填充）不一定是可解的。
- 只需要根据以上规则，验证已经填入的数字是否有效即可。
- 空白格用 `'.'` 表示。

**示例 1：**

```
输入：board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：true
```

**示例 2：**

```
输入：board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：false
解释：除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。 但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```

> **思路一：遍历9行，遍历9列，遍历9个九宫格，分别检查重复。**

```csharp
public class Solution {
    public bool IsValidSudoku(char[][] board) {
        // 检查行是否重复
        for(int i=0; i<9; i++) {
            char[] row = board[i]; // 该行的元素组成的字符数组
            if(CheckDuplicate(row)) {
                return false;
            }
        }

        // 检查列是否重复
        for(int i=0; i<9; i++) {
            char[] col = GetCol(i, board);
            if(CheckDuplicate(col)) {
                return false;
            }
        }

        // 检查九宫格是否重复
        for(int i=0; i<9; i++) {
            char[] subBoard = GetSubBoard(i, board);
            if(CheckDuplicate(subBoard)) {
                return false;
            }
        }

        return true;
    }


    /** 辅助函数：得到某一列组成的字符串数组 */
    public char[] GetCol(int col, char[][] board) {
        char[] res = new char[9];
        for(int row=0; row<9; row++) {
            res[row] = board[row][col];
        }

        return res;
    }

    /** 辅助函数：得到第n个九宫格 */
    public char[] GetSubBoard(int n, char[][] board) {
        char[] res = new char[9];
        int startRow = (n/3)*3;
        int startCol = (n%3)*3;
        int index = 0;
        for(int row=startRow; row<startRow+3; row++) {
            for(int col=startCol; col<startCol+3; col++) {
                res[index++] = board[row][col];
            }
        }

        return res;
    }

    /** 辅助函数：检查某个字符数组是否含有重复元素 */
    public bool CheckDuplicate(char[] target) {
        HashSet<char> set = new HashSet<char>();
        for(int i=0; i<9; i++) {
            char c = target[i];
            if(c != '.') {
                if(set.Contains(c)) {
                    // 找到重复元素，直接返回true
                    return true;
                } else {
                    set.Add(c);
                }
            }
        }
        return false;
    }
}
```

> **思路二：遍历每个元素，检查是否重复，并标记该数字在对应的行、列、九宫格内已存在。**

```csharp
public class Solution {
    public bool IsValidSudoku(char[][] board) {
        // 使用二维数组代替锯齿数组
        bool[,] row = new bool[9, 9]; // 记录某行的某个数字是否已存在
        bool[,] col = new bool[9, 9]; // 记录某列的某个数字是否已存在
        bool[,] subBoard = new bool[9, 9]; // 记录第n个九宫格内某个数字是否已存在

        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] == '.') {
                    continue;
                }

                int value = board[i][j] - '1'; // 将字符转换为数字（0-8）
                int boardId = (i / 3) * 3 + (j / 3); // 计算九宫格索引（0-8）

                // 检查是否重复
                if (row[i, value] || col[j, value] || subBoard[boardId, value]) {
                    return false;
                }

                // 标记数字已存在
                row[i, value] = true;
                col[j, value] = true;
                subBoard[boardId, value] = true;
            }
        }

        return true;
    }
}
```

## [54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

**示例 1：**

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

**示例 2：**

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

> **思路一：方向标记+动态更新边界。**

```csharp
public class Solution {
    public IList<int> SpiralOrder(int[][] matrix) {
        int m = matrix.Length; // 行数
        int n = matrix[0].Length; // 列数

        int dirFlag = 0; // 方向：0=右，1=下，2=左，3=上
        int row = 0, col = 0; // 当前坐标
        int rowCount = 0, colCount = 0; // 已遍历的行和列的层数
        IList<int> res = new List<int>();

        while (res.Count < m * n) {
            res.Add(matrix[row][col]); // 将当前元素加入结果

            if (dirFlag == 0) { // 向右
                if (col == n - 1 - colCount / 2) {
                    row++;      // 下移一行
                    dirFlag = 1; 
                    rowCount++; // 完成一“行层”
                } else {
                    col++;
                }
            } else if (dirFlag == 1) { // 向下
                if (row == m - 1 - rowCount / 2) {
                    col--;      // 左移一列
                    dirFlag = 2; 
                    colCount++; // 完成一“列层”
                } else {
                    row++;
                }
            } else if (dirFlag == 2) { // 向左
                if (col == 0 + colCount / 2) {
                    row--;      // 上移一行
                    dirFlag = 3; 
                    rowCount++; // 完成一“行层”
                } else {
                    col--;
                }
            } else if (dirFlag == 3) { // 向上
                if (row == 0 + rowCount / 2) { // 关键修正点：上边界为已完成的“行层”+1
                    col++;      // 右移一列
                    dirFlag = 0; 
                    colCount++; // 完成一“列层”
                } else {
                    row--;
                }
            }
        }

        return res;
    }
}
```

> **思路二：四个指针转圈圈**
>
> - **从左到右，顶部一层遍历完往下移一位，top++；**
> - **从上到下，遍历完右侧往左移一位，right--；**
> - **从右到左，判断`top <= bottom`，即是否上下都走完了。遍历完底部上移，bottom--；**
> - **从下到上，判断`left <= right`，遍历完左侧右移，left++；**
> - **当矩阵只有一行或一列时，完成前两个方向的遍历（从左到右、从上到下）后，边界可能已经交叉。此时如果不加判断直接执行后续方向，会导致重复添加元素。**

```csharp
public class Solution {
    public IList<int> SpiralOrder(int[][] matrix) {
        int m = matrix.Length; // 行数
        int n = matrix[0].Length; // 列数

        int left = 0; int right = n - 1; // 左右边界
        int top = 0; int bottom = m - 1; // 上下边界

        // 储存结果的列表
        IList<int> res = new List<int>();

        while(left <= right && top <= bottom) {
            // 从左到右
            for(int i=left; i<=right; i++) {
                res.Add(matrix[top][i]);
            }
            top ++; // 上边界下移一行
            // 从上到下
            for(int i=top; i<=bottom; i++) {
                res.Add(matrix[i][right]);
            }
            right --; // 右边界左移一列

            if(top <= bottom) {
                // 从右到左
                for(int i=right; i>=left; i--) {
                    res.Add(matrix[bottom][i]);
                }
            }
            bottom --; // 下边界上移一行
            // 从下到上
            if(left <= right) {
                for(int i=bottom; i>=top; i--) {
                    res.Add(matrix[i][left]);
                }               
            }
            left ++; // 左边界右移一列
        }

        return res;
    }
}
```

## [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)

给定一个 *n* × *n* 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在**[ 原地](https://baike.baidu.com/item/原地算法)** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。

**示例 1：**

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

**示例 2：**

```
输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

>  **思路一：根据旋转矩阵寻找坐标规律，发现顺时针旋转90°后，新的坐标`(x', y')`**
>
> - **x' = y**
> - **y' = n - 1 - x**

```csharp
public class Solution {
    public void Rotate(int[][] matrix) {
        int n = matrix.Length;
        int[][] result = new int[n][];
        // 初始化结果矩阵
        for(int i=0; i<n; i++) {
            result[i] = new int[n];
        }
        
        // 建立坐标映射
        for(int x=0; x<n; x++) {
            for(int y=0; y<n; y++) {
                result[x][y] = matrix[n-1-y][x]; // 关键映射
            }
        }
        
        // 复制回原矩阵
        for(int x=0; x<n; x++) {
            for(int y=0; y<n; y++) {
                matrix[x][y] = result[x][y];
            }
        }
    }
}
```

**思路二：从外层到内层，每一层由四个数交换完成原地旋转。**

```csharp
public class Solution {
    public void Rotate(int[][] matrix) {
        int n = matrix.Length;

        int center = n/2; // 中心坐标
        int layer = 0; // 当前旋转的层数
        while(layer <= center) {
            for(int i=layer; i<(n-1)-layer; i++) {
                ExchangeFourNums(n, layer, i, matrix);
            }
            layer ++;
        }
    }

    /** 辅助函数，交换(row, col)对应的四个数 */
    private void ExchangeFourNums(int n, int row, int col, int[][] matrix) {
        // 保存左上角元素
        int temp = matrix[row][col];
        
        // 左上<-左下
        matrix[row][col] = matrix[n - 1 - col][row];
        // 左下<-右下
        matrix[n - 1 - col][row] = matrix[n - 1 - row][n - 1 - col];
        // 右下<-右上
        matrix[n - 1 - row][n - 1 - col] = matrix[col][n - 1 - row];
        // 右上<-左上（使用保存的temp）
        matrix[col][n - 1 - row] = temp;
    }
}
```

> **思路三：矩阵转置+水平反转每行。**

```csharp
public class Solution {
    public void Rotate(int[][] matrix) {
        int n = matrix.Length;

        // 1.转置矩阵
        for(int i=0; i<n; i++) {
            for(int j=i; j<n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // 2.水平翻转每行
        for(int i=0; i<n; i++) {
            Array.Reverse(matrix[i]);
        }
    }
}
```

## [73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

给定一个 `m x n` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **原地** 算法**。**

**示例 1：**

```
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
```

**示例 2：**

```
输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

## [289. 生命游戏](https://leetcode.cn/problems/game-of-life/)

根据百度百科， **生命游戏** ，简称为 **生命** ，是英国数学家约翰·何顿·康威在 1970 年发明的细胞自动机。

给定一个包含 `m × n` 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态： `1` 即为 **活细胞** （live），或 `0` 即为 **死细胞** （dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

1. 如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
2. 如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
3. 如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
4. 如果死细胞周围正好有三个活细胞，则该位置死细胞复活；

下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是 **同时** 发生的。给你 `m x n` 网格面板 `board` 的当前状态，返回下一个状态。

给定当前 `board` 的状态，**更新** `board` 到下一个状态。

**注意** 你不需要返回任何东西。

**示例 1：**

```
输入：board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
输出：[[0,0,0],[1,0,1],[0,1,1],[0,1,0]]
```

**示例 2：**

```
输入：board = [[1,1],[1,0]]
输出：[[1,1],[1,1]]
```

> **思路一：用一个额外的二维数组记录每个位置周围的活细胞个数。**
>
> 1. **统计周围活细胞数量**
>    **遍历每个细胞，若当前是活细胞（值为1），则对其周围8个相邻格子的活细胞计数进行贡献。**
>    **使用辅助数组 `lifeCount` 记录每个位置的活细胞邻居数。**
> 2. **根据规则更新状态**
>    - **活细胞：若周围活细胞数 `<2` 或 `>3`，则死亡（置0）。**
>    - **死细胞：若周围活细胞数 `=3`，则复活（置1）。**

```csharp
public class Solution {
    public void GameOfLife(int[][] board) {
        int m = board.Length;
        if (m == 0) return;
        int n = board[0].Length;

        int[][] lifeCount = new int[m][];
        for (int i = 0; i < m; i++) {
            lifeCount[i] = new int[n];
        }

        // 统计每个活细胞对周围格子的贡献
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 1) {
                    // 遍历周围8个格子
                    for (int row = i - 1; row <= i + 1; row++) {
                        for (int col = j - 1; col <= j + 1; col++) {
                            if (row == i && col == j) continue; // 跳过自身
                            if (row >= 0 && row < m && col >= 0 && col < n) {
                                lifeCount[row][col]++;
                            }
                        }
                    }
                }
            }
        }

        // 根据规则更新细胞状态
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 1) {
                    // 活细胞：检查邻居数是否导致死亡
                    if (lifeCount[i][j] < 2 || lifeCount[i][j] > 3) {
                        board[i][j] = 0;
                    }
                } else {
                    // 死细胞：检查是否复活
                    if (lifeCount[i][j] == 3) {
                        board[i][j] = 1;
                    }
                }
            }
        }
    }
}
```

> **思路二：使用状态标记，O(1)空间复杂度，原地转换。**
>
> 1. **状态标记**
>    - **`-1`：原为活细胞（1），下一代死亡（0）。**
>    - **`2`：原为死细胞（0），下一代复活（1）。**
> 2. **统计邻居时的逻辑**
>    **在 `CountLiveNeighbors` 方法中，统计的是原始活细胞的数量。标记 `-1` 表示细胞即将死亡，但此时仍需视为活细胞参与邻居的统计（因为其他细胞的状态更新依赖于当前原始状态）。**
> 3. **状态转换**
>    **最后遍历网格，将标记转换为最终状态：`-1` → `0`，`2` → `1`。**

```csharp
public class Solution {
    public void GameOfLife(int[][] board) {
        if (board == null || board.Length == 0) return;
        int m = board.Length, n = board[0].Length;

        // 定义状态标记：
        // -1 表示原为活细胞，现在死亡（1 → 0）
        // 2  表示原为死细胞，现在复活（0 → 1）
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int liveNeighbors = CountLiveNeighbors(board, i, j, m, n);
                
                if (board[i][j] == 1) {
                    // 活细胞根据规则死亡
                    if (liveNeighbors < 2 || liveNeighbors > 3) {
                        board[i][j] = -1; // 标记为即将死亡
                    }
                } else {
                    // 死细胞根据规则复活
                    if (liveNeighbors == 3) {
                        board[i][j] = 2; // 标记为即将复活
                    }
                }
            }
        }

        // 将标记转换为最终状态
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == -1) {
                    board[i][j] = 0;
                } else if (board[i][j] == 2) {
                    board[i][j] = 1;
                }
            }
        }
    }

    private int CountLiveNeighbors(int[][] board, int x, int y, int m, int n) {
        int count = 0;
        for (int i = x - 1; i <= x + 1; i++) {
            for (int j = y - 1; j <= y + 1; j++) {
                // 跳过自身
                if (i == x && j == y) continue;
                if (i >= 0 && i < m && j >= 0 && j < n) {
                    // 统计原为活细胞的状态（包括标记为-1的）
                    if (board[i][j] == 1 || board[i][j] == -1) {
                        count++;
                    }
                }
            }
        }
        return count;
    }
}
```

# 【哈希表】

## [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

给你两个字符串：`ransomNote` 和 `magazine` ，判断 `ransomNote` 能不能由 `magazine` 里面的字符构成。

如果可以，返回 `true` ；否则返回 `false` 。

`magazine` 中的每个字符只能在 `ransomNote` 中使用一次。

**示例 1：**

```
输入：ransomNote = "a", magazine = "b"
输出：false
```

**示例 2：**

```
输入：ransomNote = "aa", magazine = "ab"
输出：false
```

**示例 3：**

```
输入：ransomNote = "aa", magazine = "aab"
输出：true
```

> **思路一：用字典存储`magazine`中每个字符出现的个数，遍历`ransomNote`挨个扣除；扣到0时或字符不存在时返回`false`。**

```csharp
public class Solution {
    public bool CanConstruct(string ransomNote, string magazine) {
        int rLen = ransomNote.Length;
        int mLen = magazine.Length;
        if(rLen > mLen) {
            return false;
        }

        // 统计magazine中每个字符出现的个数
        Dictionary<char, int> mDict = new Dictionary<char, int>();
        foreach(char c in magazine) {
            if(mDict.ContainsKey(c)) {
                mDict[c] ++;
            } else {
                mDict[c] = 1;
            }
        }

        // 遍历ransomNote，依次从字典中扣除
        foreach(char c in ransomNote) {
            if(mDict.ContainsKey(c)) {
                if(mDict[c] == 0) {
                    // 碰到个数已经用完了，直接返回false
                    return false;
                } else {
                    mDict[c] --;
                }
            } else {
                // 碰到mDict中没有的字符，直接返回false
                return false;
            }
        }

        return true;
    }
}
```

> **思路二：用一个长度为26个整型数组存储magazine中每个字符出现的个数。**

```csharp
public class Solution {
    public bool CanConstruct(string ransomNote, string magazine) {
        if (ransomNote.Length > magazine.Length) return false;
        int[] counts = new int[26];
        foreach (char c in magazine) {
            counts[c - 'a']++;
        }
        foreach (char c in ransomNote) {
            int index = c - 'a';
            counts[index]--;
            if (counts[index] < 0) {
                return false;
            }
        }
        return true;
    }
}
```

## [205. 同构字符串](https://leetcode.cn/problems/isomorphic-strings/)

给定两个字符串 `s` 和 `t` ，判断它们是否是同构的。

如果 `s` 中的字符可以按某种映射关系替换得到 `t` ，那么这两个字符串是同构的。

每个出现的字符都应当映射到另一个字符，同时不改变字符的顺序。不同字符不能映射到同一个字符上，相同字符只能映射到同一个字符上，字符可以映射到自己本身。

**示例 1:**

```
输入：s = "egg", t = "add"
输出：true
```

**示例 2：**

```
输入：s = "foo", t = "bar"
输出：false
```

**示例 3：**

```
输入：s = "paper", t = "title"
输出：true
```

> **思路一：使用两个哈希表（字典）分别记录 `s→t` 和 `t→s` 的映射关系。**

```csharp
public class Solution {
    public bool IsIsomorphic(string s, string t) {
        int len = s.Length; // s和t长度相同

        // 用两个字典分别记录s→t和t→s的映射，确保双向唯一
        Dictionary<char, char> sDict = new Dictionary<char, char>();
        Dictionary<char, char> tDict = new Dictionary<char, char>();
        
        for (int i = 0; i < len; i++) {
            char sChar = s[i], tChar = t[i];
            
            // 处理s→t的映射：检查sChar是否已正确映射到tChar
            if (sDict.ContainsKey(sChar)) {
                if (sDict[sChar] != tChar) {
                    return false; // 已存在映射，但与当前tChar冲突
                }
            } else {
                sDict[sChar] = tChar; // 添加新映射
            }
            
            // 处理t→s的映射：检查tChar是否被其他字符映射过
            if (tDict.ContainsKey(tChar)) {
                if (tDict[tChar] != sChar) {
                    return false; // tChar已被映射到其他字符
                }
            } else {
                tDict[tChar] = sChar; // 添加新映射
            }
        }
        
        return true;
    }
}
```

> **思路二：使用数组代替哈希表，字符通常为ASCII（0-255），可以用两个数组分别维护 `s→t` 和 `t→s` 的映射，数组索引直接对应字符的ASCII码。**

```csharp
public class Solution {
    public bool IsIsomorphic(string s, string t) {
        int[] sMap = new int[256]; // 记录s中字符到t的映射
        int[] tMap = new int[256]; // 记录t中字符到s的映射
        Array.Fill(sMap, -1);      // 初始化为-1，表示未映射
        Array.Fill(tMap, -1);
        
        for (int i = 0; i < s.Length; i++) {
            char sChar = s[i];
            char tChar = t[i];
            
            // 检查s到t的映射是否冲突
            if (sMap[sChar] != -1) {
                if (sMap[sChar] != tChar) return false;
            } else {
                // 检查t中的字符是否已被其他s字符映射
                if (tMap[tChar] != -1) return false;
                // 更新双向映射
                sMap[sChar] = tChar;
                tMap[tChar] = sChar;
            }
        }
        return true;
    }
}
```

## [290. 单词规律](https://leetcode.cn/problems/word-pattern/)

给定一种规律 `pattern` 和一个字符串 `s` ，判断 `s` 是否遵循相同的规律。

这里的 **遵循** 指完全匹配，例如， `pattern` 里的每个字母和字符串 `s` 中的每个非空单词之间存在着双向连接的对应规律。 

**示例1:**

```
输入: pattern = "abba", s = "dog cat cat dog"
输出: true
```

**示例 2:**

```
输入:pattern = "abba", s = "dog cat cat fish"
输出: false
```

**示例 3:**

```
输入: pattern = "aaaa", s = "dog cat cat dog"
输出: false
```

> **思路一：两个字典（哈希表）双向映射。**

```csharp
public class Solution {
    public bool WordPattern(string pattern, string s) {
        // 将字符串 s 按空格分割成单词数组
        string[] words = s.Split(' ');
        
        // 如果 pattern 的长度与单词数组的长度不一致，直接返回 false
        if (pattern.Length != words.Length) return false;
        
        // 用两个字典分别记录 pattern 到单词的映射和单词到 pattern 的映射
        Dictionary<char, string> charToWord = new Dictionary<char, string>();
        Dictionary<string, char> wordToChar = new Dictionary<string, char>();
        
        for (int i = 0; i < pattern.Length; i++) {
            char currentChar = pattern[i];
            string currentWord = words[i];
            
            // 检查 pattern 到单词的映射是否冲突
            if (charToWord.ContainsKey(currentChar)) {
                if (charToWord[currentChar] != currentWord) return false;
            } else {
                // 检查单词是否已经被其他 pattern 字符映射
                if (wordToChar.ContainsKey(currentWord)) return false;
                
                // 添加新的映射
                charToWord[currentChar] = currentWord;
                wordToChar[currentWord] = currentChar;
            }
        }
        
        return true;
    }
}
```

> **思路二：通过检查 `pattern` 中每个字符和 `s` 中每个单词的首次出现索引是否一致，来判断它们是否遵循相同的映射规律。**

```csharp
public class Solution {
    public bool WordPattern(string pattern, string s) {
        // 将字符串 s 按空格分割成单词列表
        List<string> words = s.Split(' ').ToList();
        
        // 如果 pattern 的长度与单词列表的长度不一致，直接返回 false
        if (pattern.Length != words.Count) return false;
        
        // 遍历 pattern 和单词列表，检查字符和单词的索引是否一致
        for (int i = 0; i < pattern.Length; i++) {
            // 获取当前字符在 pattern 中的第一个索引
            int patternIndex = pattern.IndexOf(pattern[i]);
            // 获取当前单词在单词列表中的第一个索引
            int wordIndex = words.IndexOf(words[i]);
            
            // 如果索引不一致，说明映射关系不匹配，返回 false
            if (patternIndex != wordIndex) return false;
        }
        
        return true;
    }
}
```

## [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

给定两个字符串 `s` 和 `t` ，编写一个函数来判断 `t` 是否是 `s` 的字母异位词。

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

> **思路一：通过统计两个字符串中每个字符的出现次数，判断它们的字符分布是否完全相同。**

```csharp
public class Solution {
    public bool IsAnagram(string s, string t) {
        int len = s.Length;
        // 如果长度不同，直接返回 false
        if (len != t.Length) return false;

        // 用一个长度为 26 的数组统计 s 中每个字符出现的次数
        int[] counts = new int[26];
        for (int i = 0; i < len; i++) {
            counts[s[i] - 'a']++; // 统计 s 中字符的出现次数
        }

        // 遍历 t，每遇到一个字符就扣除 counts，如果不够扣除，返回 false
        for (int i = 0; i < len; i++) {
            int index = t[i] - 'a';
            if (counts[index] == 0) {
                return false; // t 中的字符比 s 中多
            }
            counts[index]--; // 扣除计数
        }

        return true; // 所有字符的计数匹配
    }
}
```

> **思路二：排序`s`和`t`，判断排序后的字符串是否相同。**

```csharp
public class Solution {
    public bool IsAnagram(string s, string t) {
        // 如果长度不同，直接返回 false
        if (s.Length != t.Length) return false;

        // 将字符串转换为字符数组并排序
        char[] sArray = s.ToCharArray();
        char[] tArray = t.ToCharArray();
        Array.Sort(sArray);
        Array.Sort(tArray);

        // 使用 SequenceEqual 比较数组内容
        return sArray.SequenceEqual(tArray);
    }
}
```

## [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。

**字母异位词** 是由重新排列源单词的所有字母得到的一个新单词。

**示例 1:**

```
输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**示例 2:**

```
输入: strs = [""]
输出: [[""]]
```

**示例 3:**

```
输入: strs = ["a"]
输出: [["a"]]
```

> **思路一：暴力双层循环，每次判断两个字符串是否是异位词。**

```csharp
public class Solution {
    public IList<IList<string>> GroupAnagrams(string[] strs) {
        int n = strs.Length; // 单词个数
        bool[] added = new bool[n]; // 用来标记每个单词是否已经加入列表

        IList<IList<string>> res = new List<IList<string>>(); // 结果

        for(int i=0; i<n; i++) {
            if(added[i]) {
                // 跳过已经加入列表的单词
                continue;
            }
            
            // 新初始化一个列表，把str[i]加入列表
            IList<string> list = new List<string>(){strs[i]}; 
            added[i] = true;
            for(int j=i+1; j<n; j++) {
                // 跳过已经加入列表的单词
                if(added[j]) {
                    continue;
                }

                if(IsAnagram(strs[i], strs[j])) {
                    list.Add(strs[j]); 
                    added[j] = true;
                }
            }
            res.Add(list);
        }

        return res;
    }

    /** 两个字符串是否是异位词 */
    public bool IsAnagram(string s, string t) {
        int len = s.Length;
        if(len != t.Length) return false;

        // 用一个长度为26的数组统计s中每个字符出现的次数
        int[] counts = new int[26];
        for(int i=0; i<len; i++) {
            counts[s[i] - 'a']++;
        }

        // 遍历t，每遇到一个字符就扣除counts，如果不够扣除，返回false
        for(int i=0; i<len; i++) {
            int index = t[i] - 'a';
            if(counts[index] == 0) {
                return false;
            }
            counts[index] --;
        }

        return true;
    }
}
```

> **思路二：排序法，将每个字符串排序，排序后的字符串作为键，原字符串作为值存入哈希表。**

```csharp
public class Solution {
    public IList<IList<string>> GroupAnagrams(string[] strs) {
        Dictionary<string, List<string>> dict = new Dictionary<string, List<string>>();

        foreach(string str in strs) {
            // 将字符串排序
            char[] charArray = str.ToCharArray();
            Array.Sort(charArray);
            string sortedStr = new string(charArray);

            // 将排序后的字符串作为键，原字符串作为值存入哈希表
            if(!dict.ContainsKey(sortedStr)) {
                dict[sortedStr] = new List<string>();
            }

            dict[sortedStr].Add(str);
        }

        // 返回字典中所有的值
        return new List<IList<string>>(dict.Values);
    }
}
```

## [1. 两数之和](https://leetcode.cn/problems/two-sum/)

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。

你可以按任意顺序返回答案。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

> **思路一：用一个额外的数组来记录排序后每个元素的原始下标，然后排序+双指针。**

```csharp
public class Solution {
    public int[] TwoSum(int[] nums, int target) {
        // 创建一个数组，记录每个元素的原始下标
        int[] indexes = new int[nums.Length];
        for (int i = 0; i < nums.Length; i++) {
            indexes[i] = i;
        }

        // 对数组进行排序，同时保持原始下标的对应关系
        Array.Sort(nums, indexes);

        // 双指针法
        int left = 0, right = nums.Length - 1;
        while (left < right) {
            int sum = nums[left] + nums[right];
            if (sum < target) {
                left++;
            } else if (sum > target) {
                right--;
            } else {
                // 找到目标值，返回原始下标
                return new int[] { indexes[left], indexes[right] };
            }
        }

        // 如果没有找到，返回空数组（根据题目描述，假设一定有解）
        return new int[0];
    }
}
```

> **思路二：遍历数组，对于每个元素 `nums[i]`，检查 `target - nums[i]` 是否在哈希表中。**
>
> - **如果存在，说明找到了两个数，直接返回它们的下标。**
> - **如果不存在，将当前元素的值和下标存入哈希表。**

```csharp
public class Solution {
    public int[] TwoSum(int[] nums, int target) {
        // 哈希表，存储元素值和对应的下标
        Dictionary<int, int> map = new Dictionary<int, int>();

        for (int i = 0; i < nums.Length; i++) {
            int complement = target - nums[i];
            // 检查哈希表中是否存在 complement
            if (map.ContainsKey(complement)) {
                return new int[] { map[complement], i };
            }
            // 将当前元素的值和下标存入哈希表
            map[nums[i]] = i;
        }

        // 如果没有找到，返回空数组（根据题目描述，假设一定有解）
        return new int[0];
    }
}
```

## [202. 快乐数](https://leetcode.cn/problems/happy-number/)

编写一个算法来判断一个数 `n` 是不是快乐数。

**「快乐数」** 定义为：

- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
- 然后重复这个过程直到这个数变为 1，也可能是 **无限循环** 但始终变不到 1。
- 如果这个过程 **结果为** 1，那么这个数就是快乐数。

如果 `n` 是 *快乐数* 就返回 `true` ；不是，则返回 `false` 。

**示例 1：**

```
输入：n = 19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

**示例 2：**

```
输入：n = 2
输出：false
```

> **思路一：用哈希表记录遇到过的数，如果再次遇到，说明进入循环，就不是快乐数。**

```csharp
public class Solution {
    public bool IsHappy(int n) {
        // 用一个哈希表记录遇到过的数
        HashSet<int> seen = new HashSet<int>();

        while(n != 1 && seen.Contains(n) == false) {
            seen.Add(n);
            n = GetNext(n);
        }

        return n == 1;
    }

    /** 辅助函数：计算下一个数（每个数字的平方和）*/
    private int GetNext(int n) {
        int sum = 0;
        while(n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n = n / 10;
        }
        return sum;
    }
}
```

> **思路二：快慢指针；慢指针每次计算一次平方和，快指针每次计算两次平方和。**
>
> - **如果快指针等于慢指针，说明进入了循环。**
> - **如果快指针等于 1，说明是快乐数。**

```csharp
public class Solution {
    public bool IsHappy(int n) {
        int slow = n;
        int fast = GetNext(n);

        while(fast != 1 && fast != slow) {
            slow = GetNext(slow); // 慢指针走一步
            fast = GetNext(GetNext(fast)); // 快指针走两步
        }

        // 如果快指针等于 1，说明是快乐数
        return fast == 1;
    }

    /** 辅助函数：计算下一个数（每个数字的平方和）*/
    private int GetNext(int n) {
        int sum = 0;
        while(n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n = n / 10;
        }
        return sum;
    }
}
```

## [219. 存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)

给你一个整数数组 `nums` 和一个整数 `k` ，判断数组中是否存在两个 **不同的索引** `i` 和 `j` ，满足 `nums[i] == nums[j]` 且 `abs(i - j) <= k` 。如果存在，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：nums = [1,2,3,1], k = 3
输出：true
```

**示例 2：**

```
输入：nums = [1,0,1,1], k = 1
输出：true
```

**示例 3：**

```
输入：nums = [1,2,3,1,2,3], k = 2
输出：false
```

> **思路一：哈希表**

```csharp
public class Solution {
    public bool ContainsNearbyDuplicate(int[] nums, int k) {
        Dictionary<int, int> dict = new Dictionary<int, int>();

        for(int i=0; i<nums.Length; i++) {
            int num = nums[i];
            if(dict.ContainsKey(num) == false) {
                // 字典中不存在该元素，储存它的索引
                dict[num] = i;
            } else {
                // 已存在该元素，判断索引是否和当前索引的相差 ≤ k
                int abs = Math.Abs(dict[num] - i);
                if(abs <= k) {
                    return true;
                } else {
                    // 如果重复元素不满足条件，更新索引
                    dict[num] = i;
                }
            }
        }

        return false;
    }
}
```

> **思路二：集合+滑动窗口**

```csharp
public class Solution {
    public bool ContainsNearbyDuplicate(int[] nums, int k) {
        // 用一个哈希表储存窗口中的元素
        HashSet<int> set = new HashSet<int>();
        for(int i=0; i<nums.Length; i++) {
            if(i > k) {
                // 不在窗口的元素被移除
                set.Remove(nums[i-k-1]);
            }
            int num = nums[i];
            // 检查窗口中是否有重复元素
            if(set.Contains(num)) {
                return true;
            } else {
                // 否则加入当前元素
                set.Add(num);
            }
        }

        return false;
    }
}
```

## [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

示例 2：****

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

> **思路一：先排序，再遍历数组，用 `HashSet` 记录已访问元素，动态更新最长连续序列的长度。**

```csharp
public class Solution {
    public int LongestConsecutive(int[] nums) {
        if (nums == null || nums.Length == 0) {
            return 0;
        }
        // 给数组排序，O(nlog(n))
        Array.Sort(nums);
        int maxLen = 1; int curLen = 1; // 最大子序列长度和当前遍历的子序列长度
        HashSet<int> set = new HashSet<int>();
        for(int i=0; i<nums.Length; i++) {
            int num = nums[i];
            // 不包含上一个元素，说明序列断了，需要把curLen重置成1
            if(set.Contains(num-1) == false) {
                curLen = 1;
                set.Add(num);
            } else {
                if(set.Contains(num) == false) {
                    // 包含上一个元素，但不包含自己，需要更新curLen和maxLen
                    curLen ++;
                    maxLen = Math.Max(maxLen, curLen);
                    set.Add(num);
                }
            }
        }

        return maxLen;
    }
}
```

> **思路二：使用 `HashSet`预先存储元素（去重）。**
>
> 1. **查找连续序列：对于每个元素，只有当它是某个连续序列的起点时（即 `set.Contains(num - 1)` 为 `false`），才开始计算该序列的长度。**
> 2. **计算序列长度：从当前元素开始，逐步查找下一个连续的元素，直到找不到为止，同时记录序列的长度。**
> 3. **更新最大长度：每次找到一个完整的连续序列后，更新 `maxLen`。**

```csharp
public class Solution {
    public int LongestConsecutive(int[] nums) {
        if (nums == null || nums.Length == 0) {
            return 0;
        }
        
        // 使用集合储存所有元素
        HashSet<int> set = new HashSet<int>(nums);
        int maxLen = 0;

        // 遍历集合里的每个元素
        foreach(int num in set) {
            if(set.Contains(num - 1) == false) {
                // 只有当当前数字是序列的起点时，才开始计算长度
                int curNum = num;
                int curLen = 1;

                // 循环查找下一个元素，直到集合不包含
                while(set.Contains(curNum + 1) == true) {
                    curNum ++; curLen ++;
                }

                // 更新最大长度
                maxLen = Math.Max(maxLen, curLen);
            }
        }

        return maxLen;
    }
}
```

# 【区间】

## [228. 汇总区间](https://leetcode.cn/problems/summary-ranges/)

给定一个  **无重复元素** 的 **有序** 整数数组 `nums` 。

返回 ***恰好覆盖数组中所有数字** 的 **最小有序** 区间范围列表* 。也就是说，`nums` 的每个元素都恰好被某个区间范围所覆盖，并且不存在属于某个范围但不属于 `nums` 的数字 `x` 。

列表中的每个区间范围 `[a,b]` 应该按如下格式输出：

- `"a->b"` ，如果 `a != b`
- `"a"` ，如果 `a == b`

**示例 1：**

```
输入：nums = [0,1,2,4,5,7]
输出：["0->2","4->5","7"]
解释：区间范围是：
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"
```

**示例 2：**

```
输入：nums = [0,2,3,4,6,8,9]
输出：["0","2->4","6","8->9"]
解释：区间范围是：
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"
```

> **思路：遍历数组，检查元素是否是起始点，如果是起始点则需要在`IList<int>`中新增字符串，并通过检查和后一个元素的关系得到结束点，起始点元素和结束点元素之间用`->`连接。**

```csharp
public class Solution {
    public IList<string> SummaryRanges(int[] nums) {
        int n = nums.Length;
        IList<string> res = new List<string>();
        if(n == 0) {
            return res;
        }

        for(int i=0; i<n; i++) {
            // 只有起始点才在列表中新增字符串
            if(i == 0 || nums[i] != nums[i-1] + 1) {
                StringBuilder sb = new StringBuilder(nums[i].ToString());
                int end = i;
                while(end < n-1 && nums[end] + 1 == nums[end + 1]) {
                    end ++;
                }
                if(end > i) {
                    sb.Append("->" + nums[end].ToString());
                }

                res.Add(sb.ToString());
            }
        }

        return res;
    }
}
```

## [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

**示例 1：**

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

**示例 2：**

```
输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

> **思路：把区间按照左端点升序排序，这样可合并的区间在排完序的列表中一定是连续的；再遍历排完序的列表，根据左端点是否大于结果列表中最后一个元素的右端点，判断需要新开一个区间还是合并已有区间。**

```csharp
public class Solution {
    public int[][] Merge(int[][] intervals) {
        IList<int[]> res = new List<int[]>();

        // 先对intervals根据第一个元素大小进行排序
        Array.Sort(intervals, (int[] x, int[] y) => {
            return x[0] - y[0];
        });

        for (int i = 0; i < intervals.Length; i++) {
            // 如果res为空或者当前区间的开始点大于res中最后一个区间的结束点，直接添加
            if (res.Count == 0 || intervals[i][0] > res[res.Count - 1][1]) {
                res.Add(new int[] { intervals[i][0], intervals[i][1] });
            } else {
                // 否则，合并区间，更新res中最后一个区间的结束点
                res[res.Count - 1][1] = Math.Max(res[res.Count - 1][1], intervals[i][1]);
            }
        }

        return res.ToArray();
    }
}
```

## [57. 插入区间](https://leetcode.cn/problems/insert-interval/)

给你一个 **无重叠的** *，*按照区间起始端点排序的区间列表 `intervals`，其中 `intervals[i] = [starti, endi]` 表示第 `i` 个区间的开始和结束，并且 `intervals` 按照 `starti` 升序排列。同样给定一个区间 `newInterval = [start, end]` 表示另一个区间的开始和结束。

在 `intervals` 中插入区间 `newInterval`，使得 `intervals` 依然按照 `starti` 升序排列，且区间之间不重叠（如果有必要的话，可以合并区间）。

返回插入之后的 `intervals`。

**注意** 你不需要原地修改 `intervals`。你可以创建一个新数组然后返回它。 

**示例 1：**

```
输入：intervals = [[1,3],[6,9]], newInterval = [2,5]
输出：[[1,5],[6,9]]
```

**示例 2：**

```
输入：intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出：[[1,2],[3,10],[12,16]]
解释：这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。
```

> **思路一：寻找适合的 `newInterval` 插入时机**
>
> 1. **遍历区间列表，找到 `newInterval` 应该插入的位置。**
> 2. **在插入时，检查是否需要与前后区间合并。**
> 3. **最后处理未插入的 `newInterval`。**

```csharp
public class Solution {
    public int[][] Insert(int[][] intervals, int[] newInterval) {
        IList<int[]> res = new List<int[]>();
        bool isAdded = false; // 区间newInterval是否已插入
        foreach(int[] curInterval in intervals) {
            if(!isAdded && curInterval[0] > newInterval[0]) {
                // 找到适合插入newInterval的时机
                AddOrMerge(res, newInterval);
                isAdded = true;
            }
            // 依次把每个区间加入结果
            AddOrMerge(res, curInterval);
        }

        // 如果newInterval还没有被插入，则在最后插入
        if(!isAdded) {
            AddOrMerge(res, newInterval);
        }

        return res.ToArray();
    }

    /** 辅助函数，向已有区间插入或合并一个新的区间 */
    private void AddOrMerge(IList<int[]> res, int[] interval) {
        if(res.Count == 0 || res[res.Count - 1][1] < interval[0]) {
            res.Add(interval);
        } else {
            res[res.Count - 1][1] = Math.Max(res[res.Count - 1][1], interval[1]);
        }
    }
}
```

> **思路二：动态更新 `newInterval`，使其与所有重叠的区间合并。**
>
> 1. **遍历区间列表，将不与`newInterval`重叠的区间直接加入结果。**
> 2. **将与`newInterval`重叠的区间与`newInterval`合并后动态更新 `newInterval` 。**
> 3. **最后将 `newInterval` 加入结果。**

```csharp
public class Solution {
    public int[][] Insert(int[][] intervals, int[] newInterval) {
        IList<int[]> res = new List<int[]>();
        
        foreach(int[] curInterval in intervals) {
            if(curInterval[1] < newInterval[0]) {
                // newInterval之前的不重叠的区间直接插入
                res.Add(curInterval);
            } else if(curInterval[0] > newInterval[1]) {
                // newInterval之后的不重叠的区间先插入newInterval，并替换newInterval
                res.Add(newInterval);
                newInterval = curInterval;
            } else {
                // 合并和newInterval重叠的interval
                newInterval[0] = Math.Min(newInterval[0], curInterval[0]);
                newInterval[1] = Math.Max(newInterval[1], curInterval[1]);
            }
        }
        res.Add(newInterval);

        return res.ToArray();
    }
}
```

## [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

有一些球形气球贴在一堵用 XY 平面表示的墙面上。墙面上的气球记录在整数数组 `points` ，其中`points[i] = [xstart, xend]` 表示水平直径在 `xstart` 和 `xend`之间的气球。你不知道气球的确切 y 坐标。

一支弓箭可以沿着 x 轴从不同点 **完全垂直** 地射出。在坐标 `x` 处射出一支箭，若有一个气球的直径的开始和结束坐标为 `xstart`，`xend`， 且满足  `xstart ≤ x ≤ xend`，则该气球会被 **引爆** 。可以射出的弓箭的数量 **没有限制** 。 弓箭一旦被射出之后，可以无限地前进。

给你一个数组 `points` ，*返回引爆所有气球所必须射出的 **最小** 弓箭数* 。

**示例 1：**

```
输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
解释：气球可以用2支箭来爆破:
在x = 6处射出箭，击破气球[2,8]和[1,6]。
在x = 11处发射箭，击破气球[10,16]和[7,12]。
```

**示例 2：**

```
输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4
解释：每个气球需要射出一支箭，总共需要4支箭。
```

**示例 3：**

```
输入：points = [[1,2],[2,3],[3,4],[4,5]]
输出：2
解释：气球可以用2支箭来爆破:
在x = 2处发射箭，击破气球[1,2]和[2,3]。
在x = 4处射出箭，击破气球[3,4]和[4,5]。
```

> **思路一：排序+合并区间，遍历按左端点升序排序后的区间，遇到不重叠的区间直接加入，遇到重叠的区间取交集。**

```csharp
public class Solution {
    public int FindMinArrowShots(int[][] points) {
        // 存交集
        IList<int[]> res = new List<int[]>();

        // 给区间按左端点大小升序排序
        Array.Sort(points, (int[] x, int[] y) => {
            return x[0].CompareTo(y[0]);  // 使用 CompareTo 避免溢出
        });

        // 合并区间
        foreach(var curInterval in points) {
            if(res.Count == 0 || res[res.Count-1][1] < curInterval[0]) {
                res.Add(curInterval);
            } else {
                res[res.Count-1][0] = Math.Max(res[res.Count-1][0], curInterval[0]);
                res[res.Count-1][1] = Math.Min(res[res.Count-1][1], curInterval[1]);
            }
        }

        return res.Count;
    }
}
```

> **思路二：排序+贪心算法，遍历按右端点升序排序后的区间，箭的位置为最左边的右端点，遇到不重叠的区间则增加箭的数量。**
>
> - **按右端点排序：确保每次处理的区间是结束最早的，从而最大化每支箭的覆盖范围。**
> - **选最左区间的右端点：作为射箭位置，可以覆盖尽可能多的区间，达到贪心的最优选择。**

```csharp
public class Solution {
    public int FindMinArrowShots(int[][] points) {
        // 给区间按右端点大小升序排序
        Array.Sort(points, (int[] x, int[] y) => {
            return x[1].CompareTo(y[1]);  // 使用 CompareTo 避免溢出
        });

        // 贪心算法
        int pos = points[0][1]; // 初始位置为右端点最左的区间的右端点
        int res = 1; // 需要的箭个数
        foreach(var curInterval in points) {
            if(curInterval[0] > pos) {
                // 当前位置无法射中该区间，需要新增箭并更新pos为该区间的右端点
                pos = curInterval[1];
                res ++;
            }
        }

        return res;
    }
}
```

# 【栈】

## [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

**示例 1：**

```
输入：s = "()"
输出：true
```

**示例 2：**

```
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```
输入：s = "(]"
输出：false
```

**示例 4：**

```
输入：s = "([])"
输出：true
```

> **思路：字典存储括号对的匹配关系，左括号入栈，右括号弹栈判断是否为括号对。**

```csharp
public class Solution {
    public bool IsValid(string s) {
        if (s.Length % 2 > 0) {
            return false;
        }

        // 使用字典存储括号的对应关系
        Dictionary<char, char> bracketPairs = new Dictionary<char, char> {
            { ')', '(' },
            { ']', '[' },
            { '}', '{' }
        };

        Stack<char> stack = new Stack<char>();

        foreach (char c in s) {
            if (bracketPairs.ContainsValue(c)) {
                // 左括号入栈
                stack.Push(c);
            } else if (bracketPairs.ContainsKey(c)) {
                // 右括号，检查栈顶元素是否匹配
                if (stack.Count == 0 || stack.Peek() != bracketPairs[c]) {
                    return false;
                }
                stack.Pop();
            }
        }

        return stack.Count == 0;
    }
}
```

## [71. 简化路径](https://leetcode.cn/problems/simplify-path/)

给你一个字符串 `path` ，表示指向某一文件或目录的 Unix 风格 **绝对路径** （以 `'/'` 开头），请你将其转化为 **更加简洁的规范路径**。

在 Unix 风格的文件系统中规则如下：

- 一个点 `'.'` 表示当前目录本身。
- 此外，两个点 `'..'` 表示将目录切换到上一级（指向父目录）。
- 任意多个连续的斜杠（即，`'//'` 或 `'///'`）都被视为单个斜杠 `'/'`。
- 任何其他格式的点（例如，`'...'` 或 `'....'`）均被视为有效的文件/目录名称。

返回的 **简化路径** 必须遵循下述格式：

- 始终以斜杠 `'/'` 开头。
- 两个目录名之间必须只有一个斜杠 `'/'` 。
- 最后一个目录名（如果存在）**不能** 以 `'/'` 结尾。
- 此外，路径仅包含从根目录到目标文件或目录的路径上的目录（即，不含 `'.'` 或 `'..'`）。

返回简化后得到的 **规范路径** 。

**示例 1：**

```
输入：path = "/home/"
输出："/home"
解释：应删除尾随斜杠。
```

**示例 2：**

```
输入：path = "/home//foo/"
输出："/home/foo"
解释：多个连续的斜杠被单个斜杠替换。
```

**示例 3：**

```
输入：path = "/home/user/Documents/../Pictures"
输出："/home/user/Pictures"
解释：两个点 `".."` 表示上一级目录（父目录）。
```

**示例 4：**

```
输入：path = "/../"
输出："/"
解释：不可能从根目录上升一级目录。
```

**示例 5：**

```
输入：path = "/.../a/../b/c/../d/./"
输出："/.../b/d"
解释："..."在这个问题中是一个合法的目录名。
```

> **思路：用栈，先把字符串用`/`分隔，遍历字符串数组，遇到空字符串和`.`无需处理，遇到`..`若栈中元素个数不为0则弹栈，其他情况入栈；最后再用字符串拼接得到结果。**

```csharp
public class Solution {
    public string SimplifyPath(string path) {
        string[] words = path.Split('/');
        Stack<string> stack = new Stack<string>();

        foreach (string word in words) {
            // 忽略空字符串
            if(word.Length > 0) {
                if(word != "." && word != "..") {
                    // 合法目录名入栈
                    stack.Push(word);
                } else if(word == "..") {
                    // 遇到".."弹栈
                    if(stack.Count > 0) {
                        stack.Pop();
                    }
                }
            }
        }

        // 处理返回的字符串，把栈转换成数组，每个元素用"/"分隔，拼接在第一个"/"末尾
        StringBuilder result = new StringBuilder("/");
        var reversedStack = stack.Reverse();
        result.Append(string.Join("/", reversedStack));

        return result.ToString();
    }
}
```

## [155. 最小栈](https://leetcode.cn/problems/min-stack/)

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

实现 `MinStack` 类:

- `MinStack()` 初始化堆栈对象。
- `void push(int val)` 将元素val推入堆栈。
- `void pop()` 删除堆栈顶部的元素。
- `int top()` 获取堆栈顶部的元素。
- `int getMin()` 获取堆栈中的最小元素。

**示例 1:**

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

> **思路一：辅助栈，使用一个辅助栈`minStack`维护当前栈`stack`中的最小元素，每次压入元素时比较压入元素和栈顶元素，决定是否压入`minStack`，从而保证它是递增栈，且栈顶是`stack`中的最小元素；弹栈时比较弹出元素是否是最小元素，是则`minStack`中同步弹出。**

```csharp
public class MinStack {
    private Stack<int> stack;
    // 辅助栈，栈顶永远是当前栈的最小元素
    private Stack<int> minStack;

    public MinStack() {
        stack = new Stack<int>();
        minStack = new Stack<int>();
    }
    
    public void Push(int val) {
        stack.Push(val);
        if(minStack.Count == 0 || val <= minStack.Peek()) {
            minStack.Push(val);
        }
    }
    
    public void Pop() {
        if(stack.Count == 0) {
            return;
        }
        int val = stack.Pop();
        if(minStack.Peek() == val) {
            // 说明当前出栈元素是最小元素，更新minStack
            minStack.Pop();
        }
    }
    
    public int Top() {
        return stack.Peek();
    }
    
    public int GetMin() {
        return minStack.Peek();
    }
}
```

> **思路二：主栈 `stack` 和辅助栈 `minStack` 始终保持相同数量的元素。**

```csharp
public class MinStack {
    private Stack<int> stack;
    private Stack<int> minStack;

    public MinStack() {
        stack = new Stack<int>();
        minStack = new Stack<int>();
    }
    
    public void Push(int val) {
        stack.Push(val);
        // 如果 minStack 为空，或者 val 比 minStack 的栈顶小，则压入 val
        // 否则压入 minStack 的栈顶元素（保持两个栈元素数量相同）
        if (minStack.Count == 0 || val < minStack.Peek()) {
            minStack.Push(val);
        } else {
            minStack.Push(minStack.Peek());
        }
    }
    
    public void Pop() {
        if (stack.Count == 0) return;
        stack.Pop();
        minStack.Pop();
    }
    
    public int Top() {
        return stack.Peek();
    }
    
    public int GetMin() {
        return minStack.Peek();
    }
}
```

## [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

给你一个字符串数组 `tokens` ，表示一个根据 [逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437) 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

**注意：**

- 有效的算符为 `'+'`、`'-'`、`'*'` 和 `'/'` 。
- 每个操作数（运算对象）都可以是一个整数或者另一个表达式。
- 两个整数之间的除法总是 **向零截断** 。
- 表达式中不含除零运算。
- 输入是一个根据逆波兰表示法表示的算术表达式。
- 答案及所有中间计算结果可以用 **32 位** 整数表示。

**示例 1：**

```
输入：tokens = ["2","1","+","3","*"]
输出：9
解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
```

**示例 2：**

```
输入：tokens = ["4","13","5","/","+"]
输出：6
解释：该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6
```

**示例 3：**

```
输入：tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
输出：22
解释：该算式转化为常见的中缀算术表达式为：
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

> **思路：栈模拟。**

```csharp
public class Solution {
    public int EvalRPN(string[] tokens) {
        Stack<int> stack = new Stack<int>();
        
        foreach (string token in tokens) {
            if (token == "+") {
                int b = stack.Pop(); // 第二个操作数
                int a = stack.Pop(); // 第一个操作数
                stack.Push(a + b);   // 计算结果并压入栈
            } else if (token == "-") {
                int b = stack.Pop(); // 第二个操作数
                int a = stack.Pop(); // 第一个操作数
                stack.Push(a - b);   // 计算结果并压入栈
            } else if (token == "*") {
                int b = stack.Pop(); // 第二个操作数
                int a = stack.Pop(); // 第一个操作数
                stack.Push(a * b);   // 计算结果并压入栈
            } else if (token == "/") {
                int b = stack.Pop(); // 第二个操作数
                int a = stack.Pop(); // 第一个操作数
                stack.Push(a / b);   // 计算结果并压入栈
            } else {
                // 普通数字，直接入栈
                stack.Push(int.Parse(token));
            }
        }
        
        // 最终栈中剩下的唯一元素就是结果
        return stack.Pop();
    }
}
```

## [224. 基本计算器](https://leetcode.cn/problems/basic-calculator/)

给你一个字符串表达式 `s` ，请你实现一个基本计算器来计算并返回它的值。

注意:不允许使用任何将字符串作为数学表达式计算的内置函数，比如 `eval()` 。 

**示例 1：**

```
输入：s = "1 + 1"
输出：2
```

**示例 2：**

```
输入：s = " 2-1 + 2 "
输出：3
```

**示例 3：**

```
输入：s = "(1+(4+5+2)-3)+(6+8)"
输出：23
```

> **思路：栈模拟；栈用于处理括号的嵌套。当遇到左括号 `(` 时，将当前的结果和符号压入栈中，以便在遇到右括号 `)` 时恢复；当遇到右括号 `)` 时，从栈中弹出符号和之前的结果，与当前结果结合。**
>
> **符号的处理：**
>
> - **使用变量 `sign` 表示当前数字的符号（`1` 表示正数，`-1` 表示负数）。**
> - **遇到 `+` 或 `-` 时，更新 `sign`。**
>
> **数字的处理：**
>
> - **使用 `char.IsDigit` 判断字符是否为数字。**
> - **如果当前字符是数字，则继续读取后续字符，直到遇到非数字字符，将完整的数字解析出来。**
>
> **括号的处理：**
>
> - **遇到左括号 `(` 时，将当前结果和符号压入栈中，并重置结果和符号。**
> - **遇到右括号 `)` 时，从栈中弹出符号和之前的结果，与当前结果结合。**

```csharp
public class Solution {
    public int Calculate(string s) {
        Stack<int> stack = new Stack<int>();
        int sign = 1; // 是否是负数的标志
        int n = s.Length;
        int res = 0;

        for(int i=0; i<n; i++) {
            char c = s[i];
            // 遍历到数字，一直遍历直到下一位不再是数字
            if(char.IsDigit(c)) {
                int cur = c - '0';
                while(i+1 < n && char.IsDigit(s[i+1])) {
                    cur = cur * 10 + s[i+1] - '0'; i++;
                }
                res += sign * cur;
            } else if(c == '+') {
                sign = 1;
            } else if(c == '-') {
                sign = -1;
            } else if(c == '(') {
                stack.Push(res); // 压入之前的计算结果（如果左括号是第一个字符，压入0）
                res = 0;
                stack.Push(sign); // 把左括号前面的符号也压入
                sign = 1;
            } else if(c == ')') {
                res = res * stack.Pop() /* 符号 */ + stack.Pop() /* 左括号之前的计算结果 */; 
            }
        }

        return res;
    }
}
```

> **思路二：辅助栈；栈 `ops` 用于存储括号前的符号。每当遇到左括号 `(` 时，将当前的符号压入栈中，表示进入一个新的括号层级；每当遇到右括号 `)` 时，从栈中弹出符号，表示退出当前括号层级；栈顶元素始终表示当前括号内的符号状态。**

```csharp
public class Solution {
    public int Calculate(string s) {
        Stack<int> stack = new Stack<int>();
        Stack<int> ops = new Stack<int>(); ops.Push(1); // 储存操作符的栈
        int sign = 1; // 是否是负数的标志
        int n = s.Length;
        int res = 0;

        for(int i=0; i<n; i++) {
            char c = s[i];
            // 遍历到数字，一直遍历直到下一位不再是数字
            if(char.IsDigit(c)) {
                int cur = c - '0';
                while(i+1 < n && char.IsDigit(s[i+1])) {
                    cur = cur * 10 + s[i+1] - '0'; i++;
                }
                res += sign * cur;
            } else if(c == '+') {
                sign = ops.Peek();
            } else if(c == '-') {
                sign = -ops.Peek();
            } else if(c == '(') {
                // 压入操作符
                ops.Push(sign);
            } else if(c == ')') {
                // 弹出操作符
                ops.Pop();
            }
        }

        return res;
    }
}
```

# 【链表】

## [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：`pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

*如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。

**示例 1：**

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

> **思路一：环形列表。**

```csharp
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public bool HasCycle(ListNode head) {
        if(head == null || head.next == null) {
            return false;
        }
        ListNode fast = head; ListNode slow = head;
        while(fast!= null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow) {
                return true;
            }
        }

        return false;
    }
}
```

> **思路二：用哈希集合记录已经访问过的节点。**

```csharp
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public bool HasCycle(ListNode head) {
        if(head == null || head.next == null) {
            return false;
        }
        
        HashSet<ListNode> visited = new HashSet<ListNode>();
        ListNode current = head;
        while(current != null) {
            if(visited.Contains(current) == false) {
                visited.Add(current);
            } else {
                return true;
            }
            current = current.next;
        }

        return false;
    }
}
```

## [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。 

**示例 1：**

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

**示例 2：**

```
输入：l1 = [0], l2 = [0]
输出：[0]
```

**示例 3：**

```
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

> **思路：每一位依次相加，用标志`isCarry`表示上一位是否有进位。**

```csharp
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
    public ListNode AddTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(); ListNode head = res;

        ListNode p1 = l1; ListNode p2 = l2;
        bool isCarry = false; // 是否进位的标志
        while(p1 != null && p2 != null) {
            int add = p1.val + p2.val;
            if(isCarry) add += 1;

            // 更新是否进位
            isCarry = add >= 10;
            // 更新数字和结果链表的指针
            res.next = new ListNode(add % 10); res = res.next;
            p1 = p1.next; p2 = p2.next;
        }

        // 处理p1剩下的数字
        while(p1 != null) {
            int add = p1.val;
            if(isCarry) add += 1;

            isCarry = add >= 10;
            res.next = new ListNode(add % 10); res = res.next;
            p1 = p1.next;
        }

        // 处理p2剩下的数字
        while(p2 != null) {
            int add = p2.val;
            if(isCarry) add += 1;

            isCarry = add >= 10;
            res.next = new ListNode(add % 10); res = res.next;
            p2 = p2.next;
        }

        // 处理最后的进位
        if(isCarry) {
            res.next = new ListNode(1);
        }

        return head.next;
    }
}
```

优化：

```csharp
public class Solution {
    public ListNode AddTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(); // 虚拟头节点
        ListNode head = res; // 指向结果链表的头节点

        ListNode p1 = l1, p2 = l2;
        int carry = 0; // 进位标志

        // 遍历两个链表
        while (p1 != null || p2 != null) {
            // 获取当前节点的值
            int val1 = (p1 != null) ? p1.val : 0;
            int val2 = (p2 != null) ? p2.val : 0;

            // 计算当前位的和
            int sum = val1 + val2 + carry;
            carry = sum / 10; // 更新进位
            res.next = new ListNode(sum % 10); // 创建新节点
            res = res.next; // 移动结果链表的指针

            // 移动链表指针
            if (p1 != null) p1 = p1.next;
            if (p2 != null) p2 = p2.next;
        }

        // 处理最后的进位
        if (carry > 0) {
            res.next = new ListNode(carry);
        }

        return head.next; // 返回结果链表的头节点
    }
}
```

## [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例 1：**

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2：**

```
输入：l1 = [], l2 = []
输出：[]
```

**示例 3：**

```
输入：l1 = [], l2 = [0]
输出：[0]
```

> **思路一：递归，比较两个列表的第一个元素，将较小的元素作为合并后的第一个元素，然后递归地合并剩余的部分。**

```csharp
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
    public ListNode MergeTwoLists(ListNode list1, ListNode list2) {
        if(list1 == null) {
            return list2;
        } else if(list2 == null) {
            return list1;
        } else if(list1.val < list2.val) {
            list1.next = MergeTwoLists(list1.next, list2);
            return list1;
        } else {
            list2.next = MergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```

> **思路二：迭代。**

```csharp
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
    public ListNode MergeTwoLists(ListNode list1, ListNode list2) {
        ListNode res = new ListNode();
        ListNode head = res;

        ListNode p1 = list1; ListNode p2 = list2;
        while(p1 != null && p2 != null) {
            if(p1.val < p2.val) {
                res.next = new ListNode(p1.val);
                res = res.next;
                p1 = p1.next;
            } else {
                res.next = new ListNode(p2.val);
                res = res.next;
                p2 = p2.next;
            }
        }

        while(p1 != null) {
            res.next = new ListNode(p1.val); res = res.next;
            p1 = p1.next;
        }

        while(p2 != null) {
            res.next = new ListNode(p2.val); res = res.next;
            p2 = p2.next;
        }

        return head.next;
    }
}
```

## [138. 随机链表的复制](https://leetcode.cn/problems/copy-list-with-random-pointer/)

给你一个长度为 `n` 的链表，每个节点包含一个额外增加的随机指针 `random` ，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 **[深拷贝](https://baike.baidu.com/item/深拷贝/22785317?fr=aladdin)**。 深拷贝应该正好由 `n` 个 **全新** 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 `next` 指针和 `random` 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。**复制链表中的指针都不应指向原链表中的节点** 。

例如，如果原链表中有 `X` 和 `Y` 两个节点，其中 `X.random --> Y` 。那么在复制链表中对应的两个节点 `x` 和 `y` ，同样有 `x.random --> y` 。

返回复制链表的头节点。

用一个由 `n` 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 `[val, random_index]` 表示：

- `val`：一个表示 `Node.val` 的整数。
- `random_index`：随机指针指向的节点索引（范围从 `0` 到 `n-1`）；如果不指向任何节点，则为 `null` 。

你的代码 **只** 接受原链表的头节点 `head` 作为传入参数。

**示例 1：**

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**示例 2：**

```
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```

**示例 3：**

```
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```

> **思路一：两次遍历+哈希表。**
>
> 1. **第一次遍历：创建一个新链表，同时用字典 `map` 记录原链表节点和新链表节点的映射关系。**
> 2. **第二次遍历：根据字典 `map`，将新链表的 `random` 指针指向对应的新节点。**
> 3. **返回结果：返回新链表的头节点。**

```csharp
/*
// Definition for a Node.
public class Node {
    public int val;
    public Node next;
    public Node random;
    
    public Node(int _val) {
        val = _val;
        next = null;
        random = null;
    }
}
*/

public class Solution {
    public Node CopyRandomList(Node head) {
        if (head == null) return null;

        Node res = new Node(-1);
        Node resHead = res;
        // 用一个字典保存原链表和新链表的映射关系
        Dictionary<Node, Node> map = new Dictionary<Node, Node>();
        // 用于遍历的指针
        Node p = head;

        while(p != null) {
            res.next = new Node(p.val);
            // 键：原链表节点；值：新链表节点
            map.Add(p, res.next);
            res = res.next; p = p.next;
        }

        p = head; res = resHead.next;
        while(p != null) {
            // 再次遍历，根据键值对重置random指针
            if(p.random != null) {
                res.random = map[p.random];
            }
            res = res.next; p = p.next;
        }
        return resHead.next;
    }
}
```

> **思路二：回溯+哈希表：字典里有就返回，字典里没有就复制。**
>
> 1. **递归复制节点：通过递归遍历原链表，为每个节点创建对应的新节点，并用字典缓存原节点和新节点的映射关系。**
> 2. **递归修复指针：在递归过程中，同时复制 `next` 和 `random` 指针，利用字典确保每个节点只被复制一次。**
> 3. **返回结果：递归结束时，返回新链表的头节点。**

```csharp
/*
// Definition for a Node.
public class Node {
    public int val;
    public Node next;
    public Node random;
    
    public Node(int _val) {
        val = _val;
        next = null;
        random = null;
    }
}
*/

public class Solution {
    Dictionary<Node, Node> cachedNode = new Dictionary<Node, Node>();
    public Node CopyRandomList(Node head) {
        if(head == null) {
            return null;
        }

        if (!cachedNode.ContainsKey(head)) {
            // 字典里没有，复制当前节点
            Node headNew = new Node(head.val);
            cachedNode.Add(head, headNew); // 添加到字典
            headNew.next = CopyRandomList(head.next); // 递归复制 next
            headNew.random = CopyRandomList(head.random); // 递归复制 random
        }

        return cachedNode[head];
    }
}
```

## [92. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)

给你单链表的头指针 `head` 和两个整数 `left` 和 `right` ，其中 `left <= right` 。请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。

**示例 1：**

```
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

**示例 2：**

```
输入：head = [5], left = 1, right = 1
输出：[5]
```

> **思路一：翻转部分+重新指定连接关系**
>
> - **先实现一个反转链表的辅助函数**
> - **断掉right节点的next，使用辅助函数翻转从left节点到right节点的部分**
> - **重新指定连接关系：left节点的前节点的next为right节点，left节点的next为right之后的节点**

```csharp
public class Solution {
    public ListNode ReverseBetween(ListNode head, int left, int right) {
        if (head == null || head.next == null || left == right) {
            return head;
        }

        // 伪头节点，方便处理 left = 1 的情况
        ListNode dummy = new ListNode(-1, head);
        ListNode preLeftNode = dummy;

        // 找到 left 节点的前一个节点
        for (int i = 1; i < left; i++) {
            preLeftNode = preLeftNode.next;
        }

        // 找到 right 节点
        ListNode rightNode = preLeftNode;
        for (int i = left; i <= right; i++) {
            rightNode = rightNode.next;
        }

        // 断开 right 节点之后的链表
        ListNode afterRightNode = rightNode.next;
        rightNode.next = null;

        // 翻转从 left 节点到 right 节点的部分
        ListNode reversedHead = ReverseList(preLeftNode.next);

        // 将翻转后的链表重新连接到原链表
        preLeftNode.next.next = afterRightNode; // 原来的 left 节点（现在是翻转后的尾节点）连接到 afterRightNode
        preLeftNode.next = reversedHead; // preLeftNode 连接到翻转后的头节点

        return dummy.next;
    }

    // 反转链表
    private ListNode ReverseList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;

        while (current != null) {
            ListNode nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }

        return prev; // 返回新的头节点
    }
}
```

> **思路二：与思路一相同，不封装辅助函数**

```csharp
public class Solution {
    public ListNode ReverseBetween(ListNode head, int left, int right) {
        if (head == null || head.next == null || left == right) {
            return head;
        }

        // 伪头节点，方便处理 left = 1 的情况
        ListNode dummy = new ListNode(-1, head);
        ListNode preLeftNode = dummy;

        // 找到 left 节点的前一个节点
        for (int i = 1; i < left; i++) {
            preLeftNode = preLeftNode.next;
        }

        // 初始化翻转部分的指针
        ListNode current = preLeftNode.next; // left 节点
        ListNode prev = null;
        ListNode next = null;

        // 翻转从 left 到 right 的部分
        for (int i = left; i <= right; i++) {
            next = current.next; // 保存下一个节点
            current.next = prev; // 当前节点指向前一个节点
            prev = current; // 前一个节点移动到当前节点
            current = next; // 当前节点移动到下一个节点
        }

        // 将翻转后的链表重新连接到原链表
        preLeftNode.next.next = current; // 原来的 left 节点（现在是翻转后的尾节点）连接到 right 节点之后的节点
        preLeftNode.next = prev; // preLeftNode 连接到翻转后的头节点

        return dummy.next;
    }
}
```

## [25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

给你链表的头节点 `head` ，每 `k` 个节点一组进行翻转，请你返回修改后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

**示例 1：**

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

**示例 2：**

```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

> **思路一：求余模拟**
>
> 1. **遍历链表：**
>    - **使用指针 `p` 遍历链表，计数器 `count` 记录当前节点位置。**
> 2. **分组翻转：**
>    - **当 `count % k == 0` 时，说明当前节点是第 k 个节点，即当前组的尾节点。**
>      - **断开当前组与下一组的连接，记录下一组的头节点 `nextHead`。**
>      - **翻转当前组，并将翻转后的组连接到上一组的尾节点 `lastTail`。**
>      - **更新 `lastTail` 为当前组的原始头节点（翻转后的尾节点），并连接到下一组。**
>      - **将 `p` 更新为下一组的头节点 `nextHead`，继续遍历。**
>    - **否则，直接 `p = p.next;`  继续遍历**

```csharp
public class Solution {
    public ListNode ReverseKGroup(ListNode head, int k) {
        if (head == null || head.next == null || k == 1) {
            return head;
        }

        ListNode dummy = new ListNode(-1, head);
        ListNode lastTail = dummy; // 上一组的尾节点
        ListNode p = head; // 用于遍历的指针
        int count = 0; // 计数器

        while (p != null) {
            count++;

            if (count % k == 0) {
                // 说明 p 是这一组的尾节点，获取下一组的头节点
                ListNode nextHead = p.next;
                // 断开连接
                p.next = null;
                // 用一个变量记录下当前组原来的头节点
                ListNode originalHead = lastTail.next;
                // 翻转当前组
                ListNode reversedHead = ReverseList(originalHead);
                // 将翻转后的当前组连接到上一组的后面
                lastTail.next = reversedHead;

                // 修改 lastTail 为原先的头（现在是尾）
                lastTail = originalHead;
                // lastTail 连接到下一组
                lastTail.next = nextHead;

                // 更新 p 为下一组的头节点
                p = nextHead;
            } else {
                // 继续遍历
                p = p.next;
            }
        }

        return dummy.next;
    }

    // 辅助函数，翻转链表
    private ListNode ReverseList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;

        while (current != null) {
            ListNode nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }

        return prev; // 返回新的头节点
    }
}
```

> **思路二：递归**
>
> 1. **递归终止条件：如果链表为空或剩余节点不足k个，不需要翻转，直接返回头结点**
> 2. **翻转当前组：翻转当前组的前k个节点**
> 3. **递归处理剩余链表：找到当前组的尾部，递归处理剩余链表并拼在尾部**
> 4. **返回头结点：最后返回翻转后的链表头结点**

```csharp
public class Solution {
    public ListNode ReverseKGroup(ListNode head, int k) {
        if(head == null || head.next == null || k == 1) {
            return head;
        }

        // 检查剩余节点是否足够k个
        ListNode current = head;
        int count = 0;
        while(current != null && count < k)
        {
            current = current.next;
            count ++;
        }

        // 如果剩余节点不足k个，直接返回当前头节点
        if(count < k)
        {
            return head;
        }

        /** 此时current已经是下一组的头结点了 */

        // 翻转当前组的前k个节点
        ListNode reversedHead = ReverseList(head, k);
        // 递归处理剩余链表，把结果连接到当前组的尾部（此时head是当前组的尾部）
        head.next = ReverseKGroup(current, k);

        return reversedHead;
    }

    // 辅助函数：翻转链表的前k个节点
    private ListNode ReverseList(ListNode head, int k)
    {
        ListNode prev = null;
        ListNode current = head;

        while(k > 0)
        {
            ListNode tempNext = current.next;
            current.next = prev;
            prev = current;
            current = tempNext;
            k--;
        }

        return prev;
    }
}
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**示例 1：**[]()

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

> **思路：双指针**
>
> 1. **初始化两个指针 `deleteNode` 和 `lastNode`，都指向虚拟头节点 `dummy`。**
> 2. **先让 `lastNode` 移动 `n` 步，然后同时移动 `deleteNode` 和 `lastNode`，直到 `lastNode` 到达链表末尾。**
> 3. **此时 `deleteNode` 指向倒数第 `n+1` 个节点，直接修改 `deleteNode.next` 即可删除倒数第 `n` 个节点。**

```csharp
public class Solution {
    public ListNode RemoveNthFromEnd(ListNode head, int n) {
        if (head == null) {
            return head;
        }
        ListNode dummy = new ListNode(-1, head);
        ListNode deleteNode = dummy; // 需要删除的节点
        ListNode lastNode = dummy;
        // lastNode移动到head之后的第n个节点
        for(int i=0; i<n; i++) {
            lastNode = lastNode.next;
        }

        // deleteNode和LastNode一起移动，直到deleteNode位于最后一个节点
        while(lastNode.next != null) {
            deleteNode = deleteNode.next;
            lastNode = lastNode.next;
        }

        // 删除deleteNode之后的节点
        deleteNode.next = deleteNode.next.next;

        return dummy.next;
    }
}
```

## [82. 删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/)

给定一个已排序的链表的头 `head` ， *删除原始链表中所有重复数字的节点，只留下不同的数字* 。返回 *已排序的链表* 。

```
输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]
```

```
输入：head = [1,1,1,2,3]
输出：[2,3]
```

> **思路：双指针模拟法**
>
> 1. **初始化两个指针 `prevNode` 和 `curNode`，分别指向虚拟头节点 `dummy`和头结点`head`。**
> 2. **进入`while`循环，检查当前节点和下一个节点的值是否相同**
>    1. **若相同，需要删除所有重复节点（`curNode`跳到之后第一个不重复的节点），`prevNode`连接到`curNode`。**
>    2. **若不同，`preNode`和`curNode`都右移一个节点。**

```csharp
public class Solution {
    public ListNode DeleteDuplicates(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        /** 链表至少有两个节点 */

        ListNode dummy = new ListNode(-101, head);
        ListNode prevNode = dummy;
        ListNode curNode = head; 
        

        while(curNode != null) {
            if(curNode.next != null && curNode.val == curNode.next.val) {
                // 如果当前节点和下一个节点值相同，需要删除所有重复节点
                int duplicateVal = curNode.val;
                while(curNode != null && curNode.val == duplicateVal) {
                    curNode = curNode.next;
                }
                // 跳过所有重复节点
                prevNode.next = curNode;
            } else {
                // 否则，两个指针右移动
                prevNode = prevNode.next; curNode = curNode.next;
            }
        }

        return dummy.next;
    }
}
```

## [61. 旋转链表](https://leetcode.cn/problems/rotate-list/)

给你一个链表的头节点 `head` ，旋转链表，将链表每个节点向右移动 `k` 个位置。

**示例 1：**

```
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]
```

**示例 2：**

```
输入：head = [0,1,2], k = 4
输出：[2,0,1]
```

> **思路：双指针**
>
> 1. **找到链表的长度 `len`，并计算实际需要旋转的步数 `k % len`（避免 `k` 大于链表长度）。**
> 2. **使用双指针，`newTail` 先移动 `k` 步，然后 `oldTail` 和 `newTail` 同时移动，直到 `newTail` 到达链表末尾。**
> 3. **将链表从 `oldTail` 处断开，将后半部分拼接到链表头部，形成旋转后的链表。**

```csharp
public class Solution {
    public ListNode RotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) {
            return head;
        }

        ListNode oldTail = head; // 原链表的尾部节点
        ListNode newTail = head; // 新链表的尾部节点

        int len = 1; // 链表长度
        while (oldTail.next != null) {
            oldTail = oldTail.next;
            len++;
        }

        // 计算实际需要移动的步数
        k = k % len;
        if (k == 0) {
            return head; // 如果 k 是链表长度的倍数，直接返回原链表
        }

        // newTail 先移动 k 步
        for (int i = 0; i < k; i++) {
            newTail = newTail.next;
        }

        // oldTail 和 newTail 同时移动，直到 newTail 到达链表末尾
        oldTail = head;
        while (newTail.next != null) {
            oldTail = oldTail.next;
            newTail = newTail.next;
        }

        // 把从head到oldTail的部分拼接到newTail之后
        ListNode res = oldTail.next;
        oldTail.next = null;
        newTail.next = head;

        return res;
    }
}
```

## [86. 分隔链表](https://leetcode.cn/problems/partition-list/)

给你一个链表的头节点 `head` 和一个特定值 `x` ，请你对链表进行分隔，使得所有 **小于** `x` 的节点都出现在 **大于或等于** `x` 的节点之前。

你应当 **保留** 两个分区中每个节点的初始相对位置。

**示例 1：**

```
输入：head = [1,4,3,2,5,2], x = 3
输出：[1,2,2,4,3,5]
```

**示例 2：**

```
输入：head = [2,1], x = 2
输出：[1,2]
```

> **思路一：模拟法**
>
> 1. **找到尾部节点`tail`，顺便统计链表长度`len`**
> 2. **用指针遍历链表每个元素**
>    1. **如果发现元素值比`x`大，就把节点拼接到`tail`后面，并后移`tail`，再把`tail.next`置空**
>    2. **否则移动指针**

```csharp
public class Solution {
    public ListNode Partition(ListNode head, int x) {
        if(head == null || head.next == null) {
            return head;
        }

        ListNode tail = head; // 找到尾部
        int len = 1; // 记录链表长度
        while(tail.next != null) {
            tail = tail.next;
            len ++;
        }

        ListNode dummy = new ListNode(-1, head);
        ListNode p = dummy;
        // 遍历链表每个元素
        for(int i=0; i<len; i++) {
            if(p.next != null && p.next.val >= x) {
                // 连接到尾巴
                tail.next = p.next;
                // 跳过该节点
                p.next = p.next.next;
                
                // 断开tail后面的节点连接
                tail = tail.next; tail.next = null;
            } else {
                if (p.next != null) {
                    p = p.next;
                }
            }
        }

        return dummy.next;
    }
}
```

> **思路二：双链表拼接法，创建两个伪头结点 `sDummy` 和 `bDummy`，分别用于构建小于 `x` 和大于等于 `x` 的链表。遍历原链表时，根据节点值将其连接到对应的链表中，最后将两个链表拼接起来并返回结果。**

```csharp
public class Solution {
    public ListNode Partition(ListNode head, int x) {
        if(head == null || head.next == null) {
            return head;
        }

        ListNode sDummy = new ListNode(-1); // 值比x小的节点构成的链表的伪头结点
        ListNode bDummy = new ListNode(-1); // 值比x大的节点构成的链表的伪头结点

        ListNode pointS = sDummy; ListNode pointB = bDummy;
        while(head != null) {
            if(head.val < x) {
                pointS.next = head;
                pointS = pointS.next;
            } else {
                pointB.next = head;
                pointB = pointB.next;
            }

            head = head.next;
        }

        // 拼接两个链表
        pointS.next = bDummy.next;
        pointB.next = null;

        return sDummy.next;
    }
}
```

## [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

请你设计并实现一个满足LRU (最近最少使用) 缓存约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

**示例：**

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

> **思路：有哨兵节点的双向循环链表**
>
> 1. **双向循环链表：用于维护缓存中数据的访问顺序，最近访问的节点放在链表头部，最久未访问的节点放在链表尾部。**
> 2. **哨兵节点：简化链表操作，避免处理头尾节点的特殊情况。**
> 3. **哈希表：通过`keyToNode`字典快速查找节点，确保`Get`和`Put`操作的时间复杂度为O(1)。**
> 4. **容量控制：当缓存超过容量时，移除链表尾部的节点（最久未使用的节点）。**

```csharp
public class LRUCache {
    // 双向循环链表
    public class Node {
        public int key;
        public int value;
        public Node prev;
        public Node next;

        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private int capacity; // 容量
    private Node dummy = new Node(0, 0); // 哨兵节点
    private Dictionary<int, Node> keyToNode = new Dictionary<int, Node>(); // key到节点的映射

    public LRUCache(int capacity) {
        this.capacity = capacity;
        dummy.prev = dummy;
        dummy.next = dummy;
    }
    
    public int Get(int key) {
        Node node = GetNode(key);
        return node == null ? -1 : node.value;
    }
    
    public void Put(int key, int value) {
        Node node = GetNode(key);

        // 如果节点已经存在，更新值
        if(node != null) {
            node.value = value;
            return;
        }

        // 否则尝试插入一个新的节点
        node = new Node(key, value);
        keyToNode.Add(key, node);
        PushFront(node);

        // 检查添加后是否超过容量
        if(keyToNode.Count > capacity) {
            // 删除最后一个节点
            Node lastNode = dummy.prev;
            keyToNode.Remove(lastNode.key);
            Remove(lastNode);
        }
    }

    // 取出一个节并放到最上面
    private Node GetNode(int key) {
        if(!keyToNode.ContainsKey(key)) {
            return null;
        }

        Node node = keyToNode[key];
        Remove(node); // 取出节点
        PushFront(node); // 放到最上面

        return node;
    }

    // 从双向链表中移除一个节点
    private void Remove(Node x)
    {
        x.prev.next = x.next;
        x.next.prev = x.prev;
    }

    // 在链表头添加一个节点
    private void PushFront(Node x)
    {
        x.prev = dummy;
        x.next = dummy.next;
        x.prev.next = x;
        x.next.prev = x;
    }
}
```

# 【二叉树】

## [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

**示例 1：**

```
输入：root = [3,9,20,null,null,15,7]
输出：3
```

**示例 2：**

```
输入：root = [1,null,2]
输出：2
```

> **思路一：递归式前序遍历实现DFS**

```csharp
public class Solution {
    public int MaxDepth(TreeNode root) {
        // 退出条件
        if(root == null) {
            return 0;
        }
        // 取出左右两子树的最大深度 + 1
        return Math.Max(MaxDepth(root.left), MaxDepth(root.right)) + 1;
    }
}
```

> **思路二：迭代式层序遍历实现BFS**

```csharp
public class Solution {
    public int MaxDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }

        int layer = 0;
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root); // 根节点入队

        while(queue.Count > 0) {
            int layerSize = queue.Count; // 记录当前层的节点数量
            for(int i=0; i<layerSize; i++) {
                TreeNode curNode = queue.Dequeue(); // 取出节点
                // 左右节点入队
                if(curNode.left != null) {
                    queue.Enqueue(curNode.left);
                }
                if(curNode.right != null) {
                    queue.Enqueue(curNode.right);
                }
            }
            
            layer ++; // 层数增加
        }

        return layer;
    }
}
```

## [100. 相同的树](https://leetcode.cn/problems/same-tree/)

给你两棵二叉树的根节点 `p` 和 `q` ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

**示例 1：**

```
输入：p = [1,2,3], q = [1,2,3]
输出：true
```

**示例 2：**

```
输入：p = [1,2], q = [1,null,2]
输出：false
```

**示例 3：**

```
输入：p = [1,2,1], q = [1,1,2]
输出：false
```

> **思路：递归式DFS**

```csharp
public class Solution {
    public bool IsSameTree(TreeNode p, TreeNode q) {
        /** 递归式DFS */
        // 退出条件
        if(p == null && q == null) {
            // 都为空
            return true;
        } else if (p == null || q == null){
            // 一个为空一个不为空
            return false;
        } else if(p.val != q.val) {
            // 两个都不为空且值不相等
            return false;
        }

        // 递归比较左右子树是否相同
        return IsSameTree(p.left, q.left) && IsSameTree(p.right, q.right);
    }
}
```

## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

**示例 1：**

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

**示例 2：**

```
输入：root = [2,1,3]
输出：[2,3,1]
```

**示例 3：**

```
输入：root = []
输出：[]
```

> **思路一：递归式DFS**

```csharp
public class Solution {
    public TreeNode InvertTree(TreeNode root) {
        // 退出条件
        if (root == null) {
            return null;
        }

        // 交换左右子树
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        // 递归调用
        InvertTree(root.left);
        InvertTree(root.right);

        return root;
    }
}
```

> **思路二：迭代式BFS**

```csharp
public class Solution {
    public TreeNode InvertTree(TreeNode root) {
        if(root == null) {
            return null;
        }
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        while(queue.Count > 0) {
            TreeNode node = queue.Dequeue();
            // 交换取出节点的左右子节点
            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;
            
            // 子节点入队
            if(node.left != null) {
                queue.Enqueue(node.left);
            }
            if(node.right != null) {
                queue.Enqueue(node.right);
            }
        }

        return root;
    }
}
```

## [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

**示例 1：**

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

> **思路：辅助函数+递归式DFS**

```csharp
public class Solution {
    public bool IsSymmetric(TreeNode root) {
        if(root == null) {
            return true;
        }

        return IsTwoSymmetric(root.left, root.right);
    }

    // 辅助函数：判断两颗树是否对称
    private bool IsTwoSymmetric(TreeNode root1, TreeNode root2) {
        if(root1 == null && root2 == null) {
            // 都为空
            return true;
        } else if(root1 == null || root2 == null) {
            // 有一个为空
            return false;
        } else if(root1.val != root2.val) {
            // 都不为空但值不相等
            return false;
        }

        // 两个根都不为空且值相等，递归调用
        return IsTwoSymmetric(root1.left, root2.right) && IsTwoSymmetric(root1.right, root2.left);
    }
}
```

## [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

给定两个整数数组 `preorder` 和 `inorder` ，其中 `preorder` 是二叉树的**先序遍历**， `inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

**示例 1:**

```
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

**示例 2:**

```
输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

> **思路：寻根分左右+递归构建**
>
> 1. **前序遍历的第一个节点是当前子树的根节点。**
> 2. **在中序遍历中找到根节点的位置，根节点左边是左子树，右边是右子树。**
> 3. **递归构建左子树和右子树：左子树的前序遍历范围是 `preStart + 1` 到 `preStart + leftSize`，中序遍历范围是 `inStart` 到 `inRootIndex - 1`；右子树的前序遍历范围是 `preStart + leftSize + 1` 到 `preEnd`，中序遍历范围是 `inRootIndex + 1` 到 `inEnd`。**

```csharp
public class Solution {
    public TreeNode BuildTree(int[] preorder, int[] inorder) {
        // 使用一个哈希表来存储inorder中值到索引的映射
        Dictionary<int, int> inorderMap = new Dictionary<int, int>();
        for(int i=0; i<inorder.Length; i++) {
            inorderMap.Add(inorder[i], i);
        }

        return BuildTreeHelper(preorder, 0, preorder.Length-1, inorder, 0, inorder.Length - 1, inorderMap);
    }

    // 辅助函数
    private TreeNode BuildTreeHelper(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd, 
        Dictionary<int, int> inorderMap) {
        // 递归退出条件
        if(inStart > inEnd || preStart > preEnd) {
            return null;
        }

        // 获取根节点的值，构建根节点
        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);

        // 在中序遍历中找到根节点的位置
        int inRootIndex = inorderMap[rootVal];

        // 左子树的节点数量
        int leftSize = inRootIndex - inStart;

        // 递归构建左子树
        root.left = BuildTreeHelper(preorder, preStart + 1, preStart + leftSize, inorder, inStart, inRootIndex - 1, inorderMap);
        // 递归构建右子树
        root.right = BuildTreeHelper(preorder, preStart + leftSize + 1, preEnd, inorder, inRootIndex + 1, inEnd, inorderMap);

        return root;
    }
}
```

## [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

给定两个整数数组 `inorder` 和 `postorder` ，其中 `inorder` 是二叉树的中序遍历， `postorder` 是同一棵树的后序遍历，请你构造并返回这颗 *二叉树* 。

**示例 1:**

```
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
```

**示例 2:**

```
输入：inorder = [-1], postorder = [-1]
输出：[-1]
```

> **思路：寻根分左右+递归构建**
>
> 1. **后序遍历的最后一个节点是当前子树的根节点。**
> 2. **在中序遍历中找到根节点的位置，根节点左边是左子树，右边是右子树。**
> 3. **递归构建左子树和右子树：左子树的后序遍历范围是 `postStart` 到 `postStart + leftSize - 1`，中序遍历范围是 `inStart` 到 `inRootIndex - 1`；右子树的后序遍历范围是 `postStart + leftSize` 到 `postEnd - 1`，中序遍历范围是 `inRootIndex + 1` 到 `inEnd`。**

```csharp
public class Solution {
    public TreeNode BuildTree(int[] inorder, int[] postorder) {
        Dictionary<int, int> inorderMap = new Dictionary<int, int>();
        for(int i=0; i<inorder.Length; i++) {
            inorderMap.Add(inorder[i], i);
        }

        return BuildTreeHelper(inorder, 0, inorder.Length - 1, postorder, 0, postorder.Length - 1, inorderMap);
    }

    // 辅助函数
    private TreeNode BuildTreeHelper(int[] inorder, int inStart, int inEnd, int[] postorder, int postStart, int postEnd, Dictionary<int, int> inorderMap) {
        if(inStart > inEnd || postStart > postEnd) {
            return null;
        }

        // 后序遍历的最后一个元素是根节点
        int rootVal = postorder[postEnd];
        // 构建根节点
        TreeNode root = new TreeNode(rootVal);

        // 得到根节点在中序遍历数组中的索引
        int inRootIndex = inorderMap[rootVal];

        // 计算左子树节点个数
        int leftSize = inRootIndex - inStart;

        // 递归调用构建左子树
        root.left = BuildTreeHelper(inorder, inStart, inRootIndex - 1, postorder, postStart, postStart + leftSize - 1, inorderMap);
        // 递归调用构建右子树
        root.right = BuildTreeHelper(inorder, inRootIndex + 1, inEnd, postorder, postStart + leftSize, postEnd - 1, inorderMap);

        return root;
    }
}
```

## [117. 填充每个节点的下一个右侧节点指针 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

给定一个二叉树：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL` 。

初始状态下，所有 next 指针都被设置为 `NULL` 。

**示例 1：**

```
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化输出按层序遍历顺序（由 next 指针连接），'#' 表示每层的末尾。
```

**示例 2：**

```
输入：root = []
输出：[]
```

> **思路一：层序遍历**
>
> 1. **BFS层序遍历：使用队列进行广度优先搜索。**
> 2. **记录每层节点数：在每一层开始时，记录当前层的节点数量，确保只处理当前层的节点。**
> 3. **连接next指针：遍历当前层的节点时，将每个节点的`next`指针指向队列中的下一个节点（即当前层的下一个节点），最后一个节点的`next`指针保持为`null`。**
> 4. **子节点入队：将当前节点的左右子节点加入队列，以便继续遍历下一层。**
> 5. **返回根节点：遍历完成后，返回修改后的根节点。**

```csharp
public class Solution {
    public Node Connect(Node root) {
        /** BFS层序遍历 */
        if(root == null) {
            return null;
        }

        Queue<Node> queue = new Queue<Node>();
        queue.Enqueue(root);
        int layerCount = 0; // 当前层数的节点个数

        while(queue.Count > 0) {
            layerCount = queue.Count; // 记录当前层数的节点个数
            for(int i=0; i<layerCount; i++) {
                // 出队
                Node curNode = queue.Dequeue(); 
                // next指针连接下一个元素（当前层最后一个元素不操作）
                if(i < layerCount - 1) curNode.next = queue.Peek();
                // 左右子节点入队
                if(curNode.left != null) {
                    queue.Enqueue(curNode.left);
                }
                if(curNode.right != null) {
                    queue.Enqueue(curNode.right);
                }
            }    
        }

        return root;
    }
}
```

> **思路二：两个while循环，利用当前层的 `next` 指针来连接下一层**
>
> 1. **逐层遍历：从根节点开始，利用当前层的 `next` 指针从左到右遍历每一层。**
> 2. **连接下一层：在遍历当前层时，将下一层的节点通过 `next` 指针连接起来。**
> 3. **动态找下一层起点：在遍历过程中，动态找到下一层的最左节点，作为下一层遍历的起点。**

```csharp
public class Solution {
    public Node Connect(Node root) {
        /** 利用已建立的next指针，遍历当前层并连接下一层 */
        if(root == null) {
            return null;
        }

        Node leftmost = root; // 当前层的最左节点
        while(leftmost != null) {
            Node curr = leftmost; // 当前层的遍历指针
            Node prev = null; // 下一层遍历的前一个节点
            leftmost = null; // 重置 leftmost
            
            // 遍历当前层，同时连接下一层
            while(curr != null) {
                if(curr.left != null) {
                    if(prev != null) {
                        prev.next = curr.left; // 连接左子节点
                    } else {
                        leftmost = curr.left; // 移动到下一层的最左节点（位于左子树）
                    }
                    prev = curr.left; // 移动下一层的遍历指针
                }
                if(curr.right != null) {
                    if(prev != null) {
                        prev.next = curr.right; // 连接右子节点
                    } else {
                        leftmost = curr.right; // 找到下一层的最左节点
                    }
                    prev = curr.right; // 移动下一层的遍历指针
                }
                curr = curr.next; // 移动到当前层的下一个节点
            }
        }

        return root;
    }
}
```

## [114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

给你二叉树的根结点 `root` ，请你将它展开为一个单链表：

- 展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。
- 展开后的单链表应该与二叉树 [**先序遍历**](https://baike.baidu.com/item/先序遍历/6442839?fr=aladdin) 顺序相同。

**示例 1：**

```
输入：root = [1,2,5,3,4,null,6]
输出：[1,null,2,null,3,null,4,null,5,null,6]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [0]
输出：[0]
```

> **思路一：迭代法，每次从栈中弹出一个节点，将其右子节点和左子节点依次入栈，然后将上一个访问的节点的右指针指向当前节点，同时清空左指针，最终将二叉树展开为单向链表。**

```csharp
public class Solution {
    public void Flatten(TreeNode root) {
        if(root == null) {
            return;
        }
        
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.Push(root);
        TreeNode prevNode = null; // 上一个访问的节点

        while(stack.Count > 0) {
            TreeNode node = stack.Pop();
            if(prevNode != null) {
                prevNode.right = node; prevNode.left = null;
            }
            prevNode = node;

            // 右子节点入栈
            if(node.right != null) {
                stack.Push(node.right);
            }

            // 左子节点入栈
            if(node.left != null) {
                stack.Push(node.left);
            }
        }
    }
}
```

> **思路二：Morris遍历**
>
> 1. **找到左子树的最右节点：对于当前节点 `cur`，如果它有左子树，则找到左子树的最右节点（即前驱节点）。**
> 2. **将右子树接到前驱节点：将当前节点的右子树接到前驱节点的右指针上。**
> 3. **移动子树并清空左指针：将左子树移到右子树的位置，同时清空左指针。**
> 4. **移动到下一个节点：继续处理右子节点，直到所有节点处理完毕。**

```csharp
public class Solution {
    public void Flatten(TreeNode root) {
        TreeNode cur = root;
        while(cur != null) {
            if(cur.left != null) {
                // leftTree是左子树
                TreeNode leftTree = cur.left;
                // prev是左子树中最右边的节点
                TreeNode prev = leftTree;
                while(prev.right != null) {
                    prev = prev.right;
                }
                // 左子树中最右边的节点的右节点连接到根的右子树
                prev.right = cur.right;
                cur.left = null; cur.right = leftTree;
            }
            cur = cur.right;
        }
    }
}
```

## [112. 路径总和](https://leetcode.cn/problems/path-sum/)

给你二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum` 。判断该树中是否存在 **根节点到叶子节点** 的路径，这条路径上所有节点值相加等于目标和 `targetSum` 。如果存在，返回 `true` ；否则，返回 `false` 。

**叶子节点** 是指没有子节点的节点。

**示例 1：**

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
解释：等于目标和的根节点到叶节点路径如上图所示。
```

**示例 2：**

```
输入：root = [1,2,3], targetSum = 5
输出：false
解释：树中存在两条根节点到叶子节点的路径：
(1 --> 2): 和为 3
(1 --> 3): 和为 4
不存在 sum = 5 的根节点到叶子节点的路径。
```

**示例 3：**

```
输入：root = [], targetSum = 0
输出：false
解释：由于树是空的，所以不存在根节点到叶子节点的路径。
```

> **思路一：递归深度优先搜索**
>
> 1. **递归退出条件：如果当前节点是叶子节点（无左右子节点），则判断节点值是否等于剩余的 `targetSum`。**
> 2. **递归遍历左右子树：从当前节点出发，递归检查左子树和右子树，同时更新 `targetSum` 为 `targetSum - 当前节点值`。**
> 3. **返回结果：如果左子树或右子树任一存在满足条件的路径，则返回 `true`，否则返回 `false`。**

```csharp
public class Solution {
    public bool HasPathSum(TreeNode root, int targetSum) {
        if(root == null) {
            return false;
        }
        /** 递归DFS */
        // 递归退出条件
        if(root.left == null && root.right == null) {
            return root.val == targetSum;
        }
        // 递归遍历左右子树
        int newSum = targetSum - root.val;
        return HasPathSum(root.left, newSum) || HasPathSum(root.right, newSum);
    }
}
```

> **思路二：广度优先搜索+双队列**
>
> 1. **使用两个队列：一个队列存储节点，用于层序遍历；另一个队列存储从根节点到当前节点的路径和。**
> 2. **层序遍历：从根节点开始，依次处理每个节点，将其左右子节点加入队列，并更新路径和。**
> 3. **判断叶子节点：如果当前节点是叶子节点，且路径和等于 `targetSum`，则返回 `true`。**
> 4. **遍历结束：如果遍历完所有节点仍未找到满足条件的路径，则返回 `false`。**

```csharp
public class Solution {
    public bool HasPathSum(TreeNode root, int targetSum) {
        if (root == null) {
            return false;
        }

        // 双队列：一个存储节点，一个存储路径和
        Queue<TreeNode> nodeQueue = new Queue<TreeNode>();
        Queue<int> sumQueue = new Queue<int>();

        // 初始化
        nodeQueue.Enqueue(root);
        sumQueue.Enqueue(root.val);

        while (nodeQueue.Count > 0) {
            TreeNode curNode = nodeQueue.Dequeue();
            int curSum = sumQueue.Dequeue();

            // 如果是叶子节点且路径和等于目标值，返回true
            if (curNode.left == null && curNode.right == null && curSum == targetSum) {
                return true;
            }

            // 处理左子节点
            if (curNode.left != null) {
                nodeQueue.Enqueue(curNode.left);
                sumQueue.Enqueue(curSum + curNode.left.val);
            }

            // 处理右子节点
            if (curNode.right != null) {
                nodeQueue.Enqueue(curNode.right);
                sumQueue.Enqueue(curSum + curNode.right.val);
            }
        }

        // 遍历结束未找到满足条件的路径
        return false;
    }
}
```

## [129. 求根节点到叶节点数字之和](https://leetcode.cn/problems/sum-root-to-leaf-numbers/)

给你一个二叉树的根节点 `root` ，树中每个节点都存放有一个 `0` 到 `9` 之间的数字。

每条从根节点到叶节点的路径都代表一个数字：

- 例如，从根节点到叶节点的路径 `1 -> 2 -> 3` 表示数字 `123` 。

计算从根节点到叶节点生成的 **所有数字之和** 。

**叶节点** 是指没有子节点的节点。

**示例 1：**

```
输入：root = [1,2,3]
输出：25
解释：
从根到叶子节点路径 1->2 代表数字 12
从根到叶子节点路径 1->3 代表数字 13
因此，数字总和 = 12 + 13 = 25
```

**示例 2：**

```
输入：root = [4,9,0,5,1]
输出：1026
解释：
从根到叶子节点路径 4->9->5 代表数字 495
从根到叶子节点路径 4->9->1 代表数字 491
从根到叶子节点路径 4->0 代表数字 40
因此，数字总和 = 495 + 491 + 40 = 1026
```

> **思路一：BFS + 双队列，一个存储当前节点，另一个存储从根节点到当前节点的路径和；每当遍历到一个叶子节点时，将该路径和累加到总和中。**

```csharp
public class Solution {
    public int SumNumbers(TreeNode root) {
        if(root == null) {
            return 0;
        }
        /** BFS+双队列 */
        Queue<TreeNode> nodeQueue = new Queue<TreeNode>(); // 节点队列
        Queue<int> sumQueue = new Queue<int>(); // 路径和队列
        int totalSum = 0; // 统计最终和

        nodeQueue.Enqueue(root); sumQueue.Enqueue(root.val);
        while(nodeQueue.Count > 0) {
            TreeNode curNode = nodeQueue.Dequeue();
            int curSum = sumQueue.Dequeue();

            if(curNode.left != null) {
                nodeQueue.Enqueue(curNode.left);
                sumQueue.Enqueue(curSum * 10 + curNode.left.val);
            }

            if(curNode.right != null) {
                nodeQueue.Enqueue(curNode.right);
                sumQueue.Enqueue(curSum * 10 + curNode.right.val);
            }

            // 叶子节点，需要把结果加入totalSum
            if(curNode.left == null && curNode.right == null) {
                totalSum += curSum;
            }
        }

        return totalSum;
    }
}
```

> **思路二：DFS+递归：递归函数中维护一个从根节点到当前节点的路径和。当遍历到叶子节点时，返回当前路径和；否则，递归计算左右子树的路径和并累加。**

```csharp
public class Solution {
    public int SumNumbers(TreeNode root) {
        return SumNumbersHelp(root, 0);
    }

    // 辅助函数，sum用于记录根到当前节点的和
    public int SumNumbersHelp(TreeNode root, int sum) {
        if(root == null) {
            return 0;
        }

        sum = sum * 10 + root.val; // 更新和

        // 叶子节点
        if(root.left == null && root.right == null) {
            return sum;
        }

        int leftSum = SumNumbersHelp(root.left, sum);
        int rightSum = SumNumbersHelp(root.right, sum);

        return leftSum + rightSum;
    }
}
```

## [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

二叉树中的 **路径** 被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中 **至多出现一次** 。该路径 **至少包含一个** 节点，且不一定经过根节点。

**路径和** 是路径中各节点值的总和。

给你一个二叉树的根节点 `root` ，返回其 **最大路径和** 。

**示例 1：**

```
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

**示例 2：**

```
输入：root = [-10,9,20,null,null,15,7]
输出：42
解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
```

> **思路：辅助函数递归**
>
> 1. **递归计算贡献值：**
>    **对于每个节点，递归计算其左右子节点的最大贡献值（如果贡献值为负，则取0，表示不选择该子树）。**
> 2. **更新最大路径和：**
>    **当前节点的路径和包括当前节点值加上左右子节点的贡献值，用此值更新全局最大值 `maxSum`。**
> 3. **返回单侧最大贡献值：**
>    **当前节点向上返回时，只能选择左或右子节点的贡献值中的较大者，加上当前节点值，作为当前节点的最大贡献值。**
> 4. **全局变量记录结果：**
>    **使用全局变量 `maxSum` 记录遍历过程中出现的最大路径和，最终返回该值。**

```csharp
public class Solution {
    int maxSum = int.MinValue;
    public int MaxPathSum(TreeNode root) {
        MaxGain(root);
        return maxSum;
    }

    // 辅助函数，更新节点的最大贡献值并返回单侧最大贡献值
    private int MaxGain(TreeNode node) {
        if(node == null) {
            return 0;
        }

        // 递归计算左右子节点的最大贡献值（取非负）
        int leftGain = Math.Max(MaxGain(node.left), 0);
        int rightGain = Math.Max(MaxGain(node.right), 0);

        int curGain = node.val + leftGain + rightGain;

        // 更新maxSum
        maxSum = Math.Max(maxSum, curGain);

        // 返回节点的最大贡献值，左右贡献值只能取其一
        return node.val + Math.Max(leftGain, rightGain);
    }
}
```

## [173. 二叉搜索树迭代器](https://leetcode.cn/problems/binary-search-tree-iterator/)

实现一个二叉搜索树迭代器类`BSTIterator` ，表示一个按中序遍历二叉搜索树（BST）的迭代器：

- `BSTIterator(TreeNode root)` 初始化 `BSTIterator` 类的一个对象。BST 的根节点 `root` 会作为构造函数的一部分给出。指针应初始化为一个不存在于 BST 中的数字，且该数字小于 BST 中的任何元素。
- `boolean hasNext()` 如果向指针右侧遍历存在数字，则返回 `true` ；否则返回 `false` 。
- `int next()`将指针向右移动，然后返回指针处的数字。

注意，指针初始化为一个不存在于 BST 中的数字，所以对 `next()` 的首次调用将返回 BST 中的最小元素。

你可以假设 `next()` 调用总是有效的，也就是说，当调用 `next()` 时，BST 的中序遍历中至少存在一个下一个数字。

**示例：**

```
输入
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
输出
[null, 3, 7, true, 9, true, 15, true, 20, false]

解释
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // 返回 3
bSTIterator.next();    // 返回 7
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 9
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 15
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 20
bSTIterator.hasNext(); // 返回 False
```

> **思路：等价于对二叉树进行中序遍历。**

```csharp
public class BSTIterator {
    private int index;
    private List<int> arr;

    public BSTIterator(TreeNode root) {
        index = 0;
        arr = new List<int>();
        InorderTraversal(root, arr);
    }
    
    public int Next() {
        return arr[index++];
    }
    
    public bool HasNext() {
        return index < arr.Count;
    }

    private void InorderTraversal(TreeNode root, List<int> arr) {
        if(root == null) {
            return;
        }

        InorderTraversal(root.left, arr);
        arr.Add(root.val);
        InorderTraversal(root.right, arr);
    }
}
```

## [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

给你一棵 **完全二叉树** 的根节点 `root` ，求出该树的节点个数。

完全二叉树的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 `h` 层（从第 0 层开始），则该层包含 `1~ 2h` 个节点。

**示例 1：**

```
输入：root = [1,2,3,4,5,6]
输出：6
```

**示例 2：**

```
输入：root = []
输出：0
```

**示例 3：**

```
输入：root = [1]
输出：1
```

> **思路一：常规递归式深度搜索，复杂度为O(N)。**

```cs
public class Solution {
    public int CountNodes(TreeNode root) {
        if(root == null) {
            return 0;
        }

        return CountNodes(root.left) + CountNodes(root.right) + 1;
    }
}
```

> **思路二：利用完全二叉树的性质——左右子树必然有一棵是满二叉树，知道高度就能知道个数，剩下一棵没满的递归调用计算个数，复杂度为O(logN * logN)。**

```csharp
public class Solution {
    public int CountNodes(TreeNode root) {
        if(root == null) {
            return 0;
        }

        int leftDepth = GetDepth(root.left);
        int rightDepth = GetDepth(root.right);

        if(leftDepth == rightDepth) {
            // 左子树满，最后一层的最后一个节点位于右子树
            return 1 + (1 << leftDepth) - 1 + CountNodes(root.right);
        } else {
            // 右子树满，最后一层的最后一个节点位于左子树
            return 1 + (1 << rightDepth) - 1 + CountNodes(root.left);
        }
    }

    // 辅助函数，得到某完全二叉树的高度
    private int GetDepth(TreeNode node) {
        int depth = 0;
        while(node != null) {
            depth ++;
            node = node.left;
        }
        return depth;
    }
}
```

## [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

**示例 1：**

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

**示例 2：**

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

**示例 3：**

```
输入：root = [1,2], p = 1, q = 2
输出：1
```

> **思路一：记录父节点和深度再回溯。**
>
> 1. **使用层序遍历（BFS）来遍历整个二叉树，在遍历的过程中记录每个节点的父节点和深度。**
> 2. **通过回溯的方式来找到 `p` 和 `q` 的最近公共祖先：**
>    - **比较 `p` 和 `q` 的深度，如果 `p` 的深度大于 `q` 的深度，则将 `p` 向上移动到它的父节点，反之则将 `q` 向上移动。**
>    - **重复这个过程，直到 `p` 和 `q` 指向同一个节点，就是它们的最近公共祖先。**

```cs
public class Solution
{
    public TreeNode LowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q)
    {
        if (root == null)
        {
            return null;
        }

        // 字典存储每个节点的父节点
        Dictionary<TreeNode, TreeNode> parentDict = new Dictionary<TreeNode, TreeNode>();
        // 字典存储每个节点的深度
        Dictionary<TreeNode, int> depthDict = new Dictionary<TreeNode, int>();

        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        parentDict[root] = null; // 根节点的父节点为 null
        depthDict[root] = 0; // 根节点的深度为 0

        // 层序遍历，记录每个节点的父节点和深度
        while (queue.Count > 0)
        {
            TreeNode node = queue.Dequeue();
            int currentDepth = depthDict[node];

            if (node.left != null) {
                queue.Enqueue(node.left);
                parentDict[node.left] = node; // 记录左子节点的父节点
                depthDict[node.left] = currentDepth + 1; // 记录左子节点的深度
            }
            if (node.right != null) {
                queue.Enqueue(node.right);
                parentDict[node.right] = node; // 记录右子节点的父节点
                depthDict[node.right] = currentDepth + 1; // 记录右子节点的深度
            }
        }

        // 回溯找到最近公共祖先
        while (p != q)
        {
            if (depthDict[p] > depthDict[q])
            {
                p = parentDict[p]; // 向上移动较深的节点
            } else {
                q = parentDict[q]; // 向上移动较深的节点
            }
        }

        return p; // 返回最近公共祖先
    }
}
```

> **思路二：递归。**
>
> 1. **终止条件：**
>    - **如果 `root` 是 `null`，返回 `null`。**
>    - **如果 `root` 是 `p` 或 `q`，直接返回 `root`，因为当前节点就是 `p` 或 `q`。**
> 2. **递归查找左右子树：**
>    - **在左子树中递归查找 `p` 和 `q`，结果存储在 `left` 中。**
>    - **在右子树中递归查找 `p` 和 `q`，结果存储在 `right` 中。**
> 3. **判断最近公共祖先：**
>    - **如果 `left` 和 `right` 都不为空，说明 `p` 和 `q` 分别位于当前节点的左右子树中，当前节点就是它们的最近公共祖先。**
>    - **如果 `left` 为空，说明 `p` 和 `q` 都在右子树中，返回 `right`。**
>    - **如果 `right` 为空，说明 `p` 和 `q` 都在左子树中，返回 `left`。**

```csharp
public class Solution
{
    public TreeNode LowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q)
    {
        if(root == null) {
            return null;
        }

        /** 递归深度搜索 */
        // 递归退出条件：root是p或q => 说明p或q至少有一个在root中
        if(root == p || root == q) {
            return root;
        }

        // 递归查找左子树中是否存在公共祖先
        TreeNode left = LowestCommonAncestor(root.left, p, q);
        // 递归查找右子树中是否存在公共祖先
        TreeNode right = LowestCommonAncestor(root.right, p, q);

        if(left != null && right != null) {
            // p和q分别位于左右子树中
            return root;
        }

        if(left == null) {
            // 如果left为空，说明p和q都在右子树中
            return right;
        } else {
            // 如果right为空，说明p和q都在左子树中
            return left;
        }
    }
}
```

# 【二叉树层次遍历】

## [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

**示例 1：**

```
输入：root = [1,2,3,null,5,null,4]

输出：[1,3,4]
```

**示例 2：**

```
输入：root = [1,2,3,4,null,null,null,5]

输出：[1,3,4,5]
```

**示例 3：**

```
输入：root = [1,null,3]

输出：[1,3]
```

**示例 4：**

```
输入：root = []

输出：[]
```

> **思路一：层序遍历，用`queue.Count`获取每一层的节点个数就可知每一层的最后一个节点，把最后一个节点的值加入结果。**

```csharp
public class Solution {
    public IList<int> RightSideView(TreeNode root) {
        IList<int> res = new List<int>();
        if(root == null) {
            return res;
        }
        /** 层序遍历 */
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        while(queue.Count > 0) {
            int layerSize = queue.Count; // 每一层的节点个数
            int lastNodeVal = 0; // 每一层最后一个节点的值
            for(int i=0; i<layerSize; i++) {
                TreeNode node = queue.Dequeue();
                lastNodeVal = node.val;
                // 左右节点入队
                if(node.left != null) {
                    queue.Enqueue(node.left);
                }
                if(node.right != null) {
                    queue.Enqueue(node.right);
                }
            }
            // 把最后一个节点的值加入结果
            res.Add(lastNodeVal);
        }

        return res;
    }
}
```

> **思路二：维护一个外部变量`maxDepth`，用于记录当前访问到的最大层数。递归访问节点，传入`depth`表示当前的层数，每次递归访问子节点深度为`depth+1`；通过优先访问右子树的方式，确保每一层的最右侧节点先被访问到；如果当前深度 `depth` 大于 `maxDepth`，说明访问到新的一层，把访问到的第一个节点（也即每一层最右边的节点）加入结果。**

```cs
public class Solution {
    private IList<int> res = new List<int>();
    private int maxDepth = -1; // 记录遍历到的最大深度

    public IList<int> RightSideView(TreeNode root) {
        if(root == null) {
            return res;
        }

        Traverse(root, 0);
        return res;
    }

    // 辅助函数：从右子节点开始递归遍历
    private void Traverse(TreeNode node, int depth) {
        if(node == null) {
            return;
        }

        if(depth > maxDepth) {
            res.Add(node.val); // 新的一层最右侧的节点
            maxDepth = depth; // 更新maxDepth
        }

        // 优先访问右子树
        Traverse(node.right, depth + 1);
        Traverse(node.left, depth + 1);
    }
}
```

## [637. 二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

给定一个非空二叉树的根节点 `root` , 以数组的形式返回每一层节点的平均值。与实际答案相差 `10-5` 以内的答案可以被接受。

**示例 1：**

```
输入：root = [3,9,20,null,null,15,7]
输出：[3.00000,14.50000,11.00000]
解释：第 0 层的平均值为 3,第 1 层的平均值为 14.5,第 2 层的平均值为 11 。
因此返回 [3, 14.5, 11] 。
```

**示例 2:**

```
输入：root = [3,9,20,15,7]
输出：[3.00000,14.50000,11.00000]
```

> **思路：层序遍历。**

```cs
public class Solution {
    public IList<double> AverageOfLevels(TreeNode root) {
        IList<double> res = new List<double>();
        if(root == null) {
            return res;
        }
        /** 层序遍历 */
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        while(queue.Count > 0) {
            int layerSize = queue.Count; // 每一层的节点个数
            long sum = 0; // 每一层之和
            for(int i=0; i< layerSize; i++) {
                TreeNode node = queue.Dequeue();
                sum += node.val; // 出队并累计和
                if(node.left != null) {
                    queue.Enqueue(node.left);
                }
                if(node.right != null) {
                    queue.Enqueue(node.right);
                }
            }

            res.Add((double)sum/layerSize);
        }

        return res;
    }
}
```

## [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

**示例 1：**

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

> **思路：层序遍历。**

```cs
public class Solution {
    public IList<IList<int>> LevelOrder(TreeNode root) {
        IList<IList<int>> res = new List<IList<int>>();
        if(root == null) {
            return res;
        }

        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);
        while(queue.Count > 0) {
            int layerSize = queue.Count; // 每一层的节点个数
            List<int> layerNodes = new List<int>();
            for(int i=0; i<layerSize; i++) {
                TreeNode node = queue.Dequeue();
                layerNodes.Add(node.val);
                if(node.left != null) {
                    queue.Enqueue(node.left);
                }
                if(node.right != null) {
                    queue.Enqueue(node.right);
                }
            }
            res.Add(layerNodes);
        }

        return res;
    }
}
```

## [103. 二叉树的锯齿形层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/)

给你二叉树的根节点 `root` ，返回其节点值的 **锯齿形层序遍历** 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

**示例 1：**

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[20,9],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

> **思路一：层序遍历，用`isLeft`变量标记每层是否是从左到右，如果不是，遍历完一层后再反转列表。**

```cs
public class Solution {
    public IList<IList<int>> ZigzagLevelOrder(TreeNode root) {
        IList<IList<int>> res = new List<IList<int>>();
        if(root == null) {
            return res;
        }
        bool isLeft = true; // 是否是从左到右加入列表
        Queue<TreeNode> queue = new Queue<TreeNode>();
        queue.Enqueue(root);

        while(queue.Count > 0) {
            int layerSize = queue.Count; // 每一层的节点个数
            List<int> layerNodes = new List<int>();
            for(int i=0; i<layerSize; i++) {
                TreeNode node = queue.Dequeue();
                layerNodes.Add(node.val);
                if(node.left != null) {
                    queue.Enqueue(node.left);
                }
                if(node.right != null) {
                    queue.Enqueue(node.right);
                }
            }
            if(!isLeft) {
                // 如果是从右到左，需要反转List
                layerNodes.Reverse();
            }
            res.Add(layerNodes);
            isLeft = !isLeft;
        }

        return res;
    }
}
```

> **思路二：递归。先序遍历中，每一层节点的访问顺序一定是从左到右的，可用`level`记录当前层数，`res[level]`就是当前层节点构成的列表。根据是当前层奇数层还是偶数层，决定从后面添加节点，还是从前面插入节点。**

```csharp
public class Solution {
    public IList<IList<int>> ZigzagLevelOrder(TreeNode root) {
        IList<IList<int>> res = new List<IList<int>>();
        if (root == null) {
            return res;
        }

        DFS(root, 0, res);
        return res;
    }

    private void DFS(TreeNode node, int level, IList<IList<int>> res) {
        if (node == null) {
            return;
        }

        // 如果当前层还没有列表，添加一个空列表
        if (res.Count <= level) {
            res.Add(new List<int>());
        }

        // 根据层数决定添加顺序
        if (level % 2 == 0) {
            res[level].Add(node.val); // 从左到右
        } else {
            res[level].Insert(0, node.val); // 从右到左
        }

        // 递归遍历左右子树
        DFS(node.left, level + 1, res);
        DFS(node.right, level + 1, res);
    }
}
```

