건희

<details>
<summary>접기/펼치기 버튼</summary>

``` python

  ### 
  ###	통과 (0.44ms, 10.3MB)
  ### 시간 : 30분 (문제이해하는데 소비) + 1시간 = 1
  # u가 제대로 된놈인지 보는 함수
def is_validate(u):
    count = 0
    
    # 음수가 나온다면 올바르지 않은 상황이니까 break return
    for i in u:
        add = 1 if i == '(' else -1
        count += add
        if (count < 0):
            return False
    
    # )가 더많은경우 
    return count == 0

def solution(p):
    answer = ''
    if p == "":
        return p
    count = 0
    u = ""
    v = ""
    
    #u, v 분리하기 밸런스가 맞다면 break 
    for i in range(len(p)):
        if p[i] == '(':
            count += 1
        else:
            count -= 1
        if count == 0:
            u = p[0:i+1]
            v = p[i+1:]
            break;
    
    #valid 하다면 u 뒤에 v 붙이고 끝낸다 v 는 다시 재귀적으로 solution
    if is_validate(u) == True:
        return u + solution(v)
    
    #그게 아니라면 v 앞두로 괄호 붙이고
    answer = '(' + solution(v) + ')'
    # [1 : -1]뒤집어주기
    for i in range(1,len(u)-1):
        add = '(' if u[i] == ')' else ')'
        answer += add
    
    return answer

```

</details>    

정영

<details>
<summary>접기/펼치기 버튼</summary>
<br>
최장시간 
  
  테스트 19 〉	통과 (0.11ms, 3.97MB)

``` cpp

// 22:20 ~ 22:40 + 17:40 ~ 18:00
#include <string>
#include <vector>
#include <algorithm>

using namespace std;
bool is_correct(string s) {
    int balance = 0;
    for(int i = 0; i < s.length(); i++) {
        if(s[i] == '(') balance++;
        else balance--;
        if(balance < 0) return false;
    }
    return true;    
}

string recursive(string s) {
    if(s.length() == 0) return "";
    
    int balance = 0;
    string u, v;
    for(int i = 0; i < s.length(); i++) {
        if(s[i] == '(') balance++;
        else balance--;
        if(balance == 0) {
            u = s.substr(0, i+1);
            v = s.substr(i+1, s.length() - i);
            break;
        }
    }
    
    if(is_correct(u)) {
        return u + recursive(v);
    } else {
        string sub = "(";
        sub += recursive(v);
        sub += ")";
        u.erase(0, 1);
        u.erase(u.length()-1, 1);
        for(int i = 0; i < u.length(); i++) {
            if(u[i] == ')') u[i] = '(';
            else u[i] = ')';
        }
        // u = reverse(u);
        return sub + u;
    }
}

string solution(string p) {
    p = recursive(p);
    return p;
}

```

</details>    

건률

<details>
<summary>접기/펼치기 버튼</summary>
<br>
최장시간 
  
  테스트 19 〉	통과 (0.45ms, 10.3MB)
  
``` python

def findUV(ss):
    count = 0
    for i in range(len(ss)) :
        if ss[i] == '(' :
            count += 1
        elif ss[i] == ')' :
            count -= 1
        
        if count == 0 :
            return ss[:i+1],ss[i+1:]
    return '',''
        
def isRight(ss):
    count = 0
    for char in ss:
        if char == '(' :
            count += 1
        elif char == ')' :
            count -= 1
        if count < 0 :
            return False
    return True


def find(p):
    result = ''
    if p == '' :return ''
    UV = findUV(p)
    if not isRight(UV[0]):
        mm = {'(' : ')' , ')' : '('}
        result = '('
        result += find(UV[1])
        result += ')'
        # 괄호 뒤집기가 문자열의 순서를 뒤집는게 아니라 
        # ( -> )로 , ) -> ( 로 바꾸라는 뜻이었음...
        result += ''.join([mm[char] for char in UV[0][1:-1]])
    else :
        result = UV[0]
        result += find(UV[1])
    return result

def solution(p):
    answer = find(p)
    return answer
                    
```

</details>    

문교

<details>
<summary>접기/펼치기 버튼</summary>


</details>    
