# root-ssh 직접 접속 제한
root에 대한 pw 무작위 대입 공격을 막기 위해 사용한다.

1. 접속을 제한하기 전 root 계정으로 전환을 할 수 있는 일반 사용자를 만든다
    $ useradd testuser
    
2. su 명령을 통해 root 권한을 부여받을 wheel 그룹을 생성한다.
  - 우분투의 경우 default wheel 그룹이 존재하지 않아 생성 해줘야함.
    $ groupadd wheel
    
3. su 명령 실행파일의 권한을 변경한다.
    $ chmod 4755 /bin/su
    $ chgrp wheel /bin/su
    
4. testuser 계정의 그룹을 변경한다.
    $ usermod -G wheel testuser
    
5. sudoers 수정 (sudo 명령을 통해 root 권한을 획득할 수 있는 user list를 정의해준다.)
    $ vi /etc/sudoers
    root     ALL=(ALL:ALL) ALL 아래
    testuser ALL=(ALL:ALL) ALL 추가
    
6. sshd_config 수정
    $ vi /etc/ssh/sshd_config
    PermitRootLogin yes ==> PermitRootLogin no 로 변경
    
7. ssh 서비스 재시작
    $ service ssh restart
    
향후 접속은 testuser로 하여 sudo -i 명령을 이용해 root 권한을 얻어 쓴다.
