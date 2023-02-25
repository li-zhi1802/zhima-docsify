## 寻找两个正序数组的中位数

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

**提示：**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`

**Related Topics**

* 数组
* 二分查找
* 分治

### 合并取中间值

这种应该是大多数人的第一反应

使用一个list集合存放两个数组的值后排序

判断集合长度的奇偶性

偶数，返回中间两个数的和的平均值

奇数，返回中间那个数

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        List<Integer> list = new ArrayList<>();
        for (int k : nums1) {
            list.add(k);
        }
        for (int j : nums2) {
            list.add(j);
        }
        Collections.sort(list);
        int size = list.size();
        if (size % 2 == 0) {
            return (list.get(size / 2) + list.get(size / 2 - 1)) / 2.0;
        } else {
            return list.get(size / 2);
        }
    }
}
```

### 归并

再来理一理题目的意思

有两个正序数组`num1`和`nums2`

要返回这两个正序数组的中位数

这不就是归并中合并两个有序数组一样的思路吗

但是这里不需要完全合并，只需要合并到中间的位置就可了

说干就干

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        // 总长度
        int wholeSize = nums1.length + nums2.length;
        // 去中间值
        int mid = wholeSize / 2;
        // 新建中间值+1长度的数组
        int[] temp = new int[mid + 1];
        int index1 = 0;
        int index2 = 0;
        for (int i = 0; i < temp.length; i++) {
            if (index1 == nums1.length) {
	            // num1已经放完了，直接放num2的值即可
                temp[i] = nums2[index2++];
            } else if (index2 == nums2.length) {
    	        // num1已经放完了，直接放num2的值即可
                temp[i] = nums1[index1++];
            } else if (nums1[index1] <= nums2[index2]) {
	            // num1相对比较小，所以放num1的值
                temp[i] = nums1[index1++];
            } else {
	            // num2相对比较小，所以放num2的值
                temp[i] = nums2[index2++];
            }
        }
        // 如果总长度是偶数，则返回最后两个数的和的平均数
        // 如果总长度是奇数，则直接返回最后一个数即可
        return wholeSize % 2 == 0 ? (temp[mid] + temp[mid - 1]) / 2.0 : temp[mid];
    }
}
```
