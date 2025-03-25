# 📡 정적 라우팅 네트워크 구축 프로젝트

> CCCR CAB·TA 4기 - 오픈소스 기반 클라우드 플랫폼(PaaS) 엔지니어 과정  
> 작성자: 강민석  
> 날짜: 2025.03.20

---

## 📌 프로젝트 개요

이 프로젝트는 **정적 라우팅(Static Routing)** 을 사용해 여러 서브넷 간 통신이 가능하도록 설정한 네트워크 인프라 구축 실습입니다.  
예측 가능한 경로 설정을 통해 안정적인 데이터 전송을 목표로 했습니다.

---

## 🎯 프로젝트 목표

- 서브넷 분할을 통한 효율적인 IP 주소 설계  
- `ip route` 명령어를 활용한 정적 라우팅 구성  
- 라우터 간 패킷 전달 경로를 수동 설정  
- `ping`, `show ip route` 명령어로 연결 및 라우팅 확인  

---

## 🧱 네트워크 구성 요소

- **라우터**: 4대  
- **스위치**: 4대  
- **PC**: 8대  
- 각 장비는 별도 서브넷에 연결되어 있고, **정적 라우팅으로 통신 가능**

---

## 🗺️ 네트워크 토폴로지

네트워크 구조는 총 **7개의 서브넷(A~G)** 으로 구성되어 있습니다.

![Image](https://github.com/user-attachments/assets/bdb5a988-e74a-4a0a-83a2-1003d00d4f53)

---

## 🧮 서브넷 구성표

| Network | Address        | Prefix | Subnet Mask         | IP Range              | Broadcast         |
|---------|----------------|--------|----------------------|------------------------|-------------------|
| A       | 192.168.0.192  | /26    | 255.255.255.192      | 192.168.0.193 ~ .254   | 192.168.0.255     |
| B       | 192.168.0.128  | /26    | 255.255.255.192      | 192.168.0.129 ~ .190   | 192.168.0.191     |
| C       | 192.168.0.64   | /26    | 255.255.255.192      | 192.168.0.65 ~ .126    | 192.168.0.127     |
| D       | 192.168.0.32   | /27    | 255.255.255.224      | 192.168.0.33 ~ .62     | 192.168.0.63      |
| E       | 192.168.0.0    | /30    | 255.255.255.252      | 192.168.0.1 ~ .2       | 192.168.0.3       |
| F       | 192.168.0.4    | /30    | 255.255.255.252      | 192.168.0.5 ~ .6       | 192.168.0.7       |
| G       | 192.168.0.8    | /30    | 255.255.255.252      | 192.168.0.9 ~ .10      | 192.168.0.11      |

---

## 🛠️ 정적 라우팅 설정 예시

```bash
# R1 설정 예시
R1(config)# ip route 192.168.0.64 255.255.255.192 192.168.0.2
R1(config)# ip route 192.168.0.32 255.255.255.224 192.168.0.2
```

---

## 🧪 네트워크 테스트 결과

```bash
# PC0 → PC3, PC5
PC0> ping 192.168.0.130
PC0> ping 192.168.0.66

# PC6 → PC1, PC4
PC6> ping 192.168.0.194
PC6> ping 192.168.0.65

# 라우팅 테이블 확인 (R1)
R1# show ip route

S 192.168.0.128/26 [1/0] via 192.168.0.2
S 192.168.0.64/26  [1/0] via 192.168.0.2
S 192.168.0.32/27  [1/0] via 192.168.0.2
C 192.168.0.0/30 is directly connected, GigabitEthernet0/1
```

✅ `Reply from` 메시지 확인 → **네트워크 연결 성공**  
✅ `S` 경로 확인 → **정적 라우팅 정상 설정**

---

## 🧾 주요 명령어 요약

| 명령어          | 설명                              |
|-----------------|-----------------------------------|
| `ip route`      | 정적 라우팅 경로 수동 설정         |
| `ping`          | 장비 간 연결 테스트                |
| `show ip route` | 라우팅 테이블 확인 및 경로 점검     |

---

## ⚠️ 한계점 및 개선점

- ❌ 정적 라우팅은 네트워크 변화 시 수동 수정이 필요하여 **확장성 부족**  
- ✅ OSPF(Open Shortest Path First) 같은 **동적 라우팅 프로토콜** 도입 시, 자동 경로 학습 가능

```bash
# OSPF 예시
R1(config)# router ospf 1
R1(config-router)# network 192.168.0.0 0.0.0.255 area 0
```

---

## 📹 데모 영상

👉 [네트워크 통신 데모 영상 보기](https://youtu.be/BbSJFL_IDs0)  
📱 QR 코드로도 확인 가능:

<p align="center">
  <img src="https://github.com/user-attachments/assets/12927108-079b-4aca-8e2c-e6532be1f633" width="300"/>
</p>

---

## 🧩 Appendix (부록)

- `config/` 디렉토리에 각 라우터 설정 파일 포함 예정

<p align="center">
  <img src="https://github.com/user-attachments/assets/f990a254-b89a-4bdb-9ca7-6dfb6d0ed62a" width="300"/>
  <img src="https://github.com/user-attachments/assets/614f5d15-40d5-4aca-a931-21d53b3067e9" width="300"/>
</p>

- 'test/' 디렉토리에 ping 결과 및 라우팅 로그 포함 예정
- 
<p align="center">
  <img src="https://github.com/user-attachments/assets/bd046c50-26ee-40c5-9bb0-5b6f4bc544a8" width="300"/>
  <img src="https://github.com/user-attachments/assets/5dbffc47-4159-4009-a920-a393955293c8" width="300"/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/938d5ffa-f7a0-44f6-a419-e2cda92e7424" width="300"/>
  <img src="https://github.com/user-attachments/assets/7fe5029f-0d67-4a24-abec-2f80bd3ea8bc" width="300"/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/7a6bde28-4e1d-480d-8564-85aab226fa06" width="300"/>
  <img src="https://github.com/user-attachments/assets/63c3c52b-8cde-4856-9903-02582f60bed6" width="300"/>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/4d86ff69-d4c0-48ea-992b-1b569f452e0d" width="300"/>
  <img src="https://github.com/user-attachments/assets/c6877ed5-5518-409c-8ba4-ffdbbe6c5c29" width="300"/>
</p>

---

## 📁 디렉토리 구조 (예정)

```
📁 static-routing-project
├── README.md
├── topology.png
├── config/
│   ├── R1_config.txt
│   ├── R2_config.txt
├── test/
│   └── ping_logs.txt
└── appendix/
    └── 명령어정리.md
```
