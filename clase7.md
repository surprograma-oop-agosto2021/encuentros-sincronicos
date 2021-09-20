## Clase 7

## Comunicación entre componentes

![Logo](img/logo.png)

===

## Servidor web

¿Comentarios?

--

### Cómo buscar el más consultado

¿Cuál es la edad más común?

```ts
const amigos = [
  { nombre: 'Juana', edad: 29 },
  { nombre: 'Pepe', edad: 29 },
  { nombre: 'Oso', edad: 35 },
];
```

--

Cómo lo haría yo:

```ts
compose(
  head,
  last,
  sortBy(nth(1)),
  toPairs,
  map(length),
  groupBy((it) => it.edad),
)(amigos);
```

[Probando en la consola de Ramda](https://ramdajs.com/repl/?v=0.27.0#?const%20amigos%20%3D%20%5B%20%0A%20%20%7B%20nombre%3A%20%27Juana%27%2C%20edad%3A%2029%20%7D%2C%20%0A%20%20%7B%20nombre%3A%20%27Pepe%27%2C%20edad%3A%2029%20%7D%2C%20%0A%20%20%7B%20nombre%3A%20%27Oso%27%2C%20edad%3A%2035%20%7D%0A%5D%0A%0Acompose%28%0A%20%20%2F%2F%20head%2C%0A%20%20%2F%2F%20last%2C%0A%20%20%2F%2F%20sortBy%28nth%281%29%29%2C%0A%20%20%2F%2F%20toPairs%2C%0A%20%20%2F%2F%20map%28length%29%2C%0A%20%20groupBy%28it%20%3D%3E%20it.edad%29%0A%29%28amigos%29)

--

Usando lo que encontró JM:

```ts [1|2|3]
// `mode` recibe una lista y devuelve el elemento que más se repite
const listaDeEdades = map((it) => it.edad, amigos);
mode(listaDeEdades);
```

Moraleja: no es necesario entender _cómo_ lo resuelve, sino _qué_ resuelve y cómo puedo incorporarlo en mi solución.

<!-- .element: class="fragment" -->

--

### Null object

Una manera diferente de tratar a los casos donde "no hay valor".

```ts
const moduloNulo = {
  procesar(pedido: PedidoHttp): RespuestaHttp {
    return new Respuesta(CodigoHttp.NOT_IMPLEMENTED, '', 10);
  },
};
```

--

```ts
const servidorWeb = {
  modulos: [] as Modulo[],

  obtenerRespuesta(pedido: PedidoHttp): RespuestaHttp {
    if (!pedido.esHttp()) {
      return new RespuestaHttp(...)
    }
    return this.moduloPara(pedido).procesar(pedido);
  },

  moduloPara(pedido: PedidoHttp): Modulo {
    return find(it => it.puedeProcesar(pedido), this.modulos)
      ?? moduloNulo;
  }
}
```

===

## Comunicación entre componentes

Hasta ahora veníamos modelando mecanismos de comunicación **sincrónica**: A interactúa con B enviándole un mensaje, que B responde.

```plantuml
!$BGCOLOR = "transparent"
!theme plain
Vuelo -> Avion: capacidadMaxima()
Avion --> Vuelo: 180
```

--

En algunos casos, es necesario que esa comunicación sea **asincrónica**. Motivos usuales:

- El procesamiento tarda.
- No importa la respuesta.
- Depende de un evento que no se sabe cuándo va a ocurrir.

--

Un mecanismo posible, conocido como **poll**:

```plantuml
!$BGCOLOR = "transparent"
!theme plain
Analizador -> Servidor: queHayDeNuevo()
Servidor --> Analizador: ❌ nada
Analizador -> Servidor: queHayDeNuevo()
Servidor --> Analizador: ✅ procesé 3 pedidos: ...
Analizador -> Servidor: queHayDeNuevo()
Servidor --> Analizador: ❌ nada
Analizador -> Servidor: queHayDeNuevo()
Servidor --> Analizador: ❌ nada
```

<small>Las desventajas son evidentes...</small>

--

En cambio, si le pedimos que "avise" cuando un evento ocurre:

```plantuml
!$BGCOLOR = "transparent"
!theme plain
Servidor -> Analizador: meLlegoUnPedido(pedido)
Analizador --> Servidor: 😃 ¡Gracias!
```

En objetos, a esto se lo conoce como patrón **Observer**. También puede que lo escuchen como _hook_, _webhook_ o similar.

<!-- .element: class="fragment" -->

--

La idea es siempre la misma:

1. Un componente o sistema **produce** eventos.
1. Otros componentes o sistemas están interesados en **consumir** esos eventos.
1. Los consumidores se registran en el productor.
1. El productor le avisa a todos los consumidores cuando algo pasa.

--

Otro mecanismo posible es comunicarse mediante **memoria compartida** (o _shared memory_): los componentes no se comunican entre sí directamente, lo hacen a través de algún intermediario.

--

```plantuml
!$BGCOLOR = "transparent"
!theme plain
Analizador -> ColaMensajes: queHayDeNuevo()
ColaMensajes --> Analizador: ❌ nada
Servidor -> ColaMensajes: meLlegoUnPedido(pedido)
ColaMensajes --> Servidor: 😃 ¡Gracias!
Analizador -> ColaMensajes: queHayDeNuevo()
ColaMensajes --> Analizador: ✅ Llegó 1 pedido
```

Y esto se puede seguir complejizando hasta el infinito. 😅

<!-- .element: class="fragment" -->

===

## Cómo probar dependencias externas

Lo vemos en el VScodium.

===

## Actividades de la semana

### Ejercicio Tareas

Agregarle a lo que ya tenían una interfaz por línea de comandos (CLI).

El objetivo es leer proyectos en formatos JSON y mostrar algunos datos sobre ellos.

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
