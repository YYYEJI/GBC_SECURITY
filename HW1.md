# Assignment

```shell
 $ docker run -it --privileged ccss17/security-tutorial
 $ cd crackme
```

<br/>

### 1 - crackme0x01

```shell
 $ gdb crackme0x01
 $ b main
 $ run
```

![01-01](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/01-01.png?raw=true)

- break point 걸어주고 run 시킨 화면

```shell
 $ ni
```

![01-04](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/01-04.png?raw=true)

- 비밀번호를 입력받는 scanf 함수가 나옴

![cmp](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/01-cmp.png?raw=true)

- compare에서 입력된 값과 0x149a가 일치하는지 확인
- 16진수를 10진수로 바꾸면 5274

![result](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/01-result.png?raw=true)

- Password OK :) // 성공 ! ! !

<br/>

### 2 - crackme0x02

![com](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/02-cmp.png?raw=true)

- compare 값 확인해보고
- 내가 입력한 값이 아닌 다른 값이 password
- 주소 안에 있는 값을 확인

![result](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/02-result.png?raw=true)

- Password OK :) // 성공 ! ! !

### 3 - crackme0x03

![scan](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/03-scan.png?raw=true)

- 사용자에게 값을 입력받고

![test](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/03-test.png?raw=true)

- test 함수에는 argument 4개 존재
- 첫 번째 인자는 입력받은 값
- 두 번째 인자는 비교하는 값 (52b24 -> 10진수로 변환해서 password 입력)

![result](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/03-result.png?raw=true)

- Password OK :) // 성공 ! ! !
