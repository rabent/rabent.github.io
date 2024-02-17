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
+ Email \| rabent0207@gmail.com
+ Blog \| rabent.github.io

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
프로젝트는 기존 보드게임인 오목에 카드와 마법 요소를 추가한 'Oh-Mok!'으로 Unity engine을 사용한 2인용 실시간 보드게임입니다.  
링크로 들어가시면 관련 코드와 경험에 대해 서술한 글로 이동하여 보실 수 있습니다.  

[클릭 시](https://rabent.github.io/Oh-Mok!/) 블로그 내 포스팅으로 이동합니다.  
[Github 링크](https://github.com/nilbace/Oh-MOK)  
[Play store 링크](https://play.google.com/store/apps/details?id=com.ExPStudio.magical)

## 프로젝트 경험
---
대학교 졸업프로젝트로 핵앤슬래쉬 로그라이크 게임인 '젬스톤 서바이버'의 기획 및 메인 프로그래머로써 프로젝트를 총괄하였습니다.  
'젬스톤 서바이버'는 평소 즐겨하던 게임인 'Path of Exile'과 '뱀파이어 서바이버'의 장점을 섞고 서로의 단점을 상쇄한 작품을 만들고 싶다는 생각으로 시작한 프로젝트입니다.  
서브 프로그래머 1인과 함께 github 및 Unity engine으로 작업하였으며 장르는 로그라이트 핵앤슬래시 게임입니다.

## 코드
---

### Oh-Mok! 주요 구현

<details>
<summary>UI타이머 구현(클릭 시 접기/펼치기)</summary>
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
<summary>Dotween을 이용한 애니메이션(클릭 시 접기/펼치기)</summary>
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
<summary>Photon 서버를 통해 결과를 구분(클릭 시 접기/펼치기)</summary>
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
<summary>Unity의 particle 시스템을 사용한 구현(클릭 시 접기/펼치기)</summary>
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
<summary>Photon 서버를 통한 클라이언트의 동기화(클릭 시 접기/펼치기)</summary>
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

### 젬스톤 서바이버 주요 구현

[깃허브 링크](https://github.com/rabent/gemstone)  
[포스팅 링크](https://rabent.github.io/%EC%A0%AC%EC%8A%A4%ED%86%A4-%EC%84%9C%EB%B0%94%EC%9D%B4%EB%B2%84-%EB%A6%AC%EB%B7%B0/) 링크를 누르시면 상세한 스크립트와 설명을 보실 수 있습니다.

아트팀이 따로 존재하지 않아 게임 내 모든 아트는 프리 에셋을 활용하여 제작되었습니다.  

UI 등 간단한 기능들을 생략하고 전체적인 구조를 볼 수 있는 클래스 다이어그램을 staruml을 이용하여 제작하여 스크립트 간의 관계를 한 눈에 확인할 수 있도록 하였습니다.

![클래스 다이어그램](/assets/img/클래스%20다이어그램.PNG)

![캐릭터 선택](/assets/img/캐릭터%20선택.gif)  

*캐릭터를 선택하고 게임을 시작하면 메인 scene에서 인게임 scene으로 씬이 전환되고  DontDestroyOnLoad에 의해 파괴되지 않은 UI매니저가 전달해준 캐릭터 인덱스에 따라 정해진 젬을 인벤토리에 가지고 시작하게 됩니다.*  

![젬](/assets/img/인벤토리.gif)  

*인벤토리는 4개의 석판과 젬 슬롯들로 이루어져 있습니다. 캐릭터는 젬을 석판에 장착하여 스킬을 사용하게 됩니다. 석판에 장착된 젬은 weapon매니저의 배열에 저장되어 스크립트 내에서 액티브 젬인지, 액티브 젬이라면 투사체 젬인지 마법 젬인지 등을 구별하여 각 젬에 맞는 효과를 게임 내에서 발동하게 됩니다.*

![젬 스폰](/assets/img/젬%20스.gif)  

*Enemy 오브젝트의 hp가 0이 되어 사망 페이즈를 진행할 때, 낮은 확률로 젬 스폰 페이즈에 돌입합니다.  
random 함수를 통해 정해진 범위 내의 난수를 생성, gemspawner 스크립트 내의 미리 저장된 젬 배열에서 하나를 선택하여 선택된 젬의 데이터를 가진 오브젝트를 Enemy 개체가 있던 자리에 생성합니다.  
캐릭터와 젬 오브젝트가 충돌 시 젬의 데이터는 인벤토리 내의 슬롯에 저장되고 젬 오브젝트는 비활성화됩니다.*  

![젬 이동](/assets/img/젬%20이동.gif)  

<details>
<summary>관련 코드(클릭 시 접기/펼치기)</summary>
<div markdown="1">
{% highlight c# %}
public class slot : MonoBehaviour, IBeginDragHandler, IDragHandler, IEndDragHandler, IDropHandler, IPointerEnterHandler, IPointerExitHandler, IPointerClickHandler
{
    [SerializeField]
   private gemData pgem;
   public Image slot_img;
   public bool islock=false;
   public bool isfull=false;
   public bool begin_mono=false;
   public int slot_index;
   public GameObject pannel;
   public TMP_Text title;
   public TMP_Text explain;
   public TMP_Text tags;

   public gemData g { //젬 데이터가 있다면 투명화를 해제
    get {return pgem;}
    set {
        pgem=value;
        if(pgem==null) {
            slot_img.color=new Color(1,1,1,0);
            isfull=false;
        }
        else {
            isfull=true;
            slot_img.sprite=g.spr;
            slot_img.color=new Color(1,1,1,1);
        }
        
    }
   }

   void OnDisable() {
    pannel.SetActive(false);
   }

    public void OnPointerClick(PointerEventData eventData) {
        if(eventData.button==PointerEventData.InputButton.Right) {
            if(this.g!=null) {
                g=null;
                gamemanager.instance.gold+=10;
                invenmanager.inventory.gemlist_refresh();
            }
        }
    }

   public void OnPointerEnter(PointerEventData eventData) {
    //마우스 올리면 젬의 정보 패널을 띄움
        if(this.isfull) {
            pannel.SetActive(true);
            title.text=g.gem_name;
            explain.text=g.gem_explain;
            string str="";
            foreach(string s in g.tags) {
                str+=s + ",";
            }
            if(g.ispassive) {
                foreach(string s in g.required_tag) {
                    str+="<color=#800000ff><b>" + s + "</b></color>" + ",";
                }
            }
            str=str.Remove(str.Length - 1, 1);
            this.tags.text=str;
            Debug.Log("mouse enter");
        }
   }

    public void OnPointerExit(PointerEventData eventData) {
        //마우스 뗐을 때 창 사라짐
        if(pannel.activeSelf==true) {
            pannel.SetActive(false);
            Debug.Log("mouse exit");
        }
    }

   
    public void OnBeginDrag(PointerEventData eventData)
    { //슬롯에 젬이 있을시 슬롯을 클릭하면 draggedslot에 그 슬롯의 데이터를 복사해서 넘겨줌
        pannel.SetActive(false);
        if(isfull && !islock) {
            if(this.gameObject.tag=="monoslot") begin_mono=true;
            draggedslot.instance.dragslot=this;
            draggedslot.instance.dragset(slot_img);
            draggedslot.instance.transform.position=eventData.position;
        }
    }

    public void OnDrag(PointerEventData eventData)
    { //마우스 이동에 따라 draggedslot이 이동
        if(isfull && !islock) {
            draggedslot.instance.transform.position=eventData.position;
        }
    }

    public void OnEndDrag(PointerEventData eventData)
    { //드래그가 끝났을 시 처음에 클릭했던 슬롯에서 발동하는 함수
    //드래그의 종착점이 monolith인지, 다른 슬롯인지에 따라서 필요한 절차를 진행
        if(draggedslot.instance.is_monolith==true && draggedslot.instance.is_change==false && !islock) {
            this.g=null;
            invenmanager.inventory.gemlist[slot_index]=null;
            draggedslot.instance.is_monolith=false;
        }
        else if(draggedslot.instance.is_change==true && !islock) {
            Debug.Log(draggedslot.instance.change_gd);
            this.g=draggedslot.instance.change_gd;
            int idx=draggedslot.instance.change_idx;
            if(draggedslot.instance.is_monolith) {
                invenmanager.inventory.gemlist[slot_index]=draggedslot.instance.change_gd;
                draggedslot.instance.is_monolith=false;
            }
            else {
                invenmanager.inventory.gemlist[idx]=this.g;
                if(!begin_mono) invenmanager.inventory.gemlist[slot_index]=draggedslot.instance.change_gd;
            }
            draggedslot.instance.change_gd=null;
            draggedslot.instance.change_idx=-1;
            draggedslot.instance.is_change=false;
        }
        if(begin_mono && !islock) {
             foreach(GameObject mono in invenmanager.inventory.monoliths) {
                mono.GetComponent<weaponmanager>().monolith_reset();
            }
        }
        draggedslot.instance.drag_invisible(0);
        draggedslot.instance.dragslot=null;
        begin_mono=false;
        invenmanager.inventory.gemlist_refresh();
    }

    public void OnDrop(PointerEventData eventData)
    { //enddrag보다 먼저 발동하는 함수로 드래그가 끝난 위치에 있는 슬롯에서 발동
    //드래그가 끝난 위치가 monolith라면 젬데이터를 monolith로 넘겨주고 refresh
    //드래그가 끝난 위치가 다른 슬롯이라면 그 슬롯에 draggedslot의 데이터를 넘기고 슬롯의 데이터를 받아옴
        if(draggedslot.instance.dragslot!=null && this.gameObject.tag=="monoslot" && !islock) {
            if(this.g!=null) {
                draggedslot.instance.change_idx=this.slot_index;
                draggedslot.instance.change_gd=this.g;
                draggedslot.instance.is_change=true;
            }
            this.g=draggedslot.instance.dragslot.g;
            foreach(GameObject mono in invenmanager.inventory.monoliths) {
                mono.GetComponent<weaponmanager>().monolith_reset();
            }
            draggedslot.instance.is_monolith=true;
        }
        else if(draggedslot.instance.dragslot!=null && this.gameObject.tag=="slot" && !islock) {
            draggedslot.instance.change_idx=this.slot_index;
            draggedslot.instance.change_gd=this.g;
            this.g=draggedslot.instance.dragslot.g;
            draggedslot.instance.is_change=true;
        }
    }
}
{% endhighlight %}
</div>
</details>

*인벤토리는 유니티의 EventSystem의 IDragHandler 등의 인터페이스를 활용하여 구현하였습니다. OnDrag, OnDrop 등의 함수를 적절히 사용하여 인벤토리 내의 슬롯 간 데이터 이동을 가능케 했습니다.  
인벤토리 오브젝트가 비활성화 되었을 때도 저는 캐릭터가 젬 오브젝트와 충돌 시에 인벤토리에 젬 데이터를 넣어주어야 하고, 인벤토리의 석판에 장착된 젬의 데이터대로 스킬을 발동시켜야 했습니다. 하지만 유니티에선 오브젝트가 비활성화되면 내부의 스크립트도 모두 침묵하므로 저는 UI에서의 인벤토리와 실제 인벤토리 내부의 데이터를 가진 데이터 배열로써의 인벤토리, 두 가지를 만들고 이 둘을 적절히 동기화해야 했습니다.  
저는 인벤토리 UI를 활성화할 때와 UI를 비활성화할 때마다 두 부분을 동기화하는 시퀀스를 진행하여 해결하였습니다.*  

![젬 발동](/assets/img/스킬%20발동.gif)  

<details>
<summary>관련 코드(클릭 시 접기/펼치기)</summary>
<div markdown="1">
{% highlight c# %}
public void monolith_reset() { //인벤토리에서 monolith에 젬을 장착시켰을 때
    //슬롯의 젬 데이터를 monolith로 가져오는 함수
        Debug.Log("gem set");
        for(int i=0; i<3+slot_index; i++) { //향후 3을 열린 슬롯 개수로 수정
            if(mono_slots[i].gameObject.activeSelf==true) {
                gems[i]=mono_slots[i].g;
            }
        }
    }

    public void monolith_clear() { //공격의 중복발동을 방지하기 위해 공격 발동 전에 초기화해주는 함수
        this.damage=0;
        this.count=0;
        this.prefabid=0;
        this.gem_color=0;
        this.speed=0;
        this.radius=0;
        this.penet=0;
        this.element=0;
        this.force=3;
        this.delay_percent=1;
        active_on=false;
        curse.Clear();
        tween.Kill();
        if(crt!=null) StopCoroutine(crt);
        if(spcrt!=null) special_manager.GetComponent<special>().StopCoroutine(spcrt);
    }
    public void monolith_active() {
        monolith_clear();
        //인벤토리를 끌 때 monolith가 가진 젬들을 계산하여 weaponmanager가 최종적으로 스킬을 발동함
        for(int i=0; i<gems.Length; i++) {
            if(gems[i]!=null) {
                if(gems[i].isactive && i!=0) {
                    gemData tmp=gems[0];
                    gems[0]=gems[i];
                    gems[i]=tmp;
                } 
            }
        }
        foreach(gemData gd in gems) {
            if(gd==null) continue;
            if(gd.isactive && !active_on) {
                this.damage=gd.damage;
                this.count=gd.count;
                this.prefabid=gd.id;
                this.gem_color=gd.color;
                this.speed=gd.speed;
                this.radius=gd.radius;
                this.penet=gd.penet;
                this.element=gd.element;
                this.force=gd.force;
                active_on=true;
                skill_use();
            }
            else if(gd.ispassive) {
                bool flag=true;
                foreach(string s in gd.required_tag) {
                    if(gems[0]!=null && !gems[0].tags.Contains(s)) flag=false;
                }//필요 태그가 있는지를 검색
                if(gd.required_tag.Contains("범용")) flag=true;
                if(flag) {
                    if(gd.curse!=0) curse.Add(gd.curse);
                    this.damage+=gd.damage;
                    this.speed*=gd.speed;
                    this.radius*=gd.radius;
                    this.penet+=gd.penet;
                    this.count+=gd.count;
                    this.element=gd.element;
                    this.force+=gd.force;
                    this.delay_percent*=gd.delay_reduct;
                }
            }
            else if(gd.isspecial) {
                spcrt=special_manager.GetComponent<special>().init(this);
            }
        }
    }
{% endhighlight %}
</div>
</details>

*인벤토리 내에서 석판에 장착한 젬은 동기화 시퀀스에서 각 석판 하나씩을 담당하는 weapon매니저 스크립트에 전달됩니다. weapon매니저에서는 젬 배열을 전달받아 젬의 데이터대로 실제 인게임의 스킬을 발동하는 역할을 합니다.  
weapon매니저는 하나의 액티브 젬과 액티브 젬을 강화하는 여러 패시브 젬을 장착할 수 있도록 구성되었습니다. 패시브 젬엔 검붉은 글씨로 강조되는 '필요 태그'가 있어 만약 액티브 젬이 '필요 태그'를 가지고 있다면 강화시켜주는 시스템입니다. 이 부분은 C#의 Array가 포함하고 있는 Contains 함수를 사용하여 구현하였습니다.  
weapon매니저는 석판마다 하나씩 담당하기 때문에 다른 석판에 액티브 젬을 장착하면 동시에 스킬이 발동하여 캐릭터는 최대 4개의 스킬을 동시 사용할 수 있도록 시스템을 구현하였습니다.*  

![상점](/assets/img/상점.gif)  

*게임 내에서 4분마다 스폰되는 보스 Enemy를 쓰러뜨리면 스테이지가 끝나고 상점 페이즈에 진입합니다. 인벤토리 내의 석판은 시작할 때 슬롯이 3개가 열려있고 나머지 3개는 잠겨있는데, 상점에서 골드를 소모하여 잠금을 해제할 수 있습니다.  
골드는 Enemy를 쓰러뜨릴 때에 각 오브젝트 마다 정해진 수치가 들어옵니다. 잠긴 슬롯은 실제 젬 데이터를 OnDrop 등의 함수로 받는 slot 오브젝트를 비활성화 시키고 대신 금지 스프라이트를 활성화시켜 만든 것입니다.  
잠금해제 페이즈에서 골드가 충분하다면 금지 스프라이트를 비활성화, slot 오브젝트를 활성화시켜 사용할 수 있도록 하고 골드가 부족하다면 골드 부족 알림을 띄우도록 구성하였습니다.*

---  
  
이외에 [링크](https://rabent.github.io/archive/)를 클릭하시면 c++을 사용한 문제 풀이 등의 블로그의 다른 포스팅도 열람하실 수 있습니다. 감사합니다. 