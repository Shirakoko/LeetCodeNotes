# LeetCode经典150题做题笔记

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
>
>   - **`next[j]` 表示子字符串 `needle` 的前 `j` 个字符中，最长相同前缀和后缀的长度。**
>
>   - **例如，子字符串 `"ababc"` 的 `next` 数组为 `[-1, 0, 0, 1, 2， 0]`。**
>   - **通过双指针 `i` 和 `j` 计算 `next` 数组：**
>     - **如果 `p[i] == p[j]`，则 `next[j+1] = i+1`。**
>     - **如果 `p[i] != p[j]`，则回退 `i` 到 `next[i]`，直到匹配或 `i = -1`。**
>
> - **匹配过程：**
>
>   - **主字符串和子字符串从左到右逐个字符匹配。**
>
>   - **如果字符匹配，两个指针一起右移。**
>
>   - **如果字符不匹配，子字符串的指针根据 `next` 数组回退到某个位置，主字符串的指针不需要回退。**

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

```c#
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

