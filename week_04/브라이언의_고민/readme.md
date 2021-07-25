
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
                    if (pos.size() == 1)
                        continue;
                    // 다음에 나올 소문자가 3개 이상인 경우...\
                    // AoAoAAAAoBB 인경우 1도아니고 2도아니긴한데 여기서 는 일단 rule 1로 봐주자구
                    // 2개도 하나로 할경우 뭔가 복잡하다
                    else if (pos.size() > 2)
                    {
                        continue;
                    }
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


</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
  
</details>
  
문교
<details>
<summary>접기/펼치기 버튼</summary>

</details>
