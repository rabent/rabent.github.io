---
title: "(KAKAO INTERN) 키패드 누르기"
layout: post
date: 2022-06-14 13:50
tag:
- KAKAO
- 코테
- c++
description: 카카오 인턴채용 코딩테스트 문제
---

# 문제 설명

스마트폰 전화 키패드의 각 칸에 다음과 같이 숫자들이 적혀 있습니다.

![키패드.jpg](/assets/img/%ED%82%A4%ED%8C%A8%EB%93%9C%20%EB%88%84%EB%A5%B4%EA%B8%B0.png)

이 전화 키패드에서 왼손과 오른손의 엄지손가락만을 이용해서 숫자만을 입력하려고 합니다.
맨 처음 왼손 엄지손가락은 * 키패드에 오른손 엄지손가락은 # 키패드 위치에서 시작하며, 엄지손가락을 사용하는 규칙은 다음과 같습니다.

1. 엄지손가락은 상하좌우 4가지 방향으로만 이동할 수 있으며 키패드 이동 한 칸은 거리로 1에 해당합니다.
2. 왼쪽 열의 3개의 숫자 1, 4, 7을 입력할 때는 왼손 엄지손가락을 사용합니다.
3. 오른쪽 열의 3개의 숫자 3, 6, 9를 입력할 때는 오른손 엄지손가락을 사용합니다.
4. 가운데 열의 4개의 숫자 2, 5, 8, 0을 입력할 때는 두 엄지손가락의 현재 키패드의 위치에서 더 가까운 엄지손가락을 사용합니다.
  4-1. 만약 두 엄지손가락의 거리가 같다면, 오른손잡이는 오른손 엄지손가락, 왼손잡이는 왼손 엄지손가락을 사용합니다.

순서대로 누를 번호가 담긴 배열 numbers, 왼손잡이인지 오른손잡이인 지를 나타내는 문자열 hand가 매개변수로 주어질 때, 각 번호를 누른 엄지손가락이 왼손인 지 오른손인 지를 나타내는 연속된 문자열 형태로 return 하도록 solution 함수를 완성해주세요.

## [제한사항]

+ numbers 배열의 크기는 1 이상 1,000 이하입니다.
+ numbers 배열 원소의 값은 0 이상 9 이하인 정수입니다.
+ hand는 "left" 또는 "right" 입니다.
  +"left"는 왼손잡이, "right"는 오른손잡이를 의미합니다.
+ 왼손 엄지손가락을 사용한 경우는 L, 오른손 엄지손가락을 사용한 경우는 R을 순서대로 이어붙여 문자열 형태로 return 해주세요.

## 입출력 예

numbers | hand | result 
--- | --- | ---
[1, 3, 4, 5, 8, 2, 1, 4, 5, 9, 5] | "right" | "LRLLLRLLRRL"
[7, 0, 8, 2, 8, 3, 1, 5, 7, 6, 2] | "left" | "LRLLRRLLLRR"
[1, 2, 3, 4, 5, 6, 7, 8, 9, 0] | "right" | "LLRLLRLLRL"

## 입출력 예에 대한 설명

입출력 예 #1

순서대로 눌러야 할 번호가 [1, 3, 4, 5, 8, 2, 1, 4, 5, 9, 5]이고, 오른손잡이입니다.

왼손 위치 | 오른손 위치 | 눌러야 할 숫자 | 사용한 손 | 설명
--- | --- | --- | --- | ---
\* | # |	1 |	L |	1은 왼손으로 누릅니다.
1 |	# |	3 |	R |	3은 오른손으로 누릅니다.
1 |	3 |	4 |	L |	4는 왼손으로 누릅니다.
4 |	3 |	5 |	L |	왼손 거리는 1, 오른손 거리는 2이므로 왼손으로 5를 누릅니다.
5 |	3 |	8 |	L |	왼손 거리는 1, 오른손 거리는 3이므로 왼손으로 8을 누릅니다.
8 |	3 |	2 |	R |	왼손 거리는 2, 오른손 거리는 1이므로 오른손으로 2를 누릅니다.
8 |	2 |	1 |	L |	1은 왼손으로 누릅니다.
1 |	2 |	4 |	L |	4는 왼손으로 누릅니다.
4 |	2 |	5 |	R |	왼손 거리와 오른손 거리가 1로 같으므로, 오른손으로 5를 누릅니다.
4 |	5 |	9 |	R |	9는 오른손으로 누릅니다.
4 |	9 |	5 |	L |	왼손 거리는 1, 오른손 거리는 2이므로 왼손으로 5를 누릅니다.
5 |	9 |	- |	- |	

따라서 "LRLLLRLLRRL"를 return 합니다.

입출력 예 #2

왼손잡이가 [7, 0, 8, 2, 8, 3, 1, 5, 7, 6, 2]를 순서대로 누르면 사용한 손은 "LRLLRRLLLRR"이 됩니다.

입출력 예 #3

오른손잡이가 [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]를 순서대로 누르면 사용한 손은 "LLRLLRLLRL"이 됩니다.

# 문제 풀이

각 키패드의 위치를 1부터 *=10 0=11 #=12로 두고 번호를 매겨두고 [13][13]크기의 2차원배열을 만들어 붙어있는 숫자끼리는 1, 붙어있지 않다면 0을 넣었다. 현재 손가락의 위치를 전역변수 curright, curleft로 지정하였고 가운데 키패드를 누를땐 bfs로 손가락부터 키까지의 거리를 계산하여 가까운 손가락이 누르도록 하였다. 붙어있는 키패드와 붙어있지 않은 키패드를 구별하는 것, 현재 손가락에서 가운데 키패드까지의 거리를 계산하는 것 두 가지만 유의하면 큰 어려움은 없는 문제였다.

# 코드

{% highlight c++ %}
#include <string>
#include <vector>
#include <queue>

using namespace std;

int curleft; //왼손가락 현재 위치
int curright; //오른손가락 현재 위치
int map[13][13]; //붙어있는 키패드=1 붙어있지 않다면 0

int bfs(int cur, int x) { //현재 손가락 위치부터 키패드까지의 거리 재는 함수
    if(x==0) x=11;
    if(cur==0) cur=11; //키가 0이라면 11로 바꿔준다
    int visited[13]={0,};
    int count[13]={0,};
    queue<int> q;
    q.push(cur);
    while(!q.empty()) {
        int temp=q.front();
        q.pop(); visited[temp]=1;
        if(temp==x) break;
        for(int i=1; i<=12; i++) {
            if(visited[i]==0&&map[temp][i]==1) { //방문한 적 없고 붙어있다면
                q.push(i); count[i]=count[temp]+1;
            }
        }
    }
    return count[x]; //거리를 세서 리턴
}

string solution(vector<int> numbers, string hand) {
    for(int i=0; i<13; i++) {
        for(int j=0; j<13; j++) {
            map[i][j]=0;
        }
    }
    for(int i=1; i+3<=12; i++) {
        map[i][i+3]=1; map[i+3][i]=1;
        if(i%3==1||i%3==2) {map[i][i+1]=1; map[i+1][i]=1;} 
    }
    map[10][11]=1; map[11][10]=1; map[11][12]=1; map[12][11]=1; //붙어있는 키들은 1
    string answer="";
    curleft=10; curright=12; //처음 시작위치
    vector<int>::iterator iter;
    for(iter=numbers.begin(); iter!=numbers.end(); iter++) { //예전에 짠 코드라 iterator를 썼지만 지금이라면 for(int n : numbers)로 간단하게 짰을듯 싶다
        if(*iter==1||*iter==4||*iter==7) {
          answer.push_back('L'); curleft=*iter;
        }
        else if(*iter==3||*iter==6||*iter==9) {
            answer.push_back('R'); curright=*iter;
        } //왼쪽 키는 왼손 오른쪽 키는 오른손
        else {
            int left=bfs(curleft, *iter); int right=bfs(curright, *iter); //왼손가락 오른손가락부터의 거리
            if(left>right) {
                answer.push_back('R'); curright=*iter;
            }
            else if(right>left) {
                answer.push_back('L'); curleft=*iter;
            }
            else if(right==left) {
                if(hand=="left") {answer.push_back('L'); curleft=*iter;}
                else {answer.push_back('R'); curright=*iter;}
            }
        }
    }
    return answer;
}
{% endhighlight %}