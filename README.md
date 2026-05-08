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
<img width="1144" height="719" alt="image" src="https://github.com/user-attachments/assets/304667a9-721a-484c-ad35-4dff1f075ee4" />

```
- Google OnlineSubsystem 기반 인증 결과를 서비스 서버 검증으로 연결
- 로그인 직후 인증값이 바로 준비되지 않는 상황을 고려해 재확인 흐름 구성
- 서버 응답 기준으로 기존 유저 / 신규 유저 / 약관 동의 필요 상태 분기
- 인증 토큰을 서버 API 호출 흐름과 연결해 앱 진입 상태 관리
```

## 2. 모바일 UI/UX & Onboarding Flow
<img width="1447" height="939" alt="image" src="https://github.com/user-attachments/assets/2e983d0c-4863-4f6b-aeb1-79baf16daac9" />

```
- First Loading에서 SaveGame 상태를 확인하고 Login / Main 진입 분기
- Login → Terms / Signup → Character Select → Persona / Voice Setup → Main 흐름 구성
- 캐릭터 선택, 페르소나 설정, 보이스 선택/녹음을 온보딩 단계로 연결
- 설정 완료 전 Chat 진입을 제한해 대화에 필요한 조건을 먼저 구성
- Chat과 AR Mode에서 동일한 캐릭터/보이스 설정을 이어서 사용
```

## 3. AI Chat / TTS / MetaHuman LipSync
<img width="1447" height="1087" alt="image" src="https://github.com/user-attachments/assets/d7c3c9d5-3009-4449-9624-6faf066ba713" />

```
- AI 채팅 응답을 텍스트 출력에서 끝내지 않고, 응답 메시지를 TTS 음성 생성 흐름으로 연결
- TTS로 수신한 WAV 데이터를 파싱해 런타임 오디오 객체로 변환하고, PCM 데이터를 등록해 재생 가능한 SoundWave 계열 객체로 처리
- 생성된 오디오를 AudioComponent 재생 흐름에 연결하고, MetaHuman LipSync 및 Face AnimBP 흐름과 연동
- 음성 재생과 얼굴 애니메이션 블루프린트 흐름을 연결해 AI 응답이 캐릭터 립싱크 표현으로 이어지도록 구성
- 여러 TTS 응답이 비동기로 도착하는 상황을 고려해 응답 순서를 관리하고, 현재 음성 재생 완료 후 다음 음성을 순차 재생하도록 처리
```


## 4. AR Mode / Character Placement

- Google ARCore 기반 AR Session 전환 흐름 구성
- Main 화면에서 AR Mode로 진입하고, AR 화면에서 카메라/음성/대화 UI 연결
- 전면/후면 카메라 플로우에 따라 캐릭터 배치 기준 분리
- ARFaceGeometry 기반 얼굴 감지 흐름과 AR 각도 계산 로직 구성
- 휴대폰 기울기나 카메라 방향 변화에도 캐릭터가 비정상적으로 눕지 않도록 회전 보정 흐름 구성
- 기존 캐릭터/보이스 설정이 AR Mode에서도 이어지도록 구성

[이미지첨부] AR 모드 진입, 캐릭터 배치 및 회전 보정 화면

## 5. Voice Recorder

- 음성 녹음 기능을 Widget 내부가 아닌 GameInstanceSubsystem으로 분리
- 마이크 입력을 PCM16 데이터로 누적
- 입력 신호의 RMS 값을 계산해 Wave Bar UI 데이터로 정규화
- 최소/최대 녹음 시간, 미리듣기, WAV 저장 흐름 관리
- UI는 녹음 상태와 표시용 데이터만 받아 갱신하도록 구성

[이미지첨부] Voice Recorder: 녹음 중 Wave Bar / 미리듣기 / 저장 흐름


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
