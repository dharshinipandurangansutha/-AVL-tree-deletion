# -AVL-tree-deletion
Practical program 
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.height = 1


def height(node):
    if not node:
        return 0
    return node.height


def getBalance(node):
    if not node:
        return 0
    return height(node.left) - height(node.right)


def rightRotate(y):
    x = y.left
    t2 = x.right

    x.right = y
    y.left = t2

    y.height = 1 + max(height(y.left), height(y.right))
    x.height = 1 + max(height(x.left), height(x.right))

    return x


def leftRotate(x):
    y = x.right
    t2 = y.left

    y.left = x
    x.right = t2

    x.height = 1 + max(height(x.left), height(x.right))
    y.height = 1 + max(height(y.left), height(y.right))

    return y


def minValueNode(node):
    current = node
    while current.left:
        current = current.left
    return current


def deleteNode(root, key):

    if not root:
        return root

    if key < root.key:
        root.left = deleteNode(root.left, key)

    elif key > root.key:
        root.right = deleteNode(root.right, key)

    else:
        if root.left is None:
            return root.right

        elif root.right is None:
            return root.left

        temp = minValueNode(root.right)
        root.key = temp.key
        root.right = deleteNode(root.right, temp.key)

    root.height = 1 + max(height(root.left), height(root.right))

    balance = getBalance(root)

    if balance > 1 and getBalance(root.left) >= 0:
        return rightRotate(root)

    if balance < -1 and getBalance(root.right) <= 0:
        return leftRotate(root)

    if balance > 1 and getBalance(root.left) < 0:
        root.left = leftRotate(root.left)
        return rightRotate(root)

    if balance < -1 and getBalance(root.right) > 0:
        root.right = rightRotate(root.right)
        return leftRotate(root)

    return root


def inorder(root):
    if root:
        inorder(root.left)
        print(root.key, end=" ")
        inorder(root.right)


root = Node(30)
root.left = Node(20)
root.right = Node(40)
root.left.left = Node(10)

print("AVL Tree before deletion:")
inorder(root)

root = deleteNode(root, 10)

print("\nAVL Tree after deletion:")
inorder(root)
