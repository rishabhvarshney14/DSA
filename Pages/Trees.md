```python
# Binary Search Tree
class Node:
    def __init__(self, value, parent=None):
        self.value = value
        self.parent = parent
        self.left = None
        self.right = None

class BinarySearchTree:
    def __init__(self, root=None):
        self.root = root
        
    def _reassign_nodes(self, node, new_children):
        if new_children is not None:
            new_children.parent = node.parent
        if node.parent is not None:
            if self.is_right(node):
                node.parent.right = new_children
            else:
                node.parent.left = new_children
        else:
            self.root = new_children
    
    def is_right(self, node):
        return node == node.parent.right
    
    def empty(self):
        return self.root == None
    
    def _insert(self, value):
        new_node = Node(value)
        
        if self.empty():
            self.root = new_node
        else:
            parent_node = self.root
            
            while True:
                if value < parent_node.value:
                    if parent_node.left == None:
                        parent_node.left = new_node
                        break
                    else:
                        parent_node = parent_node.left
                else:
                    if parent_node.right == None:
                        parent_node.right = new_node
                        break
                    else:
                        parent_node = parent_node.right
            new_node.parent = parent_node
            
    def insert(self, *values):
        for value in values:
            self._insert(value)
            
    def search(self, value):
        node = self.root
        if self.empty():
            print("Tree is empty")
        else:
            while node != None:
                if value == node.value:
                    return node
                elif value < node.value:
                    node = node.left
                else:
                    node = node.right
            print("value does not exist")
    
    def get_max(self, node=None):
        if node == None:
            node = self.root
        if self.empty():
            print("tree is empty")
        else:
            while node.right != None:
                node = node.right

            return node
    
    def get_min(self, node=None):
        if node == None:
            node = self.root
        
        if self.empty():
            print("Empty")
        else:
            while node.left != None:
                node = node.left
            
            return node
        
    def remove(self, value):
        node = self.search(value)
        
        if node != None:
            if node.left == None and node.right == None:
                self._reassign_nodes(node, None)
            elif node.left == None:
                self._reassign_nodes(node, node.right)
            elif node.right == None:
                self._reassign_nodes(node, node.left)
            else:
                tmp_node = self.get_max(node.left)
                
                self.remove(tmp_node.value)
                
                node.value = tmp_node.value
                
    def preorder(self, node):
        if node is not None:
            print(node.value)
            self.preorder(node.left)
            self.preorder(node.right)
            
    def inorder(self, node):
        if node is not None:
            self.inorder(node.left)
            print(node.value)
            self.inorder(node.right)
    
    def postorder(self, node):
        if node is not None:
            self.postorder(node.left)
            self.postorder(node.right)
            print(node.value)
    
    def traverse(self, type='inorder', node=None):
        if node == None:
            node = self.root
        
        if type=='inorder':
            self.inorder(node)
        elif  type=='preorder':
            self.preorder(node)
        else:
            self.postorder(node)
        
    def _height(self, node):
        if node != None:
            return 1 + max(self._height(node.left), self._height(node.right))
        return 0     
    
    def height(self, node = None):
        if node == None:
            node = self.root
            
        return self._height(node)
```


```python
testlist = (8, 3, 6, 1, 10, 14, 13, 4, 7)
t = BinarySearchTree()
for i in testlist:
    t.insert(i)
```


```python
# Tree Traversal

# Print Element from left to right
def print_left_right(node, level):
    if not node:
        return
    if level == 1:
        print(node.value)
    else:
        print_left_right(node.left, level-1)
        print_left_right(node.right, level-1)

# Print Element from right to left
def print_right_left(node, level):
    if not node:
        return 
    if level == 1:
        print(node.value)
    else:
        print_right_left(node.right, level-1)
        print_right_left(node.left, level-1)
```


```python
for h in range(1, t.height()+1):
    print_left_right(t.root, h)
```

    8
    3
    10
    1
    6
    14
    4
    7
    13



```python
for h in range(1, t.height()+1):
    print_right_left(t.root, h)
```

    8
    10
    3
    14
    6
    1
    13
    7
    4

