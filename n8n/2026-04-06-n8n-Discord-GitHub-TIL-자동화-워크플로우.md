### Context
디스코드(Discord)로 수집한 지식을 자동으로 구조화하여 깃허브(GitHub) 저장소에 TIL(Today I Learned) 문서로 기록하고, 인덱스 업데이트까지 자동화하는 워크플로우를 구축했습니다. 파편화된 메모를 LLM 에이전트가 기술 문서 형식으로 변환하여 관리 효율성을 높이는 것이 핵심입니다.

### Core
n8n 워크플로우의 주요 단계별 노드 구성과 데이터 흐름은 다음과 같습니다.

* **Discord Trigger**: 특정 채널에서 메시지가 수신되면 트리거가 발생하며, 메시지 내용을 다음 노드로 전달합니다.
* **AI Agent (LLM)**: 수신된 단편적인 텍스트를 분석하여 지정된 'TIL 템플릿'에 맞춰 Markdown 형식으로 변환합니다. (기술 용어 보정 및 구조화 수행)
* **GitHub Node (File Push)**: 변환된 Markdown 데이터를 `YYYY-MM-DD-제목.md` 규칙에 따라 지정된 경로에 신규 파일로 생성합니다.
* **GitHub Node (README Update)**: 기존 `README.md` 파일을 불러와 신규 문서에 대한 링크와 인덱스를 추가한 뒤 커밋(Commit)합니다.
* **Discord Node (Send Message)**: 모든 프로세스가 완료되면 사용자에게 깃허브 업로드 완료 및 링크가 포함된 확인 메시지를 발신합니다.

### Insight
n8n 워크플로우 설계 시 가장 중요한 핵심은 각 노드의 **Input(입력값)**과 **Output(출력값)**의 구조를 완벽히 이해하는 것입니다. 에이전트가 내보내는 JSON 객체의 속성명을 정확히 파악해야 GitHub 노드에서 파일 경로와 내용을 매핑할 때 오류를 방지할 수 있습니다. 

단순한 데이터 전달을 넘어, LLM을 중간 브릿지로 활용함으로써 비정형 데이터를 정형 기술 문서로 변환하는 자동화의 강력함을 체감했습니다. 이 입문 과정을 도와준 후츠릿님께 감사를 표합니다.

**출처:**
* [n8n Discord Trigger Documentation](https://docs.n8n.io/integrations/builtin/trigger-nodes/n8n-nodes-base.discordtrigger/)
* [n8n AI Agent Node Overview](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.aiagent/)
* [n8n GitHub Node Guide](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.github/)
