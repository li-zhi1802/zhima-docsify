---
title: 【1-Easy】两数之和
date: 2022-02-21 18:52:45
tags: 算法
categories:
- 算法
- 刷题篇
---

## 两数之和

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

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

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**
- **进阶：**你可以想出一个时间复杂度小于 `O(n2)` 的算法吗？

**Related Topics**

* 数组
* 哈希表

### 暴力枚举法

题目意思很明确，就是给一个`target`和一个数组

目标是在这个数组里面找到 **和是** `target` 的那两个数的索引数组

暴力枚举很简单，代码直接给出

> 示例代码

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < nums.length; j++) {
                // 如果找到了，那就保存结果并返回
                if (j != i && nums[j] == target - nums[i]) {
                    return new int[]{i, j};
                }
            }
        }
        // 没有符合要求的两个数
        return new int[]{-1, -1};
    }
}
```

### 哈希表

暴力枚举取到`target-nums[i]`的方法是遍历算法

关键在于如何快速知道`i`后面有没有等于`target-nums[i]`的值

那么就可以使用`Map`结构来解决这个问题

> 示例代码

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // 键为nums[i]
        // 值为索引
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            // 找到了target-nums[i]
            if (map.containsKey(target - nums[i])) {
                return new int[]{map.get(target - nums[i]), i};
            }
            map.put(nums[i], i);
        }
        // 说明没找到
        return new int[]{-1, -1};
    }
}
```

#### Tips

相信很多同学看到这种方法，第一反应可能不是这样写的

你的代码可能是这样的，差异就在`//==========================`之间的代码

先遍历一遍数组，目的是将值和其索引先放入`map`

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // 键为nums[i]
        // 值为索引
        // ==========================
        // 这里手动设置容器初始大小
        // 这样指定初始容量的原因是此map肯定不会放超过nums.length数目的元素，所以就设置默认容量为其正好要扩容的数量+1，就不会扩容了
        // 这里不懂得话，就不写，走默认的容量然后它自己扩容也是可以的
        HashMap<Integer, Integer> map = new HashMap<>((int)((float) nums.length / 0.75F + 1.0F));
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        // ==========================
        for (int i = 0; i < nums.length; i++) {
            // 找到了target-nums[i]
            if (map.containsKey(target - nums[i])) {
                return new int[]{map.get(target - nums[i]), i};
            }
            map.put(nums[i], i);
        }
        // 说明没找到
        return new int[]{-1, -1};
    }
}
```

为什么不需要提前遍历一遍将值和索引的对应关系放入`map`中，而是可以边遍历边查`map`呢？

案例：`nums = [2, 7, 11, 15],target = 13`

* 边遍历边放入`map`

当条件成立的时候，`nums[i]`是11，`target - nums[i]`是2

2是在11之前遍历的，所以2和其索引的对应关系已经在`map`中了，于是得到了答案

这时候的`map`是不完整的，只有2、7的对应关系

* 预填充`map`

当条件成立的时候，`nums[i]`是2，`target - nums[i]`是11

在遍历2的时候，11这个值的对应关系已经在`map`中了，于是得到了答案

这时候的`map`是完整的

> 总结

经过上面案例的分析，应该可以感觉到两者之间的不同了，明显感受到边遍历边放`map`的方案优于预填充的方案

> 分析

当然不是每次涉及类似题目都这样的，该预遍历还是要预遍历的，下面分析一下为啥这题可以这样

下面假设答案存在，则`target = nums[i] + nums[j] ( i < j )`

预遍历是在当前位置`i`，向后找`target - nums[i]`的位置`j`

先找到那个小的索引，然后再找大的那个索引

边遍历边存是在当前位置`j`，向前找`target - nums[j]`的位置`i`

先找到那个大的索引，然后再找小的那个索引
