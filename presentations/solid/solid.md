---
marp: true
author: Renato Kenji Aguena
theme: gaia
class: lead
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
style: |
  section.split div {
    margin: -50px 10px;
  }
  section.split {
    overflow: visible;
    display: grid;
    grid-template-columns: 50% 50%;
  }
  h3 {
    text-align: center;
  }
  h3.wrong {
    color: red;
  }
  h3.correct {
    color: lightgreen;
  }
---

# SOLID

---

<!-- São princípios utilizados na programação para criarmos códigos melhores (mantiveis e fácil de entender) -->

## O que é?

###### Princípios
###### Boas práticas

---

<!-- Acessibilidade: acesso e controle facilidade sobre os nossos objetos/entidades, evitando risco de mexer em objetos/entidades desnecessários para determinada modificação/adição -->
<!-- Refatoração: seguindo as boas práticas conseguimos criar um codebase com fraco acoplamento e fácil entedimento, facilidade quando precisamos fazer alguma refatoração -->
<!-- Extensível: não corremos risco de alterar funções que já estão funcionando, pois não estamos alterando funções já existente, mas utilizando elas como base para criamos novas funções/comportamentos -->
<!-- Debug: facilita o debug por conta do fraco acoplamento entre as funções, não sendo necessário debugar sempre a "request" inteira, mas somente uma parte onde esteja o problema de fato -->
<!-- Legibilidade: passamos a maior parte do tempo lendo e entendendo nossos código, ter um código legível e de rápido entendimento ajuda no tempo de entrega -->

## Vantagens

###### Acessibilidade
###### Refatoração
###### Extensível
###### Debug
###### Legibilidade

---

<!--
  Uma classe deve ter um, e somente um, motivo para mudar.
  Seja ela fazer uma requisição, cuidar de estados ou comunicar com terceiros, por exemplo.
-->

## **S**ingle Responsibility Principle
### Princípio da responsabilidade única

---
<!-- _class: split -->

<div>

<h3 class="wrong"> ✘ </h3>

```javascript
class Car {
  constructor (name, model, year) {
    this.name = name
    this.model = model
    this.year = year
  }

  getCar(id) {
    return this.http.get('api/cars/' + id)
  }

  saveCar() {
    return this.post('api/cars', {
      name: this.name,
      year: this.year,
      model: this.model
    })
  }
}
```

</div>

<div>

<h3 class="correct"> ✔ </h3>

```javascript
class Car {
  constructor (id, name, model, year) {
    this.id = id
    this.name = name
    this.model = model
    this.year = year
  }
}

class CarService {
  constructor (car) {
    this.car = car
  }

  getCar () {
    return this.http.get('api/cars/' + this.car.id)
  }
  saveCar () {
    this.http.post('api/cars', this.car)
  }
}
```

</div>

---

<!--
  Objetos ou entidades devem estar abertos para extensão, mas fechados para modificação
  Em outras palavras, quando precisamos adicionar novas comportamentos ou funções  em nossa aplicação, devemos extender nossas funções e não alterar elas
-->

## **O**pen-Closed Principle
### Princípio Aberto-Fechado

---
<!-- _class: split -->

<div>

```javascript
class Rectangle {
  constructor (length, width) {
    this.length = length
    this.width = width
  }

  get area () {
    return this.length * this.width
  }
}
```

</div>

<div>

```javascript
class Rectangle {
  constructor (length, width) {
    this.length = length
    this.width = width
  }

  get area () {
    return this.length * this.width
  }

+ get perimeter () {
+   return 2 * (this.length + this.width)
+ }
}
```

</div>

---
<!--
  Uma classe derivada deve ser substituível por sua classe base.
  A ideia aqui é que consigamos utilizar qualquer entidade como parâmetro da função, dado que essa entidade seja a nossa classe pai ou suas filhas. No exemplo fica mais claro
-->
## **L**iskov Substitution Principle
### Princípio da substituição de Liskov

---
<!-- _class: split -->

<div>

```javascript
class Shape {
  get area () {
    return 0
  }
}

class Square extends Shape {
  constructor (length) {
    super()
    this.length = length
  }
  get area () {
    return this.length ** 2
  }
}

class Circle extends Shape {
  constructor (radius) {
    super()
    this.radius = radius
  }
  get area () {
    return Math.PI * (this.radius ** 2)
  }
}
```

</div>

<div>

```javascript
const shapes = [
  new Square(2),
  new Circle(2),
]
for (let s of shapes) {
  console.log(s.area)
}
```

</div>

---

<!--
  Uma classe não deve ser forçada a implementar interfaces e métodos que não irão utilizar
  A idéia é bem simples, é melhor utilizarmos interfaces específicas para cada caso nosso (quando são muito parecidas podemos utilizar herança) do que utilizarmos uma classe genérica para tudo.
  Como em JavaScript não temos a funcionalidade de criar interfaces, essa regra não é possível aplicar a ele, mas como utilizamos bastante TypeScript e ele suporta a criação de interfaces é interessante aplicarmos nesses serviços de TypeScript.
-->

## **I**nterface Segregation Principle
### Princípio da Segregação da Interface

---
<!-- _class: split -->

<div>

<h3 class="wrong"> ✘ </h3>

```typescript
interface IVehicle {
  getSpeed(): number
  getVehicleType: string
  isTaxPayed(): boolean
  isLightsOn(): boolean
  isLightsOff(): boolean
  startEngine(): void
  acelerate(): number
  stopEngine(): void
  startRadio(): void
  playCd(): void
  stopRadio(): void
}
```

</div>

<div>

<h3 class="correct"> ✔ </h3>

```typescript

interface ILights {
  isLightsOn(): boolean
  isLightsOff(): boolean
}

interface IRadio {
  startRadio(): void
  playCd(): void
  stopRadio(): void
}

interface IEngine {
  startEngine(): void
  acelerate(): number
  stopEngine(): void
}

interface IVehicle extends ILights, IRadio, IEngine {
  getSpeed(): number
  getVehicleType: string
  isTaxPayed(): boolean
}
```

</div>

---

<!--
  Dependa de abstrações e não de implementações.
  A ideia desse princípio é que as classes de mais alto nível não dependem de coisas de baixo nível.
  Ficar passando uma conexão com o banco de dados, bibliotecas e etc. para a nossa classe de baixo nível não é legal, pois dessa forma a nossa classe de alto nível acaba ficando depende da classe de baixo nível, gerando um forte acoplamento
-->
## **D**ependency Inversion Principle
### Princípio da inversão da dependência

---
<!-- _class: split -->

<div>

```javascript
class ClassA {
  show () {
    console.log('I\'m class A')
  }
}

class ClassB {
  print () {
    console.log('I\'m class B')
  }
}

class ClassC {
  log () {
    console.log('I\'m class C')
  }
}
```

</div>

<div>

```javascript
class Facade {
  constructor () {
    this.a = new ClassA()
    this.b = new ClassB()
    this.c = new ClassC()
  }
}

class Foo {
  constructor () {
    this.facade = new Facade()
  }
}
```

</div>

<!--

---

# Referências

https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa

https://levelup.gitconnected.com/javascript-clean-code-solid-9d135f824180

https://levelup.gitconnected.com/javascript-clean-code-solid-9d135f824180

https://www.venturus.org.br/o-que-e-solid/

https://medium.com/desenvolvendo-com-paixao/o-que-%C3%A9-solid-o-guia-completo-para-voc%C3%AA-entender-os-5-princ%C3%ADpios-da-poo-2b937b3fc530

https://hub.packtpub.com/object-oriented-programming-typescript/#:~:text=Interface%20segregation%20principle%20(ISP)%3A,are%20of%20interest%20to%20them.

https://www.section.io/engineering-education/introduction-to-solid-principle/

-->
