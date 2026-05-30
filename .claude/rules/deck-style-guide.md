# FREEDOBY Slide Deck — 스타일 지침

이 문서는 `docs/slides/**/deckN-*.html` 형식 발표 자료의 **디자인 시스템·작성 톤·배치 흐름**을 규정한다.
새 deck을 만들거나 기존 deck을 수정할 때 **항상 이 지침을 따른다.** 기준 구현체는 `series-3/deck3-evolution.html`.

---

## 0. 절대 규칙 (비협상)

1. **글자 크기는 14px 이상.** 캡션·라벨·표 셀 무엇이든 14px 미만 금지.
2. **페이지 번호는 chrome과 foot 양쪽에 `NN / TT`** 형식으로 둔다. 슬라이드를 추가/병합/삭제하면 **전 슬라이드의 분모(TT)와 이후 번호(NN)를 모두 갱신**한다 (표지 cover-footer 포함).
3. **단일 HTML 파일** — 외부 CSS/JS 없이 `<style>` 한 블록 + 인라인. 폰트는 기존 4종(`--serif`, `--serif-ko`, `--sans`, `--mono`)만 사용.
4. **색은 팔레트 토큰만.** 임의의 hex(특히 쨍한 원색)를 새로 들이지 않는다. 의미색은 §2 규칙을 따른다.

---

## 1. 디자인 토큰 (`:root`)

```
--black   #0c0b09   다크 배경 / 라이트 텍스트
--cream   #f0ece0   다크 위 본문 텍스트
--warm    #f5f0e6   라이트 배경
--gold    #b8924a   1차 강조 · Vendor · em
--gold-lt #ddb96a   다크 위 보조 골드
--gold-dk #7a5f2e   라이트 위 골드(대비 확보)
--muted-lt #c8c0b0  다크 위 보조 텍스트
--muted-dk #6a6458  라이트 위 보조 텍스트
--rule-lt / --rule-dk   경계선 (다크 / 라이트)
--red #b04a3a · --green #7a8a4a · --blue #6a8aa6   근거·차트용
```

폰트: `--serif`(Cormorant, 영문 em/display) · `--serif-ko`(Gowun Batang, 한글 제목/본문) · `--sans`(IBM Plex Sans KR, 기본) · `--mono`(JetBrains Mono, chrome/foot/라벨/코드).

배경엔 항상 미세 도트 텍스처(`.slide::before`)가 깔린다 — 건드리지 않는다.

---

## 2. 의미색 (semantic color) — 가장 자주 깨지는 규칙

| 개념 | 색 | 적용처 |
|---|---|---|
| **Vendor / 1차 강조 / em** | `--gold` (다크), `--gold-dk` (라이트) | em, kicker, 강조 |
| **User** | `#8fb2cc` (다크), `#4f7796` (라이트) | User 관련 라벨·카드·텍스트 **전부** |
| **대화 / 런타임 / 중립** | `--muted-lt` / `--muted-dk` | 가변·부차 정보 |
| **근거·경고** | `--red` | 포화/한계/리스크 |

- **한 개념은 deck 전체에서 한 색.** 예: User를 처음 소개한 슬라이드부터 마지막까지 `#8fb2cc`. (과거 사고: User 카드만 골드, 이후 파랑 → 불일치)
- 어떤 슬라이드가 **단일 주제**면(예: 전부 Vendor 내용) 그 슬라이드의 강조는 **그 개념색 하나로 통일.**
- 강조 수단은 셋뿐: `<em>`(개념색 이탤릭) · `<strong>`(cream/black 볼드) · `<mark>`(노란 형광, **카드당 핵심 1구절만**). 한 요소에 노랑 3중첩 금지.

---

## 3. 슬라이드 골격

모든 슬라이드는 이 3단 구조:

```html
<section class="slide dark">            <!-- dark | light -->
  <div class="chrome">
    <div class="seg"><span class="dot"></span><span>섹션명</span></div>
    <div class="tag">#hashtag #hashtag</div>
    <div>NN / TT</div>
  </div>

  <div class="stage">                    <!-- 본문 (§4 레이아웃) -->
    ...
  </div>

  <div class="foot"><span>섹션 · 부제</span><span>NN / TT · → 다음</span></div>
</section>
```

- **chrome**: 좌측 `seg`(점+섹션) · 중앙 `tag`(#해시태그, 골드) · 우측 `NN / TT`.
- **foot**: 좌측 `섹션 · 부제` · 우측 `NN / TT · → 다음장단서`. 화살표 단서는 다음 슬라이드 내용과 일치시킨다.
- chrome/foot 텍스트는 mono·대문자·자간 넓게. 14px.

---

## 4. 본문 레이아웃 (`.stage`)

| 클래스 | 용도 |
|---|---|
| `.stage` | 세로 가운데 정렬 (기본) |
| `.stage.top` | 위 정렬 — 표·카드 등 **내용이 많은 슬라이드** |
| `.stage.stage-split` | 좌우 2단 (1fr 1fr, gap 56px). `.center`로 수직 가운데 |
| `.has-left-split` | 좌측을 상/하로 다시 분할 (Stage 예제 슬라이드용) |

분할 패널 정렬: **가로 2단은 양쪽 모두 `justify-content:center`.** 위/아래 수직 분할의 아래쪽만 풀필(fill) 패턴 허용. (좌측을 `flex-start`로 두는 건 수직분할 전용 — 가로분할에 쓰지 말 것)

---

## 5. 타이포 컴포넌트 — 언제 무엇을

| 클래스 | 크기 | 용도 |
|---|---|---|
| `.kicker` | 14px mono, 골드, 앞에 짧은 선 | 제목 위 **셋업 한 줄**. 풀스테이지 슬라이드엔 **필수** |
| `.kicker.dim` | 〃 muted | 분할 슬라이드 하단 보조 라벨 |
| `.display` (`.md`/`.sm`) | 거대 serif 영문 | 표지·전환의 영문 대제목 |
| `.keyword` (`.md`/`.sm`) | 큰 serif-ko | **한글 핵심 메시지.** 풀스테이지 기본은 `.keyword.md` |
| `.subline` | 15–20px serif-ko, muted | 제목 보조 설명 (2–3줄) |
| `.callout` | 가운데, 골드 상단선 | 슬라이드 **결론 한 방** |

**일관성 규칙**: 같은 섹션·같은 배경의 풀스테이지 슬라이드는 **제목 요소를 통일**한다. (예: Primer 다크 3장은 모두 `kicker + keyword.md`.) 시리즈성 슬라이드(Stage I/II/III처럼)는 자기들끼리 동일 패턴을 유지하면 별도 변형 허용.

---

## 6. 재사용 컴포넌트

- **`.card-grid` + `.card`** (2단): `card-lbl`(mono 골드 라벨) · `card-name`(serif-ko 대제목, em=개념색) · `card-by`(mono 메타) · `card-desc`(serif-ko 본문, `<strong>`/`<mark>`). 대비 개념엔 `.card.user`(파랑 틴트) 등 개념색 modifier.
- **`.mapping` + `.map-row.*`**: 표. `ctx/ctxm`(=/context 항목 매핑), `vocab`, `fwk` 등 용도별 `grid-template-columns`를 가진 modifier로 분기. 새 표는 새 modifier를 정의해 기존 것과 섞지 않는다.
- **`.context-viz`**: `/context` 막대그래프 + 범례 (mono, 200K 윈도우 시각화).
- **`.vh-quote` / `.vh-name` / `.vh-src`**: 외부 원문 **verbatim 발췌 카드**용. `vh-name`엔 출처 식별자(파일명 등), `vh-src`엔 출처 한 줄. 발췌는 영문 원문 그대로 + 한글 해설을 `card-desc`에.

새 컴포넌트가 필요하면 **기존 토큰·간격·폰트로** 만들고, 클래스명은 `블록-요소` 패턴으로.

---

## 7. 내러티브 흐름 (목차 골격)

deck은 다음 호(arc)를 따른다:

```
Cover → Opening(문제 제기) → Primer(개념·도구 정의)
      → Stage I·II·III(본론, 시리즈 패턴) → Synthesis(종합)
      → Frameworks/응용 → Fin(다음 deck으로 bridge)
```

- **배경 리듬**: 논증·Primer·Synthesis·Fin = **dark**, 섹션 전환·예제(Stage) = **light**. 다크↔라이트 교차로 챕터 경계를 만든다.
- **섹션 라벨**(chrome seg)은 호와 일치 (`Opening` / `Primer · …` / `Stage I` / `Synthesis` / `Fin`).
- 한 개념은 **소개(주장) → 측정/근거 → 분해/매핑 → 전환**의 순서로 펼친다 (Primer 3~6장이 예시).
- 슬라이드를 끼워 넣을 땐 **앞 슬라이드의 용어를 그대로 이어받는다** (예: 03·04에서 "Vendor Harness"를 썼으면 05 제목에도 등장해야 흐름이 안 끊김).

---

## 8. 작성 톤 (copywriting)

기준은 **짧고 단정한 한글 + 일관된 영문 기술용어 + em-dash 리듬.**

**해야 할 것**
- **질문 → 답** 구조: kicker나 subline이 질문을 던지고 keyword가 답한다.
- **구체로 말한다**: 추상어 대신 수치·이름·실물(`.claude/`, `/context`, 파일명).
- **em-dash(`—`)로 끊어 강조**, 한 문장 한 호흡.
- 영문 고유 용어(Vendor Harness, System prompt, MCP)는 **표기를 deck 내내 통일.**
- 발표 1인칭은 **"우리"** (제품/팀 관점).

**하지 말 것**
- **목차·내비게이션 메타 금지**: "04번 이후 본문에서…", "다음 세 장을 보기 전" 류 — 내용과 무관한 안내는 뺀다.
- **읽는 법 설명 금지**: "막대는 X, 표는 Y" 처럼 레이아웃을 해설하지 말고 **내용**을 쓴다.
- **가변값을 고정값처럼 단정 금지**: `/context` 토큰 수처럼 상황 따라 변하는 값은 "예시·스냅샷"임을 전제로, 핵심은 **구조**에 둔다. 변하는 숫자를 결론 문장에 못 박지 않는다.
- **반말("너/네") 금지** — 발표 톤에 맞춰 "우리/사용자".
- 빈 수사·과장 형용사 남발 금지. 한 단어로 될 걸 한 문단으로 늘리지 않는다.

---

## 9. 마무리 체크리스트

deck 수정/생성 후 반드시 확인:

- [ ] 페이지 번호 `NN / TT` — chrome·foot·cover 전부, 1..TT 빠짐/중복 없음
- [ ] 의미색 일관 (Vendor=골드 / User=파랑 / 대화=muted) — 첫 등장부터 끝까지
- [ ] 같은 섹션 풀스테이지 슬라이드의 제목 요소(kicker/keyword) 통일
- [ ] 14px 미만 글자 없음
- [ ] foot 화살표 단서 = 실제 다음 슬라이드
- [ ] 끼워 넣은 슬라이드가 앞 슬라이드 용어를 이어받음
- [ ] 메타·읽는법·가변값-단정·반말 없음 (§8)
- [ ] 브라우저에서 실제 렌더 확인 (세로 넘침/빈공간 점검)
```
