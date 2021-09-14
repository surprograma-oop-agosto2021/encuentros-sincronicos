## Clase 6

## Dependencias con sistemas externos

![Logo](img/logo.png)

===

## Ventas aéreas

¿Cómo les fue con Git? ¿Les costó ponerse de acuerdo?

--

### Factory method

Un patrón interesante, aunque no muy útil en este caso.

¿Qué querían lograr al incluirlo?

--

```ts [13]
export class CrearVueloCharter extends CreadorDeVuelos {
  crearVuelo(
    empresa: Empresa,
    fecha: DateTime,
    avion: Avion,
    // ...
  ): Vuelo {
    const nuevoVuelo = new VueloCharter(
      fecha,
      avion,
      // ...
    );
    empresa.vuelosALaVenta.push(nuevoVuelo);
    return nuevoVuelo;
  }
}
```

--

### Realidad VS Modelo

_¿A qué objeto le corresponde una responsabilidad?_

Hacer una analogía con el mundo real suele ser un error, porque estamos construyendo un **modelo** que nos resulta útil para nuestro contexto.

===

## Integración con sistemas externos

¿Cuánto tengo que conocer de esos sistemas?

--

¿Qué certeza tengo sobre su funcionamiento?

--

¿Qué aspectos puedo controlar?

--

¿Cómo hago pruebas sobre ese sistema? ¿Debería hacerlas?

===

## Asincronismo en TS

Ya hace algunos años se impusieron las `Promise`s como método para manejar código asincrónico.

A diferencia de otros lenguajes, JS hace explícita esta diferencia.

--

### Await (uso)

```ts
// Utilizo `await` para "esperar" el resultado.
const datos = await fetch('https://api.github.com/user/faloi');
console.log(
  `El nombre real de @faloi 
  es ${datos.name.first} ${datos.name.last}.`,
);
```

--

### Async (definición)

```ts
class Usuario {
  constructor(
    public nick: string,
    public biografia: string,
    public seguidores: number,
  ) {}
}

// 1. Explicito que mi función es asincrónica.
// 2. Al tipo que devuelve lo envuelvo dentro de Promise<>.
async function datosDelPerfil(usuario: string): Promise<Usuario> {
  const datos = await fetch(`https://api.github.com/user/${usuario}`);
  return new Perfil(datos.username, datos.bio, datos.followers);
}
```

--

Lo asincrónico se propaga: si mi función usa algo asincrónico, se vuelve asincrónica.

--

Esto también vale para los tests, que quedan prácticamente iguales:

```diff
- it('the data is peanut butter', () => {
+ it('the data is peanut butter', async () => {
    const data = await fetchData();
    expect(data).toBe('peanut butter');
  });
```

===

## Actividades de la semana

### Ejercicio Servidor web

Va a surgir la necesidad de utilizar un patrón que no charlamos hasta ahora. No se desesperen por encontrarlo, hagan la solución como les parezca.

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
