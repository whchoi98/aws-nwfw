# Network Firewall - 멀티VPC

## 구성  아키텍쳐 소개

Network Firewall의 기본 동작 이해를 위해, 가장 기본이 되는 디자인을 먼저 구성해 봅니다. 

아래 그림은 이번 Chapter에서 구성해 볼 아키텍쳐 입니다. 인터넷으로 부터 AWS 자원을 보호하기 위해, 네트워크 방화벽 서브넷을 배치하고 인터넷을 통과하는 모든 트래픽은 네트워크 방화벽을 통과하게 하는 일반적인 On Prem구성과 유사합니다.

![](.gitbook/assets/image%20%2880%29.png)

## ‌ Task1.Cloudformation 배포 ‌ 

Cloudformation을 통해 기본이 되는 VPC구성을 먼저 구성합니다.

### ‌ 1.구성 목표. ‌ 

먼저 아래의 Cloudformation을 배포합니다. Cloudformation 구성을 배포하게 되면 아래와 같은 구성이 완료됩니다.

‌ Routing Table 구성과 Network Firewall 구성은 다음단계에 별도로 진행하게 됩니다.

![](.gitbook/assets/image%20%2883%29.png)

아래 Github에서 실습에 사용할 Cloudformation yml 파일을 다운로드 받습니다.

```text
git clone https://github.com/whchoi98/aws-nwfw-source
```

### 2.Cloudformation 생성. 

#### 먼저 새로운 스택을 생성합니다.

![](.gitbook/assets/image%20%289%29.png)







