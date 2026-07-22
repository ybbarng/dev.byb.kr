# ABC — 개발 히스토리 & 아키텍처

> 상용 스마트홈이 흔치 않던 시절, 집을 직접 "스마트"하게 만들려던 **9년의 기록**(2017~2026). 자체 제작 → 결국 Home Assistant.

## 타임라인
| 시점 | 사건 | 저장소 |
|---|---|---|
| 2017-03 | **첫 BLE 해킹** — 상용 조명 스위치 "Switcher"를 리버스엔지니어링. 모든 것의 시작. | [switcher](https://github.com/ybbarng/switcher) |
| 2018-07 | LG 에어컨 **IR 라이브러리**(C++, `sendLG` 28비트) | [remote-controller](https://github.com/ybbarng/remote-controller) |
| 2018-07~08 | 아두이노 **AC 컨트롤러** → "**Audrey**"로 명명 | [myIoT](https://github.com/ybbarng/myIoT) |
| 2018-10 | **ABC 모노레포 시작** — Celery로 Audrey·Brice·Cathy·Slack·TTS 연결. 공기질 "Cathy"(Awair) 추가 | [awair](https://github.com/ybbarng/awair) |
| **2018-11-09** | **웹 "못생긴 리모컨" UI 탄생** (이 데모의 원본) | [ABC](https://github.com/ybbarng/Alexander-Bonaparte-Cust) |
| 2018-11 | **auth** SSO 서버 (Slack 로그인 → 암호화 토큰) | [auth](https://github.com/ybbarng/auth) |
| 2019~2022 | 조용. 2022-08 아카이브용 모노레포 통합(마지막 커밋) | |
| **2026** | **Home Assistant 이주** — Audrey→ThinQ, Brice→ESP32/ESPHome, Cathy→HA Awair | |

## 아키텍처 (2022 모노레포)
```
        브라우저 / Slack
             │
        [ Caddy ]  TLS · 경로별 리버스프록시
       ┌─────┴─────┐
   Flask abc      Flask auth      (:8001 / :8002)
   (웹리모컨·Slack·API)   (Slack 로그인→암호화 토큰 IdP)
       │  모든 액션 = Celery 태스크
   [ Celery + Redis ]  ── ble 큐(concurrency=1) ──┐
       │                                          │
   Audrey(AC)        Brice(조명)        Cathy(공기질)
   아두이노+IR        BLE Switcher       Awair 사설 API
```
- ⭐ **핵심 결정**: BLE는 전용 큐 + `concurrency=1`로 직렬화(bluepy/BlueZ는 동시연결 불가). 웹/Slack 워커는 응답성 유지, 읽기는 결과를 블로킹으로 받음.
- 스택: **Flask · Celery + Redis · bluepy(BLE) · 아두이노(IR) · ChaCha20-Poly1305 토큰 · SQLite**.
- 기기를 사람 이름(Audrey/Brice/Cathy)으로 부르고, 응답은 "A.B.Cust"의 정중한 1인칭 한국어. UI는 스스로 "못생긴 리모컨"이라 자백.

## 기기별 방식 (하드웨어 해킹)
- **Audrey (에어컨)**: 아두이노 + **Bluno Beetle**(BLE). LG 리모컨의 **28비트 IR 코드**를 복사해 재생(`sendLG(code, 28)`). AC 상태를 못 읽어 **마지막 코드를 기억**해 상태로 씀.
- **Brice (조명)**: BLE. 벤더 REST로 share code를 받아 인증 특성에 write → 스위치 특성(`0x15ba`)에 1바이트로 방등/복도등 on/off.
- **Cathy (공기질)**: Awair Mint **사설 API**(`internal.awair.is`). 종합점수·온습도·VOC·PM2.5를 5단계 색으로.

## 왜 폐기했나
IR은 상태를 못 읽고(단방향), BLE 스크립트는 `sudo`·리버스엔지니어링 유지보수가 무거웠다. 새 에어컨(ThinQ)·ESP32/ESPHome·Home Assistant로 옮기며 **유지보수 가능한 표준 경로**로 대체했다. 마스코트가 8년 전 약속한 "더 멋있는 녀석"이 마침내 도착한 셈.

## 관련 저장소
[Alexander-Bonaparte-Cust](https://github.com/ybbarng/Alexander-Bonaparte-Cust)(모노레포) · [switcher](https://github.com/ybbarng/switcher) · [remote-controller](https://github.com/ybbarng/remote-controller) · [myIoT](https://github.com/ybbarng/myIoT) · [awair](https://github.com/ybbarng/awair) · [auth](https://github.com/ybbarng/auth)
