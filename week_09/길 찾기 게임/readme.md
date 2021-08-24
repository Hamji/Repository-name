<a href = 'https://programmers.co.kr/learn/courses/30/lessons/42892/'> 길 찾기 게임 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>


</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
  
``` python

테스트 9 〉	통과 (270.39ms, 14.8MB)

import sys
sys.setrecursionlimit(10**6)

class Item:
    def __init__(self,node):
        self.x = node[0]
        self.y = node[1]
        self.value = node[2]
        self.left = None
        self.right = None
        
class Tree:
    def __init__(self):
        self.root = None
    
    def insert(self,item):
        if self.root == None:
            self.root = item
            return
        
        node = self.root
        preNode = None
        while node != None:
            if item.x < node.x :
                if node.left == None:
                    node.left = item
                    break
                node = node.left
            
            elif item.x > node.x :
                if node.right == None:
                    node.right = item
                    break
                node = node.right
                
    def preOrder(self,node,vector):
        if node == None: return
        
        vector.append(node.value)
        self.preOrder(node.left,vector)
        self.preOrder(node.right,vector)
        return vector
    
    def postOrder(self,node,vector):
        if node == None: return
        
        self.postOrder(node.left,vector)
        self.postOrder(node.right,vector)
        vector.append(node.value)
        return vector
        
def solution(nodeinfo):
    answer = [[]]
    
    for index,node in enumerate(nodeinfo):
        node.append(index+1)
        
    nodeinfo = sorted(nodeinfo, key = lambda x : x[1], reverse=True)
    tree = Tree()
    
    for node in nodeinfo:
        item = Item(node)
        tree.insert(item)
    
    answer = [tree.preOrder(tree.root,[]),tree.postOrder(tree.root,[])]
    return answer

```


</details>
