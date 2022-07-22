# Assignment

> System/System Hacking Techniques

### 1. NOP Sled

- NOP는 어셈블리어 명령어로 "No Operation"의 약자이다.
- 말 그대로 프로그램 실행에 아무러너 영향을 주지 않는 명령어이다.
- **프로그램 작동 중에 NOP 명령을 만났을 시 해당 명령어를 수행하지 않고 무시하고 다음 명령어로 넘어가게 된다.**

<br/>

- NOP sled는 해당 NOP명령어를 이용해 고의적으로 프로그램의 실행 흐름을 다음 명령어들로 흘러 보내는 것이다.
- ASLR이 걸려 있어 주소값이 계속 변경되는 환경에서 return-to-shellcode 기법이나 heqp Spray 형태의 공격 코드들에서 반복구문과 NOP sled를 쓴다.

  ↳ 임의의 메모리를 참조하게 되면 해당 위치에 무조건 악의적인 행위를 실행하는 코드가 있을 수만은 없다.

  ↳ NOP sled를 이용하여 참조한 위치에 악의적인 행위 코드가 없으면 자연스럽게 실행 흐름을 타고 들어간다.

  ↳ NOP 명령어들을 타고 흘러가다 악의적인 행위 코드를 포함하고 있는 위치로 갈 수 있다.

  ↳ Shell Code의 주소를 정확히 알아내기 힘들 경우 큰 메모리를 확보하여 Shell Code의 주소의 오차 범위를 크게 만들 때 사용한다.

<br/>

    - 스택은 높은 주소 ⇾ 낮은 주소
    - 메모리상에서 데이터는 낮은 주소 ⇾ 높은 주소

<br/>

### 2. RTL (Return-to-Libc)

- 프로그램이 정상적으로 종료되면 돌아갈 주소를 담고있는 RET영역에 메모리에 적재되어 있는 공유 라이브러리 함수의 주소로 변경하여 해당 함수를 호출하는 방식의 기법이다.

- 해당 기법은 DIP/NXbit 메모리 보호기법을 우회할 수 있다.

  함수 에필로그를 알면 이 기법을 더 자세히 이해할 수 있다.

  - 에필로그 과정은 push ebp, mov ebp, esp로 이루어진다.
  - 에필로그 과정은 leave, ret 과정을 거친다.

    <br/>

  - leave는 mov esp, ebp / pop ebp의 구조로 이루어져 있다.
  - mov를 통해 esp와 ebp의 위치를 같게 만들어 스택 상태를 정리해준다. (스택에 존재하는 지역 변수들을 정리해줌)
  - pop ebp를 통해 스택에 갖아 위에 있던 값을 ebp에 넣게된다.
  - 현재 스택상태를 정리해준 후 esp와 ebp의 위치가 같을 경우 스택의 가장 위에 있는 값은 SFP(이전 함수의 ebp의 주소가 저장되어 있음)가 된다.

    <br/>

  - ret는 pop eip / jum eip 로 구성되어 있다.
  - leave 단계를 거쳐 현재 스택에는 ret만 남아있을 것이다.
  - esp는 스택 가장 위에 남아있는 ret을 가르키고 있을 것이고
  - ebp는 이전 함수의 ebp 주소가 담겨져있으니 이전 함수의 ebp로 돌아갈 것이다.
  - 그 이후에 ret 단계를 처리하게 된다.
  - pop eip를 통해 ret를 eip에 넣게 된다.
  - ret에는 함수가 끝나고 돌아갈 주소가 담겨져 있을 것이고 eip는 다음에 실행해야할 명령어를 저장하는 레지스터이다.
  - 다음에 실행할 명령어에 돌아갈 주소가 담겨져 있게 되며, Jum eip를 통해 eip로 이동되고 eip가 실행되며 ret에 저장되어 있는 주소값을 찾아가게 된다.
  - ret에는 해당 함수가 끝나고 main으로 돌아갔을 때 함수의 역할이 끝난 그 후 실행해야할 명령어의 주소가 담겨져 있게 되는 것이다.

<br/>

- 해당 기법은 RET 영역에 system(), execv() 함수의 주소를 악의적으로 저장하게 되며, RET영역을 넘어선 영역까지 덮을 수 있어야 한다.
- 일반적인 payload는 | buf[64] | sfp[4] | ret[4] | argv[0] | . . . 이러한 형식의 스택 구조를 가지고 있다.

<p align="center"><img src = "https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/RTL.png?raw=true" width="30%" height="20%"></p>

- 여기서 "/bin/sh"은 해당 system() 함수의 인자값(파라미터)으åå로 받아오는 값이다.

<br/>

### 3. GOT overwrite

- GOT overwrite 기법은 PLT, GOT의 GOT를 조작하여 사용하는 기법이다.

  - PLT - 외부 프로시저를 연결해주는 테이블
  - GOT - PLT가 참조하는 테이블

- 이 기법은 PLT와 GOT는 Dynamic Link 방식에서 공유라이브러리를 사용한다.
- plt가 참조하는 got 영역의 주소가 본래의 실제함수 주소가 아닌 악의적으로 저장해 놓은 함수를 참조하는 것이다.
- 사용자가 read() 함수를 실행시키지만, 내부적으로 system() 함수가 실행되는 효과를 가져온다.

<br/>

### 4. SFP overflow (Stack Frame Pointer)

- SFP overflow는 단 한 바이트 만으로 IP(Instruction Pointer)의 흐름을 제어할 수 있는 공격 기법이다.
- SFP의 마지막 한 바이트를 쉘 코드의 주소가 저장되어 있는 스택의 주소로 변조한다면 함수 에필로그에 인해 흐름을 제어할 수 있게 된다.
- 여기서 SFP란 Saved Frame Pinter라고도 불리며, EBP 주소를 저장하고 있는 공간이다.
- 스택은 함수에서 사용하는 영역으로, 함수의 Return Address 지역 변수 등이 저장되며 사용되는 공간으로 **스택 프레임**이란 바로 함수가 호출될 때, 그 함수만의 스택 영역을 구분하기 위해서 생성되는 공간이다.(쉽게 말해 함수 각각의 영역을 표현하는 것임)
- SFP Overflow는 SFP의 1byte 만으로 간단하면서도 파괴적인 공격이 가능하다.

<br/>

### 5. UAF

- 프로그램이 실행되면 실행에 필요한 정보들이 Memory 영역에 올라가게 되는데 Code영역, Data영역, Stack영역, Heap영역이 있다.

  - Code영역 - 프로그램의 컴파일된 기계어 코드가 올라가는 곳
  - Data영역 - Global variable과 Static variable이 할당되는 곳
  - Stack영역 - Local variable과 Parameter가 저장되는 곳
  - Heap영역 - 빈 공간으로 필요에 따라 동적으로 메모리를 할당/해제하는 곳

- Use After Free (UAF)
  - 줄여서 UAF라 부른다.
  - Heap 영역에서 할당된 공간(malloc)을 free로 영역을 해제하고, 메모리를 다시 할당시 같은 공간을 재사용 하면서 생기는 취약점이다.
