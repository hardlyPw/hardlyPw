# NotiPy · LLM 기반 개발자 역할 배치

**본인 설명 기준 담당 작업은 LLM을 이용한 개발자 역할 배치 기능입니다.** 공개 저장소에서 이에 대응하는 구현을 확인했습니다. 공개 이력은 다른 작성자의 초기 통합 커밋에 포함되어 있어 개인 커밋 귀속은 추가 확인이 필요합니다.

[팀 저장소](https://github.com/Notipy-DiscordBot/Notipy) · [초기 통합 커밋](https://github.com/Notipy-DiscordBot/Notipy/commit/7a77c48f5b5304d80ccade0bdbe81367ee30b785)

## 해결하려던 문제

프로젝트의 역할 요구사항과 팀원의 개발 이력을 함께 사용해 역할 배치 후보를 만드는 기능입니다. Notion·Discord 협업 도구 안에서 프로젝트 구성원 배치를 지원하는 흐름으로 연결됩니다.

## 확인한 기술과 처리 흐름

사용 라이브러리는 **llm-axe**, 로컬 LLM 연결은 **Ollama**, 코드의 모델명은 **llama3:instruct**입니다.

1. GitHub 언어·개발 경험·공개 저장소·star/fork 등 멤버 정보를 모읍니다.
2. 프로젝트·역할 설명·기술 스택과 멤버 정보를 프롬프트로 구성합니다.
3. 멤버×역할 조합별 LLM 호출에서 JSON 점수를 요청합니다. JSON 파싱 실패 시 점수 0으로 처리합니다.
4. 점수를 높은 순서로 정렬하고 역할별 정원과 1인 1역할 제약으로 순차 배치합니다.
5. 배치 결과를 DB에 반영하고 Discord에서 역할별 멤버를 보여주는 흐름이 연결돼 있습니다.

이는 greedy 방식의 배치입니다. 전체 팀 점수의 최적해를 구하는 알고리즘이나 배치 공정성·효과가 검증된 시스템으로 소개하지 않습니다.

## 코드 근거

| 근거 | 확인 내용 |
| --- | --- |
| [backend/common.py](https://github.com/Notipy-DiscordBot/Notipy/blob/4414f3b/backend/common.py#L20) | llm-axe Agent와 Ollama 모델 설정 |
| [backend/routers/llm.py](https://github.com/Notipy-DiscordBot/Notipy/blob/4414f3b/backend/routers/llm.py#L394) | assign_users_to_roles: 입력 구성·점수화·배치 |
| [backend/services/llmservice.py](https://github.com/Notipy-DiscordBot/Notipy/blob/4414f3b/backend/services/llmservice.py#L238) | 역할 배치 DB 반영 |
| [discordbot/extensions/Projects.py](https://github.com/Notipy-DiscordBot/Notipy/blob/4414f3b/discordbot/extensions/Projects.py#L251) | 배치 API와 Discord 결과 연결 |

## 기여 근거와 확인 범위

담당 기능은 본인이 설명한 내용이며, 코드의 존재만으로 해당 코드를 모두 본인이 작성했다고 확정하지 않습니다. 확인한 공개 브랜치·커밋에서는 `hardlyPw` 이름의 작성 이력을 찾지 못했고 핵심 구현은 Code0987의 초기 커밋으로 들어와 있습니다. 이전 저장소·다른 Git 작성자 계정·개발 자료가 있으면 귀속 근거를 보강할 수 있습니다.

저장소는 개발 중지 상태를 명시합니다. 현재 운영 중인 봇, 실제 배치 품질 개선, 비용 절감 또는 실사용 성과로 주장하지 않습니다. 이번 확인은 정적 코드·이력 검토이며 API·DB·Discord를 연결한 실행 검증은 수행하지 않았습니다.
