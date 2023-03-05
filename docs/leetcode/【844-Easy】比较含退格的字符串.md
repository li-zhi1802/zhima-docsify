## 比较含退格的字符串

给定 `s` 和 `t` 两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 `true` 。`#` 代表退格字符。

**注意：**如果对空文本输入退格字符，文本继续为空。

**示例 1：**

```
输入：s = "ab#c", t = "ad#c"
输出：true
解释：s 和 t 都会变成 "ac"。
```

**示例 2：**

```
输入：s = "ab##", t = "c#d#"
输出：true
解释：s 和 t 都会变成 ""。
```

**示例 3：**

```
输入：s = "a#c", t = "b"
输出：false
解释：s 会变成 "c"，但 t 仍然是 "b"。
```

 

**提示：**

- `1 <= s.length, t.length <= 200`
- `s` 和 `t` 只含有小写字母以及字符 `'#'`

 

**进阶：**

- 你可以用 `O(n)` 的时间复杂度和 `O(1)` 的空间复杂度解决该问题吗？

### 倒序删除

倒序遍历字符串的字符数组

当前字符是`#`的时候，则跳过字符数 + 1

反之，判断跳过字符数是否比 0 大，即需要删除当前字符，如果需要，则直接跳过本字符，并且跳过字符数 - 1

将两者删除后的字符数组返回，比较即可

> 示例代码

```java
public boolean backspaceCompare(String s, String t) {
    // 得到跳过后的字符数组
    Character[] sRes = removeBackspace(s);
    Character[] tRes = removeBackspace(t);
    // 如果长度已经不一致了，直接返回false
    if (sRes.length != tRes.length) {
        return false;
    }
    for (int i = 0; i < sRes.length; i++) {
        // 逐个判断
        if (!sRes[i].equals(tRes[i])) {
            return false;
        }
    }
    return true;
}

public Character[] removeBackspace(String s) {
    LinkedList<Character> res = new LinkedList<>();
    char[] sArray = s.toCharArray();
    int remove = 0;
    for (int i = sArray.length - 1; i >= 0; i--) {
        if (sArray[i] == BACK_SPACE) {
            remove++;
            continue;
        }
        if (remove >= 1) {
            remove--;
            continue;
        }
        res.addFirst(sArray[i]);
    }
    return res.toArray(new Character[0]);
}
```

### 双指针

以上方式是分别处理字符串，接下来使用双指针同时操作两个字符串

```java
public boolean backspaceCompare(String s, String t) {
    int sIndex = s.length() - 1;
    int sRemove = 0;
    int tIndex = t.length() - 1;
    int tRemove = 0;
    while (sIndex >= 0 || tIndex >= 0) {
        // 将sIndex指向去除 # 后的第一个字符
        while (sIndex >= 0) {
            if (s.charAt(sIndex) == BACK_SPACE) {
                sRemove++;
                sIndex--;
            } else if (sRemove > 0) {
                sRemove--;
                sIndex--;
            } else {
                break;
            }
        }
        // 将tIndex指向去除 # 之后的第一个字符
        while (tIndex >= 0) {
            if (t.charAt(tIndex) == BACK_SPACE) {
                tRemove++;
                tIndex--;
            } else if (tRemove > 0) {
                tRemove--;
                tIndex--;
            } else {
                break;
            }
        }
        // 如果两个都大于0，说明两者都有可以比较的字符
        if (sIndex >= 0 && tIndex >= 0) {
            if (s.charAt(sIndex) != t.charAt(tIndex)) {
                return false;
            }
        } else {
            // 1.sIndex >= 0 tIndex < 0  
            // 2.sIndex < 0 tIndex >= 0  
            // 1 和 2 都是两个字符串中有一个已经在 # 的影响下走完了，不符合要求
            // 3.sIndex < 0 tIndex < 0 
            // 3 就是两个都在 # 的影响下都走完了，符合要求的
            if (sIndex >= 0 || tIndex >= 0) {
                return false;
            }
        }
        sIndex--;
        tIndex--;
    }
    return true;
}
```