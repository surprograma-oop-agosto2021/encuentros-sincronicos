## Clase 3

## Cualidades de diseño

![Logo](img/logo.png)

===

## TDD

¿Cómo les resultó? ¿Pudieron seguir la práctica?

===

## Errores comunes

--

### Dividir en subtareas

Cada método tiene que hacer _una sola_ cosa. Lo trabajaremos asociado a la cualidad **cohesión**.

--

### Mucho `forEach`

Intenten no usarlo en absoluto. Y si no sale, me preguntan.

--

### Factor de compresión

El enunciado pedía poder modificar _para todas a la vez_. Eso se logra con un **atributo de clase**:

```ts [2,10,15]
export class Foto extends Publicacion {
  static factorDeCompresion = 1;

  constructor(public alto: number, public ancho: number) {
    super();
  }

  override espacioQueOcupa(): number {
    return Math.ceil(
      this.alto * this.ancho * Foto.factorDeCompresion
    );
  }
}

Foto.factorDeCompresion = 0.7
```

--

### Ejemplo

```ts
const fotoEnCuzco = Foto(100, 20);

expect(fotoEnCuzco.espacioQueOcupa()).toBe(2000);

Foto.factorDeCompresion = 0.7;
expect(fotoEnCuzco.espacioQueOcupa()).toBe(1400);
```

--

### `beforeEach` en los tests

Acá me equivoqué yo. La idea es que los objetos se reinicien entre tests, para favorecer la independencia.

```ts
describe('Caralibro', () => {
  let saludoCumpleanios, fotoEnCuzco;

  beforeEach(() => {
    saludoCumpleanios = new Texto(
      'Felicidades Pepito, que los cumplas muy feliz',
    );
    fotoEnCuzco = new Foto(768, 1024);
  });
});
```

Esto aplica **solamente** para objetos compartidos entre tests.

--

### Atributos que se inicializan más tarde

TS va a chillar si un atributo puede ser nulo a la hora de usarlo.

Podemos pedirle explícitamente que no lo haga:

```ts
class Publicacion {
  constructor(public permiso: Permiso);

  // El ! significa: yo me hago cargo de que no sea nulo 😛
  autor!: Usuario;
}

foto.texto?.length ?? 0;
```

===

## Modelar estrategias

¿Qué problemas tiene esta solución?

```ts
enum Permiso {
  SOLO_AMIGOS,
  PRIVADO_PERMITIDOS,
  PUBLICO,
  PUBLICO_EXCLUIDOS,
}
```

--

```ts [3,7,10,13]
puedeVerLaPublicacion(publicacion: Publicacion): boolean {
  if (publicacion.consultarDuenio() === this) return true;
  if (publicacion.tipoDePermiso === Permiso.SOLO_AMIGOS) {
    const duenio = publicacion.consultarDuenio();
    return includes(this, duenio.amigos);
  }
  if (publicacion.tipoDePermiso === Permiso.PRIVADO_PERMITIDOS) {
    return includes(this, publicacion.listaDePermitidos);
  }
  if (publicacion.tipoDePermiso === Permiso.PUBLICO_EXCLUIDOS) {
    return !includes(this, publicacion.listaDeExcluidos);
  }
  return true; // si es publico
}
```

--

❌ Utiliza mucha información que está en la `Publicacion`.

❌ Obliga a tener atributos que no se van a usar en todos los casos.

❌ Aún teniendo pocas estrategias, el código se vuelve rápidamente complejo de leer.

--

Primer _refactor_, movemos el método a la `Publicacion`:

```ts
puedeSerVistaPor(persona: Usuario): boolean {
  if (persona === this.autor) return true;
  if (this.tipoDePermiso === Permiso.SOLO_AMIGOS) {
    return includes(persona, this.autor.amigos);
  }
  if (this.tipoDePermiso === Permiso.PRIVADO_PERMITIDOS) {
    return includes(persona, this.listaDePermitidos);
  }
  if (this.tipoDePermiso === Permiso.PUBLICO_EXCLUIDOS) {
    return !includes(persona, this.listaDeExcluidos);
  }
  return true; // si es publico
}
```

--

Ahora, modelamos al `Permiso` como una clase:

```ts
puedeSerVistaPor(persona: Usuario): boolean {
  if (persona === this.autor) return true;
  return this.permiso.puedeVer(this, persona);
}

interface Permiso {
  puedeVer(publicacion: Publicacion, persona: Usuario): boolean
}
```

👀 **Ojo:** le pasamos a la `Publicacion` y al `Usuario`, para que no sea engorrosa la creación.

--

```ts
class Publico implements Permiso {
  puedeVer(publicacion: Publicacion, persona: Usuario) {
    return true;
  }
}

class SoloAmigos implements Permiso {
  puedeVer(publicacion: Publicacion, persona: Usuario) {
    return publicacion.autor.esAmigoDe(persona);
    // return includes(persona, publicacion.autor.amigos)
  }
}

class PublicoConExcluidos implements Permiso {}

class PrivadoConPermitidos implements Permiso {}
```

Y así le damos la bienvenida al primer patrón: **Strategy**.

<!-- .element: class="fragment" -->

--

O, ya que no hay estado, podríamos usar objetos:

```ts
const publico: Permiso = {
  puedeVer(publicacion: Publicacion, persona: Usuario) {
    return true;
  },
};

const soloAmigos: Permiso = {
  puedeVer(publicacion: Publicacion, persona: Usuario) {
    return publicacion.autor.esAmigoDe(persona);
  },
};
```

--

|                               | if                                | Strategy                |
| ----------------------------- | --------------------------------- | ----------------------- |
| _Agregar o quitar estrategia_ | Buscar todos los `if` y modificar | Crear una clase nueva   |
| _¿Opciones no existentes?_    | Validar (o caer en el `else`)     | No es posible           |
| _Configuraciones / atributos_ | Dispersos por varios lugares      | Concentrado en un lugar |

===

## Cualidades de diseño

Una posible formalización de aspectos que _nos hacen ruido_.

--

Nos sirven para:

- evaluar nuestros propios diseños,
- comparar diseños,
- comunicarnos con otras personas,
- tomar decisiones,
- justificar las decisiones que tomamos.

--

### Tensiones entre las cualidades

![Tensiones](img/clase-3/soga.gif)

En muchos casos deberemos **priorizar** aquellas que nos interesan, aún cuando eso implique descuidar otras.

===

## Actividades de la semana

### Refactorizar Caralibro

Utilizar **Strategy** para los permisos y calidades de video. Lo suben al mismo repo que venían usando.

### Encontrar defectos Semillas

Detectar defectos de diseño, testear y refactorizar. En **ese** orden.

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
