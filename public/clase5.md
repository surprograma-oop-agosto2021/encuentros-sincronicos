## Clase 5

## Trabajo en equipo

![Logo](img/logo.png)

===

## Semillas al viento

Repasemos las cualidades y dónde estaban en el ejercicio.

--

### Simplicidad

_No hacer cosas de más._

- `Agricultora.comprarParcela` -> no está en el enunciado, es un invento.

--

### Robustez

_¿Cómo se comporta el sistema ante un error?_

- `Parcela.plantar` -> reporta los errores usando `println` en vez de arrojar una excepción.

--

### Redundancia mínima

_El conocimiento debe estar en un solo lugar._

- `Parcela.cantidadMaximaPlantas` -> Repite el código que ya está implementando en el método `superficie`.
- `Parcela.cantidadPlantas` -> Se puede calcular a partir del `size` de la lista de plantas, no es necesario tener otra variable.
- `Planta.daSemillas` -> La primera parte (`this.esFuerte()`) está repetida en todas las subclases.

--

### Acoplamiento

_¿Cuánto conoce un componente sobre otro?_

- `Agricultora.parcelasSemilleras` -> el cálculo para saber si `esSemillera` debería estar en la `Parcela`.
- `Agricultora.plantarEstrategicamente` -> no usa el método `plantar` de la `Parcela`, agregando la planta directamente y saltándose los chequeos que la parcela tiene.

--

### Cohesión

_¿Cuántas responsabilidades tiene un componente?_

- `Soja transgénica` -> con una sola clase se está queriendo representar a dos tipos de `Soja`, usando un booleano para diferenciarlas.
- `Planta.parcelaTieneComplicaciones` -> este método no tiene que ver con las responsabilidades de la `Planta`, debería estar en la `Parcela`.
- `Agricultora.plantarEstrategicamente` -> este método hace dos cosas: busca la parcela y luego la planta. Se podría delegar la primera en otro método.

--

### Mutaciones controladas

_Reducir al mínimo posible el efecto._

- `Planta.altura` -> debería ser `const`, porque en el enunciado dice que no va a cambiar.
- `Agricultora.parcelas` -> debería ser inmutable, porque en el enunciado dice que no se pueden comprar ni vender.

> No se puede lograr fácilmente en JS. Para lo segundo, existen [Immutable](https://immutable-js.com/) o [List](https://github.com/funkia/list).

===

## Composite

¿Les resultó complejo de aplicar?

--

### Sistema de tipos

- Maruja es una `Gallina`.
- Maruja es un `Ave`.
- Maruja es un `Animal`.

Depende el contexto, puedo usar uno u otro tipo.

--

Para hacer un **Composite**, tengo que pensar sí o sí en qué interfaz comparten mis elementos.

En TypeScript, además, tengo que crear un tipo común (puede ser `interface` o clase abstracta). Lo más común es definir métodos para esto.

===

## Manejo de fechas

¿Cuántos días pasaron desde la vuelta de la democracia en Argentina hasta hoy?

--

Usando `Date`, nativo de JS:

```ts
const vueltaDemocraciaArgentina = new Date(1983, 10, 30);
const ahora = new Date();

const milisegundos = ahora - vueltaDemocraciaArgentina;

// /1000 -> segundos -> /60 -> minutos
// -> /60 -> horas -> /24 -> días
milisegundos / 1000 / 60 / 60 / 24;

// 13795.663390914351
```

--

Usando la biblioteca `luxon`:

```ts
import { DateTime } from 'luxon';
const vueltaDemocraciaArgentina = DateTime.local(1983, 10, 30);
const ahora = DateTime.now();

ahora.diff(vueltaDemocraciaArgentina).as('days');
// 13826.666025925926
```

===

## Trabajo en equipo

- Uso de Git.
- División de tareas.
- Discusiones de diseño.
- Consistencia (necesita de acuerdos previos).
- _Pair programming_.

--

Normalmente el trabajo en equipo es **asincrónico**, aunque también son útiles las instancias **sincrónicas**.

Por ejemplo: en un comienzo, cuando es difícil dividir tareas; para ayudar a destrabar; para hacer una revisión...

--

Les voy a dejar dos videos con ejemplos.

===

## Actividades de la semana

### Ejercicio Ventas aéreas

No hay ningún patrón ni truco nuevo, la complejidad está principalmente en que hay muchos objetos para modelar.

Hagan **hasta el requerimiento 5** en equipo. Para el resto dividansé y asignen responsable.

===

# ¿Preguntas?

<div class="red-social">
  <i class="fab fa-youtube color"></i>
  <span><a href="https://youtube.com/c/elsurtambienprograma">El Sur también programa</a></span>
</div>
<div class="red-social">
  <i class="fab fa-telegram-plane color"></i>
  <span><a href="https://t.me/surprograma">@surprograma<a></span>
</div>
<div class="red-social">
  <i class="fab fa-instagram color"></i>
  <span><a href="https://instagr.am/surprograma">@surprograma<a></span>
</div>

<img width="100px" src="img/logo.png">
