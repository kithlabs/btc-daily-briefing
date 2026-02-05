# btc-daily-briefing 구현 계획

비트코인 가격 추세 분석 및 일일 브리핑을 Slack DM으로 전송하는 자동화 봇

## 개요

- **목적**: 매일 오전 9시(KST) 비트코인 시장 분석 리포트를 Slack으로 자동 전송
- **핵심 기술**: GitHub Actions (cron) + Claude Code Action + Slack MCP
- **실행 주기**: 매일 00:00 UTC (= 09:00 KST)

## 아키텍처

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────┐
│ GitHub Actions  │────▶│ Claude Code      │────▶│ Slack MCP   │
│ (cron trigger)  │     │ (분석 & 리포트)   │     │ (DM 전송)   │
└─────────────────┘     └──────────────────┘     └─────────────┘
                              │
                              ▼
                        ┌──────────────┐
                        │ Crypto APIs  │
                        │ (가격 데이터) │
                        └──────────────┘
```

## 데이터 소스

Claude가 웹 검색을 통해 수집:
- **가격 데이터**: CoinGecko, CoinMarketCap
- **뉴스/이벤트**: 주요 크립토 뉴스 사이트
- **온체인 데이터**: Glassnode 공개 지표 (선택적)

## 리포트 구성

```markdown
📊 BTC 일일 브리핑 | 2026-02-05

## 현재 가격
- 현재가: $XX,XXX
- 24h 변동: +X.X%
- 주간 변동: +X.X%

## 기술적 분석
- 주요 지지선/저항선
- 거래량 동향
- RSI/MACD 요약

## 시장 심리
- Fear & Greed Index
- 주요 뉴스 요약

## 오늘의 전망
- 단기 방향성 예측
- 주의 사항

⚠️ 이 리포트는 투자 조언이 아닙니다.
```

## 필요한 시크릿

| 시크릿 이름 | 설명 | 발급 방법 |
|------------|------|----------|
| `CLAUDE_CODE_OAUTH_TOKEN` | Claude Code Action 인증 | [console.anthropic.com](https://console.anthropic.com) |

> **Note**: kithlabs org secret으로 설정하면 모든 repo에서 공유 가능

## 마일스톤

- [x] Phase 1: 구현 계획 수립
- [x] Phase 2: GitHub repo 생성 & 워크플로우 설정
- [ ] Phase 3: 시크릿 설정 & 테스트 실행
- [ ] Phase 4: 모니터링 & 개선

## 확장 가능성

1. **멀티 코인 지원**: ETH, SOL 등 추가
2. **알림 조건**: 급등/급락 시 즉시 알림
3. **커스텀 채널**: 팀 채널로 전송 옵션
4. **히스토리 저장**: 리포트 아카이브 기능