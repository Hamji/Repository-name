
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/42890'> 후보키 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
``` python

from itertools import combinations

def solution(relation):
    row = len(relation)
    col = len(relation[0])
    
    candi = []
    # combination으로 키 경우의 수 만들어주기
    for i in range(1, col+1):
        candi.extend(combinations(range(col), i))
    
    # exclusive 가 유일성이래 알고 계셧나요?
    exclu = []
    for ks in candi:
        temp = [tuple([item[k] for k in ks]) for item in relation]
        #print(temp)
        # 추출한 키값의 길이가 row 와 같다면 candidate 의 유일성 만족한다
        if len(set(temp)) == row:
            exclu.append(ks)
    
    # 최소성 검사
    minim = set(exclu)
    for i in range(len(exclu)):
        for j in range(i + 1, len(exclu)):
            if len(exclu[i]) == len(set(exclu[i]).intersection(set(exclu[j]))):
                # print(exclu[i])
                # print(exclu[j])
                # print("---")
                minim.discard(exclu[j])
    
    #print(minim)
    return len(minim)
	
```
	
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>
``` cpp
테스트 20 〉	통과 (0.48ms, 3.84MB)
#include <string>
#include <vector>
#include <map>
#include <algorithm>

using namespace std;

bool is_inner_comb(vector<int> comb, vector<vector<int>> inner_combs) {
    for(auto inner_comb : inner_combs) {
        // inner_comb를 하나라도 가지고 있으면 실패
        bool is_new = false;
        for(auto c : inner_comb) {
            // 모두 존재했던 거라면 큰일임
            if(comb.end() == find(comb.begin(), comb.end(), c)) {
                is_new = true;
                break;
            }
        }
        if(!is_new) {
            return true;
        }
    }
    return false;
}

int solution(vector<vector<string>> relation) {
    int answer = 0;
    int tuple_size = relation.size();
    int attribute_size = relation[0].size();
    vector<vector<int>> comb_vec;
    vector<vector<int>> unique_comb;
    map<string, int> m;

    // 기본적으로는 모든 조합을 만든다. (ex 1개, 2개 ...)
    for(int i = 1; i <= attribute_size; i++) {
        // unique로 확인되지 않은 index를 가지고 조합을 만들어야 한다. (ex 2개 짜리 조합 01, 02, ...)
        vector<bool> v(attribute_size - i, false);
        v.insert(v.end(), i, true);
        do {
            vector<int> comb;
            for(int k = 0; k < attribute_size; k++) {
                if(v[k]) {
                    comb.push_back(k);
                }
            }
            if(!is_inner_comb(comb, unique_comb)) comb_vec.push_back(comb);  // 만들어낸 조합에 특정 조합이 없다면 추가
        } while (next_permutation(v.begin(), v.end()));
        if(comb_vec.size() == 0) break;                     // 조합이 만들어지지 않았으면 종료
        
        // 만들어낸 조합 전체를 확인
        for(auto comb : comb_vec) {
            bool isUnique = true;
            
            // tuple 마다 map에 추가함
            for(int x = 0; x < tuple_size; x++) {
                // 조합 index를 튜플의 string으로 변환
                string temp = "";
                for(auto y : comb) {
                    temp += relation[x][y];
                }
                
                // 해당 조합은 유니크하지 않음!
                if(m[temp] != 0) {
                    isUnique = false;
                    break;
                }

                m[temp]++;
            }
            m.clear();
            
            // 해당 조합은 유니크함!
            if(isUnique) {
                answer++;
                unique_comb.push_back(comb);
            }
        }
        comb_vec.clear();
    }
    
    return answer;
}	
```
</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>

``` python

테스트 18 〉	통과 (1.12ms, 10.4MB)

from itertools import combinations 

#특정 index가 키가 될 수 있는지 없는지 확인
def checkUnique(relation,checkIndex):
    checkSet = set()
    for row in relation:
        checkList = []
        for i in checkIndex:
            checkList.append(row[i])
        
        if tuple(checkList) in checkSet:
            return False
        checkSet.add(tuple(checkList))
    return True

#특정 index가 후보키가 될 수 있는지 없는지 확인
#set안에 존재하는 index가 subset 이면 불가능
def checkCandi(candiSet,checkIndex):
    count = 0
    for candi in candiSet:
        count = 0
        for i in checkIndex:
            if i in candi :
                count+=1
        if count == len(candi):
            return False
    return True

def solution(relation):
    answer = 0
    items = list(range(len(relation[0])))
    candiSet = set()
    for i in items :
        #가능한 경우의 수를 조합으로 생성
        for t in list(combinations(items, i+1)) :
            #check 함수의 순서를 반대로하면 시간차이가 발생
            #연산의 효율때문
            if checkCandi(candiSet,t) is True :
                if checkUnique(relation,t) is True :
                    candiSet.add(t)
        
    return len(candiSet)
```
</details>
  
문교
<details>
<summary>접기/펼치기 버튼</summary>

</details>
