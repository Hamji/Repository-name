
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/1830'> 브라이언의 고민 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
``` c++

#include <string>
#include <vector>

#define ERROR "invalid"

using namespace std;


// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
string solution(string sentence) {
    string answer = "";
    //const string ERROR = "invalid";
    
    
    vector<string> words;
    bool alpha[26] = {false};
    // rule 
    bool rule1 = false;
    bool rule2 = false;
    // 규칙단어
    char char_1 = 0;
    char char_2 = 0;
    string word = "";
    
    const int length = sentence.length();

    
    for (int i = 0; i < length; i++)
    {
        // rule1 과 2 적용 
        if (rule1 && rule2)
        {
            //upper
            if (isupper(sentence[i]))
            {
                //현재 글자 더하기
                word += sentence[i];
                
                // 다음이 범위 밖이라면?
                if(i + 1 == length)
                    return ERROR;
                // 만약 다음 문자가 대문자?
                else if (isupper(sentence[i+1]))
                    return ERROR;
                // 다음 문자가 소문잔데 규칙1도아니고 규칙2도 아닐때
                else if (char_1 != sentence[i+1] && char_2 != sentence[i+1])
                    return ERROR;
            }
            // lower
            if (islower(sentence[i]))
            {
                // 현재 문자가 rule2 12 둘다 끝낫단 의미
                if (char_2 == sentence[i])
                {
                    rule1 = false;
                    rule2 = false;
                    
                    alpha[char_1 - 'a'] = true;
                    alpha[char_2 - 'a'] = true;
                    
                    char_1 = 0;
                    char_2 = 0;
                    
                    words.push_back(word);
                    word = "";
                    continue;
                }
                // 다음 문자가 범위 밖이라면 ? 룰2 안끝나서 error
                if (i+ 1 == length)
                    return ERROR;
                else if (islower(sentence[i+1]))
                    return ERROR;
            }
        }
        // rule1
        else if(rule1)
        {
            //upper
            if (isupper(sentence[i]))
            {
                // 단어 추가
                word += sentence[i];
                
                if (i + 1 == length)
                {
                    //단어 끝낫단 소리죠
                    rule1 = false;
                    alpha[char_1 - 'a'] = true;
                    char_1 = 0;
                    words.push_back(word);
                    word = "";
                }
                // 다음 단어가 대문자일때  현재 단어 끝낫단 소리
                else if (isupper(sentence[i+1]))
                {
                    rule1 = false;
                    alpha[char_1 - 'a'] = true;
                    char_1 = 0;
                    words.push_back(word);
                    word = "";
                }
                // 다른 소문자 나왓을때 
                else if (char_1 != sentence[i+1])
                {
                    rule1 = false;
                    alpha[char_1 - 'a'] = true;
                    char_1 = 0;
                    words.push_back(word);
                    word = "";
                }
            }
            // lower
            if (islower(sentence[i]))
            {
                // 범위 벗어나면 룰 1이 안끝나니깐 return error
                if (i + 1 == length)
                    return ERROR;
                // 다음에 또 소문자 나오면 안된다..
                else if (islower(sentence[i+1]))
                    return ERROR;
            }
        }
        // rule 2
        else if (rule2)
        {
            //upper
            if (isupper(sentence[i]))
            {
                // 단어 추가
                word += sentence[i];
                
                // 다음이 범위 끝일떄
                if (i + 1 == length)
                    return ERROR;
                // 만약에 다음놈이 소문잔데 rule2거가 아닐떄
                else if (islower(sentence[i + 1]) && char_2 != sentence[i+1])
                {
                    if (char_2 == sentence[i - 1])
                    {
                        if (alpha[sentence[i+1] - 'a'])
                            return ERROR;
                        rule1 = true;
                        char_1 = sentence[i+1];
                    }
                    else 
                    {
                        return ERROR;   
                    }
                }
            }
            // lower
            if (islower(sentence[i]))
            {
                rule2 = false;
                alpha[char_2 - 'a'] = true;
                char_2 = 0;
                words.push_back(word);
                word = "";
            }
        }
        // not rule 1 and not rule2
        else
        {
            //upper
            if (isupper(sentence[i]))
            {
                word += sentence[i];
                rule1 = true;
                
                // 범위 밖이면 걍 끝난거임 
                if (i + 1 == length)
                {
                    rule1 = false;
                    words.push_back(word);
                    word = "";
                }
                // 또 다른 대문자시 또달느 단어의 시작
                else if (isupper(sentence[i+1]))
                {
                    rule1 = false;
                    words.push_back(word);
                    word = "";
                }
                else if (islower(sentence[i+1])) 
                {
                    // 다음 소문자가 사용한거라면 ?
                    if (alpha[sentence[i + 1] - 'a'])
                        return ERROR;

                    char_1 = sentence[i+1];
                    vector<int> pos;
                    for (int j = i + 1; j < length; j++)
                        if (sentence[j] == char_1)
                            pos.push_back(j);
                    // OqA 인경우 O A 이니까 룰 1꺼줄 필용벗다
                    if (pos.size() == 1 || pos.size() > 2)
                        continue;
                    // 2개라면 룰2
                    else 
                    {
                        rule1 = false;
                        char_1 = 0;
                        words.push_back(word);
                        word = "";
                    }
                }
            }
            // lower
            if (islower(sentence[i]))
            {
                if (alpha[sentence[i]-'a'])
                    return ERROR;
                if (i + 1 == length)
                    return ERROR;
                else if (islower(sentence[i+1]))
                    return ERROR;
                rule2 = true;
                char_2 = sentence[i];
            }
        }
    }
    for (int i =0; i < words.size(); i++)
        answer += (words[i] + " ");
    answer.pop_back();
    
    
    return answer;
}
	
```
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
#include <string>

using namespace std;

bool isPattern(char c) {
    return 'a' <= c && c <= 'z';
}

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
string solution(string sentence) {
    string answer = "";
    // 특수문자 정리
    for(int start = 0; start < sentence.length(); start++) {
        // 첫 특수문자 발견
        char s = sentence[start];
        if(isPattern(s)) {
            int end;
            int count = 1;
            
            sentence.erase(start, 1);
            if(isPattern(sentence[start])) return "invalid"; // 첫 글자 직후 패턴
            
            // 나머지 특수문자 정리
            bool single_word = true;
            for(int j = start+1; j < sentence.length(); j++) {
                if(sentence[j] == s) {
                    if(!single_word) return "invalid";  // 중간에 띄어쓰기
                    if(count >= 2 && isPattern(sentence[j+1])) return "invalid";    // 규칙1 패턴뒤에 패턴글자
                    count++;
                    sentence.erase(j, 1);
                    end = j;
                    j--;
                }
                if(sentence[j] == ' ') {
                    single_word = false;
                }
            }
            
            
            // 공백 만들기
            if(count == 1) {
                return "invalid";
            }
            if(count == 2) {
                if(start-2 >= 0 && end+1 < sentence.length() && sentence[start-2] == ' ' && sentence[end+1] == ' ') {
                    
                } else {
                    sentence.insert(start, " ");
                    sentence.insert(end+1, " ");
                }
            } else if(count > 2) {
                if(sentence[start-1] == ' ') return "invalid";
                if(start == 0 || end == sentence.length() ||
                  sentence[start-1] == ' ' || sentence[end] == ' ') return "invalid";
                if(end+1 < sentence.length()) {
                    sentence.insert(start-1, " ");
                    sentence.insert(end+2, " ");
                }
            }
        }
    }
    
    // 띄어쓰기 정리
    for(int i = 0; i < sentence.length(); i++) {
        if(sentence[i] == ' ') {
            for(int j = i+1; j < sentence.length() && sentence[j] == ' '; j++) {
                sentence.erase(j, 1);
                j--;
            }
        }
    }
    if(sentence[0] == ' ') {
        sentence.erase(0, 1);
    }
    if(sentence[sentence.length()-1] == ' ') {
        sentence.erase(sentence.length()-1, 1);
    }
    return sentence;
}
```

</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
  
	
``` cpp
	
테스트 1 〉	통과 (6.19ms, 4.15MB)
	
#include <string>
#include <vector>
#include <iostream>
#include <set>

using namespace std;

set<char> dup;

bool isUpper(char c){
    if('A'<=c && c<='Z') return true;
    return false;
}
bool isLower(char c){
    if('a'<=c && c<='z') return true;
    return false;
}

int check1(string str){
    char myAlpha = str[1];
    if(isUpper(str[0]) && isLower(str[1])){
        //문장의 마지막 글자가 타겟으로한 소문자이면 조건1X
        if(str.size() == str.rfind(myAlpha)+1) return -1;
        int count = 0;
        for(int i = 1; i<str.rfind(str[1])+1; i+=2){
            if(str[i] != myAlpha) return -1;
            count++;
        }
        //소문자가 2개만 존재할경우 조건 1/2 모두 가능하므로 이경우에는 조건2만 검사하기위해 조건1은 X인 것으로 취급
        if(count == 2)return -1;
        if(dup.find(myAlpha) != dup.end()) return -1;
        return str.rfind(myAlpha);
    }
    return -1;
    
}
int check2(string str){
    if(isLower(str[0]) && isLower(str[1])) return -1;
    if(isLower(str[0])){
        if(str.find(str[0],1) != str.rfind(str[0])) return -1;
        if(dup.find(str[0]) != dup.end()) return -1;
        dup.insert(str[0]);
        return str.find(str[0],1);
    }
    return -1;
}

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
string solution(string sentence) {
    string answer = "";
    dup.clear();
    vector<string> list;
    
    string temp = "";
    for(;sentence.size() != 0;){
        
        //각각 조건1,2를 만족하는 마지막 소문자의 index를 return
        int index1 = check1(sentence);
        int index2 = check2(sentence);
        
        //조건 1또는 2를 만족하면 temp에 들어있던 단어는 완성된 단어로 취급
        if(index2 != -1){
            if(temp.size() != 0) {
                list.push_back(temp);
                temp = "";
            }
            //aBcBa 라면 BcB를 리스트에 저장
            list.push_back(sentence.substr(1,index2-1));
            sentence = sentence.substr(index2+1);
        }else if(index1 != -1){
            if(temp.size() != 0) {
                list.push_back(temp);
                temp = "";
            }
            //BcB 라면 BcB를 리스트에 저장
            list.push_back(sentence.substr(0,index1+2));
            sentence = sentence.substr(index1+2);
        }else{
            //조건1을 만족하지 않으면서 첫문자가 대문자면(연속된 대문자) 단어 이므로 임시저장
            //조건2를 만족하지 않는데 첫문자가 소문자이면 올바르지 않는 조건
            if(isUpper(sentence[0])){
                temp += sentence[0];
                sentence = sentence.substr(1);
                continue;
            }
            
            return "invalid";
        }
    }
    
    if(temp.size() != 0) list.push_back(temp);
    //결과적으로 list에는 모두 대문자이거나 조건1을 만족하는 문자열만 들어있어야함
    for(auto it = list.begin(); it != list.end(); it++){
        //2번째문자가 소문자라면 조건1을 만족해야하므로 검사
        char tempAlpha = (*it)[1];
        if(isLower(tempAlpha)){
            if(it->size() %2 == 0)return "invalid";
            if(dup.find(tempAlpha) != dup.end()) return "invalid";
            dup.insert(tempAlpha);
            string tempStr = "";
            for(int i = 0; i<it->size(); i+=2){
                if(isLower((*it)[i])) return "invalid";
                //조건 1 불만족
                if(i+1 < it->size() && tempAlpha != (*it)[i+1]) return "invalid";
                tempStr+=(*it)[i];
            }
            *it = tempStr;
        }else{
        //2번째문자가 소문자가 아니라면 모두 대문자여야하므로 검사
            for(int i = 0; i<it->size(); i++){
                if(isLower((*it)[i])) return "invalid";
            }
        }
        if(it->size() == 0) return "invalid";
        
        answer += *it + " ";
    }
    answer = answer.substr(0,answer.size()-1);
    if(answer == "") return "invalid";
    return answer;
}	
```
</details>
  
문교
<details>
<summary>접기/펼치기 버튼</summary>

</details>
