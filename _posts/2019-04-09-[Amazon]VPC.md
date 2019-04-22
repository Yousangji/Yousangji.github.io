---
title: "[Amazon] SQS-SpringBoot"
date: 2019-01-25 18:26:28 -0400
layout: home
categories:
  - Amazon
---

#### Amazon VPC?

Virtual Private Cloud는 AWS 클라우드에서 가상 프라이빗 네트워크망을 만들어줄 수 있게 하는 서비스.
EC2와 같은 AWS 리소스를 VPC에서 실행할 수 있는데, IP 주소 범위와 VPC 범위를 설정하고, 서브넷을 추가해 보안 그룹을 연결한다음 라우팅 테이블을 구성한다. 

`서브넷` : VPC의 IP 주소 범위, 퍼블릭 서브넷 혹은 프라이빗 서브넷으로 설정할 수 있음. 하나의 VPC 안에는 여러개의 가용영역이있을 수 있고, 하나의 가용역역 안에 여러개의 서브넷을 생성할 수 있다. 하지만 서브넷을 가용영역에 걸쳐 생성할 순 없다.

`라우트 테이블` : 각 서브넷에는 인터넷을 연결해주는 라우터가 생성되고 이 라우터는 라우터 테이블을 가지고 있다. 라우터 테이블은 중복적용이 가능하다. 
 라우팅 경로는 target에 타입에 따라 나뉜다. 
 Local : VPC 내의 다른 subnet으로 라우팅
 Internet Gateway : 외부 인터넷으로 라우팅
 Virtual private gateway : 관리자가 정의한 목적지로 라우팅. VPN 연결을 위한 VPN gateway