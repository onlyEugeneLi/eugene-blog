# Trie

A tree with no limit of children. 

💡 [Trie Data Structure - GeeksforGeeks](https://www.geeksforgeeks.org/dsa/trie-insert-and-search/)

# 🎄 Binary Tree

List representation of a Binary tree

For a given node at index $i$, the children of that node are $(2*i)+1$ and $(2*i)+2$.

Prperties
* The maximum number of nodes at level L of a binary tree is $2^L$
* The maximum total number of nodes in a binary tree of height $H$ is $2^H – 1$
* Total number of leaf nodes in a binary tree = total number of nodes with 2 children + 1
* In a Binary Tree with $N$ nodes, the minimum possible height or the minimum number of levels is $log_2(N+1)$
* A Binary Tree with L leaves has at least $| Log_2L |+ 1$ levels

## Deleting a node

How to decide which node to replace? How to deal with children nodes? 

```python
from collections import deque

class Node:
    def __init__(self, d):
        self.data = d
        self.left = None
        self.right = None

# Function to delete a node from the binary tree
def deleteNode(root, val):
    if root is None:
        return None

    # Use a queue to perform BFS
    queue = deque([root])
    target = None

    # Find the target node
    while queue:
        curr = queue.popleft()

        if curr.data == val:
            target = curr
            break
        if curr.left:
            queue.append(curr.left)
        if curr.right:
            queue.append(curr.right)

    if target is None:
        return root

    # Find the deepest rightmost node and its parent
    last_node = None
    last_parent = None
    queue = deque([(root, None)])

    while queue:
        curr, parent = queue.popleft()
        last_node = curr
        last_parent = parent

        if curr.left:
            queue.append((curr.left, curr))
        if curr.right:
            queue.append((curr.right, curr))

    # Replace target's value with the last node's value
    target.data = last_node.data

    # Remove the last node
    if last_parent:
        if last_parent.left == last_node:
            last_parent.left = None
        else:
            last_parent.right = None
    else:
        return None
    return root

# In-order traversal
def inorder(root):
    if root is None:
        return
    inorder(root.left)
    print(root.data, end=" ")
    inorder(root.right)

if __name__ == "__main__":
    root = Node(2)
    root.left = Node(3)
    root.right = Node(4)
    root.left.left = Node(5)
    root.left.right = Node(6)

    print("Original tree (in-order): ", end="")
    inorder(root)
    print()

    val_to_del = 3
    root = deleteNode(root, val_to_del)

    print(f"Tree after deleting {val_to_del} (in-order): ", end="")
    inorder(root)
    print()
```

# Sorting

## Merge sort

split() method

merge() method

Main method
* split the list into half
* Call Main method
* Return merge() result

