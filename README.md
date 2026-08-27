# Vue Weather Dashboard

Vue 3(Composition API) 학습을 위한 실습 프로젝트입니다. OpenWeatherMap API에서 도시별 날씨를 가져와 대시보드로 보여주고, Vue Router / Pinia / Axios / Element Plus를 하나의 SPA에서 함께 사용해봅니다.

## 주요 기능

- **도시별 날씨 대시보드**: 서울/수원/부산 3개 도시의 현재 날씨를 카드 목록으로 표시
- **검색**: 도시 이름으로 필터링, 검색어는 URL 쿼리스트링(`?search=`)에 보존되어 새로고침해도 유지됨
- **상세 화면**: 카드의 "상세보기"를 누르면 `/weather/:cityId`로 이동해 습도·풍속 등 상세 정보 확인
- **온도 단위 전환**: 헤더의 스위치로 섭씨/화씨를 전역 전환 (Pinia 스토어로 상태 공유)
- **로딩/에러 처리**: API 호출 중 로딩 표시, 실패 시 에러 메시지 표시
- **잘못된 경로 처리**: 존재하지 않는 도시 ID나 정의되지 않은 경로는 404(Not Found) 화면으로 리다이렉트

## 기술 스택

| 영역 | 사용 기술 |
| --- | --- |
| 프레임워크 | [Vue 3](https://vuejs.org/) (`<script setup>`, Composition API) |
| 빌드 도구 | [Vite](https://vite.dev/) |
| 라우팅 | [Vue Router](https://router.vuejs.org/) |
| 상태 관리 | [Pinia](https://pinia.vuejs.org/) |
| HTTP 클라이언트 | [Axios](https://axios-http.com/) |
| UI 컴포넌트 | [Element Plus](https://element-plus.org/) |
| 날씨 데이터 | [OpenWeatherMap API](https://openweathermap.org/api) |
| 린트/포맷 | ESLint, oxlint, oxfmt |

## 프로젝트 구조

```
src/
├── components/weather/     # 대시보드 UI 컴포넌트 (검색바, 날씨 카드, 단위 토글 등)
├── views/                  # 라우트에 매핑되는 페이지 (홈/소개/상세/404)
├── router/                 # 라우트 정의 및 네비게이션 가드
├── stores/                 # Pinia 스토어 (온도 단위 등 전역 상태)
├── composables/            # 재사용 로직 (온도 단위 변환 등)
└── services/                # OpenWeatherMap API 호출 로직 (weatherApi.js)
```

## 시작하기

### 1. 의존성 설치

```sh
npm install
```

### 2. 환경 변수 설정

OpenWeatherMap에서 발급받은 API Key가 필요합니다. [openweathermap.org](https://openweathermap.org/api)에서 무료로 발급받을 수 있습니다.

`.env.example`을 복사해 `.env.local`을 만들고 발급받은 키를 입력하세요.

```sh
cp .env.example .env.local
```

```
VITE_OPENWEATHER_API_KEY=your_api_key_here
```

> API Key가 없으면 날씨 데이터를 불러오지 못하고 화면에 에러 메시지가 표시됩니다.

### 3. 개발 서버 실행

```sh
npm run dev
```

### 4. 빌드 / 미리보기

```sh
npm run build     # 프로덕션 빌드 (dist/ 생성)
npm run preview   # 빌드 결과 미리보기
```

### 5. 린트 / 포맷

```sh
npm run lint      # oxlint + eslint 자동 수정
npm run format    # oxfmt로 src/ 포맷팅
```

## 권장 개발 환경

- [VS Code](https://code.visualstudio.com/) + [Vue (Official)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) 확장 (Vetur는 비활성화)
- 브라우저: [Vue.js devtools](https://devtools.vuejs.org/) 확장 설치 권장
