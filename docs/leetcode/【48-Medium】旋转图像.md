## 旋转图像

给定一个 *n* × *n* 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在**[ 原地](https://baike.baidu.com/item/原地算法)** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。



**示例 1：**

![img](【48-Medium】旋转图像/mat1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

**示例 2：**

![img](【48-Medium】旋转图像/mat2.jpg)

```
输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```



**提示：**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

Related Topics

* 数组
* 数学
* 矩阵

### 解法

这种要求原地修改数组的题目一般都需要观察

目标是将所有数字顺时针旋转90°

可以先将矩阵镜面翻转，接着对每一行进行逆转

最后就得到了顺时针旋转90°的矩阵

```java
class Solution {
    public void rotate(int[][] matrix) {
        // 镜像翻转
        mirror(matrix);
        for (int[] row : matrix) {
            reverseRow(row);
        }
    }

    private void mirror(int[][] matrix) {
        int width = matrix.length;
        for (int i = 0; i < width; i++) {
            for (int j = i; j < width; j++) {
                // 将matrix[i][j]和matrix[j][i]交换
                swap(i, j, j, i, matrix);
            }
        }
    }


    /**
     * 逆转数组arr
     * 利用双指针技巧原地修改原数组
     *
     * @param arr
     */
    private void reverseRow(int[] arr) {
        // [left,right]
        int left = 0;
        int right = arr.length - 1;
        while (left <= right) {
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }

    /**
     * 交换matrix[x1][y1]和matrix[x2][y2]
     *
     * @param x1
     * @param y1
     * @param x2
     * @param y2
     * @param matrix
     */
    private void swap(int x1, int y1, int x2, int y2, int[][] matrix) {
        int temp = matrix[x1][y1];
        matrix[x1][y1] = matrix[x2][y2];
        matrix[x2][y2] = temp;
    }
}
```

