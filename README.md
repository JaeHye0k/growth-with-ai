# growth-with-ai — Claude Code Plugin Marketplace

개인 Claude Code 플러그인 마켓플레이스입니다. 여러 플러그인을 이 저장소 하나로 관리합니다.

## 설치

```bash
# 1. 마켓플레이스 등록
claude plugin marketplace add JaeHye0k/growth-with-ai

# 2. 원하는 플러그인 설치
claude plugin install coding@growth-with-ai
```

## 플러그인 목록

| 플러그인 | 설명 | 문서 |
|---|---|---|
| **coding** | AI에 코드 작성을 위임하면서도 작성력·이해도·검증력을 유지하기 위한 가드레일. 핵심 로직은 직접 작성하게 하고, 리뷰 시 직접 설명을 요구하며, 엣지 케이스/코드 퀄리티/보안 관점의 질문을 던진다. | [README](plugins/coding/README.md) |

## 저장소 구조

```
.
├── .claude-plugin/
│   └── marketplace.json     # 마켓플레이스 매니페스트 (플러그인 목록)
└── plugins/
    └── <plugin-name>/       # 플러그인 하나당 디렉토리 하나
        ├── .claude-plugin/
        │   └── plugin.json
        ├── README.md
        └── skills/
            └── <skill-name>/
                └── SKILL.md
```

## 새 플러그인 추가하기

1. `plugins/<새-플러그인>/` 디렉토리를 만들고 `.claude-plugin/plugin.json`, `skills/`, `README.md`를 채운다.
2. 루트 `.claude-plugin/marketplace.json`의 `plugins` 배열에 항목을 추가한다:
   ```json
   {
     "name": "<새-플러그인>",
     "source": "./plugins/<새-플러그인>",
     "description": "..."
   }
   ```
3. 검증 후 푸시한다:
   ```bash
   claude plugin validate .
   git add -A && git commit && git push
   ```
4. 설치된 쪽에서 갱신한다:
   ```bash
   claude plugin marketplace update growth-with-ai
   claude plugin install <새-플러그인>@growth-with-ai
   ```
