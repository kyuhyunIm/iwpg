# iwpg (I Want Play Games)

> 이 지침은 2026년 4월 기준 작성됨

## 프로젝트 개요

다양한 클래식/캐주얼 게임을 웹 기술로 구현하는 게임 컬렉션 프로젝트.

- **구조**: 루트 아래 게임별 독립 디렉토리 (예: `/tetris`, `/snake`, `/2048`)
- **각 게임은 독립 프로젝트**: 자체 `package.json`, 빌드 설정, 의존성 보유
- **루트는 조직 컨테이너**: 공유 빌드 시스템이나 공유 의존성 없음

## 기술 스택

### 기본 권장 스택

- **언어**: TypeScript (strict mode)
- **패키지 매니저**: pnpm
- **빌드**: Vite
- **렌더링**: HTML5 Canvas API
- **스타일**: CSS (UI 오버레이용)

### 게임별 선택 가능

- 복잡한 2D 렌더링이 필요한 경우: PixiJS
- 물리/충돌 등 게임 프레임워크가 필요한 경우: Phaser
- 3D 렌더링이 필요한 경우: Three.js / WebGL
- 게임별 CLAUDE.md에 선택한 스택과 이유를 기록할 것

## 게임 디렉토리 표준 구조

모든 게임은 아래 구조를 따른다. 각 파일의 상세 역할은 `.claude/rules/game-structure.md` 참고.

```
[game-name]/
  CLAUDE.md              # 게임별 컨텍스트 (규칙, 상태 머신, 특이사항)
  package.json
  tsconfig.json
  vite.config.ts
  index.html
  src/
    main.ts              # 진입점: DOM 초기화, Game 인스턴스 생성
    game.ts              # Game 클래스: 게임 루프, 상태 관리
    renderer.ts          # 렌더링 담당 (Canvas/PixiJS 등)
    input.ts             # 입력 처리 (키보드, 마우스, 터치)
    types.ts             # 타입 정의
    constants.ts         # 게임 상수 (보드 크기, 속도, 색상 등)
  public/
    assets/              # 이미지, 사운드 등 정적 파일
```

## 개발 명령어

각 게임 디렉토리 내에서 실행:

```bash
pnpm install              # 의존성 설치
pnpm dev                  # 개발 서버 (Vite HMR)
pnpm build                # 프로덕션 빌드
pnpm preview              # 빌드 결과 미리보기
pnpm exec tsc --noEmit    # 타입 체크
```

## 새 게임 추가

새 게임 생성 절차는 `.claude/rules/new-game-checklist.md` 참고.

## 코드 컨벤션

### 게임 루프

모든 게임은 `requestAnimationFrame` 기반 게임 루프를 사용한다.
고정 타임스텝(fixed timestep)으로 로직 업데이트, 가변 타임스텝으로 렌더링.
상세 패턴은 `.claude/rules/game-loop-pattern.md` 참고.

### 상태 관리

- 게임 상태는 `Game` 클래스 내에서 관리
- 기본 상태 전이: `idle` → `playing` → `paused` | `gameover`
- 외부 상태 관리 라이브러리 사용하지 않음

### 렌더링 분리

- 게임 로직(`game.ts`)과 렌더링(`renderer.ts`)을 분리
- `Game`이 상태를 업데이트하고, `Renderer`가 상태를 읽어 그린다
- 렌더러는 게임 상태를 변경하지 않는다

### TypeScript 규칙

- `strict: true` 필수
- `any` 타입 사용 금지
- 게임별 도메인 타입은 `types.ts`에 정의

## 테스트

- 주요 검증: 브라우저에서 직접 플레이
- TypeScript 타입 체크가 일차 안전망
- 게임 로직(점수 계산, 충돌 판정 등)은 필요 시 Vitest로 단위 테스트 가능
- 렌더링 테스트는 하지 않음

## 게임별 CLAUDE.md 가이드

각 게임 디렉토리의 CLAUDE.md에 포함할 내용:

- 게임 규칙 요약
- 선택한 기술 스택과 이유 (기본 스택과 다른 경우)
- 게임 상태 머신 설명
- 핵심 알고리즘 설명 (충돌 판정, AI 등)
- 입력 매핑 (키보드/마우스/터치)
- 알려진 제한사항이나 TODO
