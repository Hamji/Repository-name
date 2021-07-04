건희
<details markdown="1">
<summary>접기/펼치기</summary>

```c++
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

int solution(int n, vector<string> data) {
    int answer = 0;
    // 문자열 기본 세팅
    string name = "ACFJMNRT";
    do{
        int count = 0;
        // 조건 
        for (int i = 0; i < n; i++){
            char first_person = data[i][0];
            char second_person = data[i][2];
            char sign = data[i][3];
            int gap = data[i][4] - '0';
            int real_gap = name.find(first_person) - name.find(second_person);
            real_gap = real_gap > 0 ? real_gap : -real_gap;
            real_gap--;
            
            // 해당 조건에 맞지 않다면 순열 컷해버리기 
            if (sign == '=' && real_gap != gap) break;
            else if (sign == '>' && real_gap <= gap) break;
            else if (sign == '<' && real_gap >= gap) break;
            count++;
        }
        // 조건이 다 맞았다면 count == n 일것이고 아니면 안맞는거니까 answer를 증가시키지 않는다.
        if (count == n) answer++;
    }while(next_permutation(name.begin(), name.end()));
    // 순열을 돌면서 조건들 확인
    return answer;
}
```

</details>

정영
<details markdown="1">
<summary>접기/펼치기</summary>

```c++
// 19:55 ~ 21:05 + 10:12 ~ 10:50 + 17:45 ~ 18:15 (+ 2h)
#include <string>
#include <vector>
#include <list>
#include <algorithm>

#define MAX_SIZE 8

using namespace std;

vector<string> datas;
int answer;
int seat[8]; // 해당 위치에 누가 있는지 표시
int people[8];  // 해당 사람이 어디 있는지 표시

int parseInt(int n) {
    switch(n) {
        case 0: return 'A';
        case 1: return 'C';
        case 2: return 'F';
        case 3: return 'J';
        case 4: return 'M';
        case 5: return 'N';
        case 6: return 'R';
        case 7: return 'T';
        default: return '-';
    }
}
int parseName(char c) {
    switch(c) {
        case 'A': return 0;
        case 'C': return 1;
        case 'F': return 2;
        case 'J': return 3;
        case 'M': return 4;
        case 'N': return 5;
        case 'R': return 6;
        case 'T': return 7;
    }
}
void pushPeople(int pos, int name) {
    seat[pos] = name;
    people[name] = pos;
}
void popPeople(int pos, int name) {
    seat[pos] = -1;
    people[name] = -1;
}
bool isSeatable(string data, int first, int second, int pos) {
    int range = data[4] - '0';
    if(data[3] == '=') {
        return people[first]-range-1 == pos || people[first]+range+1 == pos;
    } else if(data[3] == '<') {
        return people[first]-range <= pos && pos <= people[first]+range;
    } else {
        return pos < people[first]-range-1 || people[first]+range+1 < pos;
    }
}
void dfs(int n) {
    if(n==-1) { // 탈출조건
        answer += 1;
        return;
    } else {    // A, B 둘 다 이미 자리를 잡았는지 확인
        int first = parseName(datas[n][0]);
        int second = parseName(datas[n][2]);
        if(people[first] == -1 && people[second] == -1) {   // 둘 다 안잡은 경우 - A 배치 후 B 배치, B 배치 후 A 배치
            for(int f = 0; f < MAX_SIZE; f++) {
                if(seat[f] != -1) continue;     // 해당 자리가 비어있지 않으면 넘어감
                pushPeople(f, first);           // f자리에 first를 배치
                for(int s = 0; s < MAX_SIZE; s++) {
                    if(seat[s] != -1) continue; // 해당 자리가 비어있지 않으면 넘어감
                    if(!isSeatable(datas[n], first, second, s)) continue;   // 조건에 맞지 않으면 넘어감
                    pushPeople(s, second);  // s자리에 second를 배치
                    dfs(n-1);
                    popPeople(s, second);   // s자리에 second를 제거
                }
                popPeople(f, first);            // f자리에 first를 제거
            }
        } else if(people[first] != -1 && people[second] != -1) { // 둘 다 자리를 잡은 경우
            int second = parseName(datas[n][2]);
            if(isSeatable(datas[n], first, second, people[second])) dfs(n-1);  // 조건에 맞으면 다음 레벨로 내려감
        } else { // 한 명은 자리를 잡은 경우 - 나머지 한 명만 배치
            // 누가 이미 자리를 차지했는지 확인해야함.
            int unseat = people[first] == -1 ? parseName(datas[n][0]) : parseName(datas[n][2]);
            int seated = people[first] != -1 ? parseName(datas[n][0]) : parseName(datas[n][2]);
            
            for(int p = 0; p < MAX_SIZE; p++) {
                if(seat[p] != -1) continue;  // 해당 자리가 비어있지 않으면 넘어감
                if(!isSeatable(datas[n], seated, unseat, p)) continue; // 조건에 맞지 않으면 넘어감
                pushPeople(p, unseat); // p자리에 unseat를 배치
                dfs(n-1);
                popPeople(p, unseat); // p자리에 unseat를 제거
            }
        }
    }
}

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
int solution(int n, vector<string> data) {
    answer = 0;
    datas = data;
    fill_n(seat, MAX_SIZE, -1);
    fill_n(people, MAX_SIZE, -1);
    dfs(n-1);
    bool batch[8] = {false, };
    int count = 0, factorial = 1;
    // 팩토리얼(배치된적 없는 사람 수) 값을 총 경우의 수에 곱해준다.
    for(auto d : datas) { // 배치된 사람 수
        int first = parseName(d[0]);
        int second = parseName(d[2]);
        if(batch[first] == false) {
            batch[first] = true;
            count++;
        }
        if(batch[second] == false) {
            batch[second] = true;
            count++;
        }
    }
    for(int i = MAX_SIZE-count; i > 0; i--) {   // 배치되지 않은 사람의 팩토리얼
        factorial *= i;
    }
    return answer * factorial;
}
```
</details>

건률
<details markdown="1">
<summary>접기/펼치기</summary>

```c++
#include <string>
#include <algorithm>
#include <vector>
#include <iostream>

using namespace std;

bool checkCondition(vector<char>& data, vector<string>& condition){
    for(auto it = condition.begin(); it != condition.end(); it++){
        int start = find(data.begin(), data.end(), (*it)[0]) - data.begin();
        int end = find(data.begin(), data.end(), (*it)[2]) - data.begin();
        
        if((*it)[3] == '='){
            if(!(abs(start-end) == ((*it)[4] - '0' + 1))) return false;
        }
        else if((*it)[3] == '>'){
            if(!(abs(start-end) > ((*it)[4] - '0' + 1))) return false;
        }
        else if((*it)[3] == '<'){
            if(!(abs(start-end) < ((*it)[4] - '0' + 1))) return false;
        }
        
    }
    
    return true;
}

int solution(int n, vector<string> data) {
    int answer = 0;
    
    vector<char> v = {'A','C','F','J','M','N','R','T'};
    
    
    do {
        if(checkCondition(v,data))
            answer++;
    } while (next_permutation(v.begin(), v.end()));

    return answer;
}
```

</details>

문교

<details markdown="1">
<summary>접기/펼치기</summary>

```

	int answer = 0;

	map<char, int> posMap;
	vector<char> people = { 'A', 'C', 'F', 'J', 'M', 'N', 'R', 'T' };

	do {
		for (int i = 0; i < (int)people.size(); ++i)
		{
			posMap[people[i]] = i;
		}

		bool bisSatisfied = true;

		for (string& s : data)
		{
			char src = s[0];
			char dest = s[2];

			int srcPos = posMap[src];
			int destPos = posMap[dest];

			int diffStep = abs(destPos - srcPos) - 1;

			int operValue = s[4] - '0';

			switch (s[3])
			{
			case '=':
			{
				bisSatisfied = diffStep == 0;
			} break;

			case '<':
			{
				bisSatisfied = diffStep < operValue;
			} break;

			case '>':
			{
				bisSatisfied = diffStep > operValue;
			} break;
			default:
				cout << "?????" << endl;
			}

			if (!bisSatisfied)
				break;
		}

		if (bisSatisfied)
		{
			++answer;
		}

	} while (next_permutation(people.begin(), people.end()));

	return answer;
    
```

</details>
