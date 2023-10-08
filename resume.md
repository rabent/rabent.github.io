---
title: PORTFOLIO
permalink: /resume/
layout: page
excerpt: 
comments: false
---

![emoji](/assets/img/1f468-1f4bb.png){: width="50" height="50"}
# **이도준 | Game Client Programmer**


## Contact & Blog
---
+ Email | rabent0207@gmail.com
+ Blog | rabent.github.io

## 목차
---
* [경력](#경력)  
* [기술 스택](#기술-스택)  
* [게임 출시 경험](#게임-출시-경험)  
* [프로젝트 경험](#프로젝트-경험)  
* [코드](#코드)   


## 경력
---
+ 홍익대학교 컴퓨터공학과 재학  (2017~)
+ 대학교 내 게임 제작 동아리 EXP에서 서브프로그래머로 프로젝트 참여
+ 대학교 졸업 프로젝트 '젬스톤 서바이버'의 기획, 메인프로그래머로 총괄

## 기술 스택
---
- Unity Engine
- C++
- C#

## 게임 출시 경험
---
홍익대학교 내 게임 및 게임 개발 동아리인 EXP에서 반년간 프로젝트에 참여하여 구글 플레이 스토어에 출시까지 성공했던 경험이 있습니다.  
프로젝트는 기존 보드게임인 오목에 카드와 마법 요소를 추가한 'Oh-Mok!'으로 링크로 들어가시면 관련 코드와 경험에 대해 서술한 글로 이동하여 보실 수 있습니다.  
첫 프로젝트였던만큼 게임 개발이 어떻게 이루어지는지, 유니티와 C#의 기초와 응용 등 정말 많은 것을 배울 수 있던 프로젝트였습니다.

[클릭 시](https://rabent.github.io/Oh-Mok!/) 블로그 내 포스팅으로 이동합니다.
[Github 링크](https://github.com/nilbace/Oh-MOK)  
[Play store 링크](https://play.google.com/store/apps/details?id=com.ExPStudio.magical)

## 프로젝트 경험
---
현재 대학교 졸업프로젝트로 핵앤슬래쉬 로그라이크 게임인 '젬스톤 서바이버'의 기획 및 메인 프로그래머로써 프로젝트를 진행하고 있습니다.  
'젬스톤 서바이버'는 평소 즐겨하던 게임인 'Path of Exile'과 '뱀파이어 서바이버'의 장점을 섞고 서로의 단점을 상쇄한 작품을 만들고 싶다는 생각으로 시작한 프로젝트입니다.  
'Path of Exile'의 젬 조합 시스템은 정말 깊이가 있으면서도 가능성이 많은 훌륭한 시스템이라 생각했지만 게임 자체의 진입장벽이 높아 하는 사람만 하는 게임이 된 것이 아까웠습니다.  
어느날 스팀에서 레트로한 스타일로 AAA게임들을 제치고 메가히트를 기록한 '뱀파이어 서바이벌'을 보며 '뱀파이어 서바이벌'의 라이트함과 'Path of Exile'의 깊이있는 시스템을 조합하고 싶어 만들게 되었습니다.  
아트팀 없이 유튜브로 공부하며 만든 게임이라 많은 어려움이 있었지만 어느정도 완성되어가는 중입니다.  
이 프로젝트를 통해 게임 제작에 있어서 아트팀과 기획팀의 중요성, 게임을 아무것도 없는 처음부터 만들어가는 감각과 툴의 경험 등 많은 것을 배울 수 있었습니다.

## 코드
---

<details>
<summary>접기/펼치기</summary>
<div markdown="1">

{% highlight c# %}

void Update() {
        if(timeron) {
            time+=Time.deltaTime; //time이란 int변수에 각 턴의 지나간 시간을 저장
            if(time>=30) {
                if(isMyTurn) endMyTurn(); //시간이 30초를 지나면 (자기턴일때) 턴을 종료
            }
        }
    }

[PunRPC] void startMyTurn()
    {
        isMyTurn = true;
        canuseCard = true;  // 카드를 사용할 수 있게 함
        timeron=true;
        for (int i = 0; i < 81; i++)
        {
            if (gomokuData[i] == 0)   // 아직 돌을 두지 않은 부분만 클릭할 수 있게 함
                gomokuTable[i].interactable = true;
        }
        PV.RPC("timermake", RpcTarget.AllBuffered); //두 클라이언트 양쪽에 모두 'timermake' 함수를 실행시킴
        NetWorkManager.instance.printScreenString("나의 턴");  // '나의 턴' 출력
    }

[PunRPC] void timermake() {
    if(timerins!=null) Destroy(timerins); //만약 타이머가 이미 있다면 파괴함
    if(isMyTurn) {
        timerins=Instantiate(timer, new Vector3(-150,-550,10), Quaternion.identity); // 자기쪽 위치
        timerins.transform.SetParent(this.transform.parent.transform,false); //timer는 unity UI의 fill image 기능을 사용하기에 캔버스 내부 오브젝트의 자식으로 만들어줌
    }
    else {
        timerins=Instantiate(timer, new Vector3(-400,830,10), Quaternion.identity); //상대쪽 위치
        timerins.transform.SetParent(this.transform.parent.transform,false);
    }
    time=0; //시간 초기화
}

{% endhighlight %}

</div>
</details>

![timer.gif](/assets/img/timer.gif)

*Unity UI의 fill image 기능을 사용하여 시계바늘이 회전하여 지나간 자리는 빨간색으로 채워주는 타이머를 구현하여 각 턴의 제한시간을 볼 수 있게 하였습니다. (녹화 프로그램 상의 문제로 빨간색이 깨져나옴) 기획 쪽의 의견으로 타이머의 위치를 자신의 턴일 때는 자신 캐릭터 옆에, 상대 턴일땐 상대 캐릭터 옆에 생성시키도록 하였습니다.*

