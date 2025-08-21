# 🧠 LangChain Memory 실험 보고서

## 📌 1. ConversationBufferMemory
모바일에서 입력한 대화가 PC에서도 이어지는 것을 확인했습니다.  
대화 전체가 차곡차곡 저장되므로, 다시 물어보면 전문을 출력하지는 않지만  
**지금까지의 대화를 요약해주는 모습**을 볼 수 있습니다.

![ConversationBufferMemory](memory1.jpg)  
![ConversationBufferMemory](memory2.PNG)  
![ConversationBufferMemory](memory3.PNG)

---

## 📌 2. ConversationBufferWindowMemory (k=4)
최근 메시지 **k개만 유지**하는 방식입니다.  
실험에서는 `k=4`로 설정했기 때문에 원래라면 5번째 질문에서 이름을 잊어야 하지만,  
실제 결과에서는 여전히 **“손용훈님”**이라고 기억하는 모습을 보였습니다.  
→ 즉, **대화 전문이 남아 있었기 때문**입니다.

![ConversationBufferWindowMemory](memory4.PNG)  
![ConversationBufferWindowMemory](memory5.PNG)

---

## 📌 3. ConversationTokenBufferMemory (max_token_limit=200)
토큰 기준으로 대화를 저장하는 방식입니다.  
`max_token_limit=200`으로 작게 설정했을 때는  
**3번째 질문에서 이미 1번째 이름 정보가 사라지는 현상**을 확인했습니다.

![ConversationTokenBufferMemory 200](memory6.PNG)  
![ConversationTokenBufferMemory 200](memory7.PNG)

---

## 📌 4. ConversationTokenBufferMemory (max_token_limit=400)
토큰 제한을 **400으로 늘렸더니**  
앞서 잃어버렸던 이름 정보를 끝까지 유지할 수 있었습니다.  
즉, 토큰 한도를 조절하면 **메모리 보존력이 달라진다**는 것을 확인했습니다.

![ConversationTokenBufferMemory 400](memory8.PNG)  
![ConversationTokenBufferMemory 400](memory9.PNG)

---

# 📚 LangChain Memory 유형 요약

## 1. ConversationBufferMemory
- 대화 전체를 순서대로 쌓아두는 기본 메모리  
- 단순하지만 길어지면 토큰 부담이 커짐  

## 2. ConversationBufferWindowMemory
- 최근 N개의 메시지만 유지  
- 맥락 유지 + 메모리 절약 가능  

## 3. ConversationTokenBufferMemory
- **토큰 기준**으로 최근 대화를 유지  
- 모델 토큰 한도에 맞춰 자동 조절  

## 4. ConversationEntityMemory
- 대화 속 **인물·장소·사물** 같은 엔티티 중심 기억  
- “민준이가 개발자였지?” 같은 사실 추적 가능  

## 5. ConversationKnowledgeGraphMemory
- 대화를 **지식 그래프**(노드-관계 구조)로 저장  
- 예: “민준 — 직업 — 개발자”  

## 6. ConversationSummaryMemory
- 전체 대화를 **요약본**으로 압축해 저장  
- 장기 대화에 유리하지만 디테일 손실 위험  

## 7. VectorStoreRetrieverMemory
- 대화를 **벡터 DB**에 저장 후 의미 기반 검색  
- 시맨틱 서치 기반 맥락 유지 가능  

## 8. SQLite Memory
- 대화를 **SQLite DB**에 저장  
- 영구 보존 + SQL 쿼리 가능  

## 9. Conversation With History
- 외부 저장소(DB/파일)에서 과거 이력을 불러와 반영  
- 단순 메모리라기보다 **기록 관리** 성격  

---

# ✅ 실험 결론
- **BufferMemory**: 전체 맥락 유지하지만 길면 토큰 부담  
- **WindowMemory**: 최근 대화만 유지 → 맥락 유지력 제한적  
- **TokenBufferMemory**: 토큰 한도 관리 가능, 제한 크기에 따라 기억 범위 달라짐 
- **SummaryMemory (추가 실험 필요)**: 장기 대화에 유리하나 세부정보 손실 가능  

👉 실험을 통해 **Memory 선택은 사용 목적(FAQ/상담/코딩 보조 등)에 따라 달라져야 한다**는 것을 알 수 있었습니다.
