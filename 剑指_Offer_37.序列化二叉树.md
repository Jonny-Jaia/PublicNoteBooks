# 题目

#### [剑指 Offer 37. 序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

难度困难162

请实现两个函数，分别用来序列化和反序列化二叉树。

**示例:** 

```
你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```

注意：本题与主站 297 题相同：https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/



#### [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

难度困难575

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

**提示:** 输入输出格式与 LeetCode 目前使用的方式一致，详情请参阅 [LeetCode 序列化二叉树的格式](https://leetcode-cn.com/faq/#binary-tree)。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。



![](https://github.com/Jonny-Jaia/PublicNoteBooks/blob/main/img/serdeser.jpg?raw=true)

**示例 1：**



```
输入：root = [1,2,3,null,null,4,5]
输出：[1,2,3,null,null,4,5]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

**示例 4：**

```
输入：root = [1,2]
输出：[1,2]
```

 

**提示：**

- 树中结点数在范围 `[0, 104]` 内
- `-1000 <= Node.val <= 1000`



# 样例代码

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
        

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        

# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.deserialize(codec.serialize(root))
```

# 题目解析

**序列化操作**--层序遍历

**反序列化**--利用递推将得到的序列化列表进行推理



## 序列化

**算法流程：**

**核心思路：借助队列对二叉树实现层序遍历，进而将所有元素进行返回**

**1. 特例处理：**当root为空返回”[]“

**2. 初始化：**当前队列queue = [root]; res = []

**3. 层序遍历：**

- **返回条件**：当queue为空

```python
while queue:
```

- **循环操作**

  - 节点出队，node

  ```python
  node = queue.popleft()
  ```

  - 若node不为空

  ```python
  res.append(str(node.val)
  queue.append(node.left)
  queue.append(node.right)
  ```

  - 为空

  ```python
  res.append("null")
  ```

- **返回值:**

  ```
  return '[' + ','.join(res) + ']'
  ```

  

## 反序列化

**算法流程：**

**核心思路：借助序列化列表的位置关系**

**1. 特例处理：**当data为空返回”null“

**2. 初始化：**序列化列表，对数据进行处理得到vals列表；root = TreeNode(int(vals[0]));空队列queue;初始化标记i=1

```python
queue.append(root)
i = 1
```

**3. 按层构建二叉树：**

- **返回条件**：当queue为空

```python
while queue:
```

- **循环操作**

  - 节点出队，node

  ```python
  node = queue.popleft()
  ```

  - 若node不为空

  ```python
  if vals[i] != "null":
      node.left = TreeNode(int(vals[i]))
      queue.append(node.left)
   i += 1
  if vals[i] != "null":
      node.right = TreeNode(int(vals[i]))
      queue.append(node.right)
   i += 1
  ```

- **返回值:**

  ```
  return root
  ```

  

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

