# 게임 디렉토리 구조 상세

## 파일별 역할과 책임

### `main.ts` — 진입점
- DOM 초기화, canvas 엘리먼트 생성/획득
- Game 인스턴스 생성 및 시작
- 게임 로직을 포함하지 않는다

### `game.ts` — 게임 코어
- Game 클래스: 게임 루프 소유 (`requestAnimationFrame`)
- `update(dt)` / `render()` 사이클 관리
- 상태 머신 소유 (idle → playing → paused | gameover)
- Renderer와 InputHandler를 주입받아 사용

### `renderer.ts` — 렌더링
- 게임 상태를 받아 Canvas에 그리는 순수 렌더링 함수/클래스
- 게임 상태를 변경(mutation)하지 않는다
- Canvas 2D Context 또는 PixiJS/WebGL 등 렌더링 백엔드 캡슐화

### `input.ts` — 입력 처리
- 키보드/마우스/터치 이벤트 리스너 등록 및 해제
- 입력 상태를 추적하여 깔끔한 API 제공 (예: `isKeyPressed('ArrowLeft')`)
- 이벤트 기반과 폴링 기반 입력 모두 지원

### `types.ts` — 타입 정의
- 게임 도메인의 모든 interface, type alias 정의
- 게임 상태, 엔티티, 설정 등의 타입

### `constants.ts` — 상수
- 매직 넘버를 명명된 상수로 추출
- 보드 크기, 속도, 색상, 키 매핑 등

## 파일 분할 기준

- 단일 파일이 **300줄을 초과**하면 책임별로 분리
- 예: `collision.ts`, `scoring.ts`, `entity.ts`, `ui.ts`
- 분할 시에도 Game 클래스가 조율자(orchestrator) 역할을 유지
