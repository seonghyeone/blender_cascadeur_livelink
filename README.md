# Blender cascaduer live link for ARP(Auto Rig Pro)
blender와 cascaduer 라이브 연동을 위한 글
---------------------------------------------------
## 다음은 arp 전용입니다 arp리그는 Aron Nemeth 유튜브에서 소개한 인간, quad 구조를 이용하면 됩니다.

Blender와 Cascadeur를 GHub로 연결하는 도구입니다. Cascadeur에서 만든 캐릭터·소품·카메라 애니메이션을 Blender에서 실시간으로 확인하고, 필요한 결과를 Blender 키프레임으로 베이크할 수 있습니다.

소품과 일반 메쉬는 Blender에서 Cascadeur로 직접 전송할 수 있습니다. 전송 후에는 Cascadeur에서 편집한 움직임을 원래 Blender 오브젝트가 따라갑니다.



## 배포 파일

| 파일 | 버전 | 용도 |
|---|---|---|
| `GHub.exe` | Unified v1.3 | Blender와 Cascadeur 사이의 통신 허브 |
| `Blender_Live_Link_GHEC_Windows_x64.zip` | v0.13.2 | Cascadeur용 스크립트와 설정창 |
| `GHEB_ARP_Direct_Blender5_2.zip` | v0.5.1 | Blender용 애드온 |

캐릭터·소품·메쉬·카메라는 모두 **GHub 하나와 Control Port `8000`**을 공유합니다. 이전의 별도 Prop Bridge나 `8008` 포트용 프로그램은 필요하지 않습니다.

## 사용 환경

- Windows x64
- **Blender 5.2 LTS** — 현재 애드온은 5.2 전용이며 다른 버전에서는 등록을 차단합니다.
- Cascadeur — 배포된 Python 스크립트를 설치할 수 있는 환경이 필요합니다. Cascadeur의 모든 버전 호환성을 보증하지 않습니다.
- 두 앱과 GHub를 **같은 컴퓨터**에서 실행하는 구성입니다.
- Human·Quadruped 수신 모드를 사용하려면 해당 모드가 지원하는 Auto-Rig Pro(ARP) 리그가 필요합니다.


## 1. 설치

### GHub

1. `GHub.exe`를 원하는 폴더에 놓습니다.
2. 이전 GHub 또는 별도 Prop Bridge가 실행 중이면 종료합니다.
3. 새 `GHub.exe`를 실행합니다. **한 개만 실행**합니다.
4. 실행 후 트레이의 GHub 상태를 확인합니다.

### Cascadeur

1. Cascadeur를 종료합니다.
2. Cascadeur용 ZIP을 압축 해제합니다.
3. 압축 내부의 `models/ghec` 폴더와 `scripts/ghec.py`를 Cascadeur 설치 폴더 아래의 대응 경로에 복사합니다.

```text
<Cascadeur 설치 폴더>/
└─ resources/
   └─ scripts/
      └─ python/
         ├─ models/
         │  └─ ghec/        ← ZIP의 models/ghec 전체
         └─ scripts/
            └─ ghec.py     ← ZIP의 scripts/ghec.py
```

기존 버전을 업데이트한다면 기존 파일을 백업한 뒤 해당 경로에 덮어씁니다. `models`나 `scripts` 상위 폴더 전체를 삭제하지 마세요.

4. Cascadeur를 실행합니다.
5. **Scripts → Blender Live Link** 메뉴가 나타나는지 확인합니다.

### Blender

1. **Edit → Preferences → Add-ons → Install from Disk**를 엽니다.
2. Blender용 ZIP을 **압축 해제하지 않고** 선택합니다.
3. **GHEB ARP Direct (experimental)** 애드온을 활성화합니다.
4. 3D Viewport에서 `N`을 눌러 사이드바의 **GHEB** 탭을 엽니다.

다른 GHEB 애드온이 활성화되어 있다면 먼저 비활성화합니다. 업데이트 후에는 Blender를 재시작합니다.

## 2. 기본 연결

1. GHub를 실행합니다.
2. Cascadeur에서 사용할 장면을 열고 **Scripts → Blender Live Link**를 엽니다.
3. Control Port를 **8000**으로 맞추고 **CONNECT**를 누릅니다.
4. 연결되면 메뉴 왼쪽 원이 **파란색**으로 바뀝니다. 설정창을 닫아도 연결은 유지됩니다.
5. Blender의 **GHEB → G-HUB Control**에서도 Port가 **8000**인지 확인합니다.

이후 사용할 기능에 따라 아래 설정을 진행합니다.

- 캐릭터: Character Slot 설정 후 **Start Entangle**
- 소품·메쉬: 해당 **Export** 버튼
- 카메라: **Connect Camera**

소품·메쉬·카메라만 사용한다면 Blender에서 캐릭터용 Start Entangle을 먼저 누를 필요는 없습니다. 해당 버튼이 필요한 Blender 연결을 시작합니다. Cascadeur 쪽 CONNECT는 켜져 있어야 합니다.

## 3. 캐릭터 연결

### 슬롯 번호 맞추기

Blender의 **C1**은 Cascadeur의 **Character 1 / char_01**, C2는 Character 2 / char_02와 연결됩니다. 연결 기준은 **같은 슬롯 번호**입니다. 오브젝트 이름이 같다는 이유만으로 자동 연결되지는 않습니다.

1. 설정을 변경하려면 Blender 캐릭터 연결은 **Stop Entangle**, Cascadeur는 **DISCONNECT** 상태로 둡니다.
2. 양쪽 **Character Slots**에서 필요한 만큼 **+**로 슬롯을 추가합니다.
3. Cascadeur 슬롯의 목록에서 전송할 캐릭터의 본 계층을 선택합니다. 새로 가져온 캐릭터가 없다면 **REFRESH**를 누릅니다.
4. 해당 슬롯의 원형 버튼을 눌러 **파란색 ON**으로 만듭니다.
5. Blender에서 같은 번호의 슬롯을 활성화하고, 수신할 **Armature**와 **Receiver**를 설정합니다.
6. Cascadeur의 **SAVE**로 슬롯 설정을 저장하고 **CONNECT**합니다.
7. Blender에서 **Start Entangle**을 누릅니다.


본이 필요 없는 오브젝트는 **Export Mesh / Background**를 사용합니다.

1. Blender의 **Object Mode**에서 보낼 메쉬를 하나 이상 선택합니다.
2. **Props → Cascadeur → Export Mesh / Background**를 누릅니다.
3. Cascadeur에 가져온 메쉬의 위치·회전·스케일을 변경합니다.
4. 원래 Blender 메쉬가 같은 변환을 따라가는지 확인합니다.

이 경로는 본·Rig Info·강체 리그를 만들지 않습니다. 오브젝트 변환을 라이브로 받으며, 버텍스 편집이나 모디파이어의 형상 변화를 계속 전송하는 기능은 아닙니다.

## 6. 소품·메쉬 연결 관리

| 버튼 | 동작 |
|---|---|
| **Stop Sync / Unlock** | 수신을 멈추고 Blender의 변환 잠금을 복원 |
| **Resume Sync** | 저장된 연결 정보를 이용해 다시 수신 |
| **Remove Link** | 연결 정보를 제거하고 잠금을 복원. 양쪽 오브젝트와 애니메이션은 유지 |

Cascadeur에서 소품을 완전히 지우려면 Blender에서 해당 연결을 **Remove Link**한 뒤 Cascadeur의 소품 오브젝트·리그·트랙을 확인해 삭제합니다.

Cascadeur의 대상을 삭제하고 새로 가져왔다면 예전 Resume Sync가 새 대상을 연결한다고 가정하지 마세요. 기존 연결을 제거한 뒤 다시 전송합니다.

## 7. 카메라 연결

1. Cascadeur에서 카메라가 있는 장면을 활성화합니다.
2. Blender의 **Camera ← Cascadeur** 패널을 엽니다.
3. **Source Camera**에 Cascadeur 카메라 이름을 입력합니다. 장면에 카메라가 하나라면 비워 둘 수 있습니다.
4. **Connect Camera**를 누릅니다.
5. Blender에 생성 또는 재연결된 전용 카메라에서 결과를 확인합니다.

위치·회전뿐 아니라 **초점거리(Focal Length)** 변화도 전달됩니다.

**Match Render Aspect**를 켜면 Cascadeur의 화면 비율에 맞춰 Blender 렌더 비율을 조정합니다. 세로 9:16 카메라라면 Blender의 기본 16:9 설정과 맞추는 데 사용할 수 있습니다. 이 옵션은 렌더 설정을 변경하므로 연결 전에 선택하세요.

카메라 수신을 끝내려면 **Stop / Unlock**을 누릅니다.

## 8. 공통 타임라인

Blender의 **Timeline & Bake → Sync Timeline**을 켜고 Cascadeur의 **TIMELINE SYNC**도 **ON**인지 확인합니다.

프레임 이동과 재생 시간을 캐릭터·소품·카메라가 함께 공유합니다. 각각의 타임라인을 따로 연결할 필요는 없습니다. 소품·카메라 연결 성공 시 Blender의 타임라인 참여가 자동으로 켜질 수도 있습니다.

두 앱의 FPS가 다르면 **같은 시간**에 대응하는 프레임으로 이동하므로 표시되는 프레임 번호가 다를 수 있습니다. 처음 사용할 때는 두 앱의 FPS를 같게 두면 확인하기 쉽습니다.

공유 시간 동기화를 끄려면 **Timeline Sync Active (STOP)**을 누릅니다. 시간 동기화와 포즈 수신은 별도 기능입니다.

## 9. Blender에 애니메이션 베이크

Live Link는 실시간 미리보기이며, 모든 프레임의 애니메이션 키를 자동 저장하는 기능은 아닙니다. Blender에서 결과를 편집·렌더·내보내기하려면 필요한 대상을 베이크합니다.

베이크 중에는 두 앱과 GHub 연결을 유지하고 다른 전송이나 베이크가 끝날 때까지 기다립니다.

### 캐릭터

1. **Timeline & Bake**에서 **Mode: Body**를 선택합니다.
2. **Character**에서 베이크할 슬롯을 선택합니다.
3. **Start / End**에 Blender 기준 출력 프레임 범위를 입력합니다. 단일 프레임 `0~0`도 가능합니다.
4. **Bake Selected Target**을 누르고 완료를 기다립니다.
5. 생성된 Action을 확인합니다. 캐릭터는 한 슬롯씩 베이크합니다.

Human은 팔 FK / 다리 IK 컨트롤러에, Quadruped는 네 다리 FK를 포함한 지원 컨트롤러와 필요한 속성에 결과를 기록합니다.

### 본이 있는 소품

**Props → Cascadeur**의 해당 소품 박스에서 **Start / End → Bake Prop**을 사용합니다.

### 카메라

**Camera ← Cascadeur**에서 **Start / End → Bake Camera**를 사용합니다. 이동·회전과 렌즈 데이터의 애니메이션을 기록합니다.

소품과 카메라는 성공 후 해당 대상의 라이브 수신을 멈추고 베이크된 Action을 사용합니다. **Resume Sync / Connect Camera**로 라이브에 돌아갈 수 있습니다. 연결 재개 시 이 도구가 만든 베이크 Action은 보관하고 활성 적용에서 분리합니다.

캐릭터·소품·카메라 베이크는 별도 버튼이며 한 번에 하나씩 실행합니다. 일반 메쉬 경로에는 현재 **전용 Bake Mesh 버튼이 없습니다.**

**Bake Delay**는 베이크 프레임 처리 사이의 추가 대기 시간입니다. 키의 재생 속도를 조절하는 설정이 아닙니다.

Blender에서 베이크 결과만 확인하려면 해당 대상의 라이브 수신을 중지하고 재생하세요. 라이브 수신을 켜 두면 실시간 자세가 베이크 자세 위에 적용될 수 있습니다.

## 10. 저장 후 다시 열기

1. Blender 파일을 `.blend`, Cascadeur 장면을 `.casc`로 저장합니다.
2. Cascadeur 슬롯 설정은 Live Link 창의 **SAVE**도 눌러 둡니다.
3. 다시 작업할 때 GHub를 실행하고 양쪽 파일을 엽니다.
4. Cascadeur의 대상 장면·캐릭터 슬롯을 확인하고 **CONNECT**합니다.
5. Blender에서 필요한 기능을 다시 시작합니다.
   - 캐릭터: **Start Entangle**
   - 소품·메쉬: **Resume Sync**
   - 카메라: **Connect Camera**
   - 타임라인: **Sync Timeline**




## 시연 영상
https://www.youtube.com/watch?v=SZ_MpMnoEFE&t=5s 를 참고하세요 - 업데이트 예정 조금만 기다려주세요.



<img width="291" height="540" alt="스크린샷 2026-10-13 202859" src="https://github.com/user-attachments/assets/04cdb39c-ff5b-4a90-a50f-5184199f4a85" />







## 감사합니다 
https://github.com/TeamGadget-JP/UNIVERSAL_REAL-TIME_SYNC_SYSTEM










