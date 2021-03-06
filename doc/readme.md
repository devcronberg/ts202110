# TypeScript

## Om TypeScript (TS)

- http://www.typescriptlang.org 
- Transpiler
- Skabt af blandt andet Anders Hejlsberg (C#, Delphi)
- Historien bag TypeScript - se [http://ithistorie.cronberg.dk](http://ithistorie.cronberg.dk?maerker=js,ts,det_vi_husker)
  - Pt i version 4.x
- Open Source
  - https://github.com/Microsoft/TypeScript
- Grundlæggende features
  - Superset af EcmaScript (alt ES er automatisk TS)
  - Typestærkt
  - Transpiler nyere ES features tilbage til ældre versioner af ES
  - Tillader brug af "moderne" syntaks og features
  - Stærk support i udviklingmiljøer og tilhørende værktøjer
- Installeres typisk via NPM/Node (eller Visual Studio)

Se https://github.com/devcronberg/undervisning-ts-opgaver

### Installering via Node

Kør som administator!

```
npm install typescript -g
```

Rå brug af transpiler

```
tsc helloworld.ts
```

Typisk benyttes en tsconfig.json fil

```
tsc --init
```

### Udviklingmiljø

De fleste benytter VS Code med extensions. 

Se [https://github.com/devcronberg/kursus](https://github.com/devcronberg/kursus/#udviklingsmij%C3%B8-og-pluginsextensions)

### Den hurtige måde at afvikle/debugge/lege med TS i VSC

https://github.com/devcronberg/ts-in-vsc

## Typer

Følgende typer er tilgængelige

- Any
- Boolean
- Number
- String
- Array
- Enum
- Tuple
- Void
- Object
- Function

### Type infernce

```typescript
let a = 1;
a = 2; // ok
// a = ""; // fejl
// a = true; // fejl
```

### Type annotation

Brug :\<type\>

```typescript
let [navn]: <type>;
```

### Erklæring af variabler

```typescript
let a: number = 1;
let b: boolean = true;
let c: string = "";
let d: Array<any> = [];
// any function
let e: Function = () => 42;
// lambda def
let f: () => number = () => 43;
```

### Argumenter til funktioner

```typescript
function add1(a: number, b: number): number {
  return a + b;
}

// Erklæring af signatur
const add2: (a: number, b: number) => number = (a: number, b: number): number => a + b;

// Infered
const add3 = (a: number, b: number): number => a + b;

// Fejl
// let res: number = add1(1, "1");
```

### Any

Any matcher JS typen. Brug altid let og const (ender i var ved transpilering til ES3 og ES5)

```typescript
// Best pratice - brug any
let a;
let b: any;
b = 1;
b = true;
b = "";

const c: any = 1;

let list: any[] = [1, true, "free"];
```

### Boolean

```typescript
let isDone: boolean = false;
```

### Number

Floating point

```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

### String

Simpel tekst - brug " eller ' som JS

```typescript
let color: string = "blue";
color = 'red';
```

Som i JS kan du bruge template strings

```typescript
let navn: string = "Mikkel";
let alder: number = 16;
let txt: string = `Hej, mit navn er ${navn} og jeg er ${alder} år gammel`;
```

### Array

Arrays kan skrives på flere måder men [] er typisk foretrukket 

```typescript
let list1: number[] = [1, 2, 3];
let list2: Array<Number> = [1, 2, 3];
```
### Tuple 

Som i andre sprog - en tuple er en sammensat datatype:

```typescript
let a: [string, number] = ["a", 1];
let b = a[0].toUpperCase();
let c = a[1] + 1;
```
### Enum

Som i C# og andre sprog er en enum en relateret konstant.

```typescript
enum kortKulør {
  Ruder,
  Klør,
  Hjerter,
  Spar
}
enum kortFarve {
  Rød = 2,
  Sort = 4
}
let a: kortKulør = kortKulør.Spar;
let b: kortFarve = FindKortFarve(a);
let c: string = kortFarve[b]; // sort
function FindKortFarve(kulør: kortKulør): kortFarve {
  switch (kulør) {
    case kortKulør.Hjerter:
    case kortKulør.Ruder:
      return kortFarve.Rød;
    default:
      return kortFarve.Sort;
  }
}
```

### void

Bruges som en standard void-returværdi

```typescript
function test(): void {
  // kode
}
```

### null og undefined

Bruges som i ES - enten som typer eller som værdier til andre typer.

```typescript
let a: undefined;
let b: null;
let c: number = undefined;
let d: number = a;
let e: boolean = b;
```

### object

object-typen er den typiske ES type

```typescript
let a: object;
a = { a: 1, b: ""};
```

object-variabler kan ikke tildeles andet end objekter.

### union types

```typescript
let a: number | string | boolean;
a = 1;
a = "";
a = true;
// a = {}; // fejl
```

### string literals

Mulighed for at sikre, at en streng udelukkende har en eller flere konkrete værdier

```typescript
let a:  "x";
a = "x";
// a = "y";  // fejl

let b: "x"|"y";
b = "x";
b = "y";
// b = "z";  // fejl
```

### Type Aliases

string literals er ikke en type - det er en variabel. Så man kan ikke

```typescript
let a: "x" | "y";
// let b: a; fejl
```

men ved hjælp af type-kodeordet kan man

```typescript
type a = "x" | "y";
let b: a = "x";
// b = "z";  // fejl

type bin = 0 | 1;
let c: bin = 1;
```

## Destructoring 

```javascript
// array
const a: string[] = ["x", "y"];
const [vA, vB] = a;

const [vC, vD] = "x y".split(" ");

const [vE, vF, ...vG] = "a b c d e f g".split(" ");
// vE = "a", vF = "b", vG = ["c", "d", "e", "f", "g"]

// object
let b = { navn: "Mikkel", alder: 16 };
let { navn, alder } = b;

// tuple
let c: [string, number] = ["a", 1];
let [d, e] = c;
```

## Interface (duck typing)

Såvel duck typing som implementering i klasser

### Duck typing

```typescript
interface IPerson {
  navn: string;
  alder: number;
  byNavn?: string;
  readonly erDansk: boolean;
  gem?(): void;
}

let a: IPerson = { navn: "a", alder: 10, erDansk: true };
// a.erDansk = false;  // fejl
let b: IPerson = { navn: "a", alder: 10, erDansk: true, byNavn: "x" };
let c: IPerson = { navn: "a", alder: 10, erDansk: true, gem: function() {} };

function test(o: IPerson) {}
```
Kan også benyttes til at beskrive funktioner:

```typescript
interface IMinFunk {
  (a: string, b: boolean): boolean;
}
let a: IMinFunk = (a, b) => true;
```

### Implementering

```typescript
interface IGem {
  Gem(): void;
}

class Person implements IGem {
  constructor(public navn: string) {}
  Gem(): void {
    console.log("Gemmer person");
  }
}

class Maskine implements IGem {
  constructor(public id: number) {}
  Gem(): void {
    console.log("Gemmer maskine");
  }
}

let lst: IGem[] = [];
lst.push(new Person("z"));
lst.push(new Maskine(1));
lst.forEach(i => i.Gem());

```


## Class

class er et ES6 kodeord som ligger til grund for ES constructor functions, og er til for at gøre syntaks mere spiselig. TS tilføje desuden en del sukker.

### Access modifiers

Brug private, protected og public som C#/Java - men husk alt er public som default, og TS tilføjer blot type sikkerhed ved transpilering.

### Felter

Standard felter med brug af readonly.

```typescript
class A {
  public felt1: number;
  protected felt2: number;
  private felt3: number;
  public readonly felt4: number;
  public readonly felt5: number = 1;

  constructor() {
    this.felt5 = 1;
  }

  test(): void {
    this.felt1 = 1;
    this.felt1 = 2;
    this.felt1 = 3;
  }
}

class B extends A {
  test(): void {
    this.felt1 = 1;
    this.felt2 = 2;
  }
}

let a: A = new A();
a.felt1 = 1;
// a.felt4 = 2;
// a.felt5 = 3;
```

#### sukker constructor

Lidt specielt kan sukker bruges til oprettelse af felter

```typescript
class A {
  constructor(private a: number, protected b: number, public c: number, readonly d: number) {}
  test(): void {
    this.a = 1;
    this.b = 1;
    this.c = 1;
    // this.d = 1;
  }
}

let a: A = new A(1, 1, 1, 1);
a.c = 1;
// a.d = 3;
```

### Egenskaber

get/set egenskaber (>=ES5)

```typescript
class A {
  private _a: string;
  public get a(): string {
    return this._a;
  }
  public set a(v: string) {
    this._a = v;
  }

  // readonly
  private _b: string;
  public get b(): string {
    return this._b;
  }
}
```

### statiske medlemmer

```typescript
class A {
  public static a: string;

  private static _b: string;
  public static get b(): string {
    return this._b;
  }
  public static set b(v: string) {
    this._b = v;
  }

  public static test(): void {}
}

A.a = "";
A.b = "";
A.test();
```

### nedarvning

brug extends og super

```typescript
class A {
  public test(): void {
    console.log("*");
  }
}
class B extends A {}
class C extends A {}

let a: A = new A();
let b: B = new B();
let c: C = new C();
a.test();
b.test();
c.test();
```

### "virtuelle" medlemmer

ingen override

```typescript
class A {
  public test(): void {
    console.log("A");
  }
}
class B extends A {
  public test(): void {
    console.log("B");
  }
}
class C extends A {
  public test(): void {
    console.log("C");
  }
}

let a: A = new A();
let b: B = new B();
let c: C = new C();
a.test();
b.test();
c.test();
```

### constructor

constructor nedarves

```typescript
class A {
  constructor(public a: number) {
    console.log("A()");
  }
}
class B extends A {}

let b: B = new B(1);
// A()
```

brug af super ved constructor

```typescript
class A {
  constructor(public a: number) {
    console.log("A()");
  }
  test(): void {
    console.log("test()");
  }
}
class B extends A {
  constructor(a: number, public b: number) {
    super(a);
    super.test();
    console.log("B()");
  }
}

let b: B = new B(1, 1);
```

### abstract

```typescript
abstract class A {
  abstract test(): void;
}
class B extends A {
  test(): void {
    // kode
  }
}
```

## Funktioner

Funktioner tilføjer typer til både argumenter og til returværdier

```typescript
function add1(a: number, b: number): number {
  return a + b;
}

// ingen type def!
const add2 = function(a: number, b: number): number {
  return a + b;
};
// type def!
const add3: (a: number, b: number) => number = function(a: number, b: number): number {
  return a + b;
};

// ingen type def!
const add4 = (a: number, b: number) => a + b;
// type def!
const add5: (a: number, b: number) => number = (a: number, b: number) => a + b;
```

### Definering af type

Som variabel

```typescript
// Definering af mulig funktion 
let calc: (a: number, b: number) => number;
calc = (a, b) => a + b;
calc = (a, b) => a - b;
```

eller som interface

```typescript
interface ICalc {
  (a: number, b: number): number;
}

const add: ICalc = (a, b) => a + b;
const sub: ICalc = (a, b) => a - b;
```

### Optional argumenter

```typescript
function add(a: number, b?: number): number {
  if (b === undefined) {
    return a + 1;
  } else {
    return a + b;
  }
}

console.log(add(1));
console.log(add(1, 2));
```

### Default værdier

```typescript
function add(a: number, b: number = 1): number {
  return a + b;
}

console.log(add(1));
console.log(add(1, 2));
```

### rest

```typescript
const add = function(a: number, ...tal: number[]): number {
  if (tal === undefined) {
    return a + 1;
  } else {
    let sum: number = 0;
    tal.forEach(v => (sum += v));
    return sum + a;
  }
};

console.log(add(1));
console.log(add(1, 2));
console.log(add(1, 2, 3));
console.log(add(1, 2, 3, 4));
```

### Overloads

Der findes ikke overloads i ES men vi kan snyde (lidt)

```typescript
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: any, b: any): any {
  if (typeof a === "number") {
    return a + b;
  }
  if (typeof a === "string") {
    return a.toString() + b.toString();
  }
}
```

### Union argumenter

```typescript
  function print(a: number | string): void {}
  print(1);
  print("1");
  // print(true);  // fejl
```

Gælder også ved returværdier

### Literal types

En noget speciel feature er literal types

```typescript
function rollDice(): 1 | 2 | 3 | 4 | 5 | 6 {
  return 1;
}
```

## Generics

Generics er (endnu) ikke en del af ES, men TS stiller forskellige features til rådighed

```typescript
function test<T>(arg: T): T {
  return arg;
}

let a: number = test<number>(1);  // test(1)
let b: string = test<string>("1"); // test("1")
let c = test(true);
console.log(c);
```

### Constrains

Man kan eventuelt skabe constrains ved et interface

```typescript
interface IGemData {
  id: number;
}

// Her kan man sende hvad som helst med
function GemSimpel<T>(o: T): void {}

function Gem<T extends IGemData>(o: T): void {
  // nu er der et id på T
}

// Gem({});  // fejl
Gem({ id: 1 });
```

### Generiske klasser

```typescript
class Point<T> {
  x: T;
  y: T;

  print(z: T) {}
}

let p1: Point<number> = new Point<number>();
p1.x = 1;
p1.y = 2;
p1.print(3);

let p2: Point<string> = new Point<string>();
p2.x = "1";
p2.y = "2";
p2.print("3");
```

Der er samme muligheder for constraints som ved funktioner.

## Modules

Traditionelt har det været noget rod at modul opdele kode i ES, og der findes en del forskellige modul loadere

- CommonJS (node)
- AMD (requirejs)
- UMD
- System
- ES6, ES2015 or ESNext (ES)

Man kan sætte det ønskede format i tsconfig.json - så vil TS kompilere til den korrekt ES.

### Modul

Hver fil er et modul i sig selv. Alt er "privat" med mindre det eksporteret

```typescript
let a: number = 100;
export let b = 100;
export const add: (a: number, b: number) => number = (a: number, b: number) => a + b;
```

### Export

Man kan eksportere på flere måder

```typescript
export let a = 100;
export const add: (a: number, b: number) => number = (a: number, b: number) => a + b;
```

```typescript
let a = 100;
const add: (a: number, b: number) => number = (a: number, b: number) => a + b;

export { a, add };
```

```typescript
let a = 100;
const add: (a: number, b: number) => number = (a: number, b: number) => a + b;

export { a, add as myAdd };
```

Der kan kun være en default export (typisk stor klasse eller lign). Så bestemmer import instruktion hvad den skal hedde.

```typescript
export let a = 100;
const add: (a: number, b: number) => number = (a: number, b: number) => a + b;
export default add;
```

### import

Import af et eller flere moduler kan ligeledes ske på flere måder

```typescript
import { a, add } from "./module1";
console.log(a);
console.log(add(1, 1));
```

```typescript
import { a as b, add as myAdd } from "./module1";
console.log(b);
console.log(myAdd(1, 1));
```

```typescript
import * as mod from "./module1";
console.log(mod.a);
console.log(mod.add(1, 1));
```
