# Git이란?

프로젝트를 한다하면 Git 을 무조건적으로 사용한다.

그렇다면 Git을 왜쓸까? 이번주차에는 Git에 대해 잘 모르는 사람들, 헷갈리는 부분을 짚고 Git의 작동원리에 대해 알아보는 시간을 갖겠다.

## **Git이란?**

우선, 깃 공식 사이트인 [https://git-scm.com/](https://git-scm.com/) 에 들어가보면 다음과 같이 설명이 되어있다.

![https://blog.kakaocdn.net/dn/oCDVu/btstHaz5Bz8/UrYpyYQjDXHuRxmBYWxVdK/img.png](https://blog.kakaocdn.net/dn/oCDVu/btstHaz5Bz8/UrYpyYQjDXHuRxmBYWxVdK/img.png)

이를 번역해보면, `Git은 소규모 프로젝트부터 대규모 프로젝트까지 모든 것을 속도와 효율성으로 처리하도록 설계된 무료 오픈 소스 분산 제어 시스템`이라고 한다. 프로젝트에 사용하는 분산 버전 제어시스템이 핵심으로, 한마디로 정리하면 Git은 **분산형 버전 관리 시스템**이다. Git을 통해 프로젝트의 버전을 관리하는 것이다.

### **그렇다면 GitHub는 무엇일까?**

말그대로, Git에 중심지, 중추를 뜻하는 Hub라는 단어를 합쳐 만든 것으로 Git에 관리되는 모든 프로젝트들이 모인 곳을 의미한다. 즉, Git의 원격 저장소가 github인 것이다.

여러가지 git 저장소가 있지만 github이 가장 유명하며 그 외에 gitlab 또한 존재한다.

### **분산형 버전 관리는 왜할까?**

한 3가지 정도의 이유가 있을 것 같은데, 하나하나 살펴보면 다음과 같다.

![https://blog.kakaocdn.net/dn/bx3ANC/btstMk9Tfeg/ClLXO6ieU6Itf9wuvMaRVK/img.png](https://blog.kakaocdn.net/dn/bx3ANC/btstMk9Tfeg/ClLXO6ieU6Itf9wuvMaRVK/img.png)

### **오프라인에서도 작업이 가능하다.**

분산되어 버전을 관리할때 분산된 각각의 노드는 local pc 단위이다. 로컬에 설치된 git을 통해 버전을 관리하고 마지막에 최종적으로 원격 저장소에 push할때만 네트워크가 필요하므로, 네트워크 없이 오프라인으로도 작업이 가능하다.

### **버전 관리를 통한 백업이 가능하다.**

버전관리를 하게되면 v1, v2, v3등의 다양한 버전이 존재한다. 이때 v3에서 문제가 발생했다. 코드가 날아갔을수도, 서버측에 문제가 생겨서 서비스가 정상적으로 작동하지 않을수도 있다. 이때 버전관리가 되어있다면 v3 -> v2로 롤백해 버전을 되돌릴 수 있다. 백업이 가능한 것이다.

### **중앙화되지 않고 독립적으로 관리가 가능하다.**

대부분의 프로젝트를 진행할때를 생각해보자, 백엔드 개발자 3명이 모여서 한 화면에서 계속 개발을 하지 않는다. 한 개발자가 각자의 pc에서 기능 개발을 하고, 해당 내용을 원격에서 합치는 작업을 반복할뿐이다. 따라서, 분산형 버전 관리 시스템을 통해 독립적으로 프로젝트 관리가 가능하다.

---

### **Git의 작동원리와 명령어**

이제 git 명령어들에 대해 알아보자.

### **git init**

우선 `git init`이다. 아래 폴더는 비어있다.

![https://blog.kakaocdn.net/dn/bfuPvj/btstG65y4uH/HRsUBEGNWUeR9qMsAaeMEk/img.png](https://blog.kakaocdn.net/dn/bfuPvj/btstG65y4uH/HRsUBEGNWUeR9qMsAaeMEk/img.png)

해당 디렉토리에 프로젝트 코드를 추가하고, 프로젝트 버전관리를 하고 싶은데 이를 git에게 맡기고 싶을때 사용하는 명령어가 `git init`이다. 아래와 같이 명령어를 실행하면 **.git**이라는 폴더가 생기는데 해당 디렉토리는 git에 의해 버전관리가 되는 디렉토리로 변하게 된것이다.

![https://blog.kakaocdn.net/dn/cua6ci/btstKXNZ5eq/CJJ7VkIOIUq7KMnbRhc9N0/img.png](https://blog.kakaocdn.net/dn/cua6ci/btstKXNZ5eq/CJJ7VkIOIUq7KMnbRhc9N0/img.png)

즉, **일반 디렉토리 -> 버전관리가 가능한 디렉토리**로 변한 것이다.

이외에도 몇가지의 git 명령어들이 더 있는데 이는 git의 동작원리와 함께 알아보자.

그전에, **Tracking**이라는 개념에 대해 알고가자.

### **Tracking이란?**

특정 파일이 Tracking, Tracked상태라면 버전관리가 되고 있다는 뜻이다. git을 통해 버전관리가 되고 있는 파일을 Tracking되는 파일, 반대로 버전관리가 되고 있지 않는 상태를 **Untracked** 상태라고 한다.

### **Git의 작동원리**

git은 아래 그림과 같이 크게는 2개의 영역, 작게는 4개의 영역으로 구분된다.

우선, **Remote Repository**는 앞서 언급한 github와 같은 원격저장소를 의미한다.

**Working Directory:** 실제 코드를 작성하고 있는 작업 영역, 해당 위치에 있는 파일들은 tracking되는 상태가 아니다.

**Staging Area:** 버전관리가 될 파일들이 모여있는 임시저장소, Tracking되고 있는 상태지만 특정 버전으로 묶인 것은 아니다.

**.git directory:** Staging Area에 있는 파일들이 모여 하나의 commit된 버전들로 존재하는 로컬 영역.

![https://blog.kakaocdn.net/dn/bev6zV/btstC3gP0Pu/cTBc76C4cgSZi4VQUXTB1K/img.png](https://blog.kakaocdn.net/dn/bev6zV/btstC3gP0Pu/cTBc76C4cgSZi4VQUXTB1K/img.png)

해당 내용을 인지하고 나머지 git 명령어들을 알아보자.

### **git clone**

github와 같은 원격저장소에 올라온 오픈소스 코드를 로컬로 받아오는 명령어.

**Remote Repository -> Working Directory**로 파일을 불러오며, 명령어 실행 후에 로컬에 있는 파일들은 버전관리가 되고 있지 않는 untracked의 파일이다.

![https://blog.kakaocdn.net/dn/bsiA7E/btstzlaZZvm/Y9Qlf9YtYTb4cpnSwppKtK/img.png](https://blog.kakaocdn.net/dn/bsiA7E/btstzlaZZvm/Y9Qlf9YtYTb4cpnSwppKtK/img.png)

```
git clone <프로젝트 주소>
```

위와 같은 형태로 명령어를 실행한다.

### **git add**

버전관리가 되지 않는 일반 작업 파일들을 버전관리하겠다고, Tracking하겠다고 선언하는 명령어. Working Directory에 있는 파일을 임시저장소인 Staging Area로 이동시킨다.

![https://blog.kakaocdn.net/dn/qIVtV/btstKXUL5Zt/dQchy4kyltRq7cmKCrFGD1/img.png](https://blog.kakaocdn.net/dn/qIVtV/btstKXUL5Zt/dQchy4kyltRq7cmKCrFGD1/img.png)

```
git add <파일 이름>
```

위의 형태로 명령어를 실행한다.

### **git commit**

Staging Area 에 있는 파일을 하나의 버전으로 묶어서 .git directory로 이동시키는 명령어. 지금까지 tracking한 파일을 하나의 버전으로 묶어 로컬에 알리는 명령어이다.

![https://blog.kakaocdn.net/dn/co4LJW/btstxOEJ6zb/OgleSJk2R7Hc4cfbGKlcx0/img.png](https://blog.kakaocdn.net/dn/co4LJW/btstxOEJ6zb/OgleSJk2R7Hc4cfbGKlcx0/img.png)

```
git commit -m "커밋 메시지"
```

위와 같은 형태로 명령어를 작성하며 **commit 메시지 컨벤션**을 팀원들과 사전에 협의 후, 프로젝트를 진행해 자신이 작성한 코드가 어떤 코드인지, 알 수 있게 설정한다.

### **git push**

local에 만든 하나의 버전을 remote에 저장하는 명령어. 이전 commit 명령어를 통해 만들어놨던 버전을 remote로 업데이트 하는 명령어이다.

![https://blog.kakaocdn.net/dn/xPRyo/btstyxizXZV/6AqcCo9v4nWpKVXTgMTrLK/img.png](https://blog.kakaocdn.net/dn/xPRyo/btstyxizXZV/6AqcCo9v4nWpKVXTgMTrLK/img.png)

### **git pull**

원격 저장소에 최신 업데이트된 내용을 로컬로 받아오는 명령어. Remote에 생긴 변화를 local로 동기화 하는 명령어이다.

![https://blog.kakaocdn.net/dn/cxKOhm/btstC3BaOxv/qUmm1gGF2AGwIVMd8izOaK/img.png](https://blog.kakaocdn.net/dn/cxKOhm/btstC3BaOxv/qUmm1gGF2AGwIVMd8izOaK/img.png)

### **이런 경우는 어떨까?**

내가 새로운 버전을 만들어 a -> b로 commit을 한 상태인데, 원격에선   a -> c로 새로운 기능이 추가된 상태이다.

이때 바로 push 명령어를 사용하면 충돌이 발생한다. 그대로 덮어쓰기가 불가능하기 때문이다. 따라서, 이를 방지하기 위해 우선 pull 명령어를 사용해 최신 내용을 로컬에 업데이트 받아야한다.

그러면 다음과 같이 2개의 경로가 생긴다. a -> b, a -> c 이를 merge를 통해 b->d, c->d로 합친 하나의 버전을 새로 만들고 이 내용을 다시 remote에 push하면 push가 정상적으로 이루어진다.