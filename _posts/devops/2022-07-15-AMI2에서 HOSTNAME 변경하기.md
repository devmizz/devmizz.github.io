---
title: "AMI2에서 HOSTNAME 변경하기"

categories:
  - DevOps

tags:
  - EC2
  - HOSTNAME

toc: true
toc_label: 목차
toc_sticky: true
toc_icon: list

date: 2022-02-21
last_modified_at: 2022-02-22
---

참고도서: 스프링 부트와 AWS로 혼자 구현하는 웹 서비스

## 변경 전

![](https://images.velog.io/images/beatoncheeze/post/089f74cd-965d-4e69-81b4-a299f8a17d0e/image.png){: .align-center}

터미널에서 EC2를 접속하면 위의 사진과 같이 'ip-000.000.000.000'과 같은 ip 주소가 나오게 된다. 너무 길고 미관상(?) 보기 좋지 않다. 책의 필자분도 같은 생각을 하신 거 같다.

> 여러 서버를 관리 중일 경우 IP만으로 어떤 서비스의 서버인지 확인이 어렵습니다.
> 그래서 각 서버가 어느 서비스인지 표현하기 위해 HOSTNAME을 변경하겠습니다.

## AMI2에서

```
sudo vim /etc/sysconfig/network
```

![](https://images.velog.io/images/beatoncheeze/post/52da4aa1-f9d8-4942-81b9-0ffbefa1dc59/image.png){: .align-center}

책과는 다르게 HOSTNAME이 없다.

책은 AMI 1을 기반으로 하기 때문인데, AMI 2에서는 아래와 같이 입력을 해야한다.

```
sudo hostnamectl set-hostname [원하는 이름]
```

그 후에 reboot를 통해 확인해도 되지만, 간단하게 `hostname` 명령어를 입력해서 변경 여부를 확인해 볼 수도 있다.
