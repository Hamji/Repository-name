
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/1836'> 리틀 프렌즈 사천성 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
테스트 1 〉	통과 (20.81ms, 4.01MB)
	
#include <string>
#include <vector>
#include <algorithm>

#define ALPHABET_SIZE 26

using namespace std;

struct Tileset {
    bool empty = true;
    vector<pair<int, int>> pos;
    pair<string, string> path_tile;
};

string FindRowTile(vector<string> board, int row, int start, int end) {
    string tiles;
    for(int col = start; col < end; col++) {
        char tile = board[row][col];
        if(tile == '*') {
            return "*";
        } else if(tile != '.') {
            tiles += tile;
        }
    }
    return tiles;
}

string FindColTile(vector<string> board, int col, int start, int end) {
    string tiles;
    for(int row = start; row < end; row++) {
        char tile = board[row][col];
        if(tile == '*') {
            return "*";
        } else if(tile != '.') {
            tiles += tile;
        }
    }
    return tiles;
}

pair<string, string> FindPathTile(vector<string> board, pair<int, int> p1, pair<int, int> p2) {
    // 같은 가로 라인일때
    if(p1.first == p2.first) {
        return {FindRowTile(board, p1.first, p1.second+1, p2.second), "*"};
    }
    
    // 같은 세로 라인일때
    if(p1.second == p2.second) {
        return {"*", FindColTile(board, p1.second, p1.first+1, p2.first)};
    }
    
    string first, second;
    if(p1.second < p2.second) { // 좌상단
        first = FindRowTile(board, p1.first, p1.second+1, p2.second);
        second = FindRowTile(board, p2.first, p1.second+1, p2.second);
    } else if(p1.second > p2.second) { // 우상단
        first = FindRowTile(board, p1.first, p2.second+1, p1.second);
        second = FindRowTile(board, p2.first, p2.second+1, p1.second+1);
    }
    if(first != "*") {
        first = FindColTile(board, p2.second, p1.first, p2.first) + first;
    }
    if(second != "*") {
        second = FindColTile(board, p1.second, p1.first+1, p2.first+1) + second;
    }
    
    return {first, second};
}

void RemoveChar(string &s, char c) {
    auto it = find(s.begin(), s.end(), c);
    if(it != s.end()) {
        s.erase(it);
    }
}

void RemoveTileFromPaths(vector<Tileset> &tileset, char tile_name) {
    for(int i = 0; i < ALPHABET_SIZE; i++) {
        if(tileset[i].empty) continue;
        RemoveChar(tileset[i].path_tile.first, tile_name);
        RemoveChar(tileset[i].path_tile.first, tile_name);
        RemoveChar(tileset[i].path_tile.second, tile_name);
        RemoveChar(tileset[i].path_tile.second, tile_name);
    }
}

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
string solution(int m, int n, vector<string> board) {
    string answer = "";
    vector<Tileset> tileset(ALPHABET_SIZE);
    
    // 두 타일의 위치 정보 정리
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            char tile = board[i][j];
            if(tile != '.' && tile != '*') {
                tileset[tile - 'A'].pos.push_back({i, j});
                tileset[tile - 'A'].empty = false;
            }
        }
    }
    
    // 두 타일 사이의 경로 정리
    for(int i = 0; i < ALPHABET_SIZE; i++) {
        if(tileset[i].empty) continue;
        tileset[i].path_tile = FindPathTile(board, tileset[i].pos[0], tileset[i].pos[1]);
        if(tileset[i].path_tile.first[0] == '*' &&
           tileset[i].path_tile.second[0] == '*') {
            return "IMPOSSIBLE";
        }
    }
    
    bool removed = true;
    while(removed) {
        removed = false;
        // 제거 할 수 있는 타일부터 알파벳 순으로 제거
        for(int i = 0; i < ALPHABET_SIZE; i++) {
            if(tileset[i].empty) continue;
            if(tileset[i].path_tile.first == "" ||
               tileset[i].path_tile.second == "") {
                removed = true;
                tileset[i].empty = true;
                char tile_name = 'A' + i;
                answer += tile_name;
                // 제거 후 모든 경로에 대해 재정리
                RemoveTileFromPaths(tileset, tile_name);
                break;
            }
        }
    }
    
    // 남은 타일이 있는지 확인
    bool possible = true;
    for(auto t : tileset) {
        if(t.empty == false) {
            possible = false;
        }
    }
    
    if(possible) return answer;
    else return "IMPOSSIBLE";
}
```

</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>

	
``` cpp
	
테스트 1 〉	통과 (95.55ms, 3.99MB)
	
#include <string>
#include <vector>
#include <map>
#include <set>
#include <iostream>

using namespace std;
map<char,bool> ableMap;
set<char> impossibleSet;
int maxY;
int maxX;
string result = "";


void checkLine(int y1,int x1,int y2,int x2,vector<string> &board){
    auto it = ableMap.find(board[y1][x1]);
    if(it != ableMap.end() && it->second == false){
        impossibleSet.insert(it->first);
        ableMap.erase(it);
        return;
    }else 
    if(it != ableMap.end() && it->second == true){
        impossibleSet.erase(it->first);
        return;
    }
    int diffX = x2-x1;
    int diffY = y2-y1;
    int tempY = 0;
    int tempX = 0;
    
    int directX = diffX > 0 ? 1 : -1;
    int directY = diffY > 0 ? 1 : -1;
    
    if(y1 == y2){
        for(int i = x1; i != x2+directX; i+= directX){
            if(board[y1][i] != '.' && board[y1][i] != board[y1][x1]) {
                ableMap[board[y1][x1]] = false;
                return;
            }
        }
        ableMap[board[y1][x1]] = true;
        return;
        
    }else if(x1 == x2){
        for(int i = y1; i != y2+directY; i+= directY){
            if(board[i][x1] != '.' && board[i][x1] != board[y1][x1]) {
                ableMap[board[y1][x1]] = false;
                return;
            }
        }
        
        ableMap[board[y1][x1]] = true;
        return;
    }else{
        bool bre = false;
        for(int i = x1; i != x2+directX; i+= directX){
            if(board[y1][i] != '.' && board[y1][i] != board[y1][x1]) {
                ableMap[board[y1][x1]] = false;
                bre = true;
                break;
            }
            tempX = i;
        }
        
        for(int i = y1; i != y2+directY; i+= directY){
            if(bre) break;
            if(board[i][tempX] != '.' && board[i][tempX] != board[y1][x1]) {
                ableMap[board[y1][x1]] = false;
                break;
            }
            tempY = i;
        }
        if(tempX == x2 && tempY == y2){
            ableMap[board[y1][x1]] = true;
            return;
        }
        
        
        for(int i = y1; i != y2+directY; i+= directY){
            if(board[i][x1] != '.' && board[i][x1] != board[y1][x1]) {
                ableMap[board[y1][x1]] = false;
                return;
            }
            tempY = i;
        }
        for(int i = x1; i != x2+directX; i+= directX){
            if(board[tempY][i] != '.' && board[tempY][i] != board[y1][x1]) {
                ableMap[board[y1][x1]] = false;
                return;
            }
        }
        
        ableMap[board[y1][x1]] = true;
        return;
    }
    
}


void findPair(int y,int x,vector<string> &board){
    if(board[y][x] == '*' || board[y][x] == '.') return;
    for(int i = 0; i<maxY; i++){
        for(int j = 0; j<maxX; j++){
            if(x == j && y == i) continue;
            if(board[y][x] == board[i][j]){
                checkLine(y,x,i,j,board);
                return;
            }
            
        }
    }
}

bool mapChack(vector<string> &board){
    //map은 기본적으로 key값이 정렬이 되어있기때문에 순차적으로 찾으면 알파벳 순서로 찾을 수 있음.
    for(auto it = ableMap.begin(); it != ableMap.end(); it++){
        //지울수 있는 블럭을 찾으면 해당 부분을 비우고 결과에 해당 알파벳 추가
        if(it->second == true) {
            for(int i = 0; i<maxY; i++){
                for(int j = 0; j<maxX; j++){
                    if(board[i][j] == it->first){
                        board[i][j] = '.';
                    }
                }
            }
            result += it->first;
            // cout<<it->first<<" ";
            ableMap.erase(it);
            return true;
        }
    }
    return false;
}
void printMap(vector<string> &board){
    for(auto it = ableMap.begin(); it!=ableMap.end(); it++){
        cout<<it->first << " "<<it->second << endl; 
    }
    
    for(int i = 0; i<maxY; i++){
        for(int j = 0; j<maxX; j++){
            cout<<board[i][j]<<" ";
        }
        cout<<endl;
    }
    cout<<endl;
}

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
string solution(int m, int n, vector<string> board) {
    maxY = m;
    maxX = n;
    ableMap.clear();
    impossibleSet.clear();
    result = "";

    do{
        for(int i = 0; i<m; i++){
            for(int j = 0; j<n; j++){
                //지울수 있는 블럭이 있는지 찾기
                findPair(i,j,board);
            }
        }
        // printMap(board);
        //지울수 잇는 블럭이 있으면 하나 지우고 반복
    }while(mapChack(board));
    
    //set에 요소가 남아있으면 짝이 안맞는 블럭이 존재한다는 뜻이므로 불가능
    if(!impossibleSet.empty()) result = "IMPOSSIBLE";
    // printMap(board);
    return result;
}
```	
	
	
</details>
  
문교
<details>
<summary>접기/펼치기 버튼</summary>

</details>
