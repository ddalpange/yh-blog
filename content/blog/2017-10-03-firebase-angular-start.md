---
title: Angular + Firebase 퀵 스타트
date: 2017-10-03 22:45:44
thumbnail: http://www.techjini.com/wp-content/uploads/2017/04/A2.jpg
banner: http://www.techjini.com/wp-content/uploads/2017/04/A2.jpg
categories: [Firebase]
tags: [Javascript, Angular, Firebase]
toc: true
---

`Angular`에서 `Firebase`를 사용하기 위한 방법을 설명합니다.
<!-- more -->

#### 앵귤러.

##### 사전 환경 세팅

```makefile
npm install -g @angular/cli
```

>명령을 실행하기 전에, ***node -v***, ***npm -v*** 명령어로 노드는 6.9.x 버전인지, npm은 3.x.x 버전인지 확인하세요. 구버전은 에러를 일으킬 수 있습니다.

##### 프로젝트를 만드세요.

```makefile
ng new angular-firebase
```

>참을성을 가지세요. 대부분의 시간은 npm package들을 인스톨 하는데에 쓰입니다.

##### 프로젝트를 실행하세요.

```makefile
cd angular-firebase
ng serve --open
```

>***ng serve***는 서버를 올리기 위한 명령어입니다. 당신들의 파일을 주시하며, 변경이 있을때마다 앱을 다시 빌드합니다. ***--open (또는 -o)*** 옵션은 자동으로 당신의 브라우저로 켜주는 명령어입니다. (http://localhost:4200/)

##### 파일구조를 살펴봅시다.

###### Src Folder

*src* 폴더 안에 모든 Angular의 컴포넌트, 탬플릿, Css, 이미지 그리고 당신의 앱을 만들기 위해 필요한 것이 들어갑니다. 그 외 다른 바깥쪽 폴더들은 당신의 앱을 빌드하기 위해 필요한 것들이 들어있습니다.

```
src
    app
        app.component.css
        app.component.html
        app.component.spec.ts
        app.component.ts
        app.module.ts
    assets
        .gitkeep
    environments
        environment.prod.ts
        environment.ts
favicon.ico
index.html
main.ts
polyfills.ts
styles.css
test.ts
tsconfig.app.json
tsconfig.spec.json
```


File | Purpose
--- | ---
app/app.component.{ts,html,css,spec.ts} | Defines the AppComponent along with an HTML template, CSS stylesheet, and a unit test. It is the root component of what will become a tree of nested components as the application evolves.
app/app.module.ts | Defines AppModule, the root module that tells Angular how to assemble the application. Right now it declares only the AppComponent. Soon there will be more components to declare.
assets/* | A folder where you can put images and anything else to be copied wholesale when you build your application.
environments/* | This folder contains one file for each of your destination environments, each exporting simple configuration variables to use in your application. The files are replaced on-the-fly when you build your app. You might use a different API endpoint for development than you do for production or maybe different analytics tokens. You might even use some mock services. Either way, the CLI has you covered.
favicon.ico | Every site wants to look good on the bookmark bar. Get started with your very own Angular icon.
index.html | The main HTML page that is served when someone visits your site. Most of the time you'll never need to edit it. The CLI automatically adds all js and css files when building your app so you never need to add any &lt;script&gt; or &lt;link&gt; tags here manually.
main.ts | The main entry point for your app. Compiles the application with the JIT compiler and bootstraps the application's root module (AppModule) to run in the browser. You can also use the AOT compiler without changing any code by passing in --aot to ng build or ng serve.
polyfills.ts | Different browsers have different levels of support of the web standards. Polyfills help normalize those differences. You should be pretty safe with core-js and zone.js, but be sure to check out the Browser Support guide for more information.
styles.css | Your global styles go here. Most of the time you'll want to have local styles in your components for easier maintenance, but styles that affect all of your app need to be in a central place.
test.ts | This is the main entry point for your unit tests. It has some custom configuration that might be unfamiliar, but it's not something you'll need to edit
tsconfig.app.json, tsconfig.spec.json | TypeScript compiler configuration for the Angular app (tsconfig.app.json) and for the unit tests (tsconfig.spec.json).


###### The Root Folder

The src/ folder is just one of the items inside the project's root folder. Other files help you build, test, maintain, document, and deploy the app. These files go in the root folder next to src/.

```
my-app
    e2e
        app.e2e-spec.ts
        app.po.ts
        tsconfig.e2e.json
node_modules/...
src/...
.angular-cli.json
.editorconfig
.gitignore
karma.conf.js
package.json
protractor.conf.js
README.md
tsconfig.json
tslint.json
```


File|Purpose
--- | ---
e2e/|Inside e2e/ live the End-to-End tests. They shouldn't be inside src/ because e2e tests are really a separate app that just so happens to test your main app. That's also why they have their own tsconfig.e2e.json.
node_modules/|Node.js creates this folder and puts all third party modules listed in package.json inside of it.|
.angular-cli.json|Configuration for Angular CLI. In this file you can set several defaults and also configure what files are included when your project is build. Check out the official documentation if you want to know more.
.edtorconfig|Simple configuration for your editor to make sure everyone that uses your project has the same basic configuration. Most editors support an .editorconfig file. See http://editorconfig.org for more information.
.gitignore|Git configuration to make sure autogenerated files are not commited to source control.
karma.conf.js|Unit test configuration for the Karma test runner, used when running ng test.
package.json|npm configuration listing the third party packages your project uses. You can also add your own custom scripts here.
protractor.config.js | End-to-end test configuration for Protractor, used when running ng e2e.
README.md |	Basic documentation for your project, pre-filled with CLI command information. Make sure to enhance it with project documentation so that anyone checking out the repo can build your app!
tsconfig.json | TypeScript compiler configuration for your IDE to pick up and give you helpful tooling.
tslint.json | Linting configuration for TSLint together with Codelyzer, used when running ng lint. Linting helps keep your code style consistent.

#### 파이어베이스

##### 파이어베이스 깔아주세요

```
npm install angularfire2 firebase --save
```
https://github.com/angular/angularfire2

##### 파이어베이스 프로젝트를 만들어주세요.

https://console.firebase.google.com/project/angular-test-146ca/database/data/items?hl=ko

웹앱으로 시작하기 버튼 누르면 나오는 설정코드 복사해주세요.

##### 파이어베이스 세팅해주세요.

enviroment.prod.ts

enviroment.ts

```javascript
/* before */
export const environment = {
  production: false,
};
/* after */
export const environment = {
  production: false,
  firebase: {
    apiKey: 'blbabasdfsdf',
    authDomain: 'sdfsdfsdf.firebaseapp.com',
    databaseURL: 'https://bababa.firebaseio.com',
    projectId: 'sdfsdf',
    storageBucket: 'sdfsdf',
    messagingSenderId: '123324345345'
  }
};
```


##### 모듈 추가해주시구요

app.module.ts
```typescript
import { environment } from './../environments/environment';
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { FirebaseTestComponent } from './firebase-test/firebase-test.component';
import { AngularFireModule } from 'angularfire2';
import { AngularFireDatabaseModule } from 'angularfire2/database';
import { AngularFireAuthModule } from 'angularfire2/auth';


@NgModule({
  declarations: [
    AppComponent,
    FirebaseTestComponent
  ],
  imports: [
    BrowserModule,
    AngularFireModule.initializeApp(environment.firebase),
    AngularFireDatabaseModule,
    AngularFireAuthModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

##### 테스트하기전에 먼저

파이어베이스 프로젝트 -> 리얼타임데이터베이스 -> 규칙 가서

```javascript
{
  "rules": {
    ".read": "true",
    ".write": "true"
  }
}
```

이렇게 바꿔야합니다. 따로 인증을 받았지 않았기 때문에 에러납니다.

##### 테스트 해봅시다.

template

```html
<ul>
  <li class="text" *ngFor="let item of items | async">
    {{item.$value}}
  </li>
</ul>
```

```typescript
import { Component, OnInit } from '@angular/core';
import { AngularFireDatabase, FirebaseListObservable } from 'angularfire2/database';
@Component({
  selector: 'app-firebase-test',
  templateUrl: './firebase-test.component.html',
  styleUrls: ['./firebase-test.component.css']
})
export class FirebaseTestComponent implements OnInit {
  items: FirebaseListObservable<any[]>;

  constructor(private _db: AngularFireDatabase) { }
를
  ngOnInit() {
    this.items = this._db.list('/items');
    this.items.push("안녕하세요 ?");
  }
}
```

새로고침할때마다 안녕하세요 ? 가 추가되는 것을 볼 수 있습니다.

