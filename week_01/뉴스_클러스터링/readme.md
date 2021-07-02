건희
<details>
<summary>접기/펼치기 버튼</summary>

``` python
def solution(str1, str2):
    list1 = []
    list2 = []
    
    for i, j in zip(str1, str1[1:]):
        if (i + j).isalpha() : list1.append((i + j).upper())
    
    for i, j in zip(str2, str2[1:]):
        if (i + j).isalpha() : list2.append((i + j).upper())
    
    if len(list1) > len(list2):
        inter = len([list1.remove(x) for x in list2 if x in list1])
    else:
        inter = len([list2.remove(x) for x in list1 if x in list2])
    
    mom = len(list1 + list2)
    
    if mom == 0:
        return 65536
    
    return int(inter / mom * 65536)
```

</details>    
    
정영
<details markdown="1">
<summary>접기/펼치기 버튼</summary>
    
``` c++
// 22:00 ~ 22:40 + 09:33 ~ 09:57 + 13:41 ~ 13:59
#include <string>
#include <vector>

using namespace std;

bool isEnglish(char c) {
    return ('a' <= c && c <='z') || ('A' <= c && c <='Z');
}
bool isSame(string a, string b) {
    return a == b;
}
void makeLower(string &s) {
    for(int i = 0; i < s.length(); i++) {
        if(isEnglish(s[i]) && ('A' <= s[i] && s[i] <= 'Z')) {
            s[i] += 'a' - 'A';
        }
    }
}
// 두 글자씩 문자열 탐색을 하면 될듯?
// 연속된 특수문자에 대한 처리가 있으면 좋을듯
// str2에 대해서는 확인할 필요가 있는 위치만 따로 관리하면 좋을 듯
int solution(string str1, string str2) {
    int answer = 0;
    int set_count = 0; // 교집합의 개수
    int union_count = 0; // 합집합의 개수
    int comb_count = 0; // 가능한 원소의 합
    vector<string> str2_comb; // str2의 조합
    makeLower(str1);
    makeLower(str2);
    
    // str2의 조합을 미리 구해놓는다.
    for(int cur = 0; cur < str2.length(); cur++) {
        if(!isEnglish(str2[cur])) continue;
        if(!isEnglish(str2[cur+1])) {
            cur++;
            continue;
        }
        str2_comb.push_back(str2.substr(cur, 2));
        comb_count++;
    }
    
    // 두 글자 단위로 읽으면서 진행
    // 가능한 원소의 합 - 교집합 수 = 합집합 수
    for(int cur = 0; cur < str1.length()-1; cur++) {
        // str1에서 불가능
        if(!isEnglish(str1[cur])) continue;
        if(!isEnglish(str1[cur+1])) {
            cur++;
            continue;
        }
        // str1에서 가능
        comb_count++;
        for(auto it = str2_comb.begin(); it != str2_comb.end(); ) { // str2_comb를 순회하면서 확인
            if(isSame(str1.substr(cur, 2), *it)) { // 글자가 같으면
                set_count++;
                it = str2_comb.erase(it);
                break;
            } else {
                it++;
            }
        }
    }
    if(comb_count == 0) return 65536;
    union_count = comb_count - set_count;
    answer = (float)set_count / union_count * 65536;
    return answer;
}
```
</details>
    
건률(파이썬)
<details>
<summary>접기/펼치기 버튼</summary>
  
### 06-28 23:50 완료

``` python
def inDict(text):
    dic = {}
    for i in range(len(text)-1) :
        if not ('A' <= text[i] <= 'Z' and 'A' <= text[i+1] <= 'Z') :
            continue
        if text[i:i+2] in dic :
            dic[text[i:i+2]] += 1
        else :
            dic[text[i:i+2]] = 1
    return dic

def unionDict(dict1, dict2):
    dic = {}
    for key,value in dict1.items() :
        dic[key] = value
    for key,value in dict2.items() :
        if key in dic :
            dic[key] = value if value > dic[key] else dic[key]
        else :
            dic[key] = value
    return dic


def interDict(dict1, dict2):
    temp = {}
    dic = {}
    for key,value in dict1.items() :
        temp[key] = value
    for key,value in dict2.items() :
        if key in temp :
            dic[key] = value if value < temp[key] else temp[key]
    return dic


def solution(str1, str2):
    str1 = str1.upper()
    str2 = str2.upper()
    answer = 0
    
    dict1 = inDict(str1)
    dict2 = inDict(str2)
    
    unionSum = 0
    interSum = 0
    
    for key,value in unionDict(dict1,dict2).items():
        unionSum += value
    for key,value in interDict(dict1,dict2).items():
        interSum += value
    
    if unionSum == 0 :
        return 65536
    else :
        return int((interSum/unionSum)*65536)
    
```
</details>
  
문교
```
copy here
```
