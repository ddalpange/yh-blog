---
title: 간단한 메모장 만들기 2 - 프로토타이핑
date: 2017-10-03 22:48:55
thumbnail: /images/memo/memoBanner.png
banner: /images/memo/memoBanner.png
categories: [SimpleMemo]
tags: [Javascript, Angular, Ionic, SimpleMemo]
toc: true

---

포스트 2에서는 아이오닉 컴포넌트를 활용하여 프로토 타이핑을 진행하도록 하겠습니다.

<!-- more -->

### 시작하기 전에.


먼저 아이오닉 프로젝트가 없으신 분들은 이전 포스트를 참고하여 만들어주세요.

[간단한 메모장 만들기 1](https://ddalpange.github.io/2017/07/09/%EA%B0%84%EB%8B%A8%ED%95%9C-%EB%A9%94%EB%AA%A8%EC%9E%A5-%EB%A7%8C%EB%93%A4%EA%B8%B0-1/)

### 페이지 생성을 시작하겠습니다.

루트프로젝트에서 쉘 또는 명령 프롬프트를 실행한 후 아래의 명령어를 입력하여주세요.

```bash
$ ionic generate page sign-up
$ ionic generate page sign-in
$ ionic generate page memo-list
$ ionic generate page memo-detail
$ ionic generate page memo-create
```

이 정도면 충분할것 같네요. 부족한 페이지가 있으면 작업중에 생성하도록 하겠습니다.

루트 모듈(app.module.ts)에 위 5개의 페이지를 임포트하겠습니다.

```typescript src/app/app.module.ts
// 주의 !! Ionic 2버전입니다.
import { BrowserModule } from '@angular/platform-browser';
import { HttpModule } from '@angular/http';

import { ErrorHandler, NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { IonicApp, IonicErrorHandler, IonicModule } from 'ionic-angular';
import { SplashScreen } from '@ionic-native/splash-screen';
import { StatusBar } from '@ionic-native/status-bar';

import { MyApp } from './app.component';
import { SignInPage } from './../pages/sign-in/sign-in';
import { MemoDetailPage } from './../pages/memo-detail/memo-detail';
import { MemoListPage } from './../pages/memo-list/memo-list';
import { SignUpPage } from '../pages/sign-up/sign-up';
import { MemoCreatePage } from '../pages/memo-create/memo-create';

function getPages() {
  return [
    MyApp,
    SignInPage,
    SignUpPage,
    MemoListPage,
    MemoDetailPage,
    MemoCreatePage
  ]
}

@NgModule({
  declarations: getPages(),
  imports: [
    BrowserModule,
    HttpModule,
    FormsModule,
    IonicModule.forRoot(MyApp),
  ],
  bootstrap: [IonicApp],
  entryComponents: getPages(),
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler}
  ]
})
export class AppModule {}

```

모든 페이지를 리턴해주는 _getPages()_ 라는 함수를 작성하고, declations, entryComponents 두 속성에 값을 넣어줍니다.

**잠깐! 왜 페이지 별로 모듈을 따로 빼지 않는것이죠 ?**
> AngularJS의 기본 단위는 **모듈**입니다. 그럼으로 상위 모듈이 모든 페이지(컴포넌트)를 바로 임포트하여 선언하는건 옳은 방식이 아니죠. 일단은 편의성을 위해 루트 모듈에 모든 컴포넌트를 선언해놓았지만, 나중에 폴더구조를 바꾸면서 모든 페이지(컴포넌트)는 각각의 모듈을 가지도록 바꿀 것이니 걱정하지 마세요.

<!-- more -->

**정정합니다.**
> Ionic3 업데이트로 각각의 페이지는 모듈을 가지도록 바뀌어서 기본적으로 게으른 로딩(lazy loading)을 지원하도록 바뀌었습니다. cli를 통해 페이지를 만들 때 모듈 파일이 같이 있다면 앱 모듈에서 바로 페이지를 declation 할 수 없습니다. 각각의 모듈을 임포트해야하죠.

```typescript src/app/app.module.ts
import { SignUpPageModule } from './../pages/sign-up/sign-up.module';
import { SignInPageModule } from './../pages/sign-in/sign-in.module';
import { MemoListPageModule } from './../pages/memo-list/memo-list.module';
import { MemoDetailPageModule } from './../pages/memo-detail/memo-detail.module';
import { MemoCreatePageModule } from './../pages/memo-create/memo-create.module';
import { BrowserModule } from '@angular/platform-browser';
import { HttpModule } from '@angular/http';
import { ErrorHandler, NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { IonicApp, IonicErrorHandler, IonicModule } from 'ionic-angular';
import { SplashScreen } from '@ionic-native/splash-screen';
import { StatusBar } from '@ionic-native/status-bar';
import { MyApp } from './app.component';

@NgModule({
  declarations: [MyApp],
  imports: [
    BrowserModule,
    HttpModule,
    FormsModule,
    IonicModule.forRoot(MyApp),
    MemoCreatePageModule,
    MemoDetailPageModule,
    MemoListPageModule,
    SignInPageModule,
    SignUpPageModule
  ],
  bootstrap: [IonicApp],
  entryComponents: [MyApp],
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler},
  ]
})
export class AppModule {}
```

**잊지마세요!**
루트모듈만 바꾸면 되는게 아닙니다. 루트 컴포넌트(app.component.ts)에 가서 루트 페이지(시작 페이지)도 같이 바꿔줘야합니다.

```typescript src/app/app.component.ts
import { SignInPage } from './../pages/sign-in/sign-in';
import { Component } from '@angular/core';
import { Platform } from 'ionic-angular';
import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';

@Component({
  templateUrl: 'app.html'
})
export class MyApp {
  rootPage:any = SiginInPage;
  constructor(platform: Platform, statusBar: StatusBar, splashScreen: SplashScreen) {
    platform.ready().then(() => {
      // Okay, so the platform is ready and our plugins are available.
      // Here you can do any higher level native things you might need.
      statusBar.styleDefault();
      splashScreen.hide();
    });
  }
}
```

여기까지 하셨으면 반은 성공한겁니다 !

### 퍼블리싱을 시작하겠습니다.

#### 로그인페이지 (sign-in)

```typescript src/pages/sign-in/sign-in.ts
import { Component, OnInit } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';

import { SignUpPage } from './../sign-up/sign-up';
import { MemoListPage } from './../memo-list/memo-list';

@IonicPage()
@Component({
  selector: 'page-sign-in',
  templateUrl: 'sign-in.html',
})
export class SignInPage {

  emailAddress: string;
  password: string;

  constructor(
    public navCtrl: NavController, 
    public navParams: NavParams,
  ) {}

  ionViewDidLoad() {
    console.log('ionViewDidLoad SignInPage');
  }

  ngOnInit() {
  }

  onClickDDalpange() {
    window.location.href = "https://ddalpange.github.io";
  }

  onChangeEmailAddress(event: any) {
    this.emailAddress = event.target.value;
  }

  onChangePassword(event: any) {
    this.password = event.target.value;
  }
  
  onClickEmailLogIn() {
    this.navCtrl.setRoot(MemoListPage);    
  }

  onClickFacebookLogin() {

  }

  onClickSignUp() {
    this.navCtrl.push(SignUpPage);
  }
}
```

```html src/pages/sign-in/sign-in.html
<ion-content padding>
  <div class="image-wrap">
    <img src="/images/gyul.png" 
      (click)="onClickDDalpange()"/>
    <p>달팽이의 메모 앱</p>
    <ion-list>
      <ion-item>
        <ion-input type="email" placeholder="이메일"></ion-input>
      </ion-item>
      <ion-item>
        <ion-input type="password" placeholder="비밀번호"></ion-input>
      </ion-item>
    </ion-list>
  </div>
  <button ion-button block color="light" (click)="onClickEmailLogIn()">이메일 로그인</button>
  <button ion-button block color="primary" (click)="onClickFacebookLogin()">페이스북 로그인</button>
  <p class="sign-up-text" (click)="onClickSignUp()">계정이 없으신가요?</p>
</ion-content>
```

```scss src/pages/sign-in/sign-in.scss
page-sign-in {
  .image-wrap {
    width:100%;
    text-align: center;
    >img {
      width: 80%;
      margin: 0 auto;
    }
  }
  .sign-up-text {
    text-decoration: underline;
    text-align: center;
  }
}

```


#### 회원가입 페이지 (sign-up)

```typescript src/pages/sign-up/sign-up.ts
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';

/**
 * Generated class for the SignUpPage page.
 *
 * See http://ionicframework.com/docs/components/#navigation for more info
 * on Ionic pages and navigation.
 */
@IonicPage()
@Component({
  selector: 'page-sign-up',
  templateUrl: 'sign-up.html',
})
export class SignUpPage {

  constructor(public navCtrl: NavController, public navParams: NavParams) {
  }

  onClickDDalpange() {
    window.location.href = "https://ddalpange.github.io";
  }

  onClickSignUp() {
    this.navCtrl.pop();    
  }

  onClickNavBack() {
    this.navCtrl.pop();
  }

}

```

```html src/pages/sign-up/sign-up.html
<ion-content padding>
   <div class="image-wrap">
    <img src="/images/gyul.png" 
      (click)="onClickDDalpange()"/>
    <p>달팽이의 메모 앱</p>
    <ion-list>
      <ion-item>
        <ion-input type="email" placeholder="이메일"></ion-input>
      </ion-item>
      <ion-item>
        <ion-input type="password" placeholder="비밀번호"></ion-input>
      </ion-item>
    </ion-list>
  </div>
  <button ion-button block color="light" (click)="onClickSignUp()">가입하기</button>
  <button ion-button block color="danger" (click)="onClickNavBack()">뒤로가기</button>
</ion-content>

```

```scss src/pages/sign-up/sign-up.scss
page-sign-up {
  .image-wrap {
    width:100%;
    text-align: center;
    >img {
      width: 80%;
      margin: 0 auto;
    }
  }
}
```


#### 메모리스트 페이지 (memo-list)

```typescript src/pages/memo-list/memo-list.ts
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';

import { MemoCreatePage } from './../memo-create/memo-create';
import { MemoDetailPage } from './../memo-detail/memo-detail';

@IonicPage()
@Component({
  selector: 'page-memo-list',
  templateUrl: 'memo-list.html',
})
export class MemoListPage {

  constructor(public navCtrl: NavController, public navParams: NavParams) {
  }

  ionViewDidLoad() {
    console.log('ionViewDidLoad MemoListPage');
  }

   onClickViewMemoDetail() {
    this.navCtrl.push(MemoDetailPage);
  }

  onClickCreateMemo() {
    this.navCtrl.push(MemoCreatePage);
  }
}

```

```html src/pages/memo-list/memo-list.html
<ion-header>
  <ion-navbar color="primary">
    <ion-title>메모들</ion-title>
  </ion-navbar>
</ion-header>

<ion-content>
  <ion-card (click)="onClickViewMemoDetail()">
    <ion-card-header>
      제목1
    </ion-card-header>
    <ion-card-content>
      내용내용내용내용내용내용내용내용내용내용내용내용
    </ion-card-content>
  </ion-card>
  <ion-card (click)="onClickViewMemoDetail()">
    <ion-card-header>
      제목2
    </ion-card-header>
    <ion-card-content>
      내용내용내용내용내용내용내용내용내용내용내용내용
    </ion-card-content>
  </ion-card>
  <ion-card (click)="onClickViewMemoDetail()">
    <ion-card-header>
      제목3
    </ion-card-header>
    <ion-card-content>
      내용내용내용내용내용내용내용내용내용내용내용내용
    </ion-card-content>
  </ion-card>
  <ion-fab right bottom>
    <button ion-fab icon-only><ion-icon name="add" big></ion-icon></button>
  </ion-fab>
</ion-content>
```

```scss src/pages/memo-list/memo-list.scss
page-memo-list {
  .content {
    background: #f4f4f4;
  }
  .card-header {
    font-weight: 600;
  }
  .card-content {
    overflow-y: hidden;
    text-overflow: ellipsis;
    white-space:nowrap;
  }
}
```

#### 메모 상세 페이지 (memo-detail)


```typescript src/pages/memo-detail/memo-detail.ts
import { Component, OnInit } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';
import { MemoCreatePage } from './../memo-create/memo-create';

@IonicPage()
@Component({
  selector: 'page-memo-detail',
  templateUrl: 'memo-detail.html',
})
export class MemoDetailPage {

  constructor(public navCtrl: NavController, public navParams: NavParams) {
  }

  ngOnInit() {
  }

  onOpenEditMemo() {
    this.navCtrl.push(MemoCreatePage);
  }

  onDeleteMemo() {
    this.navCtrl.pop();
  }
}


```

```html src/pages/memo-detail/memo-detail.html
<ion-header>
  <ion-navbar color="danger">
    <ion-title>메모 상세</ion-title>
    <ion-buttons end>
      <button ion-button icon-only (click)="onDeleteMemo()">
        <ion-icon name="trash"></ion-icon>
      </button>
      <button ion-button icon-only (click)="onOpenEditMemo()">
        <ion-icon name="hammer"></ion-icon>
      </button>
    </ion-buttons>
  </ion-navbar>
</ion-header>

<ion-content>
  <ion-card>
    <ion-card-header>
      <h1>메모 제목</h1>
    </ion-card-header>
    <ion-card-content>
      <p>메모 내용</p>
      <div class="etc">
        <p>
          <span>저자</span>
          메모 저자
        </p>
        <p>
          <span>발행일</span>
          메모 발행일
        </p>
        <p>
          <span>최근 수정일</span>
          메모 최근 수정일
        </p>
      </div>
    </ion-card-content>
  </ion-card>
</ion-content>

```

```scss src/pages/memo-detail/memo-detail.scss
page-memo-detail {
  .content {
    background: #f4f4f4;
  }
  .card-header {
    font-weight: 600;
  }
  .etc {
    text-align: right;
    font-size: 0.8em;
    padding-top: 50px;
    color: #dedede;
  }
}
```


#### 메모 만들기 페이지 (memo-create)

```typescript src/pages/memo-create/memo-create.ts
import { Component, OnInit } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';

@IonicPage()
@Component({
  selector: 'page-memo-create',
  templateUrl: 'memo-create.html',
})
export class MemoCreatePage {

  title: string;
  contents: string;

  constructor(public navCtrl: NavController, public navParams: NavParams) {
  }

  ngOnInit() {
  }

  onChangeTitle(event: KeyboardEvent) {
    this.title = event.target['value'];
  }

  onChangeContents(event: KeyboardEvent) {
    this.contents = event.target['value'];
  }
  
  onSaveMemo() {
    this.navCtrl.pop();
  }
}

```

```html src/pages/memo-create/memo-create.html
<ion-header>
  <ion-navbar color="secondary">
    <ion-title>메모 만들기</ion-title>
  </ion-navbar>
</ion-header>
<ion-content padding>
  <ion-list>
    <ion-item>
      <ion-input placeholder="제목을 입력해주세요." [value]="title" (change)="onChangeTitle($event)"></ion-input>
    </ion-item>
    <ion-item>
      <ion-textarea rows="50" placeholder="내용을 입력해 주시겠어요 ?" [value]="contents" (change)="onChangeContents($event)"></ion-textarea>
    </ion-item>
  </ion-list>
  <ion-fab right bottom>
    <button ion-fab icon-only color="secondary" (click)="onSaveMemo()"><ion-icon name="checkmark"></ion-icon></button>
  </ion-fab>
</ion-content>

```

```scss src/pages/memo-create/memo-create.scss
page-memo-create {
}
```


![로그인 페이지](/images/memo/sign-in-page.png)
![회원가입 페이지](/images/memo/sign-up-page.png)
![메모 리스트 페이지](/images/memo/memo-list-page.png)
![메모 상세 페이지](/images/memo/memo-detail-page.png)
![메모 만들기 페이지](/images/memo/memo-create-page.png)

---

참고 링크
- [해당 포스트에 작성된 모든 코드는 여기에 있습니다!](https://github.com/ddalpange/simple-memo)
- [해당 프로젝트는 여기서 볼 수 있습니다 !!](https://memo-28314.firebaseapp.com)

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAwMzg1OTM4Nyw2MzYyODk5XX0=
-->