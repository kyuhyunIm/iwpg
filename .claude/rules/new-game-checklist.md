# 새 게임 추가 체크리스트

## 부트스트래핑 절차

1. 루트에서 게임 디렉토리 생성 및 Vite 프로젝트 초기화
   ```bash
   pnpm create vite@latest [game-name] --template vanilla-ts
   cd [game-name]
   pnpm install
   ```

2. 불필요한 기본 파일 정리
   - `src/counter.ts` 삭제
   - `src/main.ts` 내용 비우기
   - `src/style.css` 내용 정리

3. `tsconfig.json`에서 `strict: true` 확인

4. `index.html`에 canvas 엘리먼트 추가
   ```html
   <canvas id="game-canvas"></canvas>
   ```

5. 표준 파일 구조 생성
   - `src/main.ts` — 진입점
   - `src/game.ts` — Game 클래스
   - `src/renderer.ts` — 렌더러
   - `src/input.ts` — 입력 처리
   - `src/types.ts` — 타입 정의
   - `src/constants.ts` — 게임 상수
   - `public/assets/` — 정적 파일 디렉토리

6. 게임별 `CLAUDE.md` 작성 (루트 CLAUDE.md의 "게임별 CLAUDE.md 가이드" 참고)

7. 루트 `README.md`의 게임 목록에 추가
