<a href = 'https://programmers.co.kr/learn/courses/30/lessons/42892/'> 길 찾기 게임 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
테스트 11 〉	통과 (14.06ms, 10.1MB)
#include <string>
#include <vector>
#include <map>
#include <set>
#include <stack>

using namespace std;

map<int, set<pair<int,int>>> setmap;
vector<set<pair<int,int>>> nodeset;

class Node {
public:
    int val;    // index
    int pos;    // position-x
    Node* left;
    Node* right;
    Node(int val, int pos) {
        this->val = val;
        this->pos = pos;
        this->left = NULL;
        this->right = NULL;
    }
};

vector<vector<int>> solution(vector<vector<int>> nodeinfo) {
    vector<vector<int>> answer;
    if(nodeinfo.size() == 1) {
        return {{1},{1}};
    }
    
    // init
    for(int i = 0; i < nodeinfo.size(); i++) {
        setmap[nodeinfo[i][1]].insert({nodeinfo[i][0], i+1});
    }
    for(auto it = setmap.begin(); it != setmap.end(); ++it) {
        nodeset.push_back(it->second);
    }
    
    // make tree
    Node* root = new Node(nodeset[nodeset.size()-1].begin()->second, nodeset[nodeset.size()-1].begin()->first);
    
    for(int i = nodeset.size()-2; i >= 0; i--) {
        for(auto it = nodeset[i].begin(); it != nodeset[i].end(); ++it) {
            int depth = 2;
            auto cur = root;
            while(nodeset.size() != i + depth++) {
                if(it->first < cur->pos) cur = cur->left;
                else cur = cur->right;
            }
            if(it->first < cur->pos) cur->left = new Node(it->second, it->first);
            else cur->right = new Node(it->second, it->first);
        }
    }
    
    // order init
    stack<Node*> s;
    vector<int> pre;
    s.push(root);
    // get preorder
    while(s.size()) {
        auto cur = s.top();
        s.pop();
        
        pre.push_back(cur->val);
        if(cur->right != NULL) {
            s.push(cur->right);
        }
        if(cur->left != NULL){
            s.push(cur->left);
        }
    }
    
    stack<int> post;
    vector<int> _post;
    s.push(root);
    // get postorder
    while(s.size()) {
        auto cur = s.top();
        s.pop();
        
        post.push(cur->val);
        if(cur->left != NULL){
            s.push(cur->left);
        }
        if(cur->right != NULL) {
            s.push(cur->right);
        }
    }
    while(post.size()) {
        _post.push_back(post.top());
        post.pop();
    }
    
    return {pre, _post};
}
```

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
