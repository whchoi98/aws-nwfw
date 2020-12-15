# Network Firewall 기본 구성

## 구성  아키텍쳐 소개

![\[Network Firewall &#xAE30;&#xBCF8; &#xAD6C;&#xC131; &#xC544;&#xD0A4;&#xD14D;&#xCCD0;\]](../.gitbook/assets/image%20%287%29.png)





## Cloudformation 배포

### 1.구성 목표. 

먼저 아래의 Cloudformation을 배포합니다. Cloudformation 구성을 배포하게 되면 아래와 같은 구성이 완료됩니다.

![\[Cloudformation &#xAE30;&#xBC18;&#xC758; &#xBC30;&#xD3EC; &#xC544;&#xD0A4;&#xD14D;&#xCCD0;\]](../.gitbook/assets/image%20%283%29.png)

### 2.Cloudformation 생성. 

![](../.gitbook/assets/image%20%286%29.png)

![](../.gitbook/assets/image%20%282%29.png)

![](../.gitbook/assets/image%20%2810%29.png)

![](../.gitbook/assets/image%20%288%29.png)

![](../.gitbook/assets/image%20%289%29.png)



## Network Firewall 기본 구성. 

### 1.Network Firewall 생성. 



Service - VPC - AWS Network Firewall - Firewall - Create

![](../.gitbook/assets/image%20%285%29.png)

먼저 Firewall을 생성하고, Firewall Policy 생성하여 연결합니다. 기존에 Firewall Policy가 있다면 생성한 Firewall에 연결할 수 있습니다.

![](../.gitbook/assets/image%20%284%29.png)

![](../.gitbook/assets/image.png)

### 2.Network Firewall 확인. 

![](../.gitbook/assets/image%20%281%29.png)

## Route Table 구성

### 1. VPC Ingress 라우팅 테이블 구성. 

### 2. FW Subnet 라우팅 테이블 구성. 

### 3. Protect Subnet 테이블 구성. 



## Network Firewall 상세 구성

### 1.Firewall 구성 이해

### 2.Firewall Rule의 이해와 구성



## Cloudwatch Monitoring



