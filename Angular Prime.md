# Angular Prime

# 구동 흐름

1. `angular(-cli).json` 에 설정된 대로 빌드된 결과 `index` 항목으로 설정된 `src/index.html`, `main` 항목으로 설정된 `src/main.js`가 생성됨
1. 브라우저에 `src/index.html` 파일이 로딩되고, html 파일 내 `<script>`로 지정된 `src/main.js`가 로딩됨
1. `src/main.js`의 원래 내용은 `angular(-cli).json`에 설정된대로 `src/main.ts`임
1. `src/main.ts`의 전형적인 내용은 다음과 같으며 환경 설정 내용을 읽고 부트스트랩 모듈을 로딩

    ```typescript
    import { enableProdMode } from '@angular/core';
    import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

    import { AppModule } from './app/app.module';
    import { environment } from './environments/environment';

    if (environment.production) {
      enableProdMode();
    }

    platformBrowserDynamic().bootstrapModule(AppModule);
    ```

1. 부트스트랩 모듈인 `AppModule`은 `src/app/app.module.ts`에 있으며, 다음과 같이 `bootstrap` 항목으로 지정한 루트 컴포넌트를 로딩

    ```typescript
    @NgModule({
      declarations: [AppComponent],
      imports: BrowserModule,
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule {
    }
    ```

1. `src/app/app.component.ts`에 있는 `AppComponent`가 렌더링하는 내용이 `src/index.html`에 있는 루트 컴포넌트 디렉티브 `<app-root>`에 표시됨


# 웹 컴포넌트

재사용 가능한 HTML 커스텀 요소를 만드는 컴포넌트의 모음으로, 다음 네 가지 스펙을 기반으로 한다.

- Custom Elements
- Shadow DOM
- ES Modules
- HTML Template

앵귤라는 일반적으로 다음의 네 가지 파일이 하나의 컴포넌트를 구성한다.

- `컴포넌트이름.component.ts`: 모델 및 화면 로직
- `컴포넌트이름.html`: 템플릿
- `컴포넌트이름.module.ts`: 모듈 단위 export 정보
- `컴포넌트이름.scss`: 스타일

컴포넌트와 템플릿의 데이터 및 이벤트 교환은 다음과 같다.

> 프로퍼티 바인딩으로 `component.ts`에 있는 모델 데이터를 템플릿에 전달
>
> 이벤트 바인딩으로 `html` 템플릿의 이벤트를 `component.ts`에 전달


# 바인딩

컴포넌트와 템플릿은 바인딩을 통해 상호 작용

## 프로퍼티 바인딩

컴포넌트 -> 템플릿 단방향 바인딩

> <element [property]="expression">...\</element>

`{{expression}}` 형태로 사용하는 interpolation은 프로퍼티 바인딩의 편리한 표기방식

## 애트리뷰트 바인딩

컴포넌트 -> 템플릿 단방향 바인딩

><element [attr.attribute-name]='expression'>...\</element>

### 프로퍼티와 애트리뷰트의 차이

- 프로퍼티는 DOM 트리에 속한 노드 객체에 속하는 속성으로 동적으로 변한다.
- 애트리뷰트는 HTML 문서에 존재하며 불변이다.

예를 들어, `<input id="nickname" type="text" value="angular">`가 있을 때, 다음과 같이 `value`의 값을 바꾸고,

`document.getElementById("nickname").value = '앵귤라'`

`document.getElementById("nickname").value`를 출력해보면 `앵귤라`가 표시되지만,  
`document.getElementById("nickname").getAttribute('value')`를 출력해보면 원래 값인 `angular`가 표시된다.

## 이벤트 바인딩

템플릿 -> 컴포넌트 단방향 바인딩

><element (event)="statement">...\</element>  
><element (click)="aPublicMethodOfComponent('angular')">...\</element>  
><input type="text" [value]="alias" (input)="setAlias($event)">...\</element>  

1. 템플릿에서 이벤트가 발생하면 `statement`로 지정된 이벤트 핸들러가 호출되고,
1. 컴포넌트에 정의되어 있는 이벤트 핸들러가 컴포넌트의 프로퍼티 값을 변경하면,
1. 프로퍼티 바인딩에 의해 변경된 값이 템플릿에 반영된다.

## 클래스 바인딩

컴포넌트 -> 템플릿 단방향 바인딩

><element [class]="class-name">...\</element>  
><element [class.class-name]="booleanExpression">...\</element>

`booleanExpression`의 값이 참이면 `class-name`이 `class` 애트리뷰트의 값으로 추가된다. 이미 동일한 `class-name`이 있으면 따로 추가되지는 않으며, 이미 동일한 `class-name`이 있는데 `booleanExpression`이 거짓이면, 기존에 있는 `class-name`이 제거된다.

`class-name`에는 직접 클래스 이름이 올 수도 있고 이 때는 단항 클래스 바인딩이며, 여러 클래스 이름을 문자열로 가지고 있는 변수가 올 수도 있고 이를 다항 클래스 바인딩이라고 한다.

## 스타일 바인딩

컴포넌트 -> 템플릿 단방향 바인딩

><element [style.style-property]="expression">...\</element>  
><div [style.font-size.px]="'16'">




