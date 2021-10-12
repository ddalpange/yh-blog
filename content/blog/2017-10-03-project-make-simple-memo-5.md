---
title: 간단한 메모장 만들기 5 - CRUD
date: 2017-10-03 22:49:02
thumbnail: /images/memo/memoBanner.png
banner: /images/memo/memoBanner.png
categories: [SimpleMemo]
tags: [Javascript, Angular, Ionic, SimpleMemo]
toc: true

---

이번 시간에는 메모리스트를 파이어베이스의 데이터베이스를 사용하여 `CRUD`를 해볼게요. `html`이나 `ts`에서 기능 개선을 위해 변경한 코드가 일부 있으니, 감안하여 봐주세요.

<!-- more -->


**CRUD**
> CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말이다. 사용자 인터페이스가 갖추어야 할 기능(정보의 참조/검색/갱신)을 가리키는 용어로서도 사용된다.

**Real Time Database**
> Firebase 실시간 데이터베이스는 클라우드 호스팅 데이터베이스입니다. **데이터는 JSON으로 저장되며 연결된 모든 클라이언트에 실시간으로 동기화됩니다.** iOS, Android 및 자바스크립트 SDK로 교차 플랫폼 앱을 개발하면 모든 클라이언트가 하나의 실시간 데이터베이스 인스턴스를 공유하고 자동 업데이트로 최신 데이터를 수신합니다.


### 인터페이스 변경하여 적용하기.

```typescript src/models/memo/memo.interface.ts
export interface Memo {
    uid: string;                    // 유저넘버
    author: string;                 // 작성자
    title: string;                  // 제목
    contents: string;               // 본문
    publishedDate: Date;           // 작성일
    recentUpdatedDate: Date;      // 최근 수정일
}
```

**key**가 사라지고 **uid**가 추가되었습니다.

### 메모매니저 파이어베이스 연동

```typescript src/providers/memo-manager/memo-manager.ts
import { AuthManagerProvider } from './../auth-manager/auth-manager';
import { Memo } from './../../models/memo/memo.interface';
import { Injectable } from '@angular/core';
import { Http } from '@angular/http';
import { AngularFireDatabase, FirebaseListObservable } from 'angularfire2/database';

/*
  Generated class for the MemoManagerProvider provider.

  See https://angular.io/docs/ts/latest/guide/dependency-injection.html
  for more info on providers and Angular DI.
*/
@Injectable()
export class MemoManagerProvider {

  memoList: FirebaseListObservable<any>

  constructor(
    public http: Http,
    public afDB: AngularFireDatabase,
    public authManager: AuthManagerProvider
  ) {
    this.initMemoList();
  }

  initMemoList() {
    this.memoList = this.afDB.list(`/memoList/${this.authManager.getUserInfo().uid}`);
  }

  getMemoList(): FirebaseListObservable<Memo> {
    return this.memoList;
  }

  createMemo(title: string, contents: string) {

    let memo: Memo = {
      uid: this.authManager.getUserInfo().uid,
      title: title,
      contents: contents,
      author: this.authManager.getUserInfo().email,
      publishedDate: new Date(),
      recentUpdatedDate: new Date(),
    }

    this.memoList.push(memo);
  }

  deleteMemo(deleteMemo: any) {
    this.memoList.remove(deleteMemo);
  }

  editMemo(memoToChange: Memo, title: string, contents: string) {
    memoToChange.title = title;
    memoToChange.contents = contents;
    memoToChange.recentUpdatedDate = new Date();
  }
}
```

변경 및 생성을 하면 자동으로 **Observable**이 구독하여 변경사항이 반영됩니다.


### 메모리스트페이지에 반영하기

```html src/pages/memo-list/memo-list.html
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
<ion-content>
  <ng-template ngFor let-memo [ngForOf]="memoList | async" let-i="index">
    <ion-card (click)="onClickViewMemoDetail(memo)" *ngIf="filterMemo(memo)">
      <ion-card-header>
        {{ memo.title }}
      </ion-card-header>
      <ion-card-content>
        {{ memo.contents }}
      </ion-card-content>
    </ion-card>
  </ng-template>
  <ion-fab right bottom>
    <button ion-fab icon-only (click)="onClickCreateMemo()">
      <ion-icon name="add" big></ion-icon>
    </button>
  </ion-fab>
</ion-content>
```

```typescript src/pages/memo-list/memo-list.ts
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
  memoList: FirebaseListObservable<Memo>;

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
    const actionSheet = this.getMoreOptionActionSheet();
    actionSheet.present();
  }

  getMoreOptionActionSheet() {
    return this.actionSheetCtrl.create({
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
  }
}
```

### 그 외 html 변경사항들입니다.

```html src/pages/memo-detail/memo-detail.html
<ion-header>
  <ion-navbar>
    <ion-buttons end>
      <button ion-button icon-only (click)="onDeleteMemo(memo)">
        <ion-icon name="trash"></ion-icon>
      </button>
      <button ion-button icon-only>
        <ion-icon name="more"></ion-icon>
      </button>
    </ion-buttons>
  </ion-navbar>
</ion-header>
<ion-content>
  <ion-card (click)="onOpenEditMemo(memo)">
    <ion-card-header>
      <h1>{{ memo.title }}</h1>
    </ion-card-header>
    <ion-card-content>
      <p>
        {{ memo.contents }}
      </p>
      <div class="etc">
        <p>
          <span>저자</span> 
          {{ memo.author }}
        </p>
        <p>
          <span>발행일</span>
          {{ memo.publishedDate }}
        </p>
        <p>
          <span>최근 수정일</span>
           {{ memo.recentUpdatedDate }}
        </p>
      </div>
    </ion-card-content>
  </ion-card>
</ion-content>
```


```html src/pages/memo-detail/memo-detail.html
<ion-header>
  <ion-navbar color="secondary">
  </ion-navbar>
</ion-header>
<ion-content>
  <ion-list>
    <ion-item>
      <ion-label floating>제목을 입력해주세요.</ion-label>
      <ion-input type="text" [(ngModel)]="title"></ion-input>
    </ion-item>
    <ion-item>
      <ion-label floating>내용을 입력해주세요.</ion-label>
      <ion-textarea rows="18" type="text" [(ngModel)]="contents"></ion-textarea>
    </ion-item>
  </ion-list>
  <ion-fab right bottom>
    <button ion-fab icon-only color="secondary" (click)="onSaveMemo()"><ion-icon name="checkmark"></ion-icon></button>
  </ion-fab>
</ion-content>
```

---

참고 링크
- [해당 포스트에 작성된 모든 코드는 여기에 있습니다!](https://github.com/ddalpange/simple-memo)
- [해당 프로젝트는 여기서 볼 수 있습니다 !!](https://memo-28314.firebaseapp.com)
