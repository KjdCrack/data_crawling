### **텍스트 데이터**

- **출처:** Reddit 공개 JSON 엔드포인트
- **대상 서브레딧:**
    - r/Bitcoin
    - r/CryptoCurrency
- **수집 방식:**
    - new.json + after 페이징을 이용한 최근 3개월 데이터 수집
    - 각 게시글의 .json 엔드포인트를 통해 댓글까지 포함 수집
- **수집 도구:** Python (requests, pandas, tqdm)

---

## **2. 사용한 데이터 종류**

### **🔹 1) 가격 데이터 (Market Data)**

- 시가 (open)
- 고가 (high)
- 저가 (low)
- 종가 (close)
- 거래량 (volume)
- 시간 정보 (UTC → KST 변환)

### **🔹 2) Reddit 텍스트 데이터 (Text Data)**

- 게시글 (post)
- 댓글 (comment)
- 작성 시간 (created_utc → KST 변환)
- 점수 (score)
- 정제된 텍스트 (text_clean)
- 키워드 기반 토픽 태그 (topic_tag)
    - btc
    - macro
    - geopolitics
- 시장 활용 힌트 라벨 (use_case_hint)
    - 1: 시장 국면 분석용
    - 2: 감성 분석용
    - 3: 둘 다 활용 가능

---

## **3. 데이터 형식**

### **📁 1) Reddit 원본 정제 데이터**

파일명: reddit_3months_labeled_clean.csv

| **컬럼명** | **설명** |
| --- | --- |
| source | 데이터 출처 (reddit) |
| kind | post / comment |
| subreddit | 서브레딧 이름 |
| id | 고유 ID |
| score | 추천 점수 |
| created_dt_kst | 한국시간 기준 작성 시각 |
| hour_kst | 시간 단위 정렬용 컬럼 |
| text_clean | 정제된 텍스트 |
| keyword_hits | 매칭된 키워드 |
| topic_tag | 토픽 분류 |
| use_case_hint | 분석 목적 힌트 |

## **. 데이터 전처리 과정**

1. HTML / URL / 마크다운 제거
2. 공백 정리
3. 키워드 기반 필터링 (코인, 거시, 지정학 관련)
4. 시간 기준 KST 변환
5. 시간 단위 그룹화 (1시간 단위 집계)
