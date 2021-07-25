
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/1836'> 리틀 프렌즈 사천성 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
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
  
</details>
  
문교
<details>
<summary>접기/펼치기 버튼</summary>

</details>
