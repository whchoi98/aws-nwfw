---
description: 'Update : 2020-12-15'
---

# Network Firewall 기본 구성

## 구성  아키텍쳐 소개

Network Firewall의 기본 동작 이해를 위해, 가장 기본이 되는 디자인을 먼저 구성해 봅니다. 

아래 그림은 이번 Chapter에서 구성해 볼 아키텍쳐 입니다. 인터넷으로 부터 AWS 자원을 보호하기 위해, 네트워크 방화벽 서브넷을 배치하고 인터넷을 통과하는 모든 트래픽은 네트워크 방화벽을 통과하게 하는 일반적인 On Prem구성과 유사합니다.

![\[Network Firewall &#xAE30;&#xBCF8; &#xAD6C;&#xC131; &#xC544;&#xD0A4;&#xD14D;&#xCCD0;\]](.gitbook/assets/image%20%2810%29.png)

## Cloudformation 배포

Cloudformation을 통해 기본이 되는 VPC구성을 먼저 구성합니다.

### 1.구성 목표. 

먼저 아래의 Cloudformation을 배포합니다. Cloudformation 구성을 배포하게 되면 아래와 같은 구성이 완료됩니다.

Routing Table 구성과 Network Firewall 구성만 별도로 진행하게 됩니다.

![\[Cloudformation &#xAE30;&#xBC18;&#xC758; &#xBC30;&#xD3EC; &#xC544;&#xD0A4;&#xD14D;&#xCCD0;\]](.gitbook/assets/image%20%286%29.png)

아래 Github에서 실습에 사용할 Cloudformation yml 파일을 다운로드 받습니다.

```text
git clone https://github.com/whchoi98/aws-nwfw-source
```

### 2.Cloudformation 생성. 

먼저 새로운 스택을 생성합니다.

![](.gitbook/assets/image%20%289%29.png)

앞서 다운로드 받은 git 파일 중에서 ``**`"singleaz-vpc1-az-a.yml"`** 파일을 업로드 합니다.

![](.gitbook/assets/image%20%282%29.png)

stack의 상세내용을 정의합니다.

* stack name : Stack name을 정의합니다.
* VPC Parameters - AvailablilityZoneA : AZ를 선택합니다.
* KeyPair:사용할 KeyPair를 선택합니다. \(사전에 keypair를 생성해 두어야 합니다.\)
* LatestAmiId: 최신의 Amazon Linux2 이미지가 자동 선언됩니다.

![](.gitbook/assets/image%20%2813%29.png)

Cloudformation이 IAM에 접근하여 사용할 수 있도록 체크합니다.

![](.gitbook/assets/image%20%2811%29.png)

10분 후면 모든 자원이 생성됩니다.

![](.gitbook/assets/image%20%2812%29.png)

{% hint style="info" %}
**본 랩에서는 EC2의 자원들에 손쉽게 접근 할 수 있도록 모두 Session Manager 접근 구성을 Cloudformation으로 배포합니다. AWS 콘솔이나, 다른 배포 도구로 구성하셔도 랩을 진행하는데는 이슈가 없습니다.**
{% endhint %}

## Network Firewall 기본 구성. 

먼저 Network Firewall을 생성합니다. 기본 Firewall Policy가 생성되어 있지 않기 때문에, 함께 생성합니다.

### 1.Network Firewall 및 Policy 생성.

**`service - VPC - AWS Network Firewall - Firewall - Create`** 를 선택하고, Firewall과 Firewall Policy를 생성합니다.

![](.gitbook/assets/image%20%288%29.png)

먼저 Firewall을 생성하고, Firewall Policy 생성하여 연결합니다. 기존에 Firewall Policy가 있다면 생성한 Firewall에 연결할 수 있습니다.

1. **Name** : 방화벽 이름을 정의합니다. 
2. **Description\(Optional\)** : 방화벽에 대한 설명을 정의합니다.
3. **VPC** : 생성한 VPC를 선택합니다. \(eg. VPC1\)
4. **Firewall subnets - Availability Zone** : AZ Zone을 선택합니다. \(eg. us-west-2a\) **Firewall subnets - Subnet** : 생성한 방화벽용 Subnet을 선택합니다. \(eg. VPC1-FWSubnet1\)
5. **New Firewall policy name :** 신규 생성한 NWFW의 방화벽 정책이름을 정의합니다.
6. **Description\(Optional\)** : 방화벽 정에 대한 설명을 정의합니다.   
7. **Firewall Tag :** Firewall 자원에 대한 Tag를 정의합니다.

![](.gitbook/assets/image%20%287%29.png)

![](.gitbook/assets/image.png)

### 2.Network Firewall 확인. 

방화벽을 생성하고 나면, **`provisoning`** 상태가 진행되며 완료까지 5분 내외가 소요됩니다.

![](.gitbook/assets/image%20%281%29.png)

정상적으로 설치되면 아래 그림처럼 **`Status:Ready`** 상태로 변경됩니다.

![](.gitbook/assets/image%20%285%29.png)

생성한 Firewall을 선택하고 **`Firewall details`** 를 선택하면, 해당 서브넷에 Endpoint가 정상적으로 생성된 것을 확인 할 수 있습니다.

![](.gitbook/assets/image%20%283%29.png)

**`Service-VPC-Virtual Private Cloud-Endpoint`** 메뉴에서 Firewall Endpoint를 확인 할 수 있습니다.

![](.gitbook/assets/image%20%284%29.png)

{% hint style="info" %}
**Endpoint 메뉴에서 특이점을 발견할 수 있습니다. Endpoint type이 GatewayLoadBalancer 라는 것입니다. 이것은 Firewall Endpoint가 별도로 생성되지 않고, GatewayLoadBalancer를 그대로 사용한다는 것입니다. 즉 동작방식이 GatewayLoadBalancer를 이용한다는 것을 알 수 있습니다.**
{% endhint %}

## Route Table 구성

이제 라우팅 테이블을 정의하고, 인터넷과 EC2간의 통신을 확인해 봅니다.

![](.gitbook/assets/image%20%2822%29.png)

### 1. VPC Ingress 라우팅 테이블 구성. 

외부 인터넷 트래픽인 Firewall Endpoint를 경유하도록 라우팅 테이블을 구성하기 위해서는 InternetGateway 의 라우팅 테이블 구성이 필요합니다. AWS에서는 이러한 Edge에서의 라우팅 테이블 구성이 가능하도록 VPC Ingress Routing을 지원합니다.

먼저 VPC Ingress Route Table을 생성합니다.

**`Service - VPC - Virtual Private Cloud - Route Table - Create route table`**

![](.gitbook/assets/image%20%2835%29.png)

신규 생성한 InternetGateway용 라우팅 테이블을 선택하고, **`Edge Associations`** 를 선택합니다.

![](.gitbook/assets/image%20%2827%29.png)

InternetGateway용 라우팅 테이블을 InternetGateway\(이하 IGW\)에 연결합니다.

![](.gitbook/assets/image%20%2815%29.png)

연결하면 아래와 같이 정상적으로 IGW에 라우팅 테이블\(Ingress Routing\)이 생성됩니다.

![](.gitbook/assets/image%20%2829%29.png)

이제 인터넷에서 유입되는 트래픽이 Firewall Endpoint를 향하도록 Ingress Routing을 설정합니다.

**`Route - Edit Routes`** 를 선택하고 수정합니다.

![](.gitbook/assets/image%20%2816%29.png)

목적지는 **`0.0.0.0/0`**을 설정하고, Target은 **`Gateway Load Balancer Endpoint`**를 선택합니다.​

{% hint style="info" %}
**Target이 Gateway Load Balancer Endpoint가 되어야 하는 이유는 앞서 설명하였습니다.**
{% endhint %}

![](.gitbook/assets/image%20%2834%29.png)

Gateway Load Balancer Endpoint를 선택하게 되면, Network Firewall을 생성한 이후에 자동 생성된 VPC Endpoint를 확인할 수 있습니다. 해당 Endpoint를 선택하고 라우팅 테이블을 완료합니다.

![](.gitbook/assets/image%20%2824%29.png)

이제 10.1.1.0/24 로 외부에서 인입되는 트래픽은 모두 Firewall을 경유하게 됩니다.

![](.gitbook/assets/image%20%2819%29.png)

### 2. FW Subnet 라우팅 테이블 구성. 

FW Subnet은 인터넷으로 향하는 트래픽에 대한 라우팅 생성을 합니다.

![](.gitbook/assets/image%20%2823%29.png)

![](.gitbook/assets/image%20%2825%29.png)

![](.gitbook/assets/image%20%2817%29.png)

### 3. Protect Subnet 테이블 구성. 



![](.gitbook/assets/image%20%2826%29.png)

![](.gitbook/assets/image%20%2821%29.png)

![](.gitbook/assets/image%20%2833%29.png)

### 4. 트래픽 흐름 확인   



![](.gitbook/assets/image%20%2818%29.png)

![](.gitbook/assets/image%20%2832%29.png)

## Network Firewall 상세 구성

### 1.Firewall 구성 이해

### 2.Firewall Rule의 이해와 구성



## Cloudwatch Monitoring



