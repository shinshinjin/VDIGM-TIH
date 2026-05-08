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
    <td>
      Unreal Engine · Android
    </td>
  </tr>
  <tr>
    <td><strong>Client Development</strong></td>
    <td>
      C++ · Widget Blueprint · Animation Blueprint · Face AnimBP · C++ Function Library · GameInstanceSubsystem · SaveGame
    </td>
  </tr>
  <tr>
    <td><strong>Authentication / Release</strong></td>
    <td>
      Google OnlineSubsystem · Google Cloud Console · Android Keystore / SHA · AAB Packaging · Google Play Console
    </td>
  </tr>
  <tr>
    <td><strong>AR</strong></td>
    <td>
      ARCore · AR Session · ARFaceGeometry
    </td>
  </tr>
  <tr>
    <td><strong>Audio / Voice</strong></td>
    <td>
      TTS · WAV · PCM16 · RMS · SoundWave · SoundWaveProcedural · AudioComponent
    </td>
  </tr>
  <tr>
    <td><strong>Character / LipSync</strong></td>
    <td>
      MetaHuman LipSync · Face Animation Flow · Runtime Audio Playback
    </td>
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

이번 프로젝트에서는 Unreal 기반 Android 앱도 일반 게임 화면이 아니라 모바일 서비스 흐름으로 설계해야 한다는 점을 많이 느꼈습니다. 단순히 Widget을 배치하는 것보다, 
저장 상태, 인증 상태, 서버 응답, 권한, 오디오 재생, AR Mode 전환이 서로 꼬이지 않게 나누는 것이 중요했습니다.

Blueprint는 UI 흐름과 사용자 입력에 집중하도록 두고, 인증, 서버 응답, TTS 오디오 처리, Voice Recorder처럼 예외가 많은 영역은 C++ 구조로 분리했습니다. 
특히 TTS 응답을 SoundWave/SoundWaveProcedural로 변환하고 MetaHuman LipSync / Face AnimBP 흐름에 연결한 부분은 AI 아바타 앱의 핵심 표현을 만드는 작업이었습니다.

AR Mode에서는 카메라 화면 위에 캐릭터를 배치하는 것뿐 아니라, 휴대폰 기울기와 카메라 방향에 따라 캐릭터가 어색하게 눕지 않도록 회전 보정 흐름을 구성한 점이 중요했습니다.

개선한다면 인증, 채팅, TTS, LipSync, AR Mode를 더 독립적인 모듈 단위로 분리하고, 음성 응답 큐와 AR 회전 보정 로직을 전용 매니저로 정리하고 싶습니다.


---
