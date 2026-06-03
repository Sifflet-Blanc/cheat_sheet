[Home](README.md)
# Angular

Framwork + library

Made by google

Typescript based 

Dependance injection

Model [MVC](acronyme.md#mvc)

[SPA](acronyme.md#spa)

New major every 6 mounth (every version LTS when a new release (only 2 LTS))
- 17.0 new identity and big change 
- 21.0 (Mai 2025)
- 21.0 (November 2025)
- 22.0 (June 2026)

Command on the [CLI](acronyme.md#cli) to update version.


## CLI
Install `npm install -g @angular/cli@21`

Create a project `ng new my-angular-app`

Run the project `ng serve`

Create a component `ng generate component <COMPONENT_NAME>`
- --dry-run no change are actually made 

Create a service `ng generate service <SERVICE_NAME>`

Create a directive `ng generate directive <DIRECTIVE_NAME>`

## Component 
UserComponent
- `user.component.ts` _behaviour_
- `user.component.scss` _style_
- `user.component.html` _template_
- `user.component.spec.ts` _test_

## Binding
Lien entre votre template et votre modèle
- synchronous vue and model
- Notiﬁer une interaction utilisateur sur la vue ou le modèle

### Many type
- Interpolation : {{}} access directly to the component 
- Property binding : [] a variable in TS used in HTML template
- Attribute binding : [] a variable to define an attribute of a HTML element
- Class & Style binding : [] a variable to define the CSS of a HTML element
- Event binding : () an interact on a template linked to a TS method
- Two-way binding : [()] a variable link to the HTML value and the ts value (banana in the box)

[] = ts to template
() = template to ts 

## Interaction

To call a class (template use ts)
```html
<developer-card
    [developer]="developers[0]">
</developer-card>
``` 
```ts
import { Component, Input } from '@angular/core';

@Component({
    selector: 'developer-card',
    templateUrl: './developer-card.component.html',
    ...
})
export class DeveloperCardComponent {
    @Input({required: true}) developer: Developer;
}
```

Child call parrent 
```ts
export class DeveloperCardComponent {
    …
    @Output() remove = new EventEmitter<Developer>();

    removeCard(): void {
        this.remove.emit(this.developer);
    }
}
```
```html
<developer-card
    (remove)="removeDeveloper($event)">
</developer-card>
```
When removeCard() is call it emit `remove` and so the function removeDeveloper() are call in the parrent.


## Life cycle 
- Initialyse 
- Change detect 
- Destruct

## Service

@Injectable = Service
- provideIn: root = singleton

Inject it 
```ts
@Component({
    providers: [
        HeroService
    ]
})
```

Use it 
```ts
@Component({...})
export class HeroComponent {
    heroService = inject(HeroService)
}
```

## Module
Used to group together related items
```ts 
@NgModule
```
declare the component :
- Components
- Services
- Directives
- Pipes

## Built-in Control Flow
in HTML file
```html
@if (a > b) {
    {{a}} is greater than {{b}}
} @else if (b > a) {
    {{a}} is less than {{b}}
} @else {
    {{a}} is equal to {{b}}
}
```

```html
@switch (condition) {
    @case (caseA) {
        Case A.
    }
    @case (caseB) {
        Case B.
    }
    @default {
        Default case.
    }
}
```

```html
@for (item of items; track item.id) {
    {{ item.name }}
} @empty {
    <li> There are no items.</li>
}
```

## Directive

```ts
import {Directive, ElementRef} from '@angular/core';
@Directive({
    standalone: true,
    selector: '[appHighlight]',
})
export class HighlightDirective {
    constructor(private el: ElementRef) {
        this.el.nativeElement.style.backgroundColor = 'yellow';
    }
}
```
```html
<p appHighlight>Highlight me!</p>
```

## Pipe
Only on HTML size 
```html
<div>{{ user.name | uppercase }} </div>
```
result `NAME`
```html
<div>{{ currentDate | date : “mm:ss” }} </div>
```
result `43:13`
```html
<div>{{ 0.259 | currency }} </div>
```
result `$0.26`

Custom Pipe :
```ts 
@Pipe({name: 'exponentialStrength' })
export class ExponentialStrengthPipe implements PipeTransform {
    transform (value: number, exponent = 1): number {
        return Math.pow(value, exponent);
    }
}
```


# RxJS
ReactiveX

## Observer
Permit to have a multiple asynchronous value
```ts
of(a: any): Observable
```
```ts
from(a: any): Observable
```
an is an array or iterable get value one by one
Both behind are synchronous
```ts
fromEvent(): Observable
```
Ex : 
```ts
const resultFromEvent$ = fromEvent(document, 'click');

resultFromEvent$.subscribe(
    event => console.log(event.timeStamp)
);
```
```ts
subscribe(o: Observer): Subscription
```
get value (we can relaunch it indefinitly)
```ts
pipe(...ops: Operator[]): Observable<T>
```
chain operator

Ex :

```ts
const data$: Observable = interval(1000);

const souscription = data$.subscribe({
    next: value => console.log(value),
    error: err => console.error(err),
    complete: () => console.log('DONE!')
}

souscription.unsubscribe();
```

Don't forget to unsubscribe !

## Hot/Cold
Hot always emit if we listen it or not.

Cold is what we describe earlier.

## Subject
Child of Observable and Observer
```ts
const subject$ = new Subject<number>();
subject$.subscribe(r => console.log(`A: ${r}`));

subject$.next(1);
subject$.next(2);

subject$.subscribe(r => console.log(`B: ${r}`));

subject$.next(3);
```
result :
A: 1

A: 2

A: 3
B: 3

### BehaviorSubject
```ts
const subject$ = new BehaviorSubject(0);
subject$.subscribe(r => console.log(`A: ${r}`));

subject$.next(1);
subject$.next(2);

subject$.subscribe(r => console.log(`B: ${r}`));

subject$.next(3);
```
result :
A: 0

A: 1

A: 2

B: 2

A: 3
B: 3

### ReplaySubject 
```ts
const subject$ = new ReplaySubject<number>(2);

subject$.subscribe(r => console.log(`A: ${r}`));

subject$.next(1);
subject$.next(2);

subject$.subscribe(r => console.log(`B: ${r}`));

subject$.next(3);
```
result :
A: 1

A: 2

B: 1
B: 2

A: 3
B: 3


## Pipe 
```ts
const source$ = from([{x: 55, y: 65, label: ‘here’}, {x: 30, y: 55, label: ‘ici’}] )

const example = source$.pipe(
    map((event) => [event.x,event.y]),
    filter([x,y] => x > 50 && y > 50)
);

const subscribe = example.subscribe(val => console.log(val));
//Log : [55, 65]
```

## High order observable 
Permit a chain of observable (Ex get user then get his card)
Wrong :
```ts
this.service.getData().subscribe(
    result => this.service2.getMoreData(result).subscribe(
        finalResult => this.result = finalResult
    );
);
```

Right :
```ts
this.service.getData().pipe(
    concatMap(result => this.service2.getMoreData(result))
).subscribe(
    finalResult => this.result = finalResult
);
```

In most of case use `switchMap`

## Unsubscribe 
```ts
class A implements OnInit, OnDestroy {
    souscription: Souscription;

    ngOnInit(): void{
        souscription = interval(1000).subscribe();
    }

    ngOnDestroy(): void{
        souscription.unsubscribe();
    }
}
```

Unsubscribe when x next are done.
```ts
class A implements OnInit {
    ngOnInit(): void{
        interval(1000).pipe(take(1)).subscribe();
    }
}
```

/!\ nobody do that :
```ts
class A implements OnInit, OnDestroy {
    private destroyed$ = new Subject<void>();
    
    ngOnInit(): void {
        interval(1000).pipe(takeUntil(this.destroyed$)).subscribe();
    }
    
    ngOnDestroy(): void {
        this.destroyed$.next();
        this.destroyed$.complete();
    }
}
```
We now do that :
```ts
class A implements OnInit {
    private destroyRef = inject(DestroyRef);
    
    constructor() {
        interval(1000).pipe(takeUntilDestroyed()).subscribe();
    }
    
    ngOnInit(): void {
        interval(1000).pipe(takeUntilDestroyed(this.destroyRef)).subscribe();
    }
}
```

Pipe async
```ts
user$: Observable <User>;
```
```html
<div>{{ user$ | async }} </div>
```
or
```html
@if(user$ | async; as user){
    <span>{{ user.firstname }}</span>
    <span>{{ user.lastname }}</span>
}
```

## Signal
Wrapper around a value witch permit to track it 
Synchronous
Déclarative

Store 

Ex:
```ts
private products = signal([...]);
private filter = signal('');

filteredProducts = computed(() => 
    this.products().filter(product => product.includes(this.filter()))
);

isFilteredProductsEmpty = computed(
    () => this.filteredProducts().length === 0
);

setFilter(event: Event): void {
    const input = event.target as HTMLInputElement;
    this.filter.set(input.value);
}
```


## Effect 
callback without consider the value 
Ex:
```ts
count = signal(0);

showCount = effect(() => console.log(`The current count is ${this.count()}`));
```

## Router 
Router outlet (directive)

### Lazy loading 
load the module only when we need it 
reduce the size of the module

### Navigation
Use routerLink with element `<a>`
Use `Router` service

### Parameter
use `:` in the path define

### Route config
We can define redirect 
wildcar ** to match all (be aware)