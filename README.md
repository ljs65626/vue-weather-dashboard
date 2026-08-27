# Vue Weather Dashboard

Vue 3(Composition API) 학습을 위한 실습 프로젝트입니다. OpenWeatherMap API에서 도시별 날씨를 가져와 대시보드로 보여주며, **Vue Router(라우팅) → Pinia(전역 상태) → Axios(API 통신) → Element Plus(UI)** 를 하나의 SPA 안에서 실제로 어떻게 엮는지 보여주는 것이 목적입니다.

## 전체 동작 흐름

```
main.js
  └─ createApp(App) + Pinia/Router 등록
       └─ App.vue (헤더 + RouterView)
            └─ router/index.js 가 현재 경로에 맞는 View를 렌더링
                 ├─ WeatherHomeView  → WeatherDashboard (목록/검색)
                 ├─ WeatherDetailView (상세)
                 ├─ WeatherAboutView (소개)
                 └─ NotFoundView (404)
```

데이터는 `services/weatherApi.js`(Axios)가 OpenWeatherMap을 호출 → View/컴포넌트의 `ref`에 저장 → 화면에 표시, 순서로 흐릅니다. 온도 단위(섭씨/화씨)처럼 여러 화면이 공유해야 하는 값은 `stores/configStore.js`(Pinia)에 두고 어디서든 꺼내 씁니다.

## 디렉터리 구조와 각 파일의 역할

```
src/
├── main.js                      # 앱 진입점: Vue 인스턴스 생성, Pinia/Router/Element Plus 등록
├── App.vue                      # 최상위 레이아웃(헤더, 네비게이션, 단위 토글) + <RouterView>
│
├── router/
│   └── index.js                 # 경로 ↔ 화면 매핑 정의, 잘못된 cityId 접근 시 404로 리다이렉트하는 가드
│
├── views/                       # 라우트에 직접 연결되는 "페이지" 컴포넌트
│   ├── WeatherHomeView.vue      # "/"       → WeatherDashboard를 렌더링만 함
│   ├── WeatherDetailView.vue    # "/weather/:cityId" → 도시 상세 날씨 조회·표시
│   ├── WeatherAboutView.vue     # "/about"  → 서비스 소개, provide/inject로 앱 제목 표시
│   └── NotFoundView.vue         # 정의되지 않은 경로 / 잘못된 cityId 접근 시 표시
│
├── components/weather/          # 날씨 대시보드를 구성하는 UI 조각들
│   ├── WeatherDashboard.vue     # 목록 화면의 컨테이너: 검색·로딩·에러 상태를 관리하고 API 호출
│   ├── SearchBar.vue            # 검색어 입력(el-input), 값은 부모에게 emit으로 전달 (상태를 직접 갖지 않음)
│   ├── WeatherCard.vue          # 도시 1개의 요약 카드, 클릭 시 상세로 이동
│   ├── UnitToggler.vue          # 헤더의 섭씨/화씨 스위치, Pinia 스토어 값을 직접 변경
│   └── BaseDashboardCard.vue    # 제목 슬롯 + 본문 슬롯을 가진 카드 레이아웃 (재사용 wrapper)
│
├── stores/
│   └── configStore.js           # Pinia 스토어: 온도 단위(unit) 전역 상태 + 전환 함수
│
├── composables/
│   └── useTemperature.js        # 섭씨 값을 스토어의 단위 설정에 맞춰 변환해주는 재사용 로직
│
├── services/
│   └── weatherApi.js            # Axios 인스턴스 + OpenWeatherMap 호출/응답 정규화 함수
│
└── assets/                      # 전역 CSS
```

> `components/HelloWorld.vue`, `TheWelcome.vue`, `WelcomeItem.vue`, `components/icons/*`, `views/HomeView.vue`, `views/AboutView.vue`, `stores/counter.js`는 `npm create vue@latest`가 생성한 기본 스캐폴드 파일로, 라우터에서 참조되지 않는 **미사용 예시 코드**입니다. 실제 앱은 `Weather*` 접두사가 붙은 파일들로 동작합니다.

## 화면(라우트) 구성

| 경로 | 이름 | 컴포넌트 | 설명 |
| --- | --- | --- | --- |
| `/` | `WeatherHome` | `WeatherHomeView` → `WeatherDashboard` | 도시 목록 + 검색 |
| `/weather/:cityId` | `WeatherDetail` | `WeatherDetailView` | 도시 상세 날씨. `cityId`가 등록된 도시(`city_01~03`)가 아니면 라우터 가드가 `/not-found`로 보냄 |
| `/about` | `WeatherAbout` | `WeatherAboutView` | 서비스 소개, `App.vue`가 `provide`한 `appTitle`을 `inject`로 사용 |
| `/not-found`, 그 외 모든 경로 | `NotFound` | `NotFoundView` | 404 화면 |

## 컴포넌트 계층 (목록 화면 기준)

```
WeatherHomeView
└─ WeatherDashboard              (검색어/목록/로딩/에러 상태 보유, fetchWeatherList 호출)
   ├─ BaseDashboardCard          (검색 영역 래핑)
   │  └─ SearchBar                → @update-query 로 검색어를 부모에 전달
   └─ BaseDashboardCard          (목록 영역 래핑)
      └─ WeatherCard (v-for)      → @select-card, @click-detail 을 부모에 전달
         └─ useTemperature()      composable로 단위(℃/℉) 변환
```

상태는 항상 위(`WeatherDashboard`)에서 아래로 props로 내려가고, 자식의 이벤트(`emit`)가 다시 위로 올라가는 단방향 흐름입니다. 온도 단위처럼 형제 컴포넌트(`UnitToggler` ↔ `WeatherCard`)가 공유해야 하는 값만 Pinia 스토어를 거칩니다.

## 상태 관리 지점 정리

- **로컬 상태 (`ref`)**: 검색어, 로딩 여부, 에러 메시지, 조회된 날씨 목록 — 해당 화면에서만 쓰이므로 컴포넌트 안에 둠 (`WeatherDashboard.vue`, `WeatherDetailView.vue`)
- **URL 상태**: 검색어는 `router.replace`로 `?search=` 쿼리스트링에도 반영되어, 새로고침/뒤로가기 후에도 유지됨
- **전역 상태 (Pinia)**: 온도 단위(`unit`)만 전역으로 관리 — 헤더의 `UnitToggler`와 각 `WeatherCard`가 서로 다른 트리에 있어도 같은 값을 공유해야 하기 때문
- **provide/inject**: `App.vue`가 내려주는 `appTitle`처럼, 자주 바뀌지 않는 정적 값을 props 없이 하위에 전달할 때 사용 (`WeatherAboutView.vue`)

## API 연동 (`services/weatherApi.js`)

- `fetchWeatherList()`: 등록된 3개 도시(서울/수원/부산)의 날씨를 병렬(`Promise.all`)로 조회 → 목록 화면에서 사용
- `fetchWeatherDetail(cityId)`: 특정 도시 하나만 조회 → 상세 화면에서 사용
- 두 함수 모두 OpenWeatherMap 응답을 `{ id, name, temp, status, humidity, wind }` 형태로 정규화해서 반환하므로, 컴포넌트는 API 응답 구조를 몰라도 됨
- `VITE_OPENWEATHER_API_KEY`가 없으면 요청 전에 에러를 던지도록 방어 코드가 있음 (`assertApiKey`)

## 시작하기

```sh
npm install                        # 의존성 설치
cp .env.example .env.local         # VITE_OPENWEATHER_API_KEY 값 입력 필요
npm run dev                        # 개발 서버 실행
```

빌드/린트가 필요하면 `npm run build`, `npm run lint`를 사용하세요.
