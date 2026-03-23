# leetcode226.-

<img width="1103" height="868" alt="image" src="https://github.com/user-attachments/assets/62e024ca-5264-4e3f-b1ac-9f66684b3303" />

### （1）带有详细注释的代码

```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # 递归的终止条件：如果当前访问的节点为空，则直接返回 None
        if root is None:
            return None
            
        # 递归深入当前节点的左子树，并将彻底反转后的左子树根节点暂存到变量 left 中
        left = self.invertTree(root.left)  # 翻转左子树
        
        # 递归深入当前节点的右子树，并将彻底反转后的右子树根节点暂存到变量 right 中
        right = self.invertTree(root.right)  # 翻转右子树
        
        # 将当前节点的左侧指针，指向刚才暂存的已经反转好的右子树
        root.left = right  # 交换左右儿子
        
        # 将当前节点的右侧指针，指向刚才暂存的已经反转好的左子树
        root.right = left
        
        # 返回当前已经完成左右子树交换的节点，将其提供给上一层调用者
        return root
```

---

### （2）代码逐行详细中文解释



* `class Solution:`
    定义一个名为 `Solution` 的类，作为算法逻辑的封装外壳。
* `def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:`
    定义类中的核心方法 `invertTree`。该方法接收一个名为 `root` 的参数，类型为二叉树节点 `TreeNode`（可能为空）。方法执行完毕后，要求返回反转后的二叉树根节点。
* `if root is None:`
    设立递归算法的基础边界条件（Base Case）。在不断向下递归的过程中，如果越过了叶子节点，当前传入的 `root` 就会变成空值（`None`）。
* `return None`
    当遇到空节点时，由于空节点没有子树可以反转，因此直接返回 `None` 给上一层，结束当前分支的深入。
* `left = self.invertTree(root.left)`
    针对当前节点的左子树发起递归调用。程序会暂停当前层的后续动作，优先去处理整棵左子树的反转。当左子树的所有反转操作全部彻底完成后，将其返回的最顶层节点（即反转后的左子树新根节点）赋值给局部变量 `left`。
* `right = self.invertTree(root.right)`
    针对当前节点的右子树发起递归调用。执行逻辑与上一行完全一致，程序去处理整棵右子树的反转，并将完成后返回的新根节点赋值给局部变量 `right`。
* `root.left = right`
    开始执行当前节点层面的核心操作：将当前节点 `root` 的左指针（原本指向旧的左子树），修改为指向刚才处理好的、原本属于右侧的子树结构（即变量 `right`）。
* `root.right = left`
    继续核心操作：将当前节点 `root` 的右指针（原本指向旧的右子树），修改为指向刚才处理好的、原本属于左侧的子树结构（即变量 `left`）。至此，当前节点下方的左右两大部分完美互换。
* `return root`
    当前节点及其所有子树均已完成反转动作。将这个处理完毕的当前节点 `root` 作为结果，返回给上一层调用它的父节点，以便父节点继续完成自身的交换逻辑。

---

### （3）具体数值算例与变量变化表格

假设存在一棵包含 3 个节点的简单二叉树，结构如下：
* 根节点值为 `2`
* 根的左子节点值为 `1`，且无子节点
* 根的右子节点值为 `3`，且无子节点

原始树的形状示意：
```text
      2
     / \
    1   3
```

调用 `invertTree(root)` 进行反转。预期结果为左子节点变为 `3`，右子节点变为 `1`。

**执行过程表格展示：**

| 步骤 | 当前所在函数 | 传入节点 `root` | 执行动作细节 | 变量 `left` | 变量 `right` | 节点操作与返回值 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Step 1** | `invertTree(节点 2)` | `2` | 准备执行 `invertTree(root.left)`，进入左子树 | 待定 | 待定 | 等待左右子树结果 |
| **Step 2** | `invertTree(节点 1)` | `1` | 准备执行 `invertTree(root.left)`，进入节点 1 的左侧 | 待定 | 待定 | 等待左右子树结果 |
| **Step 3** | `invertTree(None)` | `None` | 节点 1 的左侧为空，触发 `if root is None` | 无 | 无 | 返回 `None` 给 Step 2 |
| **Step 4** | `invertTree(节点 1)` | `1` | 得到左侧结果，准备探寻节点 1 的右侧 | `None` | 待定 | 等待右子树结果 |
| **Step 5** | `invertTree(None)` | `None` | 节点 1 的右侧为空，触发 `if root is None` | 无 | 无 | 返回 `None` 给 Step 4 |
| **Step 6** | `invertTree(节点 1)` | `1` | 得到右侧结果。执行指针交换：`root.left = right`, `root.right = left` | `None` | `None` | 节点 1 左右指针皆设为 `None`。返回节点 1 给 Step 1 |
| **Step 7** | `invertTree(节点 2)` | `2` | 得到左子树结果，准备探寻节点 2 的右侧 | `节点 1` | 待定 | 等待右子树结果 |
| **Step 8** | `invertTree(节点 3)` | `3` | 准备执行 `invertTree(root.left)`，进入节点 3 的左侧 | 待定 | 待定 | 等待左右子树结果 |
| **Step 9** | `invertTree(None)` | `None` | 节点 3 的左侧为空，触发 `if root is None` | 无 | 无 | 返回 `None` 给 Step 8 |
| **Step 10**| `invertTree(节点 3)` | `3` | 得到左侧结果，准备探寻节点 3 的右侧 | `None` | 待定 | 等待右子树结果 |
| **Step 11**| `invertTree(None)` | `None` | 节点 3 的右侧为空，触发 `if root is None` | 无 | 无 | 返回 `None` 给 Step 10 |
| **Step 12**| `invertTree(节点 3)` | `3` | 得到右侧结果。执行指针交换：`root.left = right`, `root.right = left` | `None` | `None` | 节点 3 左右指针皆设为 `None`。返回节点 3 给 Step 7 |
| **Step 13**| `invertTree(节点 2)` | `2` | 得到右子树结果。执行核心交换逻辑 | `节点 1` | `节点 3` | 节点 2 的左指针指向 `节点 3`，右指针指向 `节点 1`。返回节点 2。 |

经过完整的后序遍历（先处理左右子树，最后处理根节点自身）推导，最终二叉树完美翻转为：
```text
      2
     / \
    3   1
```


