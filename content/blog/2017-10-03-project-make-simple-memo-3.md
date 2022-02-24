---
title: 간단한 메모장 만들기 3 - 목데이터 사용
date: 2017-10-03 22:48:57
thumbnail: /images/memo/memoBanner.png
banner: /images/memo/memoBanner.png
categories: [SimpleMemo]
tags: [Javascript, Angular, Ionic, SimpleMemo]
toc: true
---

이번 시간에서는 목 객체와 Angular2의 서비스(보통 아이오닉에선 provider 라고 명칭합니다.)를 이용해서 메모가 어떻게 만들어지고, 수정되고, 삭제되는지 알아볼겁니다. 틀리거나 안되는것이 있다면 바로바로 댓글로 달아주세요!

<!-- more -->

**mock objects?**

> 목 객체는 실제 객체의 동작을 흉내내는 시뮬레이션 객체입니다. 보통 프론트단에서 api와의 의존성을 없애고 테스트를 쉽게 하기 위해 하드코딩된 데이터들을 mock data라고 칭합니다.

**service?**

> 서비스는 애플리케이션 전반에 걸쳐 코드를 구성하고 공유하는 데 사용되며, 일반적으로 데이터 액세스 방법을 생성하는 곳이기도 합니다. 즉 재사용성이 높은 코드들을 모아놓은 것이며, Angular2에서 기본으로 제공해주는 패턴중의 하나입니다. 서비스를 이해할려면 DI(Dependency Injection)을 이해해야하는데, 포스트가 너무 길어지니 다음에 하죠.

### 메모 인터페이스 정의하기

**src**폴더에서 **models**, **memo** 폴더를 차례대로 만들어 주신 후 그 안에 **memo.interface.ts**파일을 만들어주세요

```typescript
export interface Memo {
  key: number
  author: string // 작성자
  title: string // 제목
  contents: string // 본문
  publishedDate: Date // 작성일
  recentUpdatedDate: Date // 최근 수정일
}
```

### 목 메모리스트 정의하기

**src**폴더에서 **mocks**, **memo** 폴더를 만들어 주신 후

그 안에 **memo-list.mock.ts**파일을 만들어주세요

```typescript
import { Memo } from "./../../models/memo/memo.interface"
export const MEMOLIST: Memo[] = [
  {
    key: 0,
    title: "제목 1",
    contents:
      "본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문",
    author: "ddalpange@gmail.com",
    recentUpdatedDate: new Date(),
    publishedDate: new Date(),
  },
  {
    key: 1,
    title: "제목 2",
    contents:
      "본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문본문",
    author: "ddalpange@gmail.com",
    recentUpdatedDate: new Date(),
    publishedDate: new Date(),
  },
]
```

### 메모 관리 서비스 만들기 !

```bash
$ ionic g provider memo-manager
```

위 명령어를 실행하고 나면 **providers**라는 폴더가 **src** 폴더에 생길거에요!
Ionic Cli로 만든 provider는 앱 모듈에 자동으로 추가(registering)됩니다.

```typescript
import { MEMOLIST } from "./../../mocks/memo/memo-list.mock"
import { Memo } from "./../../models/memo/memo.interface"
import { Injectable } from "@angular/core"
import { Http } from "@angular/http"

/*
  Generated class for the MemoManagerProvider provider.

  See https://angular.io/docs/ts/latest/guide/dependency-injection.html
  for more info on providers and Angular DI.
*/
@Injectable()
export class MemoManagerProvider {
  memoList: Memo[]

  constructor(public http: Http) {
    this.initMemoList()
  }

  initMemoList() {
    this.memoList = MEMOLIST
  }

  getMemoList(): Memo[] {
    return this.memoList
  }

  getMemo(key: number): Memo {
    let index = this.memoList.findIndex((memo: Memo, i: number) => {
      return memo.key === key
    })

    return this.memoList[index] || null
  }

  createMemo(title: string, contents: string, author: string) {
    let lastMemo = this.memoList[this.memoList.length - 1]
    let lastMemoKey = lastMemo ? lastMemo.key : -1
    let key = lastMemoKey + 1

    let memo: Memo = {
      key: key,
      title: title,
      contents: contents,
      author: author,
      publishedDate: new Date(),
      recentUpdatedDate: new Date(),
    }

    this.memoList.push(memo)
  }

  deleteMemo(deleteMemo: Memo) {
    let index = this.memoList.findIndex((memo: Memo, i: number) => {
      return memo.key === deleteMemo.key
    })

    this.memoList.splice(index, 1)
  }

  editMemo(changeMemo: Memo, title: string, contents: string) {
    let index = this.memoList.findIndex((memo: Memo, i: number) => {
      return memo.key === changeMemo.key
    })

    this.memoList[index].title = title
    this.memoList[index].contents = contents
    this.memoList[index].recentUpdatedDate = new Date()
  }
}
```

잘 되는지 메모 리스트 페이지에서 메모 매니저를 주입(DI) 받아보겠습니다.

메모 리스트페이지의 constructor 부분을 아래와 같이 바꿔주세요.

```typescript
...
import { MemoManagerProvider } from './../../providers/memo-manager/memo-manager';

...

export class MemoListPage {

  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public memoManager: MemoManagerProvider) {
  }
  .....
}
```

**여기까지 !**

잠시 숨 고르시고 에러 안나는지 살펴보세요.

이제 페이지들의 뷰에 각 데이터들을 연동하고, 메모 매니저를 주입(DI)받아서 메모를 쓰기, 수정 삭제, 보기 할 수 있는 기능을 만들거에요.

```typescript
import { Memo } from "./../../mod els/memo/memo.interface"
import { Component, OnInit } from "@angular/core"
import { IonicPage, NavController, NavParams } from "ionic-angular"

import { MemoCreatePage } from "./../memo-create/memo-create"
import { MemoDetailPage } from "./../memo-detail/memo-detail"

import { MemoManagerProvider } from "./../../providers/memo-manager/memo-manager"
@IonicPage()
@Component({
  selector: "page-memo-list",
  templateUrl: "memo-list.html",
})
export class MemoListPage {
  memoList: Memo[] = []

  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public memoManager: MemoManagerProvider
  ) {}

  ngOnInit() {
    this.memoList = this.memoManager.getMemoList()
    console.log(this.memoList)
  }

  onClickViewMemoDetail(memo: Memo) {
    this.navCtrl.push(MemoDetailPage, { memo: memo })
  }

  onClickCreateMemo() {
    this.navCtrl.push(MemoCreatePage)
  }
}
```

```html **src/pages/memo-list/memo-list.html**
<ion-header>
  <ion-navbar color="primary">
    <ion-title>메모들</ion-title>
  </ion-navbar>
</ion-header>
<ion-content>
  <ng-template ngFor let-memo [ngForOf]="memoList" let-i="index">
    <ion-card (click)="onClickViewMemoDetail(memo)">
      <ion-card-header> {{ memo.title }} </ion-card-header>
      <ion-card-content> {{ memo.contents }} </ion-card-content>
    </ion-card>
  </ng-template>
  <ion-fab right bottom>
    <button ion-fab icon-only (click)="onClickCreateMemo()">
      <ion-icon name="add" big></ion-icon>
    </button>
  </ion-fab>
</ion-content>
```

```typescript
import { MemoManagerProvider } from "./../../providers/memo-manager/memo-manager"
import { Memo } from "./../../models/memo/memo.interface"
import { Component, OnInit } from "@angular/core"
import { IonicPage, NavController, NavParams } from "ionic-angular"

import { MemoCreatePage } from "./../memo-create/memo-create"

@IonicPage()
@Component({
  selector: "page-memo-detail",
  templateUrl: "memo-detail.html",
})
export class MemoDetailPage {
  memo: Memo
  title: string
  contents: string

  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public memoManager: MemoManagerProvider
  ) {}

  ngOnInit() {
    this.memo = this.navParams.get("memo")
  }

  onOpenEditMemo(memo: Memo) {
    this.navCtrl.push(MemoCreatePage, { memo: memo })
  }

  onDeleteMemo(memo: Memo) {
    this.memoManager.deleteMemo(memo)
    this.navCtrl.pop()
  }
}
```

```html **src/pages/memo-detail/memo-detail.html**
<ion-header>
  <ion-navbar color="danger">
    <ion-title>메모 상세</ion-title>
    <ion-buttons end>
      <button ion-button icon-only (click)="onDeleteMemo(memo)">
        <ion-icon name="trash"></ion-icon>
      </button>
      <button ion-button icon-only (click)="onOpenEditMemo(memo)">
        <ion-icon name="hammer"></ion-icon>
      </button>
    </ion-buttons>
  </ion-navbar>
</ion-header>
<ion-content>
  <ion-card>
    <ion-card-header>
      <h1>{{ memo.title }}</h1>
    </ion-card-header>
    <ion-card-content>
      <p>{{ memo.contents }}</p>
      <div class="etc">
        <p>
          <span>저자</span>
          {{ memo.author }}
        </p>
        <p>
          <span>발행일</span>
          {{ memo.publishedDate | date }}
        </p>
        <p>
          <span>최근 수정일</span>
          {{ memo.recentUpdatedDate | date }}
        </p>
      </div>
    </ion-card-content>
  </ion-card>
</ion-content>
```

```typescript
import { MemoManagerProvider } from "./../../providers/memo-manager/memo-manager"
import { Memo } from "./../../models/memo/memo.interface"
import { Component, OnInit } from "@angular/core"
import { IonicPage, NavController, NavParams } from "ionic-angular"

@IonicPage()
@Component({
  selector: "page-memo-create",
  templateUrl: "memo-create.html",
})
export class MemoCreatePage {
  memo: Memo
  title: string
  contents: string
  constructor(
    public navCtrl: NavController,
    public navParams: NavParams,
    public memoManager: MemoManagerProvider
  ) {}

  ngOnInit() {
    let memo = this.navParams.get("memo")
    if (memo) {
      this.memo = memo
      this.title = memo.title
      this.contents = memo.contents
    }
  }

  onChangeTitle(event: KeyboardEvent) {
    this.title = event.target["value"]
  }

  onChangeContents(event: KeyboardEvent) {
    this.contents = event.target["value"]
  }

  onSaveMemo() {
    if (this.memo) {
      this.memoManager.editMemo(this.memo, this.title, this.contents)
    } else {
      this.memoManager.createMemo(
        this.title,
        this.contents,
        "ddalpange@gmail.com"
      )
    }
    this.navCtrl.pop()
  }
}
```

```html **src/pages/memo-create/memo-create.html**
<ion-header>
  <ion-navbar color="secondary">
    <ion-title>메모 만들기</ion-title>
  </ion-navbar>
</ion-header>
<ion-content padding>
  <ion-list>
    <ion-item>
      <ion-input
        placeholder="제목을 입력해주세요."
        [value]="title"
        (change)="onChangeTitle($event)"
      ></ion-input>
    </ion-item>
    <ion-item>
      <ion-textarea
        rows="50"
        placeholder="내용을 입력해 주시겠어요 ?"
        [value]="contents"
        (change)="onChangeContents($event)"
      ></ion-textarea>
    </ion-item>
  </ion-list>
  <ion-fab right bottom>
    <button ion-fab icon-only color="secondary" (click)="onSaveMemo()">
      <ion-icon name="checkmark"></ion-icon>
    </button>
  </ion-fab>
</ion-content>
```

### 완성되었습니다!!

---

참고 링크

- [해당 포스트에 작성된 모든 코드는 여기에 있습니다!](https://github.com/ddalpange/simple-memo)
- [해당 프로젝트는 여기서 볼 수 있습니다 !!](https://memo-28314.firebaseapp.com)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM3MTEyODA0OCwxNDU2ODEwNTgyLDYxOT
g3NjM2OF19
-->
