# 题目

- #### [剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

  难度中等286

  输入一个字符串，打印出该字符串中字符的所有排列。

  你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

  **示例:**

  ```
  输入：s = "abc"
  输出：["abc","acb","bac","bca","cab","cba"]
  ```

  **限制：**

  ```
  1 <= s 的长度 <= 8
  ```



# 样例代码

```python
class Solution:
    def permutation(self, s: str) -> List[str]:
```

# 题目解析

- 输出对象总共个数有n！
- 排除包含重复字符的结果
- 初始化固定一个元素x，递归得到结果
- 递归需要的工作-将

## 算法过程

**1.终止条件：**

```
x =  len(c)-1
```

**2. 递归参数：** x

**3. 递归内容：**

- 判断当前是否为重复元素

```
if c[i] in temp
```

- 按照先后顺序固定当前元素-交换元素

```python
c[i], c[x] = c[x], c[i]
```

- 进行下一层递归

  ```python
  dfs(x+1)
  ```

- 还原交换

  ```
  c[i], c[x] = c[x], c[i]
  ```

**4. 初始化：**

```python
# 将字符串类型转成列表类型
c, res = list(s), []
```

**5.构建递归函数**

```python
dfs(x):
    # 递归结束条件
    if x == len(c)-1:
        res.append(''.join(c))
        return 
    # 初始化过渡变量
    tempdict = set()
    for i in range(x, len(c)):
        if c[i] in tempdict:continue
        tempdict.add(c[i])
        c[i], c[x] = c[x], c[i]
        dfs(x+1)
        c[i], c[x] = c[x], c[i]
```

# 通过代码

```python
class Solution:
    def permutation(self, s: str) -> List[str]:
        c, res = list(s), []
        def dfs(x):
            if x == len(c)-1:
                res.append(''.join(c))
                return
        	temp = set()
            for i in range(x, len(c)):
                if c[i] in temp:continue
                temp.add(c[i])
                c[i],c[x]=c[x],c[i]
                
                c[i],c[x]=c[x],c[i]
        dfs(0)
        return res
```

