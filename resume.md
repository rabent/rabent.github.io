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
* [게임 개발 프로젝트](#게임-개발-프로젝트-경험)  
* [활동 내용](#활동-내용)  


## 경력
---
+ 홍익대학교 컴퓨터공학과 재학  (2017~)
+ 대학교 내 게임 제작 동아리 EXP에서 서브프로그래머로 프로젝트 참여
+ 대학교 졸업 프로젝트 '젬스톤 서바이버'의 기획, 메인프로그래머로 활동  

## 기술 스택
---
- Unity Engine
- C++
- C#  
- Mysql  
- JAVA  
- Spring  


## 게임 개발 프로젝트 경험
---
Oh-Mok!
- Photon 서버를 이용한 1대1 오목 보드게임
- 구글 플레이스토어 출시
- 클라이언트 : Unity Engine
- 서버 : Photon
- 개발 기간 : 8개월
- 관련 링크 :   
[[블로그 내 포스팅]](https://rabent.github.io/Oh-Mok!/)  
[[Github 링크]](https://github.com/nilbace/Oh-MOK)  
[[Play store 링크]](https://play.google.com/store/apps/details?id=com.ExPStudio.magical)

젬스톤 서바이버  
- 정통 핵앤슬래시의 시스템을 접목한 로그라이트 핵앤슬래시 게임
- 홍익대학교 졸업프로젝트
- 클라이언트 : Unity Engine  
- 개발 기간 : 10개월  
- 관련 링크 :  
[[블로그 내 포스팅]](https://rabent.github.io/%EC%A0%AC%EC%8A%A4%ED%86%A4-%EC%84%9C%EB%B0%94%EC%9D%B4%EB%B2%84-%EB%A6%AC%EB%B7%B0/)  
[[Github 링크]](https://github.com/rabent/gemstone)   

## 활동 내용
---

### Oh-Mok! 주요 구현  

![timer.gif](/assets/img/timer.gif)  

<details>
<summary>UI타이머 구현(클릭 시 접기/펼치기)</summary>
<div markdown="1">

<details>
<summary>상세 코드</summary>
<div markdown="2">
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

*Unity UI의 fill image 기능을 사용하여 시계바늘이 회전하여 지나간 자리는 빨간색으로 채워주는 타이머를 구현하여 유저가 자신의 턴의 제한시간을 볼 수 있게 하고, 타이머가 끝까지 돌아가면 강제로 상대의 턴으로 넘어가는 로직을 구현하였습니다.  
기획서대로 타이머의 위치를 자신의 턴일 때는 자신 캐릭터 옆에, 상대 턴일땐 상대 캐릭터 옆에 생성시키도록 하였습니다.*

</div>
</details>  
&nbsp;
&nbsp;

![particle.gif](/assets/img/part.gif)  

<details>
<summary>Dotween, Particle System을 사용한 순차적 애니메이션과 vfx 구현</summary>
<div markdown="1">

<details>
<summary>Dotween을 이용한 애니메이션 코드</summary>
<div markdown="2">
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
<summary>Photon 서버를 통해 결과를 구분하는 코드</summary>
<div markdown="2">
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
<summary>Unity의 particle 시스템을 사용한 vfx 구현 코드</summary>
<div markdown="2">
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

*오목 게임인 만큼 돌 5개가 이어지게 만들면 5개가 완성됬음을 알려주는 이펙트와 함께 상대를 타격하여 데미지를 주는 이펙트가 필요했습니다.  
Unity의 에셋인 Dotween을 사용하여 돌 5개가 이어졌을 때 돌들이 가운데 돌로 모이는 애니메이션을 제작하였고  
Unity의 Particle system을 사용하여 각 지점에서 생성된 입자들이 한 점으로 모이는 애니메이션을 제작하여 돌이 가운데로 모이는 애니메이션 동시에 재생되도록 구현했습니다.  
애니메이션이 끝나면 상대 초상화로 목적지가 지정된 입자들이 날아가 상대 초상화와 충돌판정이 일어나면 폭발 애니메이션을 재생하도록 구현했습니다.*  

</div>
</details>  
&nbsp;
&nbsp;

![draw.gif](/assets/img/draw.gif)  

<details>
<summary>Photon 서버를 통한 클라이언트의 동기화(클릭 시 접기/펼치기)</summary>
<div markdown="1">  

<details>
<summary>상세 코드</summary>
<div markdown="2">  

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

*2개의 클라이언트가 서버를 통해 실시간으로 통신하는 게임의 특성 상 한 쪽의 클라이언트의 정보를 다른 쪽의 클라이언트와 동기화 시키는 작업이 많았습니다.  
먼저 상대가 카드를 사용하면 카드가 뒤집히는 애니메이션이 재생되어 상대가 어떤 카드를 사용했는지 알 수 있는 게임의 구조 상 두 클라이언트의 손패를 동기화하는 코드가 필요하여 구현했습니다.  
또한 기획 상 화해 버튼을 누르면 상대가 알게되고 양쪽 모두 화해 버튼을 누르면 무승부로 끝나는 시스템을 구현하기 위해 한쪽에서 화해 버튼을 누를 시 상대 클라이언트에도 해당 변화를 동기화시키고, 두 클라이언트에서 모두 화해 버튼을 눌렀을 시 게임을 무승부로 종료하는 함수를 구현했습니다.*  

</div>
</details>  
&nbsp;
&nbsp;

### 젬스톤 서바이버 주요 구현

[깃허브 링크](https://github.com/rabent/gemstone)  
[포스팅 링크](https://rabent.github.io/%EC%A0%AC%EC%8A%A4%ED%86%A4-%EC%84%9C%EB%B0%94%EC%9D%B4%EB%B2%84-%EB%A6%AC%EB%B7%B0/)  
링크를 누르시면 상세한 스크립트와 설명을 보실 수 있습니다.

아트팀이 따로 존재하지 않아 게임 내 모든 아트는 프리 에셋과 생성형 AI를 활용하여 제작되었습니다.  

<details>
<summary>클래스 다이어그램을 통한 스크립트 도식화</summary>
<div markdown="1">  

![클래스 다이어그램](/assets/img/클래스%20다이어그램.PNG)  

UI 등 간단한 기능들을 생략하고 전체적인 구조를 볼 수 있는 클래스 다이어그램을 staruml을 이용하여 제작하여 스크립트 간의 관계를 한 눈에 확인할 수 있도록 하였습니다.

</div>
</details>  
&nbsp;
&nbsp;  
<details>
<summary>게임의 전체적인 흐름</summary>
<div markdown="1">  

![캐릭터 선택](/assets/img/캐릭터%20선택.gif)  

*캐릭터를 선택하고 게임을 시작하면 메인 scene에서 인게임 scene으로 씬이 전환되고  DontDestroyOnLoad에 의해 파괴되지 않은 UI매니저가 전달해준 캐릭터 인덱스에 따라 정해진 젬을 인벤토리에 가지고 시작하게 됩니다.*  

![젬](/assets/img/인벤토리.gif)  

*인벤토리는 4개의 석판과 젬 슬롯들로 이루어져 있습니다. 캐릭터는 젬을 석판에 장착하여 스킬을 사용하게 됩니다. 석판에 장착된 젬은 weapon매니저의 배열에 저장되어 스크립트 내에서 액티브 젬인지, 액티브 젬이라면 투사체 젬인지 마법 젬인지 등을 구별하여 각 젬에 맞는 효과를 게임 내에서 발동하게 됩니다.*

![젬 스폰](/assets/img/젬%20스.gif)  

*Enemy 오브젝트의 hp가 0이 되어 사망 페이즈를 진행할 때, 낮은 확률로 젬 스폰 페이즈에 돌입합니다.  
random 함수를 통해 정해진 범위 내의 난수를 생성, gemspawner 스크립트 내의 미리 저장된 젬 배열에서 하나를 선택하여 선택된 젬의 데이터를 가진 오브젝트를 Enemy 개체가 있던 자리에 생성합니다.  
캐릭터와 젬 오브젝트가 충돌 시 젬의 데이터는 인벤토리 내의 슬롯에 저장되고 젬 오브젝트는 비활성화됩니다.*  

</div>
</details>  
&nbsp;
&nbsp; 

![젬 이동](/assets/img/젬%20이동.gif)  

<details>
<summary>EventSystem을 활용한 인벤토리 구현</summary>
<div markdown="1">  

<details>
<summary>관련 코드(클릭 시 접기/펼치기)</summary>
<div markdown="2">
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

*인벤토리는 유니티의 EventSystem의 IDragHandler 등의 인터페이스를 활용하여 구현하였고 OnDrag, OnDrop 등의 함수를 적절히 사용하여 인벤토리 내의 슬롯 간 데이터 이동을 가능케 했습니다.  
기획 상 인벤토리 객체가 비활성화 되었을 때도 캐릭터가 젬 오브젝트와 충돌하면 인벤토리에 젬 데이터를 추가해야 했고, 인벤토리의 석판에 장착된 젬의 데이터대로 스킬을 발동시켜야 했습니다. 하지만 유니티에선 오브젝트가 비활성화되면 내부의 스크립트도 모두 침묵하므로 제대로 작동하지 않는 문제가 있었습니다.  
저는 UI에서의 인벤토리와 실제 인벤토리 내부의 데이터를 가진 데이터 배열로써의 인벤토리, 두 가지를 만들고 이 두 부분을 인벤토리를 닫을 때, 열 때 등의 타이밍에 적절히 동기화하는 방식으로 문제를 해결했습니다.*  

</div>
</details>  
&nbsp;
&nbsp;

![젬 발동](/assets/img/스킬%20발동.gif)  

<details>
<summary>인벤토리에서 장착한 객체의 스킬을 발동</summary>
<div markdown="1">

<details>
<summary>관련 코드(클릭 시 접기/펼치기)</summary>
<div markdown="2">
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

*인벤토리 내에서 석판에 장착한 젬은 동기화 시퀀스에서 각 석판 하나씩을 담당하는 WeaponManager 스크립트에 전달됩니다. WeaponManager에서는 젬 배열을 전달받아 젬의 데이터대로 실제 인게임의 스킬을 발동하는 역할을 합니다.  
WeaponManager는 하나의 액티브 젬과 액티브 젬을 강화하는 여러 패시브 젬을 장착할 수 있도록 구성되었습니다. 패시브 젬엔 검붉은 글씨로 강조되는 '필요 태그'가 있어 만약 액티브 젬이 '필요 태그'를 가지고 있다면 액티브 젬의 스킬을 강화시켜주도록 구현했습니다. 이 부분은 C#의 Array가 포함하고 있는 Contains 함수를 사용하여 구현하였습니다.  
WeaponManager는 석판마다 하나씩 담당하기 때문에 다른 석판에 액티브 젬을 장착하면 동시에 스킬이 발동하여 캐릭터는 최대 4개의 스킬을 동시 사용할 수 있도록 시스템을 구현하였습니다.*  

</div>
</details>  
&nbsp;
&nbsp;

![상점](/assets/img/상점.gif)  

<details>
<summary>상점 페이즈 구현</summary>
<div markdown="1">

*게임 내에서 4분마다 스폰되는 보스 Enemy를 쓰러뜨리면 스테이지가 끝나고 상점 페이즈에 진입합니다. 인벤토리 내의 석판은 시작할 때 슬롯이 3개가 열려있고 나머지 3개는 잠겨있는데, 상점에서 골드를 소모하여 잠금을 해제할 수 있습니다.  
골드는 Enemy를 쓰러뜨릴 때에 각 오브젝트 마다 정해진 수치가 들어옵니다. 잠긴 슬롯은 실제 젬 데이터를 OnDrop 등의 함수로 받는 slot 오브젝트를 비활성화 시키고 대신 금지 스프라이트를 활성화시켜 만든 것입니다.  
잠금해제 페이즈에서 골드가 충분하다면 금지 스프라이트를 비활성화, slot 오브젝트를 활성화시켜 사용할 수 있도록 하고 골드가 부족하다면 골드 부족 알림을 띄우도록 구성하였습니다.*  

</div>
</details>  
  
  
## 그 외  

### 깃허브 액션을 통한 CI/CD  

![성공](/assets/img/깃허브%20액션%20성공.png)  

<details>
<summary>gradle.yml</summary>
<div markdown="1">

```  

# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.
# This workflow will build a Java project with Gradle and cache/restore any dependencies to improve the workflow execution time
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-gradle

name: Java CI with Gradle

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    defaults :
      run:
       working-directory: ./toy

    steps:
    - uses: actions/checkout@v4
    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        java-version: '17'
        distribution: 'temurin'

    - name : chmod
      run : chmod +x ./gradlew

    # Configure Gradle for optimal use in GitHub Actions, including caching of downloaded dependencies.
    # See: https://github.com/gradle/actions/blob/main/setup-gradle/README.md
    - name: Setup Gradle
      uses: gradle/actions/setup-gradle@af1da67850ed9a4cedd57bfd976089dd991e2582 # v4.0.0

    - name: Build with Gradle Wrapper
      run: ./gradlew build

    - name: Upload JAR Artifact
      uses: actions/upload-artifact@v4
      with: 
        name: DemoJAR
        path: toy/build/libs/demo-0.0.1-SNAPSHOT.jar

    # NOTE: The Gradle Wrapper is the default and recommended way to run Gradle (https://docs.gradle.org/current/userguide/gradle_wrapper.html).
    # If your project does not have the Gradle Wrapper configured, you can use the following configuration to run Gradle with a specified version.
    #
    # - name: Setup Gradle
    #   uses: gradle/actions/setup-gradle@af1da67850ed9a4cedd57bfd976089dd991e2582 # v4.0.0
    #   with:
    #     gradle-version: '8.9'
  deploy:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Download JAR Artifact
        uses: actions/download-artifact@v4
        with: 
          name: DemoJAR
          path: bulid/libs/

      - name: Show structure of downloaded files
        run: |
          ls -alh /home/runner/work/toy-web/toy-web/bulid/libs

      - name: Upload to EC2
        run: |
          echo "${ { secrets.EC2_SSH_KEY } }" > SSH_key.pem
          chmod 600 SSH_key.pem
          scp -i SSH_key.pem -o StrictHostKeyChecking=no /home/runner/work/toy-web/toy-web/bulid/libs/demo-0.0.1-SNAPSHOT.jar ${ { secrets.EC2_USERNAME } }@${ { secrets.EC2_IP } }:/home/${ { secrets.EC2_USERNAME } }/clone/toy-web/toy/build/libs/demo-0.0.1-SNAPSHOT.jar

      - name: ssh pipelines
        uses: appleboy/ssh-action@master
        with:
          host: ${ { secrets.EC2_IP } }
          username: ${ { secrets.EC2_USERNAME } }
          key: ${ { secrets.EC2_SSH_KEY } }
          port: ${ { secrets.EC2_PORT } }
          script: |
            cd /home/ubuntu/clone/toy-web/toy/build/libs
            nohup java -jar demo-0.0.1-SNAPSHOT.jar /home/${ { secrets.EC2_USERNAME } }/log/app_log.out 2>&1 &
            exit

```

</div>
</details>  

추후 있을 협업에서 반드시 필요한 경험이라 생각하여 CI/CD 툴 사용 및 서버 배포를 시도하게 되었습니다. 많은 시행착오가 있었지만 목표했던 부분을 구현하는데 성공하였고, 얕지만 CI/CD가 무엇이고 어떤 식으로 이루어지는지와 기술을 제대로 배우지 않고 사용하면 어떤 일이 벌어지는 지를 깨닫게 된 좋은 경험이었습니다.  

<details>
<summary>상세 설명</summary>
<div markdown="2">

Spring을 배우던 중 배운 내용을 실제로 사용하고 프로젝트에 녹여내어 체득할 필요성을 느껴 토이프로젝트를 시작하게 되었습니다. 다른 분들은 어떤 식으로 진행하는지 여러 자료를 참고하던 중 토이프로젝트 수준에서도 CI/CD와 AWS로의 배포를 하는 것을 보고 나중에 있을 협업을 위해 저러한 경험이 필요하겠다고 생각하여 시도하게 되었습니다.  
CI/CD가 필요한 규모가 이니기도 하고 Continuous Integration의 경우 여러 명의 팀원이 있어야 의미가 있다고 생각하지만 공부를 위해 시도하게 되었고, 목표는 git push를 트리거로 빌드와 테스트를 진행한 후 EC2 서버에 올려 배포하는 과정까지로 잡았습니다.  
툴은 깃허브에서 제공하여 별다른 연동이 필요없고, 호환성이 좋으며 무엇보다 프리티어를 제공하는 이유로 github action을 선택하였고, 서버는 이러한 토이프로젝트에서 가장 범용적으로 사용되어 참고자료가 많으며 무엇보다 프리티어를 제공하는 EC2 서버를 사용하기로 결정했습니다.  
결과적으로 build 과정에서는 빌드 후 만들어진 jar 파일을 artifact로 업로드 하고, deploy 과정에서 artifact를 다운로드하고 EC2 서버에 SCP로 업로드 후 SSH로 연결하여 실행하는 방식으로 성공적으로 구현되었습니다.  
그 과정에서 SSH의 기본 포트가 22인 것을 모르고 헤메고, EC2 instance는 설정된 OS마다 SSH 접근 시 기본 username이 다른 것을 모르고 헤메는 등, 많은 시행착오를 겪었습니다. 그러면서 기술을 제대로 공부하지 않고 사용법만 배우면 어떤 일이 벌어지는 지를 깨닫게 된 좋은 경험이었다고 생각합니다.  

</div>
</details>  
&nbsp;


### 알고리즘, C++ 공부  

[![Solved.ac Profile](http://mazassumnida.wtf/api/v2/generate_badge?boj=rabent0207)](https://solved.ac/rabent0207/)

개발을 하며 부족함을 많이 느껴 학부 시절부터 계속 배워온 C++로 지속적으로 알고리즘 공부를 하고 있습니다.  
[링크](https://rabent.github.io/archive/)를 클릭하시면 공부하며 포스팅한 내용들을 보실 수 있습니다.