# TIH Client Projects

<p align="center">
  <strong>Unreal Engine 기반 AI Avatar Client 포트폴리오</strong>
</p>

<p align="center">
  Android 모바일 클라이언트와 Windows 데스크톱 클라이언트를 각각의 플랫폼 목적에 맞게 분리해 구현했습니다.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Engine-Unreal%20Engine-313131?style=for-the-badge&logo=unrealengine&logoColor=white" />
  <img src="https://img.shields.io/badge/Platform-Android%20%7C%20Windows-3DDC84?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Client-Blueprint%20%7C%20C++-00599C?style=for-the-badge&logo=cplusplus&logoColor=white" />
  <img src="https://img.shields.io/badge/Portfolio-Public%20README-181717?style=for-the-badge&logo=github&logoColor=white" />
</p>

---


# 🎬 Demo Video

<br>

<p align="center">
  <a href="https://www.youtube.com/watch?v=XvWagOY7w-8" target="_blank">
    <img src="https://img.shields.io/badge/YouTube-Demo%20Video-FF0000?style=for-the-badge&logo=youtube&logoColor=white" />
  </a>
</p>

<p align="center">
  <a href="https://www.youtube.com/watch?v=XvWagOY7w-8">
    TIH Demo Video 바로가기
  </a>
</p>

---

# 👥 Development Team

<br>

<table>
  <tr>
    <th align="center">신혁진</th>
    <th align="center">손준성</th>
    <th align="center">3D 모델링 팀</th>
  </tr>
  <tr>
    <td align="center">
      <strong>Backend / AI / Infra</strong>
    </td>
    <td align="center">
      <strong>Unreal Engine Client</strong>
    </td>
    <td align="center">
      <strong>3D Modeling</strong>
    </td>
  </tr>
  <tr>
    <td align="center">
      UE5 클라이언트 개발<br>
      UI/UX 플로우 구현<br>
      서버 API 연동<br>
      MetaHuman 연동<br>
      LipSync 흐름 구성
    </td>
    <td align="center">
      시스템 설계<br>
      전체 백엔드 개발<br>
      AI 에이전트<br>
      CI/CD<br>
      비용 시뮬레이터
    </td>
    <td align="center">
      MetaHuman 캐릭터 모델링<br>
      캐릭터 리소스 제작<br>
      애니메이션 에셋 제작
    </td>
  </tr>
</table>


---


## 📂 Project List

| Project | Platform | Summary |
|---|---|---|
| **TIH Android Client** | Android Mobile | Google Login, 서버 인증, AI Chat, TTS/LipSync, AR Mode, Voice Recorder, Android 배포 흐름 |
| **TIH Window Client** | Windows Desktop | 캐릭터 기반 데스크톱 클라이언트, 방송 준비, 대본 생성, 보이스 선택, 카메라/배경 편집, 플러그인 기반 협업 구조 |

---

<br>

<details>
<summary><strong>📱 TIH(Android) - Unreal Engine 기반 Android AI Avatar Conversation App</strong></summary>

<br>

# TIH Android Client

> Unreal Engine 기반 Android AI Avatar Conversation App  
> Google Login, 서버 인증, AI Chat, TTS/LipSync, MetaHuman Face AnimBP, ARCore 기반 AR Mode, Android 배포 경험을 포함한 모바일 클라이언트 프로젝트입니다.

---

# 📌 Overview

<br>

`TIH Android Client`는 Unreal Engine 기반 Android AI Avatar Conversation App입니다.

사용자는 Google Login 이후 서비스 인증을 거쳐 캐릭터, 페르소나, 보이스 설정을 완료하고, AI Chat 또는 AR Mode로 진입합니다.  
저는 Android 클라이언트 플러그인 영역을 담당하며 모바일 UI/UX 흐름, 인증 분기, 서버 연동, TTS/LipSync, AR Mode, Android 배포 과정을 하나의 앱 흐름으로 연결했습니다.

---

# 🧑‍💻 My Role

<br>

- Android 클라이언트 플러그인 영역 개발
- 모바일 UI/UX 플로우 구성
- Google Login 이후 서비스 서버 인증, 약관 동의, 회원가입 분기 연결
- SaveGame 기반 앱 재진입 및 Login / Main 분기 흐름 구성
- 캐릭터 선택, 페르소나 설정, 보이스 선택/녹음 온보딩 흐름 구현
- AI Chat → TTS WAV → SoundWave/SoundWaveProcedural → AudioComponent → MetaHuman LipSync 흐름 연결
- MetaHuman 얼굴 Animation Blueprint / LipSync 재생 흐름 구성
- 복수 TTS 응답을 Index/Priority 기반으로 순서 관리
- ARCore 기반 AR Mode UI, 전/후면 카메라 플로우, 캐릭터 배치 및 회전 보정 흐름 구성
- Voice Recorder를 GameInstanceSubsystem으로 분리해 PCM16 녹음, RMS 기반 Wave Bar, 미리듣기, WAV 저장 관리
- Google Cloud Console OAuth 설정, Android Keystore/SHA 설정, AAB 패키징, Google Play Console 업로드 경험

---

# ✨ Key Features

<br>

## 1. 구글 로그인 & 서버 인증

<p align="center">
  <img width="1144" height="719" alt="Google Login Flow" src="https://github.com/user-attachments/assets/304667a9-721a-484c-ad35-4dff1f075ee4" />
</p>

<p align="center">
  <sub>Google 로그인 결과를 Unreal 클라이언트 인증 상태와 서비스 서버 검증 흐름으로 연결한 구조</sub>
</p>

### Overview

Google 로그인은 외부 인증 화면 호출에서 끝나지 않도록 구성했습니다.  
Unreal Android 클라이언트에서 Google OnlineSubsystem의 인증 결과를 받아 서비스 서버 검증으로 전달하고, 응답 결과에 따라 기존 유저 / 신규 유저 / 약관 동의 필요 상태를 분기했습니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. Google Login** | Google OnlineSubsystem 기반으로 Android Google Login 호출 |
| **2. Auth Result** | 로그인 완료 후 ID Token 또는 ServerAuthCode 조회 |
| **3. Retry Flow** | 로그인 직후 인증값이 바로 준비되지 않는 상황을 고려해 재확인 흐름 구성 |
| **4. Server Auth** | Google 인증 결과를 서비스 서버 검증 요청으로 연결 |
| **5. User Branch** | 서버 응답을 기준으로 기존 유저 / 신규 유저 / 약관 동의 필요 상태 분기 |
| **6. App State** | 인증 토큰을 저장하고 이후 서버 API 호출 및 앱 진입 상태에 연결 |

### Highlights

- Google OnlineSubsystem 기반 인증 결과를 서비스 서버 검증 흐름으로 연결
- Android 환경에서 인증값 준비 타이밍이 지연될 수 있는 상황을 고려해 Retry 처리
- 서버 응답 결과를 Blueprint UI 분기에 사용할 수 있도록 클라이언트 상태로 정리
- 기존 유저는 Main으로, 신규 유저는 약관 동의 / 회원가입 플로우로 이동하도록 구성

---

## 2. 모바일 UI/UX & Onboarding Flow

<p align="center">
  <img width="1447" height="939" alt="Mobile UI UX Flow" src="https://github.com/user-attachments/assets/2e983d0c-4863-4f6b-aeb1-79baf16daac9" />
</p>

<p align="center">
  <sub>앱 실행부터 로그인, 캐릭터 설정, 메인 진입, Chat / AR Mode까지 이어지는 모바일 UI 흐름</sub>
</p>

### Overview

모바일 앱의 첫 진입 흐름은 저장 데이터와 인증 상태에 따라 달라지도록 구성했습니다.  
기존 설정이 있는 사용자는 Main으로 빠르게 진입하고, 신규 사용자는 로그인, 약관 동의, 캐릭터 선택, 페르소나 / 보이스 설정을 거쳐 앱을 사용할 수 있도록 온보딩 단계를 나누었습니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. First Loading** | 앱 실행 시 초기 로딩 화면에서 저장 데이터 확인 |
| **2. SaveGame Check** | SaveGame 존재 여부에 따라 Login 또는 Main 진입 분기 |
| **3. Login / Signup** | Google Login 이후 기존 유저와 신규 유저 흐름 분리 |
| **4. Character Select** | 사용할 캐릭터 선택 |
| **5. Persona / Voice Setup** | 캐릭터 대화에 필요한 페르소나와 보이스 설정 |
| **6. Main** | 설정 완료 후 Main 화면 진입 |
| **7. Chat / AR Mode** | Main에서 일반 채팅 또는 AR 채팅 흐름으로 분기 |

### Highlights

- First Loading에서 SaveGame 상태를 확인하고 Login / Main 진입 분기
- Login → Terms / Signup → Character Select → Persona / Voice Setup → Main 흐름 구성
- 캐릭터 선택, 페르소나 설정, 보이스 선택 / 녹음을 온보딩 단계로 연결
- Customize 캐릭터는 Persona / Voice 설정이 완료되기 전 Chat 진입을 제한
- Standard 캐릭터는 기본 설정을 활용해 바로 Chat 흐름으로 진입 가능하도록 구성
- Chat과 AR Mode에서 동일한 캐릭터 / 보이스 설정을 이어서 사용

---

## 3. AI Chat / TTS / MetaHuman LipSync

<p align="center">
  <img width="1447" height="1087" alt="AI Chat TTS MetaHuman LipSync Flow" src="https://github.com/user-attachments/assets/d7c3c9d5-3009-4449-9624-6faf066ba713" />
</p>

<p align="center">
  <sub>AI 응답이 TTS 음성 재생과 MetaHuman 얼굴 애니메이션으로 이어지는 처리 구조</sub>
</p>

### Overview

AI 채팅 응답을 텍스트 출력에서 끝내지 않고, TTS 음성 생성과 MetaHuman 립싱크 표현까지 이어지도록 구성했습니다.  
TTS로 수신한 WAV 데이터를 런타임 오디오 객체로 변환하고, AudioComponent 재생 흐름과 MetaHuman LipSync / Face AnimBP 흐름을 연결했습니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. AI Chat Response** | AI 채팅 응답 수신 후 텍스트 UI 출력과 TTS 요청 흐름으로 연결 |
| **2. TTS WAV** | TTS 응답으로 WAV 데이터 수신 |
| **3. WAV Parsing** | WAV 데이터를 파싱하고 PCM 구간 추출 |
| **4. Runtime Audio** | `SoundWave / SoundWaveProcedural` 계열 객체로 변환 |
| **5. Audio Playback** | `AudioComponent`를 통해 런타임 재생 흐름 구성 |
| **6. LipSync / Face AnimBP** | `MetaHuman LipSync` 및 `Face AnimBP` 흐름과 연동 |
| **7. Sequential Playback** | 복수 TTS 응답이 도착할 경우 순서를 관리해 순차 재생 처리 |

### Highlights

- AI 채팅 응답을 텍스트 출력에서 끝내지 않고 TTS 음성 생성 흐름으로 확장
- TTS로 수신한 WAV 데이터를 파싱해 SoundWave 계열 런타임 오디오 객체로 변환
- PCM 데이터를 등록해 Unreal 클라이언트에서 즉시 재생 가능한 오디오 흐름 구성
- 생성된 오디오를 AudioComponent 재생 흐름에 연결
- MetaHuman LipSync 및 Face AnimBP 흐름과 연동해 캐릭터 립싱크 표현으로 연결
- 여러 TTS 응답이 비동기로 도착하는 상황을 고려해 현재 음성 재생 완료 후 다음 음성을 순차 재생하도록 처리

---

## 4. AR Mode / Character Placement

[이미지첨부] AR 모드 진입, 캐릭터 배치 및 회전 보정 화면

<p align="center">
  <sub>ARCore 기반 AR 화면에서 캐릭터 배치, 카메라 전환, 대화 UI를 연결한 흐름</sub>
</p>

### Overview

AR Mode에서는 일반 채팅 화면과 동일한 캐릭터 / 보이스 설정을 유지하면서, AR 화면 안에서 아바타와 대화할 수 있도록 구성했습니다.  
전면 / 후면 카메라 흐름을 나누고, ARFaceGeometry 기반 얼굴 감지 및 각도 계산 흐름을 활용해 캐릭터 배치와 회전 보정 로직을 정리했습니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. AR Mode Entry** | Main 화면에서 AR Mode로 진입 |
| **2. AR Session** | Google ARCore 기반 AR Session 전환 |
| **3. Camera Flow** | 전면 / 후면 카메라 플로우에 따라 캐릭터 배치 기준 분리 |
| **4. Character Placement** | AR 화면 안에 캐릭터를 배치하고 대화 UI와 연결 |
| **5. Face / Angle Calculation** | ARFaceGeometry 기반 얼굴 감지 및 각도 계산 흐름 구성 |
| **6. Rotation Correction** | 휴대폰 기울기나 카메라 방향 변화에도 캐릭터가 비정상적으로 눕지 않도록 회전 보정 |
| **7. AR Chat** | 기존 캐릭터 / 보이스 설정을 AR Mode에서도 이어서 사용 |

### Highlights

- Google ARCore 기반 AR Session 전환 흐름 구성
- Main 화면에서 AR Mode로 진입하고, AR 화면에서 카메라 / 음성 / 대화 UI 연결
- 전면 / 후면 카메라 플로우에 따라 캐릭터 배치 기준 분리
- ARFaceGeometry 기반 얼굴 감지 흐름과 AR 각도 계산 로직 구성
- 휴대폰 기울기나 카메라 방향 변화에도 캐릭터가 자연스럽게 서 있도록 회전 보정 흐름 구성
- 기존 캐릭터 / 보이스 설정이 AR Mode에서도 이어지도록 구성

---

## 5. Voice Recorder

[이미지첨부] Voice Recorder: 녹음 중 Wave Bar / 미리듣기 / 저장 흐름

<p align="center">
  <sub>모바일 음성 녹음 상태를 UI와 분리해 관리한 Voice Recorder 구조</sub>
</p>

### Overview

Voice Recorder는 Widget 내부에 녹음 로직을 직접 넣지 않고, GameInstanceSubsystem으로 분리했습니다.  
녹음 상태, PCM16 데이터, RMS 기반 Wave Bar, 미리듣기, WAV 저장 흐름을 Subsystem에서 관리하고, UI는 필요한 상태값과 표시용 데이터만 받아 갱신하도록 역할을 나누었습니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. Start Recording** | 사용자가 녹음 시작 버튼을 누르면 녹음 상태로 전환 |
| **2. PCM16 Capture** | 마이크 입력을 PCM16 데이터로 누적 |
| **3. RMS Calculation** | 입력 신호의 RMS 값을 계산 |
| **4. Wave Bar UI** | RMS 값을 UI 표시용 Wave Bar 데이터로 정규화 |
| **5. Time Control** | 최소 / 최대 녹음 시간 조건 관리 |
| **6. Preview** | 녹음 완료 후 미리듣기 상태로 전환 |
| **7. WAV Save** | Confirm 시 WAV 파일로 저장 |

### Highlights

- 음성 녹음 기능을 Widget 내부가 아닌 GameInstanceSubsystem으로 분리
- 마이크 입력을 PCM16 데이터로 누적
- 입력 신호의 RMS 값을 계산해 Wave Bar UI 데이터로 정규화
- 최소 / 최대 녹음 시간, 미리듣기, WAV 저장 흐름 관리
- UI는 녹음 상태와 표시용 데이터만 받아 갱신하도록 구성

---

# 🛠 Problem Solving

<br>

## Google Login 이후 앱 진입 분기

- **문제:** Google 인증 성공만으로는 서비스 내부 유저 상태를 알 수 없었습니다.
- **해결:** Google 인증 결과를 서비스 서버 인증으로 넘기고, 서버 응답 기준으로 기존 유저 / 신규 유저 / 약관 동의 상태를 분기했습니다.

## 복수 TTS 응답 순차 재생

- **문제:** 여러 WAV 응답이 비동기로 도착하면 대화 순서와 재생 순서가 어긋날 수 있었습니다.
- **해결:** 각 TTS 응답을 Index/Priority 기반으로 관리하고, 현재 음성 재생이 끝난 뒤 다음 SoundWave를 재생하도록 정리했습니다.

## MetaHuman LipSync / Face AnimBP 연결

- **문제:** TTS 음성이 재생되는 것만으로는 AI 아바타가 실제로 말하는 느낌이 부족했습니다.
- **해결:** TTS WAV를 SoundWave/SoundWaveProcedural로 변환한 뒤 AudioComponent 재생과 MetaHuman LipSync, 얼굴 Animation Blueprint 흐름에 연결했습니다.

## AR 모드 캐릭터 회전 보정

- **문제:** 모바일 AR 환경에서는 휴대폰 기울기와 카메라 방향에 따라 캐릭터가 화면에서 눕거나 어색한 각도로 보일 수 있었습니다.
- **해결:** AR 화면에서 카메라/얼굴 기준 각도 계산 흐름을 구성하고, 캐릭터가 화면 안에서 자연스럽게 서 있는 방향을 유지하도록 회전 보정 로직을 적용했습니다.

---

# 🚀 Android Release Experience

<br>

- Google Cloud Console OAuth 설정
- Android Keystore/SHA 설정
- Unreal Android AAB 패키징
- Google Play Console 업로드
- Android 실기기에서 Google Login 및 서버 인증 흐름 확인
- OAuth / 서명키 / SHA / 패키징 설정 간 연동 이슈 대응

[이미지첨부] Android AAB 패키징 또는 Google Play Console 업로드 화면 - 민감정보 마스킹 필수

---

# 🧰 Tech Stack

<br>

<table>
  <tr>
    <th align="left">Category</th>
    <th align="left">Stack</th>
  </tr>
  <tr>
    <td><strong>Engine / Platform</strong></td>
    <td>Unreal Engine · Android</td>
  </tr>
  <tr>
    <td><strong>Client Development</strong></td>
    <td>C++ · Widget Blueprint · Animation Blueprint · Face AnimBP · C++ Function Library · GameInstanceSubsystem · SaveGame</td>
  </tr>
  <tr>
    <td><strong>Authentication / Release</strong></td>
    <td>Google OnlineSubsystem · Google Cloud Console · Android Keystore / SHA · AAB Packaging · Google Play Console</td>
  </tr>
  <tr>
    <td><strong>AR</strong></td>
    <td>ARCore · AR Session · ARFaceGeometry</td>
  </tr>
  <tr>
    <td><strong>Audio / Voice</strong></td>
    <td>TTS · WAV · PCM16 · RMS · SoundWave · SoundWaveProcedural · AudioComponent</td>
  </tr>
  <tr>
    <td><strong>Character / LipSync</strong></td>
    <td>MetaHuman LipSync · Face Animation Flow · Runtime Audio Playback</td>
  </tr>
</table>

---

# 🔒 Security Notice

<br>

공개 README에서는 아래 정보를 제외했습니다.

- 실제 서버 URL
- API 경로
- OAuth 값
- Token / JWT / AuthCode
- Keystore / SHA
- 패키지명
- 사용자 정보
- 내부 응답 JSON
- 비공개 Blueprint 그래프
- 비공개 캐릭터 리소스 세부 사양

---

# 📝 Retrospective

<br>

이번 프로젝트에서는 Unreal 기반 Android 앱도 일반 게임 화면이 아니라 모바일 서비스 흐름으로 설계해야 한다는 점을 많이 느꼈습니다.  
단순히 Widget을 배치하는 것보다, 저장 상태, 인증 상태, 서버 응답, 권한, 오디오 재생, AR Mode 전환이 서로 꼬이지 않게 나누는 것이 중요했습니다.

Blueprint는 UI 흐름과 사용자 입력에 집중하도록 두고, 인증, 서버 응답, TTS 오디오 처리, Voice Recorder처럼 예외가 많은 영역은 C++ 구조로 분리했습니다.  
특히 TTS 응답을 SoundWave/SoundWaveProcedural로 변환하고 MetaHuman LipSync / Face AnimBP 흐름에 연결한 부분은 AI 아바타 앱의 핵심 표현을 만드는 작업이었습니다.

AR Mode에서는 카메라 화면 위에 캐릭터를 배치하는 것뿐 아니라, 휴대폰 기울기와 카메라 방향에 따라 캐릭터가 어색하게 눕지 않도록 회전 보정 흐름을 구성한 점이 중요했습니다.

개선한다면 인증, 채팅, TTS, LipSync, AR Mode를 더 독립적인 모듈 단위로 분리하고, 음성 응답 큐와 AR 회전 보정 로직을 전용 매니저로 정리하고 싶습니다.

---

</details>

<br>

---

<br>

<details>
<summary><strong>🖥️ TIH(Window) - Unreal Engine 기반 Windows Desktop Virtual Broadcasting Client</strong></summary>

<br>

# TIH Window Client

> Unreal Engine 기반 Windows Desktop Virtual Broadcasting Client  
> 캐릭터 기반 콘텐츠 제작, 방송 준비, 대본 생성, 보이스 선택, 카메라/배경 편집 흐름을 포함한 데스크톱 클라이언트 프로젝트입니다.

---

# 📌 Overview

<br>

`TIH Window Client`는 Unreal Engine 기반 Windows 데스크톱 클라이언트입니다.

사용자는 PC 환경에서 클라이언트를 실행한 뒤 저장된 진행 상태에 따라 로그인 또는 메인 화면으로 진입합니다.  
이후 캐릭터 선택, 페르소나 설정, 보이스 설정을 진행하고, 방송 준비 화면에서 카메라 편집, 배경 편집, 팟캐스트 제작, 대본 생성, 팟캐스트 재생 흐름으로 이어집니다.

이 프로젝트는 2인이 병렬로 작업하고, 추후 하나의 프로젝트로 병합할 것을 고려해 기능 단위 플러그인 구조로 분리했습니다.  
UI, 서버 연동, 캐릭터, 맵, PDF 처리, 립싱크, 오디오 관련 기능을 독립된 구조로 관리해 작업 충돌을 줄이고, 기능별 유지보수가 가능하도록 구성했습니다.

---

# 🧑‍💻 My Role

<br>

- Windows 데스크톱 클라이언트 플러그인 영역 개발
- 2인 협업 및 추후 병합을 고려한 기능별 플러그인 구조 설계
- Loading / Login / Main / Character / Persona / Voice / Broadcast / Podcast 계열 UI 플로우 구성
- SaveGame 기반 클라이언트 재진입 및 Login / Main 분기 흐름 구현
- GameInstance 기반 사용자 상태, 선택 캐릭터, 페르소나 완료 여부, 보이스, 카메라 상태 관리
- 방송 준비 화면에서 카메라 편집 / 배경 편집 / 팟캐스트 제작 흐름 연결
- 대본 생성 UI에서 서버 API 호출 및 결과 콜백 처리 구조 구현
- 보이스 파일 선택, WAV 데이터 처리, 선택 결과 UI 반영 흐름 구성
- 팟캐스트 재생 화면에서 좌우 버튼 기반 카메라 전환 및 화면 연출 흐름 구성
- PDF 기반 입력 텍스트를 대본 생성 흐름과 연결할 수 있는 구조 구성

---

# ✨ Key Features

<br>

## 1. 플러그인 기반 협업 구조

[이미지첨부] 플러그인 폴더 구조 또는 Unreal Plugin Browser 화면

<p align="center">
  <sub>2인 개발 및 추후 병합을 고려해 기능 단위로 분리한 플러그인 구조</sub>
</p>

### Overview

이 프로젝트는 모든 기능을 프로젝트 기본 Content 영역에 직접 쌓지 않고, UI / 서버 연동 / 캐릭터 / 맵 / 립싱크 / PDF / 오디오 기능을 플러그인 단위로 분리했습니다.

처음부터 2인 작업과 나중에 하나의 프로젝트로 합칠 것을 고려했기 때문에, 서로 다른 기능 영역이 동일한 에셋과 폴더를 동시에 수정하면서 발생하는 충돌을 줄이는 것이 중요했습니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. Core Client Plugin** | 핵심 UI, 서버 연동, 상태 관리, 방송 흐름을 중심 플러그인으로 구성 |
| **2. Character Plugin** | 캐릭터, 페르소나, 얼굴, 애니메이션 리소스를 별도 영역으로 분리 |
| **3. Map / Stage Plugin** | 맵, 스테이지, 배경 리소스를 독립 플러그인으로 관리 |
| **4. PDF Utility Plugin** | PDF 기반 입력 처리 기능을 독립 기능으로 분리 |
| **5. Runtime Audio / LipSync Plugin** | 런타임 오디오와 립싱크 기능을 별도 의존성으로 관리 |
| **6. Merge-Ready Structure** | 추후 병합 시 기능별 의존성을 확인하고 단계적으로 통합할 수 있도록 구성 |

### Highlights

- 2인 개발 시 Content 충돌을 줄이기 위해 기능별 플러그인 구조로 분리
- UI / 서버 / 캐릭터 / 맵 / PDF / 립싱크 기능을 독립적으로 관리
- 추후 하나의 프로젝트로 병합할 때 필요한 기능만 단계적으로 통합 가능한 구조 구성
- 기능 단위 테스트와 유지보수가 가능한 작업 단위 확보
- 포트폴리오 공개 시 내부 구현명을 과도하게 노출하지 않고 구조 중심으로 설명 가능

---

## 2. Desktop UI/UX & SaveGame Flow

[이미지첨부] Loading / Login / Main UI 흐름

<p align="center">
  <sub>클라이언트 실행 시 저장 상태를 확인하고 Login 또는 Main으로 분기하는 데스크톱 UI 흐름</sub>
</p>

### Overview

Windows 클라이언트의 첫 진입 흐름은 저장 데이터와 사용자 상태를 기준으로 분기되도록 구성했습니다.  
초기 로딩 화면에서 저장 데이터 존재 여부를 확인하고, 기존 사용자의 경우 저장된 진행 상태를 복원한 뒤 적절한 화면으로 이동합니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. Client Launch** | 클라이언트 실행 후 초기 Loading UI 진입 |
| **2. Save Data Check** | 로컬 저장 데이터 존재 여부 확인 |
| **3. Load Save Data** | 기존 데이터가 존재하면 저장 상태를 로드 |
| **4. Global State Sync** | 로드한 데이터를 전역 클라이언트 상태와 동기화 |
| **5. UI Branch** | 저장 상태에 따라 Login, Main, 설정 화면으로 분기 |
| **6. State Restore** | 캐릭터 선택, 페르소나 완료 여부 등 진행 상태 복원 |

### Highlights

- 초기 로딩 화면에서 저장 데이터 존재 여부를 확인하고 진입 화면 분기
- SaveGame과 GameInstance를 연결해 클라이언트 진행 상태 복원
- 매번 처음부터 설정하지 않아도 이전 진행 상태를 기준으로 앱 흐름 유지
- Login / Main / 설정 단계의 진입 조건을 저장 상태 기반으로 정리
- UI는 화면 전환에 집중하고, 지속 상태는 전역 상태 관리 구조에서 담당

---

## 3. Character / Persona / Voice Setup

[이미지첨부] Character Select / Persona Setup / Voice Setup UI

<p align="center">
  <sub>캐릭터 선택, 페르소나 입력, 보이스 파일 선택으로 이어지는 데스크톱 설정 흐름</sub>
</p>

### Overview

사용자는 방송 또는 콘텐츠 제작에 사용할 캐릭터를 선택하고, 페르소나 설정과 보이스 설정을 진행합니다.  
선택한 캐릭터, 페르소나 완료 여부, 보이스 정보는 이후 Main, Broadcast, Podcast 흐름에서 이어서 사용할 수 있도록 전역 상태와 연결했습니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. Character Select** | 캐릭터 선택 UI에서 사용 캐릭터 선택 |
| **2. Character Preview** | 선택한 캐릭터를 화면에 표시하거나 미리보기 상태로 반영 |
| **3. Persona Input** | 페르소나 입력 상태를 확인하고 다음 단계 진입 조건 판단 |
| **4. Persona State** | 페르소나 완료 여부를 전역 상태에 저장 |
| **5. Voice File Select** | 보이스 파일을 선택하고 클라이언트에 로드 |
| **6. Voice Callback** | 파일 로드 완료 후 성공 여부와 오디오 데이터를 UI에 반영 |
| **7. Reuse State** | 선택된 캐릭터 / 페르소나 / 보이스 상태를 방송 및 팟캐스트 흐름에 재사용 |

### Highlights

- 캐릭터 선택 결과를 이후 방송/콘텐츠 제작 흐름에서 재사용할 수 있도록 상태화
- 페르소나 입력 완료 여부에 따라 다음 단계 진입 조건 제어
- 보이스 파일 선택과 로드 결과를 UI에 반영하는 흐름 구성
- 선택한 WAV 데이터와 파일 경로를 이후 미리듣기 또는 서버 업로드 흐름으로 확장 가능하게 정리
- 여러 화면에서 동일한 설정값을 사용할 수 있도록 전역 상태 중심으로 관리

---

## 4. Broadcast Ready / Camera / Background Edit

[이미지첨부] Broadcast Ready / Camera Edit / Background Edit 화면

<p align="center">
  <sub>방송 시작 전 카메라와 배경을 조정하는 준비 단계 UI 흐름</sub>
</p>

### Overview

메인 화면에서 방송 시작을 선택하면 방송 준비 흐름으로 진입합니다.  
방송 준비 화면에서는 카메라 편집과 배경 편집 UI로 이동할 수 있으며, 사용자가 콘텐츠 제작 전에 장면 구도와 배경을 조정할 수 있도록 구성했습니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. Main Entry** | Main 화면에서 방송 시작 버튼 클릭 |
| **2. UI Transition** | 기존 화면을 정리하고 방송 준비 UI로 전환 |
| **3. Broadcast Ready** | 방송 시작 전 설정 화면 표시 |
| **4. Camera Edit** | 카메라 편집 화면으로 이동해 구도 조정 |
| **5. Background Edit** | 좌우 버튼 기반 배경 선택 / 변경 흐름 구성 |
| **6. Return Flow** | 편집 후 방송 준비 또는 팟캐스트 제작 흐름으로 복귀 |

### Highlights

- Main 화면에서 Broadcast Ready 화면으로 전환하는 UI 플로우 구성
- 방송 준비, 카메라 편집, 배경 편집을 기능별 화면으로 분리
- 카메라와 배경 편집을 방송 시작 전 준비 단계로 구성
- 필요 이벤트만 연결하고, 사용하지 않는 UI 이벤트는 비활성 상태로 정리
- 장면 설정과 방송 진행 UI를 분리해 유지보수성 확보

---

## 5. Podcast / Script Creation / Camera Playback

[이미지첨부] Podcast Script Creation / Podcast Play / Camera Transition 화면

<p align="center">
  <sub>PDF 또는 입력 텍스트 기반 대본 생성과 팟캐스트 재생, 카메라 전환을 연결한 콘텐츠 제작 흐름</sub>
</p>

### Overview

Window Client의 핵심 제작 흐름은 팟캐스트 제작과 대본 생성입니다.  
사용자는 대본 제목, 길이, 수정 텍스트 또는 PDF 기반 입력 내용을 바탕으로 서버에 대본 생성을 요청하고, 생성된 결과를 팟캐스트 재생 UI와 연결할 수 있습니다.

### Implementation

| Step | Description |
|------|-------------|
| **1. Podcast Entry** | 팟캐스트 제작 UI 진입 |
| **2. Script Input** | 대본 제목, 길이, 수정 텍스트 또는 PDF 기반 텍스트 입력 |
| **3. Script Request** | 입력 데이터를 서버의 대본 생성 요청으로 전달 |
| **4. Result Callback** | 요청 결과를 콜백으로 수신하고 성공 여부 확인 |
| **5. Script Result UI** | 생성된 대본 결과를 UI에 반영 |
| **6. Podcast Play** | 팟캐스트 재생 화면으로 이동 |
| **7. Camera Transition** | 좌우 버튼으로 장면/카메라 상태를 변경하고 화면 연출 적용 |

### Highlights

- 대본 생성 버튼 클릭 시 서버 API 요청 흐름 구성
- 사용자 입력 텍스트와 PDF 기반 텍스트를 대본 생성 입력으로 연결 및 화면 출력
- API 결과를 콜백으로 받아 UI 상태에 반영
- 팟캐스트 재생 중 좌우 버튼 기반 장면 전환 흐름 구성
- 카메라 전환 기능을 통해 콘텐츠 진행에 맞는 시점 변경 구현
- UI 제작 흐름과 실제 재생 흐름을 분리해 관리

---

# 🛠 Problem Solving

<br>

## 2인 개발과 추후 병합을 고려한 플러그인화

- **문제:** 2인이 동시에 같은 Unreal 프로젝트에서 작업하면 Content, Blueprint, Map, Character 에셋 충돌이 쉽게 발생할 수 있었습니다.
- **해결:** 핵심 UI/서버 연동, 캐릭터, 맵, PDF 처리, 립싱크, 오디오 기능을 플러그인 단위로 분리했습니다.

## SaveGame 기반 데스크톱 진입 분기

- **문제:** 클라이언트 실행 시 사용자가 이전에 어디까지 설정했는지에 따라 진입 화면이 달라져야 했습니다.
- **해결:** 초기 로딩 단계에서 저장 데이터를 확인하고, 저장된 진행 상태를 전역 상태와 동기화해 Login / Main / 설정 화면 분기를 구성했습니다.

## Blueprint에서 서버 API 호출 단순화

- **문제:** 각 Widget Blueprint에 서버 요청 로직이 직접 섞이면 유지보수와 디버깅이 어려워질 수 있었습니다.
- **해결:** 서버 요청은 Blueprint에서 호출 가능한 공통 C++ 레이어로 분리하고, 결과는 콜백 기반으로 UI에 전달되도록 구성했습니다.

## 보이스 파일 선택 결과 처리

- **문제:** 보이스 파일 선택은 단순 파일 경로뿐 아니라 성공 여부, WAV 데이터, 이후 미리듣기/업로드 흐름까지 고려해야 했습니다.
- **해결:** 파일 선택 후 결과 콜백에서 파일 로드 성공 여부와 오디오 데이터를 함께 처리하도록 구성했습니다.

## 팟캐스트 재생 중 카메라 전환


---

# 🚀 Windows / Plugin Architecture Experience

<br>

- Unreal Engine Windows Desktop 클라이언트 UI 흐름 구현
- 기능 단위 플러그인 분리 구조 설계
- 2인 협업 및 추후 병합을 고려한 작업 단위 분리
- SaveGame / GameInstance 기반 사용자 상태 복원
- Broadcast Ready / Camera Edit / Background Edit UI 흐름 구성
- Podcast 제작 / 재생 / 대본 생성 흐름 구성
- PDF 기반 입력 텍스트를 서버 대본 생성 요청과 연결
- Blueprint UI와 C++ 공통 레이어 기반 서버 API 호출 구조 구성
- 내부 변수명 / 함수명 / Blueprint 그래프 전체를 공개하지 않고 기능 단위로 문서화

[이미지첨부] Windows Client 실행 화면 또는 플러그인 구조 화면

---

# 🧰 Tech Stack

<br>

<table>
  <tr>
    <th align="left">Category</th>
    <th align="left">Stack</th>
  </tr>
  <tr>
    <td><strong>Engine / Platform</strong></td>
    <td>Unreal Engine · Windows Desktop</td>
  </tr>
  <tr>
    <td><strong>Client Development</strong></td>
    <td>C++ · Widget Blueprint · GameInstance · SaveGame · PlayerController · Blueprint Function Library</td>
  </tr>
  <tr>
    <td><strong>UI / UX</strong></td>
    <td>UMG · Loading · Login · Main · Character · Persona · Voice · Broadcast · Podcast UI</td>
  </tr>
  <tr>
    <td><strong>Server / Content</strong></td>
    <td>HTTP API · Delegate Callback · Script Creation · PDF/Text Input Flow</td>
  </tr>
  <tr>
    <td><strong>Broadcast / Camera</strong></td>
    <td>Camera Edit · Background Edit · Camera Transition · Scene Flow</td>
  </tr>
  <tr>
    <td><strong>Plugin Structure</strong></td>
    <td>Project Plugin · Content Plugin · Runtime Plugin · PDF Utility · Runtime Audio / LipSync Plugin</td>
  </tr>
</table>

---

# 🔒 Security Notice

<br>

공개 README에서는 아래 정보를 제외했습니다.

- 실제 서버 URL
- API 경로
- Token / Session 값
- 내부 계정 정보
- 개인 로컬 경로
- 내부 응답 JSON
- 비공개 Blueprint 그래프 전체
- 내부 변수명 / 함수명
- 비공개 캐릭터 / 맵 / 방송 리소스 세부 사양
- 병합 전 내부 작업 경로

---

# 📝 Retrospective

<br>


---

</details>
