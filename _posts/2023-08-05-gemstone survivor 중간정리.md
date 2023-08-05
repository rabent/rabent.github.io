---
title: "핵앤슬래시 로그라이크 게임 Gemstone Survivor"
layout: post
date: 2023-08-05 12:37
tag:
- game
- 프로젝트
- c#
- unity
description: 졸업 프로젝트 'Gemstone Survivor' 중간 정리
---

# Repository

[github](https://github.com/rabent/gemstone)

# 게임 특징

+ Unity
+ 핵 앤 슬래시
+ 로그라이크
+ 육성의 자유도
+ 메인 프로그래머
+ 기획 겸 메인 프로그래머 1, 서브 프로그래머 1
+ 플랫폼 미정

# 설명

홍익대학교 컴퓨터공학과 졸업 프로젝트로서 시작한 프로젝트이다. 기확과 메인 프로그래머로써 현재 활동중이다. 게임의 컨셉은 굉장히 간단하고, 저예산이면서 레트로함에도 불구하고 스팀을 휩쓴 '뱀파이어 서바이벌'과 굉장히 복잡하고 진입장벽이 높은, 입문하기 어려운 'Path of Exile'을 둘 다 재밌게 한 입장에서 '뱀파이어 서바이벌'의 가벼움, 입문 용이함, 레트로함 등의 장점과 'Path of Exile'의 육성의 자유도란 장점을 조합하면 재밌을 것 같아 두 게임의 조합으로 잡게 되었다.

'뱀파이어 서바이벌'과 같이 한 게임에 30분 진행, 5분마다 스테이지가 더 어려워지며  끊임없이 나오는 적들을 처치하고, 게임이 끝나도 남는 요소가 없는 로그라이크 형식으로 진행된다. 이러한 기틀 위에 'Path of Exile'의 젬 시스템을 결합한 것이 특징으로 캐릭터는 몬스터를 잡아서 나온 젬을 인벤토르의 석판에 장착하여 젬에 담긴 스킬을 사용할 수 있다. 젬은 액티브 젬과 패시브 젬으로 나뉘며 석판에 장착한 액티브 젬 하나와 여러 패시브 젬을 장착하여 강화된 스킬을 사용하게 된다.

공격엔 얼음, 불, 번개 세 가지 속성과 속성이 없는 물리 공격이 있다. 속성 공격은 처음에는 몹의 기본 저항에 막혀 물리보다 약하지만 패시브 젬으로 저항 무시, 저항 감소 저주 등을 붙여 물리보다 고점이 높은 느낌으로, 물리는 저점이 높고 패시브 젬을 붙이는대로 쉽게 강해질 수 있는 느낌으로 설계하였다.

상점 시스템을 추가할 예정으로 상점에선 석판의 젬 슬롯 개방, 패시브 젬의 효과를 강화시켜주는 링크의 개방 및 리롤 등의 기능을 수행할 예정이다. 상점은 몹을 잡을 때 낮은 확률로 떨어지는 젬 대신 대부분 떨어지는 골드로 이용할 수 있다.

현재 인벤토리와 젬 시스템이 완료되었고 캐릭터가 무한히 이동하는 맵에서 스폰되는 몬스터를 잡으며 떨어지는 젬을 장착하여 스킬을 사용하는 게임의 기본적인 구조는 모두 완성되었다. 게임의 컨셉에 맞게 젬의 종류를 여러 종류 추가할 예정이며 아트팀이 없어 무료 아트를 구하는 것이 난관인 상황이다.

게임 제작에 들어가기 전 한달 가량의 기획 및 설계 과정을 거쳐 ui 기획서, 젬 기획서, 프로젝트 제안서 등 여러 서류를 작성하였다. 그 중 일부와 1학기 최종 발표때 사용한 시연 데모를 이미지로 첨부하였다.

![ui 기획서.jpg](/assets/img/기획1.PNG)

![제안서 1.jpg](/assets/img/기획2.PNG)

![제안서 2.jpg](/assets/img/기획3.PNG)

![시연 데모.gif](/assets/img/시연%20데모.gif)

# 게임 구조

스크립트 중 일부를 캡쳐하여 게임의 전체적인 구조를 설명해보았다.

{% highlight c# %}
void Awake() //게임 초기화
    {
        if(instance==null) {
            instance=this;
            DontDestroyOnLoad(this.gameObject);
        }
        else Destroy(this.gameObject);

        uimng=GameObject.Find("UImanager");
        ui=uimng.GetComponent<uimanager>();
        char_num=ui.char_num;
    }
    

    private void Update()
    {
        gameTime += Time.deltaTime;

        if(gameTime > maxGameTime){
            gameTime = maxGameTime;
        }

        if(Input.GetKeyDown(KeyCode.I)) { //인벤토리 오픈 및 초기화
            inventory.SetActive(true);
            invenmanager.slot_refresh();
            Time.timeScale=0;
            inv_active=true;
        }

        if(inv_active==true && Input.GetKeyDown(KeyCode.Escape)) {//인벤토리 켜져있을시 닫고 인벤이 꺼져있으면 설정창을 on
            GameObject[] monoliths=invenmanager.monoliths;
            foreach(GameObject mono in monoliths) {
                mono.GetComponent<weaponmanager>().monolith_active();
            }
            inventory.SetActive(false);
            inv_active=false;
            Time.timeScale=1;
        }
    }

    public void merchant_phase() {
        //상점 페이즈 진입시 인게임의 모든 오브젝트 파괴 후 상점 ui on
        foreach(List<GameObject> pool in poolmng.pools) {
            foreach(GameObject obj in pool) {
                Destroy(obj);
            }
        }
        ui.merchant_on();
    }

{% endhighlight %}
> gamemanager에서는 싱글톤 기법으로 게임 매니저가 하나만 작동할 수 있도록 관리하며 uimanager에 인게임의 ui정보를 넘겨준다. ui매니저가 인게임이 아닌 게임의 스타트화면부터 작동을 하기에 필요한 과정이다. 
또한 인게임의 시간을 측정하여 스테이지를 변화시킨다. ui매니저가 완성되기 전에 만들어졌기 때문에 현재 인벤토리의 on off 기능이 ui매니저가 아닌 게임 매니저에서 수행중이다. 추후 ui매니저로 이동시킬 예정이다. 
상점 페이즈의 구현을 시작하기 위해 인게임을 초기화하고 상점 패널을 띄우는 기능을 추가하였다.

{% highlight c# %}

private void Awake() {
        rigid = GetComponent<Rigidbody2D>();
        spriter = GetComponent<SpriteRenderer>();
        anim = GetComponent<Animator>();
    }
    
    void Update() { //캐릭터 이동
        inputvec.x=Input.GetAxis("Horizontal")*0.1f;
        inputvec.y=Input.GetAxis("Vertical")*0.1f;
    }

    private void FixedUpdate() { //area와 함께 이동시켜줌
        rigid.MovePosition(rigid.position + inputvec);
    }


    private void OnTriggerEnter2D(Collider2D collision) { //젬과 충돌시 인벤의 젬리스트에 추가
        if(collision.gameObject.tag == "gem") {
            Debug.Log("gem");
            gemData gd = collision.gameObject.GetComponent<gem>().GemData;
            inv.add_gem(gd);
            collision.gameObject.SetActive(false);
        }
    }
    void LateUpdate(){ //걷는 애니메이션 재생
        anim.SetFloat("Speed", inputvec.magnitude);

        if (inputvec.x != 0) {
            spriter.flipX = inputvec.x < 0;
        }
    }

{% endhighlight %}

> playermanager에서는 캐릭터의 이동과 이동 시 애니메이션의 재생, 젬과 충돌 시 인벤토리에 젬을 추가하고 충돌한 젬 오브젝트를 비활성화 시키는 기능을 수행한다.

{% highlight c# %}

public GameObject[] prefabs;
    public List<GameObject>[] pools; 
    private void Awake() { //리스트에 담긴 프리팹들을 가져옴
        pools=new List<GameObject>[prefabs.Length];
        for(int i=0; i<prefabs.Length; i++) {
            pools[i]=new List<GameObject>();
        }
    }

    public GameObject pulling(int num) { //풀에 담긴 원하는 오브젝트를 생성하고 inactive인 오브젝트가 있으면 active하여 재활용함
        GameObject prefab=null;
        foreach(GameObject obj in pools[num]) {
            if(!obj.activeSelf) {
                prefab=obj; obj.SetActive(true); break;
            }
        }
        if(prefab==null) {
            prefab=Instantiate(prefabs[num],new Vector3(0,0,0),Quaternion.identity);
            pools[num].Add(prefab);
        }
        return prefab;
    }


{% endhighlight %}

> poolmanager에서는 미리 리스트에 넣어둔 prefab들을 리스트의 index를 이용해 자유롭게 생성하고 이미 생성됬으며 비활성화중인 오브젝트가 있다면 활성화하여 재활용하는 기능을 수행한다.

{% highlight c# %}

void Update() //게임 진행 시간에 따라 게임의 스테이지 레벨이 증가
    {
        timer += Time.deltaTime;
        level = Mathf.Min(Mathf.FloorToInt(gamemanager.instance.gameTime / 10f), spawnData.Length);

        if(timer > spawnData[level].spawnTime)
        {
            timer = 0;
            Spawn();
        }
    }
    void Spawn() //풀에서 몬스터를 정해진 스폰포인트에서 랜덤하게 pulling
    {
        GameObject Enemy = gamemanager.instance.poolmng.pulling(2);
        Enemy.transform.position = spwanPoint[Random.Range(1, spwanPoint.Length)].position;
        Enemy.GetComponent<Enemy>().Init(spawnData[level]);
    }

{% endhighlight %}

> spawner에서는 캐릭터 주위에 지정된 스폰 포인트들 중에서 랜덤하게 선택하여 pool에서 몬스터 prefab을 사용하여 생성하고, 게임 시간의 진행에 따라 스테이지가 변화하여 더 강한 몬스터를 소환하도록 하는 기능을 수행한다.

{% highlight c# %}

public void slot_refresh() { // 인벤토리 슬롯을 리스트와 동기화시켜줌
        for(int i=0; i<slots.Length; i++) {
            if(gemlist[i]!=null){
                slots[i].GetComponent<slot>().g=gemlist[i];
            }
            Debug.Log("slot refresh");  
        }
        //for(int i=gemcount; i<slots.Length; i++) {
           // slots[i].GetComponent<slot>().g=null;
        //}
    }

    public void gemlist_refresh() { //인벤토리 내 젬의 위치변경 등이 있을때 리스트에도 반영해줌
        gemcount=0;
        for(int i=0; i<slots.Length; i++) {
            gemlist[i]=slots[i].GetComponent<slot>().g;
            if(gemlist[i]!=null) gemcount++;
        }
        Debug.Log("gemlist refresh");  
    }

    public void add_gem(gemData gd) {
        if(gemcount<slots.Length) {
            for(int i=0; i<slots.Length; i++) {
                if(gemlist[i]==null) {
                    gemlist[i]=gd;
                    gemcount++;
                    break;
                }
            }
        }
        else {
            Debug.Log("slot full");
        }
    }

{% endhighlight %}

> 이 게임의 인벤토리는 인벤토리 내부의 젬 정보를 가지는 리스트와 실제 인벤토리를 켰을 때 ui내의 인벤토리 및 석판의 슬롯들로 이루어진다. 
이러한 구조를 가지는 이유는 기본적으로 게임 진행은 인벤토리를 비활성화한 채로 젬의 습득 등의 활동을 하게되는데 유니티에서는 비활성화된 오브젝트를 수정할 수 없기 때문이다. 
따라서 인벤토리 ui가 비활성화중일때도 젬을 습득하여 list에 넣고 인벤토리 ui를 킬 때 list에 담긴 젬 데이터를 ui의 슬롯들에 동기화시켜주는 작업이 필요한데 invenmanager가 그 작업을 수행한다. 
그 외에도 인벤토리 내부에서 드래그 등을 통해 슬롯간의 젬 위치변경이 있었을 때도 슬롯의 데이터와 리스트의 데이터를 동기화시켜주는 역할을 한다.

{% highlight c# %}

public void OnBeginDrag(PointerEventData eventData)
    { //슬롯에 젬이 있을시 슬롯을 클릭하면 draggedslot에 그 슬롯의 데이터를 복사해서 넘겨줌
        if(isfull) {
            if(this.gameObject.tag=="monoslot") begin_mono=true;
            draggedslot.instance.dragslot=this;
            draggedslot.instance.dragset(slot_img);
            draggedslot.instance.transform.position=eventData.position;
        }
    }

    public void OnDrag(PointerEventData eventData)
    { //마우스 이동에 따라 draggedslot이 이동
        if(isfull) {
            draggedslot.instance.transform.position=eventData.position;
        }
    }

    public void OnEndDrag(PointerEventData eventData)
    { //드래그가 끝났을 시 처음에 클릭했던 슬롯에서 발동하는 함수
    //드래그의 종착점이 monolith인지, 다른 슬롯인지에 따라서 필요한 절차를 진행
        if(draggedslot.instance.is_monolith==true && draggedslot.instance.is_change==false) {
            this.g=null;
            invenmanager.inventory.gemlist[slot_index]=null;
            draggedslot.instance.is_monolith=false;
        }
        else if(draggedslot.instance.is_change==true) {
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
        if(begin_mono) {
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
        if(draggedslot.instance.dragslot!=null && this.gameObject.tag=="monoslot") {
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
        else if(draggedslot.instance.dragslot!=null && this.gameObject.tag=="slot") {
            draggedslot.instance.change_idx=this.slot_index;
            draggedslot.instance.change_gd=this.g;
            this.g=draggedslot.instance.dragslot.g;
            draggedslot.instance.is_change=true;
        }
    }

{% endhighlight %}

> 우리가 생각하는 인벤토리의 기능(드래그하여 장착, 위치변경 등등)의 역할을 실제적으로 수행하는 slot 스크립트이다. 
구현하는데 꽤 애를 먹었는데 일단 유니티의 IBeginDragHandler, IDragHandler, IEndDragHandler, IDropHandler 라이브러리를 사용하여 슬롯의 드래그 앤 드롭 기능을 구현하였다. 
슬롯을 드래그할 때 드롭이 정상적으로 완수되지 않으면 슬롯이 그대로여야 하므로 슬롯 자체를 움직이는것이 아닌 평소에 투명화된 draggedslot 오브젝트를 생성하여 드래그를 시작한 slot의 데이터를 복사하여 담도록 하였다. 드래그가 끝났을 때 드래그를 시작한 슬롯에서 발동하는 onenddrag 함수와 onenddrag 함수보다 먼저 발동하면서 드래그가 끝난 위치의 슬롯에서  발동하는 ondrop 함수를 사용하여 드래그 앤 드롭을 통한 젬의 위치변경, 석판으로의 젬의 장착, 석판에서의 젬의 장착 해제 등 여러 기능을 구현하였다.

{% highlight c# %}

public void skill_use() { //gem color에 따라 종류에 맞는 함수를 발동시킴
        if(gem_color==1) {
            crt=StartCoroutine(projectile(2f));
        }
        else if(gem_color==2) {
            crt=StartCoroutine(magicuse(2f));
        }
        else if(gem_color==3) {
            crt=StartCoroutine(swing(3f));
        }
    }

    public void monolith_reset() { //인벤토리에서 monolith에 젬을 장착시켰을 때
    //슬롯의 젬 데이터를 monolith로 가져오는 함수
        Debug.Log("gem set");
        for(int i=0; i<3; i++) {
            gems[i]=mono_slots[i].g;
        }
    }

    public void monolith_clear() {
        this.damage=0;
        this.count=0;
        this.prefabid=0;
        this.gem_color=0;
        this.speed=0;
        this.radius=0;
        this.penet=0;
        this.element=0;
        this.force=3;
        curse.Clear();
        if(crt!=null) StopCoroutine(crt);
        if(spcrt!=null) special_manager.GetComponent<special>().StopCoroutine(spcrt);
    }
    public void monolith_active() {
        monolith_clear();
        //인벤토리를 끌 때 monolith가 가진 젬들을 계산하여 weaponmanager가 최종적으로 스킬을 발동함
        foreach(gemData gd in gems) {
            if(gd==null) continue;
            if(gd.isactive) {
                this.damage=gd.damage;
                this.count=gd.count;
                this.prefabid=gd.id;
                this.gem_color=gd.color;
                this.speed=gd.speed;
                this.radius=gd.radius;
                this.penet=gd.penet;
                this.element=gd.element;
                this.force=gd.force;
                skill_use();
            }
            else if(gd.ispassive) {
                if(gd.curse!=0) curse.Add(gd.curse);
                this.damage+=gd.damage;
                this.speed+=gd.speed;
                this.radius+=gd.radius;
                this.penet+=gd.penet;
                this.count+=gd.count;
                this.element=gd.element;
                this.force=gd.force;
            }
            else if(gd.isspecial) {
                spcrt=special_manager.GetComponent<special>().init(this);
            }
        }
    }

{% endhighlight %}

> 우리 게임의 차별화된 점, 특징이라 할 수 있는 젬 시스템의 실질적인 발동부분을 담당하는 weaponmanager 스크립트이다. 
weaponmanager 스크립트는 석판 하나를 담당하여 석판의 slot에 담긴 데이터를 인벤토리를 끌 때 monolith_reset 함수로 가져온다. 
그리고 가져온 젬 데이터들을 계산하여 젬 색깔에 따라 투사체를 발사하는 fire 코루틴, 근접무기를 휘두르는 swing 코루틴, 각 프리팹에 담긴 유니크한 발동을 수행하는 magic 코루틴 중 하나를 발동시킨다. 
발동한 스킬은 액티브 젬의 스탯, 패시브 젬의 추가 효과 등을 계산하여 최종적으로 나온 스탯 및 효과를 가지고 발동한다.
추후 링크 시스템을 추가하여 석판의 링크가 연결되지 않은 슬롯은 패시브 젬이 50%의 효과만 발휘하도록 추가할 예정이다.

# 여담

첫 프로젝트의 경험이 있지만 메인 프로그래머로서, 그리고 기획부터 시작하는 것은 처음이라 꽤나 걱정도 많고 힘들었지만 밑에서부터 하나 둘 쌓아올라가다 보니 어느새 기틀이 완성되었다. 팀원과의 조율, 게임 제작에 있어서 아트팀의 중요성, 우리가 아무렇지 않게 쓰는 게임의 기능들이 얼마나 정교하고 복잡하게 만들어져 있는지 등 많은 것을 배우고 있는 프로젝트이다. 부디 잘 완성되어 2학기 졸업프로젝트를 무사히 마감하길 바랄 뿐이다.