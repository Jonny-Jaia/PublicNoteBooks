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

首先序列化的核心在于将原有的不同数据结构进行转化，以实现实际需求。

从序列化来看：

粗略分析需求: 1.遍历二叉树 2.保存元素并返回

详细分析：

1. 遍历（层序遍历）

   1.1 判断当前元素是否为空，不为空则加入返回列表
   为空的情况分别为：

   	- 左右子节点存在一处为空->此时只需要往结果里面添加None
    - 左右根节点之后为空->判断当前同一层级节点是否有后续子节点
      	- 如果有的话，当前根节点添加两个None
         	- 

2. 将列表数据直接str进行转化返回
