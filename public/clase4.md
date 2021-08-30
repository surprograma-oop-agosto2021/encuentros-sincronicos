## Clase 4

## Patrón Composite

![Logo](img/logo.png)

===

## Strategy

¿Les resultó complejo de aplicar?

--

### ¿Y el `if` ya pasó de moda?

**NO.** Siempre hay que tener la simplicidad como bandera.

--

### Preguntas para orientar la decisión

- **¿La estrategia puede cambiar?** Si no, por ahí alcanza con subclases.
<!-- .element: class="fragment" -->

- **¿Hay solo dos opciones?** Se puede arrancar con un `if` y después cambiar.
<!-- .element: class="fragment" -->

- **¿Las estrategias tienen configuraciones propias?** Tal vez sea mejor usar _Strategy_ desde el principio.
<!-- .element: class="fragment" -->

- **¿Es altamente probable que aparezcan nuevas estrategias?** Tal vez sea mejor usar _Strategy_ desde el principio.
<!-- .element: class="fragment" -->

===

## Objetos sin clase

¿Qué sentido tiene?

--

### ¿Cómo se hace?

--

### ¿En qué casos lo usaría?

- Si no hay estado.
- Si quiero compartir ese estado en todas partes.

--

### Cómo quedaría el Strategy

===

## Otros comentarios sobre el ejercicio

--

### Una pavada

> para los videos de HD 1080p el tamaño es el doble de los HD 720p + 300 bytes fijos

Se puede escribir así, para no repetir lo que ya está en la otra clase:

```ts
class HD1080p extends HD720p {
  espacioQueOcupa(duracion: number): number {
    return super(duracion) * 2 + 300;
  }
}
```

--

### Una sutileza

```ts
class HD720p implements CalidadVideo {
  espacioQueOcupa(duracion: number): number {
    return duracion * 3;
  }
}
```

¿Cuál elegirían y por qué?

```ts
class HD720p implements CalidadVideo {
  espacioQueOcupa(video: Video): number {
    return video.duracion * 3;
  }
}
```

--

### Setters y getters

No hacen falta.

===

## Cualidades de diseño

¿Las leyeron? ¿Quieren comentar alguna?

===

## Actividades de la semana

### Encontrar defectos Semillas

Detectar defectos de diseño, testear y refactorizar. En **ese** orden.

### Ejercicio Tareas

Apliquen la misma idea que en el **Strategy**: en vez de hacer un `if`, creo otra clase.

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
