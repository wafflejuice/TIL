# 2023-02-26
## swap memory
swap memory : 디스크 일부를 확장된 memory처럼 사용할 수 있게 해주는 기술
커널은 실제 memory에 올라와 있는 memory 블록들 중에서 당장 쓰이지 않는 것을 디스크에 저장하여, 사용 가능한 memory 영역을 늘립니다.

memory 및 swap memory 확인
```
$ free -h
               total        used        free      shared  buff/cache   available
Mem:           966Mi       178Mi       160Mi       0.0Ki       627Mi       641Mi
Swap:             0B          0B          0B
```

```
// swap 파일을 생성해 (메모리 상태 확인 시 swap이 있었지만 디렉토리 파일은 만들어줘야한다.)
$ sudo mkdir /var/spool/swap
$ sudo touch /var/spool/swap/swapfile
$ sudo dd if=/dev/zero of=/var/spool/swap/swapfile count=2048000 bs=1024

// swap 파일을 설정
$ sudo chmod 600 /var/spool/swap/swapfile
$ sudo mkswap /var/spool/swap/swapfile
$ sudo swapon /var/spool/swap/swapfile

// swap 파일을 등록
$ sudo vi /etc/fstab

// 파일 하단에 다음 내용 입력 후 저장
/var/spool/swap/swapfile    none    swap    defaults    0 0

// 메모리 상태 확인
$ free -h
               total        used        free      shared  buff/cache   available
Mem:           966Mi       155Mi        69Mi       0.0Ki       741Mi       647Mi
Swap:          2.0Gi       0.0Ki       2.0Gi
```
