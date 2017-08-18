### 随手笔记

```
{{}}            双大括号插值表达式

``              ES2015 的模板字符串可以写一个多行模板，使我们的模板更具可读性。

ngModel         往界面元素上添加双向数据绑定

[(ngModel)]     双向数据绑定语法。是一个Angular语法，用与把组件的属性绑定到输入框中。 它的数据流是双向的：从属性到输入框，并且从输入框回到属性。

FormsModule     虽然NgModel是一个有效的Angular指令，但它默认情况下却是不可用的。 它属于一个可选模块FormsModule。 我们必须选择使用那个模块。从@angular/forms库中导入符号FormsModule。 然后把FormsModule添加到@NgModule元数据的imports数组中，它是当前应用正在使用的外部模块列表。

imports         imports的数组，它是当前应用正在使用的外部模块列表。

*ngFor          ngFor的*前缀表示<li>及其子元素组成了一个主控模板。

*ngFor="let hero of heroes"
                引号中赋值给ngFor的那段文本表示“从heroes数组中取出每个英雄，存入一个局部的hero变量，并让它在相应的模板实例中可用”。

<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
                圆括号标识<li>元素上的click事件是绑定的目标。 等号右边的onSelect(hero)表达式调用AppComponent的onSelect()方法，并把模板输入变量hero作为参数传进去。 它是我们前面在ngFor指令中定义的那个hero变量。

*ngIf           

[class.selected]="hero === selectedHero"
                当表达式(hero === selectedHero)为true时，Angular会添加一个CSS类selected。为false时则会移除selected类。

@Component      要定义一个组件，我们总是要先导入符号Component。@Component装饰器为组件提供了Angular元数据。 CSS选择器的名字hero-detail会匹配元素的标签名，用于在父组件的模板中标记出当前组件的位置。 本章的最后，我们会把<hero-detail>添加到AppComponent的模板中。

<hero-detail [hero]="selectedHero"></hero-detail>
                在等号的左边，是方括号围绕的hero属性，这表示它是属性绑定表达式的目标。 我们要绑定到的目标属性必须是一个输入属性，否则Angular会拒绝绑定，并抛出一个错误。首先，修改@angular/core导入语句，使其包含符号Input。首先，修改@angular/core导入语句，使其包含符号Input。然后，通过在hero属性前面加上@Input装饰器，来表明它是一个输入属性。

@Input() hero: Hero;

declarations: [
  AppComponent,
  HeroDetailComponent
],
                通常，declarations数组包含应用中属于该模块的组件、管道和指令的列表。 组件在被其它组件引用之前必须先在一个模块中声明过。 这个模块只声明了两个组件：AppComponent 和 HeroDetailComponent。

import { Injectable } from '@angular/core';

@Injectable()
export class HeroService {
}
                可注入的服务:注意，我们导入了 Angular 的Injectable函数，并作为@Injectable()装饰器使用这个函数。不要忘了写圆括号！如果忘了写，就会导致一个很难诊断的错误。当 TypeScript 看到@Injectable()装饰器时，就会记下本服务的元数据。 如果 Angular 需要往这个服务中注入其它依赖，就会使用这些元数据。虽然此时HeroService还没有任何依赖，但我们还是得加上这个装饰器。 作为一项最佳实践，无论是出于提高统一性还是减少变更的目的， 都应该从一开始就加上@Injectable()装饰器。

constructor(private heroService: HeroService) { }
                不要new出HeroService:
                该如何在运行中获得一个具体的HeroService实例呢？

                你可能想用new来创建HeroService的实例，就像这样：
                heroService = new HeroService(); // don't do this
                但这不是个好主意，有很多理由，例如：

                我们的组件得弄清楚该如何创建HeroService。 如果有一天我们修改了HeroService的构造函数，我们不得不找出创建过此服务的每一处代码，并修改它。 围着补丁代码转圈很容易导致错误，还会增加测试负担。
                我们每次使用new都会创建一个新的服务实例。 如果这个服务需要缓存英雄列表，并把这个缓存共享给别人呢？怎么办？ 没办法，做不到。
                我们把AppComponent锁定到HeroService的一个特定实现。 我们很难在不同的场景中切换实现。 例如，能离线操作吗？能在测试时使用不同的模拟版本吗？这可不容易。

                注入 HeroService

                你可以用两行代码代替用new时的一行：

                添加一个构造函数，并定义一个私有属性。
                添加组件的providers元数据。
                添加构造函数：
                constructor(private heroService: HeroService) { }
                构造函数自己什么也不用做，它在参数中定义了一个私有的heroService属性，并把它标记为注入HeroService的靶点。
                现在，当创建AppComponent实例时，Angular 知道需要先提供一个HeroService的实例。

                我们还得注册一个HeroService提供商，来告诉注入器如何创建HeroService。 要做到这一点，我们在@Component组件的元数据底部添加providers数组属性如下：

                providers: [HeroService]

                providers数组告诉 Angular，当它创建新的AppComponent组件时，也要创建一个HeroService的新实例。 AppComponent会使用那个服务来获取英雄列表，在它组件树中的每一个子组件也同样如此。

ngOnInit        只要我们实现了 Angular 的 ngOnInit 生命周期钩子，Angular 就会主动调用这个钩子。 Angular提供了一些接口，用来介入组件生命周期的几个关键时间点：刚创建时、每次变化时，以及最终被销毁时。

import { OnInit } from '@angular/core';

export class AppComponent implements OnInit {
  ngOnInit(): void {
  }
}
                这是OnInit接口的基本轮廓

export class AppComponent implements OnInit {}
                往export语句中添加OnInit接口的实现

getHeroes(): Promise<Hero[]> {
  return Promise.resolve(HEROES);
}
                把HeroService的getHeroes方法改写为返回承诺的形式.

this.heroService.getHeroes().then(heroes => this.heroes = heroes);
                我们把回调函数作为参数传给承诺对象的then方法.回调中所用的 ES2015 箭头函数 比等价的函数表达式更加简洁，能优雅的处理 this 指针。
            
getHeroesSlowly(): Promise<Hero[]> {
  return new Promise(resolve => {
    // Simulate server latency with 2 second delay
    setTimeout(() => resolve(this.getHeroes()), 2000);
  });
}
                像getHeroes()一样，它也返回一个承诺。 但是，这个承诺会在提供模拟数据之前等待两秒钟。

 Angular NgModule
                Angular 路由器是一个可选的外部 Angular NgModule，名叫RouterModule。 路由器包含了多种服务(RouterModule)、多种指令(RouterOutlet、RouterLink、RouterLinkActive)、 和一套配置(Routes)

import { RouterModule }   from '@angular/router';
                导入RouterModule

RouterModule.forRoot([
      {
        path: 'heroes',
        component: HeroesComponent
      }
    ])
                这里使用了forRoot()方法，因为我们是在应用根部提供配置好的路由器。 forRoot()方法提供了路由需要的路由服务提供商和指令，并基于当前浏览器 URL 初始化导航。

<head>
  <base href="/">
                打开index.html，确保它的<head>区顶部有一个<base href="...">元素（或动态生成该元素的脚本）

<router-outlet></router-outlet>
                如果我们把路径/heroes粘贴到浏览器的地址栏，路由器会匹配到'Heroes'路由，并显示HeroesComponent组件。 我们必须告诉路由器它位置，所以我们把<router-outlet>标签添加到模板的底部。 RouterOutlet是由RouterModule提供的指令之一。 当我们在应用中导航时，路由器就把激活的组件显示在<router-outlet>里面。

template: `
   <h1>{{title}}</h1>
   <a routerLink="/heroes">Heroes</a>
   <router-outlet></router-outlet>
 `
                注意，锚标签中的[routerLink]绑定。 我们把RouterLink指令（ROUTER_DIRECTIVES中的另一个指令）绑定到一个字符串。 它将告诉路由器，当用户点击这个链接时，应该导航到哪里。

                由于这个链接不是动态的，我们只要用一次性绑定的方式绑定到路由的路径 (path) 就行了。 回来看路由配置表，我们清楚的看到，这个路径 —— '/heroes'就是指向HeroesComponent的那个路由的路径。

{
  path: '',
  redirectTo: '/dashboard',
  pathMatch: 'full'
},
                路由重定向

{
  path: 'detail/:id',
  component: HeroDetailComponent
},
                路径中的冒号 (:) 表示:id是一个占位符，当导航到这个HeroDetailComponent组件时，它将被填入一个特定英雄的id

// Keep the Input import for now, you'll remove it later:
import { Component, Input, OnInit } from '@angular/core';
import { ActivatedRoute, ParamMap } from '@angular/router';
import { Location }                 from '@angular/common';

import { HeroService } from './hero.service';
import 'rxjs/add/operator/switchMap';

constructor(
  private heroService: HeroService,
  private route: ActivatedRoute,
  private location: Location
) {}

ngOnInit(): void {
  this.route.paramMap
    .switchMap((params: ParamMap) => this.heroService.getHero(+params.get('id')))
    .subscribe(hero => this.hero = hero);
}

                注意switchMap运算符如何将可观察的路由参数中的 id 映射到一个新的Observable， 即HeroService.getHero()方法的结果。

                如果用户在 getHero 请求执行的过程中再次导航这个组件，switchMap 再次调用HeroService.getHero()之前， 会取消之前的请求。

                英雄的id是数字，而路由参数的值总是字符串。 所以我们需要通过 JavaScript 的 (+) 操作符把路由参数的值转成数字。

                我需要取消订阅吗？
                正如以前在路由与导航章的ActivatedRoute：一站式获取路由信息部分讲过的，Router管理它提供的可观察对象，并使订阅局部化。当组件被销毁时，会清除 订阅，防止内存泄漏，所以我们不需要从路由参数Observable取消订阅。

getHero(id: number): Promise<Hero> {
  return this.getHeroes()
             .then(heroes => heroes.find(hero => hero.id === id));
}


this.location.back();
                回退一次导航历史。回退太多步会跑出我们的应用。 在真实的应用中，我们需要使用CanDeactivate守卫对此进行防范。

<a *ngFor="let hero of heroes"  [routerLink]="['/detail', hero.id]"  class="col-1-4">
                我们绑定了一个包含链接参数数组的表达式。 该数组有两个元素，目标路由和一个用来设置当前英雄的 id 值的路由参数。

守卫服务        

重构路由为一个路由模块
                AppModule中有将近 20 行代码是用来配置四个路由的。 绝大多数应用有更多路由，并且它们还有守卫服务来保护不希望或未授权的导航。 （要了解守卫服务的更多知识，参见路由与导航页的路由守卫） 路由的配置可能迅速占领这个模块，并掩盖其主要目的，即为 Angular 编译器设置整个应用的关键配置。我们应该重构路由配置到它自己的类。 
                
                什么样的类呢？ 当前的RouterModule.forRoot()产生一个Angular ModuleWithProviders，所以这个路由类应该是一种模块类。 它应该是一个路由模块。要想了解更多，请参阅路由与导航一章的里程碑2：路由模块部分。

{{selectedHero.name | uppercase}} is my hero
                注意，英雄的名字全被显示成大写字母。那是uppercase管道的效果，借助它，我们能干预插值表达式绑定的过程。可以管道操作符 ( | ) 后面看到它。

                管道擅长做下列工作：格式化字符串、金额、日期和其它显示数据。 Angular 自带了一些管道，我们也可以写自己的管道。

<a routerLink="/dashboard" routerLinkActive="active">Dashboard</a>
                Angular路由器提供了routerLinkActive指令，我们可以用它来为匹配了活动路由的 HTML 导航元素自动添加一个 CSS 类。 我们唯一要做的就是为它定义样式。真好！

// Imports for loading & configuring the in-memory web api
import { InMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService }  from './in-memory-data.service';
@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    InMemoryWebApiModule.forRoot(InMemoryDataService),
    AppRoutingModule
  ],
  ...
                导入InMemoryWebApiModule并将其加入到模块的imports数组。 InMemoryWebApiModule将Http客户端默认的后端服务 — 这是一个辅助服务，负责与远程服务器对话 — 替换成了内存 Web API服务：
                InMemoryWebApiModule.forRoot(InMemoryDataService),
                forRoot()配置方法需要InMemoryDataService类实例，用来向内存数据库填充数据： 往app目录下新增一个文件in-memory-data.service.ts，填写下列内容：


src/app/in-memory-data.service.ts
 content_copy
import { InMemoryDbService } from 'angular-in-memory-web-api';
export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    const heroes = [
      { id: 0,  name: 'Zero' },
      { id: 11, name: 'Mr. Nice' },
      { id: 12, name: 'Narco' },
      { id: 13, name: 'Bombasto' },
      { id: 14, name: 'Celeritas' },
      { id: 15, name: 'Magneta' },
      { id: 16, name: 'RubberMan' },
      { id: 17, name: 'Dynama' },
      { id: 18, name: 'Dr IQ' },
      { id: 19, name: 'Magma' },
      { id: 20, name: 'Tornado' }
    ];
    return {heroes};
  }
}
                和angular-in-memory-web-api配合的内存数据库。


import { Injectable }    from '@angular/core';
import { Headers, Http } from '@angular/http';

import 'rxjs/add/operator/toPromise';

import { Hero } from './hero';

private heroesUrl = 'api/heroes';  // URL to web api
 
constructor(private http: Http) { }
 
getHeroes(): Promise<Hero[]> {
  return this.http.get(this.heroesUrl)
             .toPromise()
             .then(response => response.json().data as Hero[])
             .catch(this.handleError);
}
 
private handleError(error: any): Promise<any> {
  console.error('An error occurred', error); // for demo purposes only
  return Promise.reject(error.message || error);
}


import 'rxjs/add/operator/toPromise';
                Angular 的http.get返回一个 RxJS 的Observable对象。 Observable（可观察对象）是一个管理异步数据流的强力方式。 后面我们还会进一步学习可观察对象。

                现在，我们先利用toPromise操作符把Observable直接转换成Promise对象，回到已经熟悉的地盘。

                content_copy.toPromise()
                不幸的是，Angular 的Observable并没有一个toPromise操作符... 没有打包在一起发布。Angular的Observable只是一个骨架实现。

                有很多像toPromise这样的操作符，用于扩展Observable，为其添加有用的能力。 如果我们希望得到那些能力，就得自己添加那些操作符。 那很容易，只要从 RxJS 库中导入它们就可以了，就像这样：

.then(response => response.json().data as Hero[])
                在 promise 的then()回调中，我们调用 HTTP 的Reponse对象的json方法，以提取出其中的数据。这个由json方法返回的对象只有一个data属性。 这个data属性保存了英雄数组，这个数组才是调用者真正想要的。 所以我们取得这个数组，并且把它作为承诺的值进行解析。

.catch(this.handleError);
                这是一个关键的步骤！ 我们必须预料到 HTTP 请求会失败，因为有太多我们无法控制的原因可能导致它们频繁出现各种错误。
              
private handleError(error: any): Promise<any> {
  console.error('An error occurred', error); // for demo purposes only
  return Promise.reject(error.message || error);
}
                在这个范例服务中，我们把错误记录到控制台中；在真实世界中，我们应该用代码对错误进行处理。但对于演示来说，这就够了。

                我们还要通过一个被拒绝 (rejected) 的承诺来把该错误用一个用户友好的格式返回给调用者， 以便调用者能把一个合适的错误信息显示给用户。

getHero(id: number): Promise<Hero> {
  const url = `${this.heroesUrl}/${id}`;
  return this.http.get(url)
    .toPromise()
    .then(response => response.json().data as Hero)
    .catch(this.handleError);
}
               const url = `${this.heroesUrl}/${id}`; 通过ID获取对象数据。 


Promise和可观察对象 (Observable)
                请求-取消-新请求的序列对于Promise来说是很难实现的，但是对Observable来说则很容易。


```


### 用得漂亮的词汇：

我们不能把这 60 来行 CSS 粘贴到组件元数据的styles中，否则它会淹没组件的工作逻辑。反之，我们应该在独立的*.css文件中编辑这些CSS。

this.heroService.getHeroes().then(heroes => this.heroes = heroes);
                我们把回调函数作为参数传给承诺对象的then方法.回调中所用的 ES2015 箭头函数 比等价的函数表达式更加简洁，能优雅的处理 this 指针。



### angular http 增删改查

* 查询全部
```
getHeroes(): Promise<Hero[]> {
  return this.http.get(this.heroesUrl)
             .toPromise()
             .then(response => response.json().data as Hero[])
             .catch(this.handleError);
}
 
private handleError(error: any): Promise<any> {
  console.error('An error occurred', error); // for demo purposes only
  return Promise.reject(error.message || error);
}

```

* id查询
```
getHero(id: number): Promise<Hero> {
  const url = `${this.heroesUrl}/${id}`;
  return this.http.get(url)
    .toPromise()
    .then(response => response.json().data as Hero)
    .catch(this.handleError);
}
```

* 修改：
```
private headers = new Headers({'Content-Type': 'application/json'});

update(hero: Hero): Promise<Hero> {
  const url = `${this.heroesUrl}/${hero.id}`;
  return this.http
    .put(url, JSON.stringify(hero), {headers: this.headers})
    .toPromise()
    .then(() => hero)
    .catch(this.handleError);
}
```

* 新增：
```
create(name: string): Promise<Hero> {
  return this.http
    .post(this.heroesUrl, JSON.stringify({name: name}), {headers: this.headers})
    .toPromise()
    .then(res => res.json().data as Hero)
    .catch(this.handleError);
}
```

* 删除：
```
delete(id: number): Promise<void> {
  const url = `${this.heroesUrl}/${id}`;
  return this.http.delete(url, {headers: this.headers})
    .toPromise()
    .then(() => null)
    .catch(this.handleError);
}
```