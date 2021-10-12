---
title: 간단한 메모장 만들기 4 - Auth
date: 2017-10-03 22:49:00
thumbnail: /images/memo/memoBanner.png
banner: /images/memo/memoBanner.png
categories: [SimpleMemo]
tags: [Javascript, Angular, Ionic, SimpleMemo]
toc: true
---

이번시간에는 파이어베이스를 이용한 로그인 연동, 회원가입, 로그아웃을 해보겠습니다.

<!-- more -->

https://console.firebase.google.com

![프로젝트 추가 버튼을 클릭하세요](/images/firebaseConsole.png)

해당 프로젝트로 들어가시면 정 가운데 세번째 버튼에 **웹 앱으로 배포하기**라는 버튼이 있을겁니다. 클릭해주세요.


```html
<script src="https://www.gstatic.com/firebasejs/4.1.3/firebase.js"></script>
<script>
  // Initialize Firebase
  var config = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_NAME-YOUR_PROJCET_NUMBER.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT_NAME-YOUR_PROJCET_NUMBER.firebaseio.com",
    projectId: "YOUR_PROJECT_NAME-YOUR_PROJCET_NUMBER",
    storageBucket: "YOUR_PROJECT_NAME-YOUR_PROJCET_NUMBER.appspot.com",
    messagingSenderId: "12345678"
  };
  firebase.initializeApp(config);
</script>
```

config 변수 부분을 복사해주세요 딴건 필요없답니다.


### 아이오닉 프로젝트에 추가하기

Ionic에서 지원하는 파이어베이스 패키지들을 인스톨하여주세요.

```bash
npm install --save angularfire2 firebase
```

그 다음에 루트 모듈 ( 보통은 app.module.ts )에 파이어베이스 모듈을 추가할게요.

변경 후
```typescript
...

import { AngularFireModule } from 'angularfire2'; // 파이어베이스 루트 모듈입니다.
import { AngularFireDatabaseModule } from 'angularfire2/database'; // 파이어베이스 데이터베이스 모듈입니다.
import { AngularFireAuthModule } from 'angularfire2/auth'; // 파이버베이스 인증 모듈입니다.

...

// 방금 복사했던 config를 가져오시면 됩니다.
export const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_NAME-YOUR_PROJCET_NUMBER.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT_NAME-YOUR_PROJCET_NUMBER.firebaseio.com",
  projectId: "YOUR_PROJECT_NAME-YOUR_PROJCET_NUMBER",
  storageBucket: "YOUR_PROJECT_NAME-YOUR_PROJCET_NUMBER.appspot.com",
  messagingSenderId: "12345678"
};


@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    HttpModule,
    TranslateModule.forRoot({
      loader: {
        provide: TranslateLoader,
        useFactory: HttpLoaderFactory,
        deps: [Http]
      }
    }),
    IonicModule.forRoot(AppComponent, {
      preloadModules: true
    }),
    IonicStorageModule.forRoot(),
    // 설정파일을 인자로 넘겨주어 앱을 초기화합니다.
    AngularFireModule.initializeApp(firebaseConfig),
    AngularFireDatabaseModule,
    AngularFireAuthModule,
    ...
  ],
  entryComponents: [AppComponent],
  bootstrap: [IonicApp],
  providers: []
})
export class AppModule { }

```

### 로그인 서비스 활성화시키기

파이어베이스에 들어가신 후

**Authentication** -> **로그인 방법**을 클릭하신 후

이메일 로그인 방식과 페이스북 로그인 방식을 활성화해주세요

![인증서비스](/images/memo/auth-open.png)


### 인증 서비스파일 만들기.

```bash
$ ionic g provider auth-manager
```

**src/pages/memo-manager/memo-manager.ts**

```typescript
import { Injectable } from '@angular/core';
import { Http } from '@angular/http';
import { AngularFireAuth } from 'angularfire2/auth';
import { Observable } from 'rxjs/Observable';
import * as firebase from 'firebase/app';

import 'rxjs/add/operator/map';

@Injectable()
export class AuthManagerProvider {

  userInfo: firebase.User = null;
  authState: Observable<firebase.User>

  constructor(
    public http: Http, 
    public afAuth: AngularFireAuth) {
    this.initAuth();
  }

  initAuth() {
    this.authState = this.afAuth.authState;
    this.authState.subscribe(
      (user: firebase.User) => {
        if(user) {
          this.setUserInfo(user);
        } else {
          this.setUserInfo(null);
        }
      }
    )
  }

  getAuthState(): Observable<firebase.User> {
    return this.authState;
  }

  getUserInfo() {
    return this.userInfo;
  }

  setUserInfo(userInfo: firebase.User) {
    this.userInfo = userInfo;
  }

  loginUser(email: string, password: string) {
    return this.afAuth.auth.signInWithEmailAndPassword(email, password);
  }

  signUpUser(email: string, password: string) {
    return this.afAuth.auth.createUserWithEmailAndPassword(email, password)
  }

  resetPassword(email: string) {
    return this.afAuth.auth.sendPasswordResetEmail(email);
  }

  logoutUser() {
    return this.afAuth.auth.signOut();
  }
}
```

### 로그인상태 여부에따라 최초 로딩페이지 바꿔주기

**src/app/app/component.ts**
```typescript
import { Component } from '@angular/core';
import { Platform } from 'ionic-angular';
import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';
import { AuthManagerProvider } from './../providers/auth-manager/auth-manager';

import { SignInPage } from './../pages/sign-in/sign-in';
import { MemoListPage } from './../pages/memo-list/memo-list';

@Component({
  templateUrl: 'app.html'
})

export class MyApp {
  rootPage:any;

  constructor(
    platform: Platform, 
    statusBar: StatusBar, 
    splashScreen: SplashScreen,
    authManager: AuthManagerProvider,
  ) {
    platform.ready().then(() => {
      // Okay, so the platform is ready and our plugins are available.
      // Here you can do any higher level native things you might need.
      statusBar.styleDefault();
      splashScreen.hide();
    });
    authManager.getAuthState().subscribe((user => {
      if(user)
        this.rootPage = MemoListPage;
      else 
        this.rootPage = SignInPage;
    }));
  }  
}
```

이제 **로그인**, **회원가입**, **로그아웃만 구현하면 됩니다.**

### 회원가입 페이지

**src/pages/sign-up/sign-up.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { IonicPageModule } from 'ionic-angular';
import { SignUpPage } from './sign-up';
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [
    SignUpPage,
  ],
  imports: [
    IonicPageModule.forChild(SignUpPage),
    FormsModule
  ],
  exports: [
    SignUpPage
  ]
})
export class SignUpPageModule {}

```

**src/pages/sign-up/sign-up.ts**

```typescript
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';
import { AuthManagerProvider } from '../../providers/auth-manager/auth-manager';
import { LoadingController, AlertController } from 'ionic-angular';

@IonicPage()
@Component({
  selector: 'page-sign-up',
  templateUrl: 'sign-up.html',
})
export class SignUpPage {

  emailAddress: string = '';
  password: string = '';

  constructor(
    public navCtrl: NavController, 
    public navParams: NavParams,
    public authManager: AuthManagerProvider,
    public loadingCtrl: LoadingController,
    public alertCtrl: AlertController,
  ) {
  }
  
  onClickDDalpange() {
    window.location.href = "https://ddalpange.github.io";
  }
  
  onClickSignUp() {
    let loader = this.loadingCtrl.create({
      content: '회원가입 중입니다 ..'
    });
    loader.present();
 
    this.authManager.signUpUser(this.emailAddress, this.password)
    .then(user => {
      console.log('성공!', user);
      loader.dismiss();
      const alert = this.getSuccessAlert();
      alert.present();
    })
    .catch(err => { 
      loader.dismiss();
      const alert = this.getFailAlert(err.message);
      alert.present();
    });
  }

  getSuccessAlert() {
    return this.alertCtrl.create({
      title: 'success!!',
      subTitle: '회원가입에 성공하였습니다.',
      buttons: [{
        text: '확인',
      }]
    });
  }

  getFailAlert(message: string) {
    return this.alertCtrl.create({
      title: 'failed..',
      subTitle: message,
      buttons: [{
        text: '확인'
      }]
    });
  }
  
  onClickNavBack() {
    this.navCtrl.pop();
  }
}
```

**src/pages/sign-up/sign-up.html**

```html
<ion-content padding>
   <div class="image-wrap">
    <img src="/images/gyul.png" 
      (click)="onClickDDalpange()"/>
    <p>달팽이의 메모 앱</p>
    <ion-list>
      <ion-item>
        <ion-input type="email" placeholder="이메일" [(ngModel)]="emailAddress" (keyup.enter)="onClickSignUp()"></ion-input>
      </ion-item>
      <ion-item>
        <ion-input type="password" placeholder="비밀번호" [(ngModel)]="password" (keyup.enter)="onClickSignUp()"></ion-input>
      </ion-item>
    </ion-list>
  </div>
  <button ion-button block color="light" (click)="onClickSignUp()">가입하기</button>
  <button ion-button block color="danger" (click)="onClickNavBack()">뒤로가기</button>
</ion-content>
```

### 로그인 페이지

**src/pages/sign-in/sign-in.module.ts**

```typescript
import { FormsModule } from '@angular/forms';
import { NgModule } from '@angular/core';
import { IonicPageModule } from 'ionic-angular';
import { SignInPage } from './sign-in';

@NgModule({
  declarations: [
    SignInPage,
  ],
  imports: [
    IonicPageModule.forChild(SignInPage),
    FormsModule
    
  ],
  exports: [
    SignInPage
  ]
})
export class SignInPageModule {}

```

**src/pages/sign-in/sign-in.ts**

```typescript
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams, AlertController, LoadingController } from 'ionic-angular';

import { SignUpPage } from './../sign-up/sign-up';
import { MemoListPage } from './../memo-list/memo-list';
import { AuthManagerProvider } from '../../providers/auth-manager/auth-manager';

@IonicPage()
@Component({
  selector: 'page-sign-in',
  templateUrl: 'sign-in.html',
})

export class SignInPage {
  
  emailAddress: string = '';
  password: string = '';

  constructor(
    public navCtrl: NavController, 
    public navParams: NavParams,
    public alertCtrl: AlertController,
    public loadingCtrl: LoadingController,
    public authManager: AuthManagerProvider
  ) {}

  onClickDDalpange() {
    window.location.href = "https://ddalpange.github.io";
  }

  onClickEmailLogIn() {
    let loader = this.loadingCtrl.create({
      content: '로그인 중입니다 ..'
    });

    loader.present();

    this.authManager.loginUser(this.emailAddress, this.password)
      .then(user => {
      console.log('성공!', user);
      loader.dismiss();
      this.navCtrl.setRoot(MemoListPage);
    })
    .catch(err => { 
      console.error('실패!', err);
      loader.dismiss();
      const alert = this.getFailAlert(err.message);
      alert.present();
    });;
  }

  onClickFacebookLogin() {
    const alert = this.getFailAlert('미완성 기능입니다.');
    alert.present();
  }

  getFailAlert(message: string) {
    return this.alertCtrl.create({
      title: 'failed..',
      subTitle: message,
      buttons: [{
        text: '확인'
      }]
    });
  }

  onClickSignUp() {
    this.navCtrl.push(SignUpPage);
  }
}
```

**src/pages/sign-in/sign-in.html**

```html
<ion-content padding>
  <div class="image-wrap">
    <img src="/images/gyul.png" 
      (click)="onClickDDalpange()"/>
    <p>달팽이의 메모 앱</p>
    <ion-list>
      <ion-item>
        <ion-input type="email" placeholder="이메일" [(ngModel)]="emailAddress" (keyup.enter)="onClickEmailLogIn()"></ion-input>
      </ion-item>
      <ion-item>
        <ion-input type="password" placeholder="비밀번호" [(ngModel)]="password" (keyup.enter)="onClickEmailLogIn()"></ion-input>
      </ion-item>
    </ion-list>
  </div>
  <button ion-button block color="light" (click)="onClickEmailLogIn()">이메일 로그인</button>
  <button ion-button block color="primary" (click)="onClickFacebookLogin()">페이스북 로그인</button>
  <p class="sign-up-text" (click)="onClickSignUp()">계정이 없으신가요?</p>
</ion-content>
```

### 로그아웃 기능(리스트 페이지)

**src/pages/memo-list/memo-list.module.ts**

```typescript
import { FormsModule } from '@angular/forms';
import { NgModule } from '@angular/core';
import { IonicPageModule } from 'ionic-angular';
import { MemoListPage } from './memo-list';

@NgModule({
  declarations: [
    MemoListPage,
  ],
  imports: [
    IonicPageModule.forChild(MemoListPage),
    FormsModule
  ],
  exports: [
    MemoListPage
  ]
})
export class MemoListPageModule {}
```

**src/pages/memo-list/memo-list.ts**

```typescript
import { AuthManagerProvider } from './../../providers/auth-manager/auth-manager';
import { FirebaseListObservable } from 'angularfire2/database';
import { Memo } from './../../models/memo/memo.interface';
import { Component, OnInit } from '@angular/core';
import { IonicPage, NavController, NavParams, LoadingController, ActionSheetController } from 'ionic-angular';

import { MemoCreatePage } from './../memo-create/memo-create';
import { MemoDetailPage } from './../memo-detail/memo-detail';

import { MemoManagerProvider } from './../../providers/memo-manager/memo-manager';
@IonicPage()

@Component({
  selector: 'page-memo-list',
  templateUrl: 'memo-list.html',
})

export class MemoListPage {

  searchKeyword: string = '';
  memoList: Memo = [];

  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public loadingCtrl: LoadingController,
    public actionSheetCtrl: ActionSheetController,
    public authManager: AuthManagerProvider,
    public memoManager: MemoManagerProvider) {
  }
 
  ngOnInit() {
    this.memoList = this.memoManager.getMemoList();
  }

  filterMemo(memo: Memo): boolean {
    return memo.title.includes(this.searchKeyword) || memo.title.includes(this.searchKeyword);
  }

  onClickViewMemoDetail(memo: Memo) {
    this.navCtrl.push(MemoDetailPage, { memo: memo });
  }
  
  onClickCreateMemo() {
    this.navCtrl.push(MemoCreatePage);
  }

  onClickMoreOption() {
    const actionSheet = this.actionSheetCtrl.create({
      buttons: [
        {
          text: 'Logout',
          role: 'destructive',
          handler: () => {
            this.authManager.logoutUser();
          }
        }, {
          text: 'Cancel',
          role: 'cancel'
        }
      ]
    });
    actionSheet.present();
  }
}
```

**src/pages/memo-list/memo-list.html**
```html
<ion-header>
  <ion-navbar color="primary">
    <ion-searchbar [(ngModel)]="searchKeyword"></ion-searchbar>
    <ion-buttons end>
      <button ion-button icon-only (click)="onClickMoreOption()">
        <ion-icon name="more">
        </ion-icon>
      </button>
    </ion-buttons>
  </ion-navbar>
</ion-header>
<!-- 콘텐츠 내용... -->
```


로그인, 로그아웃, 회원가입 총 3가지 기능을 완성하였습니다.

다음 포스트 에서는 파이어베이스의 리얼타임 데이터베이스를 이용해 데이터를 관리해볼게요!


---

참고 링크
- [해당 포스트에 작성된 모든 코드는 여기에 있습니다!](https://github.com/ddalpange/simple-memo)
- [해당 프로젝트는 여기서 볼 수 있습니다 !!](https://memo-28314.firebaseapp.com)
