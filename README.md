# Vue Weather Dashboard

## 프로젝트 개요

이번 Vue 실습에서는 OpenWeatherMap API를 이용하여 지역별 날씨를 확인하는 대시보드를 만들었다.

서울, 수원, 부산의 현재 날씨를 카드 형태로 보여 주고, 검색 기능과 상세 페이지를 추가하였다. 수업에서 배운 Vue Router, Pinia, Axios, props와 emit 등을 한 프로젝트 안에서 사용해 보는 것을 목표로 하였다.

## 구현한 기능

- 서울, 수원, 부산의 현재 날씨 조회
- 도시 이름 검색
- 도시별 상세 날씨 조회
- 섭씨(℃)와 화씨(℉) 단위 변경
- 검색어를 URL query에 반영
- 잘못된 주소에 대한 404 페이지 표시
- 데이터 로딩 및 오류 메시지 표시

홈 화면의 날씨 카드에서는 도시 이름, 현재 기온, 날씨 상태를 확인할 수 있다. 상세보기 버튼을 누르면 해당 도시의 습도와 풍속을 추가로 확인할 수 있다.

## 사용한 기술

| 기술 | 사용 내용 |
| --- | --- |
| Vue 3 | Composition API와 `<script setup>` 방식으로 컴포넌트 작성 |
| Vue Router | 홈, 소개, 상세, 404 페이지 구성 |
| Pinia | 온도 단위 상태 공유 |
| Axios | OpenWeatherMap API 요청 |
| Element Plus | 입력창, 버튼, 스위치 사용 |
| Vite | 프로젝트 실행 및 빌드 |

## 프로젝트 실행 방법

### 1. 패키지 설치

```sh
npm install
```

### 2. API Key 설정

OpenWeatherMap에서 API Key를 발급받은 후 `.env.example` 파일을 복사한다.

```sh
cp .env.example .env.local
```

`.env.local` 파일에 발급받은 키를 입력한다.

```dotenv
VITE_OPENWEATHER_API_KEY=발급받은_API_KEY
```

### 3. 개발 서버 실행

```sh
npm run dev
```

## 프로젝트 구조

```text
src/
├── assets/
│   └── main.css
├── components/
│   └── weather/
│       ├── BaseDashboardCard.vue
│       ├── SearchBar.vue
│       ├── UnitToggler.vue
│       ├── WeatherCard.vue
│       └── WeatherDashboard.vue
├── composables/
│   └── useTemperature.js
├── router/
│   └── index.js
├── services/
│   └── weatherApi.js
├── stores/
│   └── configStore.js
├── views/
│   ├── WeatherHomeView.vue
│   ├── WeatherDetailView.vue
│   ├── WeatherAboutView.vue
│   └── NotFoundView.vue
├── App.vue
└── main.js
```

### `main.js`

Vue 앱이 시작되는 파일이다. Pinia, Vue Router, Element Plus를 앱에 등록한 뒤 `App.vue`를 화면에 연결한다.

### `App.vue`

모든 페이지에서 공통으로 보이는 헤더를 작성하였다. 홈과 소개 페이지로 이동하는 메뉴, 온도 단위 변경 스위치, 현재 페이지를 보여 주는 `RouterView`가 들어 있다.

### `views`

URL에 따라 화면에 표시되는 페이지 컴포넌트를 모아 둔 폴더이다.

- `WeatherHomeView.vue`: 날씨 대시보드를 보여 주는 홈 화면
- `WeatherDetailView.vue`: 선택한 도시의 기온, 습도, 풍속을 보여 주는 상세 화면
- `WeatherAboutView.vue`: 프로젝트에 대한 간단한 소개 화면
- `NotFoundView.vue`: 존재하지 않는 주소로 이동했을 때 보여 주는 화면

### `components/weather`

날씨 화면을 구성하는 컴포넌트를 모아 둔 폴더이다.

- `WeatherDashboard.vue`: 날씨 목록, 검색어, 로딩 상태와 오류 상태를 관리
- `WeatherCard.vue`: 도시 한 곳의 날씨를 카드로 출력
- `SearchBar.vue`: 도시 이름을 입력하는 검색창
- `UnitToggler.vue`: 섭씨와 화씨를 변경하는 스위치
- `BaseDashboardCard.vue`: 검색 영역과 날씨 목록에서 공통으로 사용한 카드 틀

### `services/weatherApi.js`

OpenWeatherMap API를 호출하는 부분이다. 서울, 수원, 부산의 위도와 경도를 저장해 두고 현재 날씨를 요청한다.

목록 화면에서는 `Promise.all`을 사용하여 세 도시의 날씨를 한 번에 불러왔다. API 응답에서 화면에 필요한 값만 골라 다음 형태로 정리하였다.

```js
{
  id: 'city_01',
  name: '서울',
  temp: 23.4,
  status: '맑음',
  humidity: 58,
  wind: 2.1,
}
```

### `stores/configStore.js`

현재 온도 단위를 Pinia로 관리한다. 기본값은 섭씨이며 헤더의 스위치를 누르면 화씨로 변경된다. 이 값은 목록 화면과 상세 화면에서 같이 사용한다.

### `composables/useTemperature.js`

API에서 받은 섭씨 온도를 현재 단위에 맞게 계산한다. Pinia에 저장된 단위가 화씨이면 아래 공식을 사용하고, 화면에 표시할 때는 반올림하였다.

```text
화씨 = (섭씨 × 9 / 5) + 32
```

### `router/index.js`

프로젝트에서 사용하는 경로는 다음과 같다.

| 경로 | 화면 |
| --- | --- |
| `/` | 날씨 목록과 검색 화면 |
| `/weather/:cityId` | 도시 상세 날씨 화면 |
| `/about` | 프로젝트 소개 화면 |
| `/not-found` | 404 화면 |

상세 페이지에서 사용하는 도시 ID는 서울 `city_01`, 수원 `city_02`, 부산 `city_03`이다. 이 값이 아닌 주소로 접근하면 Router guard를 통해 404 페이지로 이동한다.

## 주요 동작 과정

### 날씨 목록 불러오기

1. `WeatherDashboard`가 화면에 나타나면 `fetchWeatherList()`를 실행한다.
2. `weatherApi.js`에서 세 도시의 날씨를 요청한다.
3. 응답받은 데이터를 `weatherList`에 저장한다.
4. `v-for`를 사용하여 도시 수만큼 `WeatherCard`를 출력한다.

### 도시 검색

1. `SearchBar`에 도시 이름을 입력한다.
2. 입력값을 emit으로 `WeatherDashboard`에 전달한다.
3. `computed`에서 도시 이름과 검색어를 비교한다.
4. 검색어가 포함된 카드만 화면에 남긴다.
5. `watch`를 사용하여 검색어를 URL query에도 반영한다.

### 상세 페이지 이동

1. `WeatherCard`의 상세보기 버튼을 누른다.
2. 카드에서 도시 ID를 emit으로 전달한다.
3. `WeatherDashboard`가 Router를 이용하여 상세 페이지로 이동한다.
4. `WeatherDetailView`가 도시 ID에 맞는 날씨를 다시 요청하여 표시한다.

### 온도 단위 변경

1. `UnitToggler`에서 온도 단위 스위치를 누른다.
2. Pinia의 `unit` 값이 변경된다.
3. `useTemperature`에서 표시할 온도를 다시 계산한다.
4. 홈과 상세 화면의 온도가 함께 변경된다.

## 실습 내용 정리

이번 프로젝트에서는 컴포넌트를 기능별로 나누고 서로 데이터를 주고받는 과정을 연습하였다. 검색창과 날씨 카드는 부모가 값을 내려 주고 자식이 이벤트를 올려 주는 props와 emit 방식으로 연결하였다.

날씨 목록이나 로딩 상태처럼 한 화면에서만 사용하는 값은 각 컴포넌트의 `ref`로 관리하였다. 반면 온도 단위는 여러 화면에서 사용하므로 Pinia에 저장하였다. 이를 통해 모든 상태를 전역으로 관리하는 것이 아니라 사용 범위에 따라 위치를 나누어야 한다는 점을 확인하였다.

또한 API 호출 코드를 컴포넌트에서 분리하고, 반복해서 사용하는 온도 계산을 composable로 작성하였다. Vue Router를 이용한 페이지 이동과 404 처리까지 구현하면서 Vue 프로젝트의 전체적인 구조를 정리할 수 있었다.

현재 프로젝트는 정해진 세 도시의 현재 날씨만 조회한다. 이번 실습에서는 기능을 많이 추가하기보다 수업에서 배운 Vue 기능을 하나의 프로젝트 안에서 연결하는 데 중점을 두었다.
