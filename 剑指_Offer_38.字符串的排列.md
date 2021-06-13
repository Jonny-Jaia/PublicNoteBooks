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







# 通过代码

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        if not root:return "[]"
        # 初始化
        queue = collections.deque()
        res = []
        queue.append(root)
        while queue:
            node = queue.popleft()
            if node:
                res.append(str(node.val))
                queue.append(node.left)
                queue.append(node.right)
            else:res.append("null")
        return '['+','.join(res)+']'

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        if data == "null":return
        vals, i = data[1:-1].split(','),1
        queue = collections.deque()
        root = TreeNode(int(vals[i]))
		queue.append(root)
        while queue:
            node = queue.popleft()
            if node != "null":
                node.left = TreeNode(int(vals[i]))
                queue.append(node.left)
            i += 1
            if node != "null":
                node.right = TreeNode(int(vals[i]))
                queue.append(node.right)
            i += 1
        return root 
# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.deserialize(codec.serialize(root))
```

