# 멘토원 플래시카드 프로젝트

## 프로젝트 개요
생활스포츠지도사 2급 구술시험 대비 플래시카드 웹앱.
단일 HTML 파일, 빌드 도구 없음, 바닐라 JS.

## 파일 구조
```
mentorone-flashcard/
├── index.html         # 메인 허브 (과목 선택)
├── anatomy.html       # 기능해부학 (60문제)
├── physiology.html    # 운동생리학 (44문제)
├── training.html      # 트레이닝방법론 (65문제)
├── nutrition.html     # 운동영양학 (43문제)
└── regulation.html    # 보디빌딩 규정 (162문제)
```

## 배포
- GitHub Pages: `leeone21/26mentoronebbflash` 레포, `main` 브랜치
- 작업 브랜치: `claude/inspiring-noether-bx5Mi` → PR 후 main 머지

## 카드 데이터 형식

### 일반 카드
```js
{
  part: 'PART 1 · 섹션명',
  category: 'PART 1',
  q: '질문',
  keywords: ['빈칸1', '빈칸2', '빈칸3'],
  wrongs: ['오답1', '오답2', '오답3'],
  answerText: '앞부분 {빈칸1} 중간 {빈칸2} 끝 {빈칸3}',
  a: '전체 정답 텍스트',
}
```

### Pool 카드 (랜덤 빈칸)
```js
{
  part: 'PART 1 · 섹션명',
  category: 'PART 1',
  q: '질문',
  pool: ['항목1', '항목2', '항목3', '항목4', '항목5'],
  pickCount: 3,   // 매 렌더링마다 pool에서 랜덤으로 N개 선택해 빈칸
  wrongs: ['오답1', '오답2', '오답3'],
  a: '전체 정답 텍스트',
}
```
- pool 카드: answerText 없음, renderBlank()에서 동적 생성
- pool에서 빈칸으로 안 뽑힌 항목이 오답 보기 1순위

## localStorage 키
| 과목 | 키 |
|------|----|
| 기능해부학 | `mentorone_wrong_anatomy` |
| 운동생리학 | `mentorone_wrong_physiology` |
| 트레이닝방법론 | `mentorone_wrong_training` |
| 운동영양학 | `mentorone_wrong_nutrition` |
| 보디빌딩 규정 | `mentorone_wrong_regulation` |

## JS 핵심 상태 변수
```js
const ALL_CARDS = [...];
const CATEGORIES = ['ALL', 'PART 1', ...];
const CATEGORY_LABELS = { ALL:'전체', 'PART 1':'...' };
const LS_KEY = 'mentorone_wrong_XXXXX';

let currentCards = [], currentIndex = 0;
let correctCount = 0, wrongCount = 0;
let answeredSet = new Set(), wrongSet = new Set();
let currentCategory = 'ALL', currentMode = 'flash';
let blankKeywords = [], blankFilled = [], blankResults = [];
let blankCurrentIdx = 0, blankCardCorrect = true;
let currentAnswerText = '';
```

## 새 과목 파일 추가 체크리스트
1. 기존 과목 HTML을 복사해 템플릿으로 사용
2. `<title>`, `.header-title` 텍스트 변경
3. `ALL_CARDS` 배열을 새 카드 데이터로 교체
4. `CATEGORIES`, `CATEGORY_LABELS` 업데이트
5. `LS_KEY` 변경
6. `index.html` 수정:
   - `WRONG_NOTE_KEYS`에 새 키 추가
   - `TOTAL_COUNTS`에 문제 수 추가
   - `updateUI()`에 count 엘리먼트 업데이트 추가
   - `goWrongNote()` urls에 추가
   - subject-grid에 카드 HTML 추가

## 작업 규칙
- 파일 수정 전 반드시 사용자에게 먼저 확인
- 논의 중에는 코드 변경 금지
- 커밋 후 `git push -u origin main`으로 push
