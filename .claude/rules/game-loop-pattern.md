# 표준 게임 루프 패턴

모든 게임은 아래 패턴을 기반으로 게임 루프를 구현한다.

## 핵심 원칙

- `requestAnimationFrame` 기반 루프 (setInterval 사용 금지)
- **고정 타임스텝**으로 로직 업데이트 (물리/게임 로직 일관성 보장)
- **가변 타임스텝**으로 렌더링 (디스플레이 주사율에 맞춤)

## 레퍼런스 구현

```typescript
type GameState = 'idle' | 'playing' | 'paused' | 'gameover';

class Game {
  private state: GameState = 'idle';
  private lastTime = 0;
  private accumulator = 0;
  private readonly FIXED_TIMESTEP = 1000 / 60; // 60Hz 로직 업데이트

  constructor(
    private renderer: Renderer,
    private input: InputHandler,
  ) {}

  start(): void {
    this.state = 'playing';
    this.lastTime = performance.now();
    requestAnimationFrame((t) => this.loop(t));
  }

  private loop(currentTime: number): void {
    if (this.state !== 'playing' && this.state !== 'paused') return;

    const deltaTime = currentTime - this.lastTime;
    this.lastTime = currentTime;
    this.accumulator += deltaTime;

    // 고정 타임스텝으로 로직 업데이트
    while (this.accumulator >= this.FIXED_TIMESTEP) {
      if (this.state === 'playing') {
        this.update(this.FIXED_TIMESTEP);
      }
      this.accumulator -= this.FIXED_TIMESTEP;
    }

    // 매 프레임 렌더링
    this.renderer.render(this.getState());
    requestAnimationFrame((t) => this.loop(t));
  }

  private update(dt: number): void {
    // 게임 로직 업데이트
  }
}
```

## 상태 전이

```
idle ──(start)──→ playing ──(pause)──→ paused
                    │                     │
                    │                  (resume)
                    │                     │
                    ├←────────────────────┘
                    │
                 (lose/win)
                    │
                    ▼
                 gameover ──(restart)──→ idle
```

## 일시정지/재개 처리

- 일시정지 시 `lastTime`을 리셋하여 delta spike 방지
- 재개 시 `this.lastTime = performance.now()` 후 루프 재시작
