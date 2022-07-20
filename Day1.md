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

```shell
 $ ni
```

![01-02](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/01-02.png?raw=true)

![01-03](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/01-03.png?raw=true)

![01-04](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/01-04.png?raw=true)

![cmp](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/01-cmp.png?raw=true)

- compare에서 입력된 값과 0x149a가 일치하는지 확인
- 16진수를 10진수로 바꾸면 5274

![result](https://github.com/YYYEJI/GBC_SECURITY/blob/master/img/01-result.png?raw=true)

- Password OK :) // 성공

<br/>

### 2 - crackme0x02

### 3 - crackme0x03
