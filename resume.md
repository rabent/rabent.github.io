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

### Oh-Mok! 주요 구현

<details>
<summary>UI타이머 구현</summary>
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

*Unity UI의 fill image 기능을 사용하여 시계바늘이 회전하여 지나간 자리는 빨간색으로 채워주는 타이머를 구현하여 각 턴의 제한시간을 볼 수 있게 하였습니다.  
(녹화 프로그램 상의 문제로 빨간색이 깨져나옴)  
기획 쪽의 의견으로 타이머의 위치를 자신의 턴일 때는 자신 캐릭터 옆에, 상대 턴일땐 상대 캐릭터 옆에 생성시키도록 하였습니다.*

<details>
<summary>Dotween을 이용한 애니메이션</summary>
<div markdown="1">
{% highlight c# %}
void dolmove(Image img) { //돌 5개가 모이면 가운데 돌로 돌들이 이동하는 애니메이션
    Vector3 tmp=img.transform.position;
    Sequence seq=DOTween.Sequence();
    seq.Join(img.transform.DOMove(charging.center,0.75f));
    seq.Join(img.transform.DOScale(new Vector3(0,0,0),3f));
    seq.Join(img.DOFade(0, 2f).SetEase(Ease.InQuad));
    seq.Append(img.transform.DOMove(tmp,0));
    seq.Join(img.transform.DOScale(new Vector3(1,1,1),0));
}
{% endhighlight %}
</div>
</details>

<details>
<summary>Photon 서버를 통해 결과를 구분</summary>
<div markdown="1">
{% highlight c# %}
if(PhotonNetwork.IsMasterClient)  // 검은 돌이 오목을 완성한 경우. 내가 MasterClient이면 내가 검은 돌을 두는 사람이므로 내가 공격에 성공한 것임 → 상대방 HP를 깎음
    {
        StartCoroutine(enemyshoot()); //충돌 시 폭발하는 파티클 투사체를 상대 캐릭터를 향해 발사함
        PlayerManager.enemyPlayerManager.GetDamaged();
    }
    else
    {
        StartCoroutine(myshoot()); //투사체를 내 캐릭터를 향해 발사함
        PlayerManager.myPlayerManager.GetDamaged();
    }
{% endhighlight %}
</div>
</details>

<details>
<summary>Unity의 particle 시스템을 사용한 구현</summary>
<div markdown="1">
{% highlight c# %}
using System.Collections;
using UnityEngine;
[RequireComponent(typeof(ParticleSystem))]
public class charging : MonoBehaviour {
	ParticleSystem ps;
	ParticleSystem.Particle[] m_Particles;
	public static Vector3 center;
	float speed = 5f;
	int numParticlesAlive;
	void Start () {
		ps = GetComponent<ParticleSystem>();
		if (!GetComponent<Transform>()){
			GetComponent<Transform>();
		}
	}
	void Update () {
		m_Particles = new ParticleSystem.Particle[ps.main.maxParticles];
		numParticlesAlive = ps.GetParticles(m_Particles);
		float step = speed * Time.deltaTime;
		for (int i = 0; i < numParticlesAlive; i++) {
			m_Particles[i].position = Vector3.LerpUnclamped(m_Particles[i].position, center, step);
		}
		ps.SetParticles(m_Particles, numParticlesAlive);
	}
}
{% endhighlight %}
</div>
</details>

![particle.gif](/assets/img/part.gif)
*오목 게임인 만큼 돌 5개가 이어지게 만들면 5개가 완성됬음을 알려주는 이펙트와 함께 상대를 타격하여 데미지를 주는 이펙트가 필요했습니다.  
Unity의 인기 에셋인 Dotween을 사용하여 돌 5개가 이어졌을 때 돌들이 가운데 돌로 모이는 애니메이션을 제작하였고  
Unity의 Particle system을 사용하여 각 지점에서 생성된 입자들이 한 점으로 모이는 애니메이션을 제작하여 돌이 가운데로 입자와 함께 모이는 애니메이션,  
그리고 애니메이션이 끝나면 상대 초상화로 목적지가 지정된 입자들이 날아가 상대 초상화와 충돌판정이 일어나면 폭발 애니메이션을 재생하는 효과를 제작하였습니다.*

<details>
<summary>Photon 서버를 통한 클라이언트의 동기화</summary>
<div markdown="1">
{% highlight c# %}
[PunRPC] void cardsyncro(int[] indexs) {
    PlayerManager.enemyPlayerManager.cardDataBuffer=new List<CardData>(100); 
    for(int i=0; i<indexs.Length; i++) {
        CardData item = PlayerManager.enemyPlayerManager.cardDataSO.items[indexs[i]];
        PlayerManager.enemyPlayerManager.cardDataBuffer.Add(item); // 상대 클라이언트에서 보이는 나의 손패를 실제 내 클라이언트에서의 나의 손패와 동기화시킴
    }
    PlayerManager.enemyPlayerManager.AddFiveCard();
}

public void draw() 
    {
        PlayerManager.myPlayerManager.character_img.GetComponent<SpriteRenderer>().sprite=PlayerManager.myPlayerManager.drawimg; //캐릭터 초상화를 화해제안 이미지로 교체
        PlayerManager.myPlayerManager.character_img.GetComponent<SpriteRenderer>().transform.localScale=new Vector3(0.15f,0.15f,0.15f);
        PlayerManager.myPlayerManager.drawready=true;
        this.gameObject.GetComponent<AudioSource>().Play(); //화해제안 효과음을 play
        PV.RPC("drawsyncro", RpcTarget.OthersBuffered);
        if(PlayerManager.myPlayerManager.drawready==true && PlayerManager.enemyPlayerManager.drawready==true) {
            GameManager.instance.draw();
            PV.RPC("drawstop", RpcTarget.AllBuffered); //양쪽 모두 화해 버튼을 눌렀을 시 게임을 종료하고 무승부 결과창을 띄움
        }    
    }

[PunRPC] void drawsyncro() { //상대 클라이언트에 내 클라이언트에서 화해 버튼을 누른 결과를 동기화하는 함수
    this.gameObject.GetComponent<AudioSource>().Play();
    PlayerManager.enemyPlayerManager.character_img.GetComponent<SpriteRenderer>().sprite=PlayerManager.enemyPlayerManager.drawimg;
    PlayerManager.enemyPlayerManager.character_img.GetComponent<SpriteRenderer>().transform.localScale=new Vector3(0.15f,0.15f,0.15f);
    PlayerManager.enemyPlayerManager.drawready=true;
}
{% endhighlight %}
</div>
</details>

![draw.gif](/assets/img/draw.gif)
*Photon 서버를 이용하여 상대의 클라이언트와 나의 클라이언트의 손패를 동기화시키는 코드입니다.  
또한 기획상 화해 버튼을 누르면 상대가 알게되고 양쪽 모두 화해 버튼을 누를 수 무승부로 끝나는 시스템을  
구현하기 위해 Photon 서버를 사용하여 한쪽에서 화해 버튼을 누를 시 상대 클라이언트를 변화시키도록 구현하였습니다.*

### 젬스톤 서바이버

상기한 코드들과 달리 젬스톤 서바이버에서는 메인 프로그래머를 담당했기에 구현을 전부 게시하기가 어려워 [깃허브 링크](https://github.com/rabent/gemstone)를 첨부하였습니다.  
대신 스크립트간의 관계를 간략히 볼 수 있는 클래스 다이어그램을 staruml으로 만들었습니다. ui등 간단한 기능은 생략하였습니다.  

![클래스 다이어그램](/assets/img/클래스%20다이어그램.PNG)

저희 게임은 로그라이크 핵앤슬래시 게임으로 playermanager이 관리하는 캐릭터가 invenmanager가 관리하는 인벤토리의 석판에 젬을 장착하여 스킬을 사용합니다.  
적은 미리 정해진 SpawnData의 수치대로 초기화된 후 Spawner를 통해 필드에 소환됩니다. GemSpawner는 Enemy가 사망하면 Dead함수에서 젬 리스트중 하나를 자리에 스폰합니다.  
인벤토리는 여러개의 slot과 slot이 달린 석판으로 이루어져 있습니다. slot은 Unity의 eventhandler 기능으로 내용물의 설명패널 띄우기, 드래그 앤 드랍을 통한 스왑을 지원합니다.  
각 석판엔 weaponmanager이 붙어있어 석판의 slot에 담긴 액티브 젬과 패시브 젬을 모두 취합한 결과를 젬의 종류에 따라 투사체, 마법 등 다르게 사용하게 됩니다.  
투사체, enemy, 마법 등 계속해서 생성이 필요한 오브젝트는 poolmanager에서 pool에 없다면 생성하고 pool에 있지만 inactive된 오브젝트는 다시 active하여 가져옵니다.  
게임의 라운드가 끝나면 gamemanager가 UImanager를 참조하여 상점 페이즈를 시작합니다. shopmanager가 관리하는 상점 페이즈에서는 골드를 소모하여 인벤토리 석판의 잠긴 슬롯을 개방할 수 있습니다.  
게임을 처음부터 개발해본 경험이 없어 Bottom-up 방식으로 개발되었습니다.