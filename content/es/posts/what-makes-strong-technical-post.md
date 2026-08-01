---
title: "Qué hace que una entrada técnica de blog sea sólida"
date: 2024-03-10
draft: true
tags: ["escritura", "carrera", "aprender-en-público"]
summary: "Una guía para escribir publicaciones técnicas que demuestren tu pensamiento, ayuden a otros y construyan tu marca profesional."
---

## Por qué esto importa

Cuando empecé a escribir publicaciones técnicas, pensaba que necesitaba explicar algoritmos complejos o construir algo revolucionario. Estaba equivocado.

Las mejores publicaciones técnicas no se tratan de presumir—se tratan de mostrar tu pensamiento. Los reclutadores y gerentes de contratación quieren ver:
- Cómo abordas los problemas
- Cómo aprendes de los fracasos
- Cómo comunicas conceptos técnicos
- Que puedes terminar las cosas

Esta publicación es una plantilla para escribir contenido técnico sólido, basada en lo que he aprendido de escribir, leer publicaciones de otros y recibir retroalimentación.

## El planteamiento del problema

**Empieza con el problema, no con la solución.**

Mal ejemplo:
> "Construí una app de React con Redux."

Buen ejemplo:
> "Estaba construyendo una función de carrito de compras y necesitaba compartir el estado del carrito entre múltiples componentes. El prop drilling se estaba complicando, así que necesitaba una mejor solución de gestión de estado."

### Por qué esto funciona

- Da contexto a tus decisiones
- Muestra que entiendes el "por qué" no solo el "cómo"
- Ayuda a los lectores a determinar si tu solución aplica a su problema
- Demuestra habilidades de resolución de problemas

### Plantilla

```markdown
## El problema

Estaba trabajando en [proyecto/funcionalidad] cuando encontré [problema específico].

El desafío era [describe la restricción o dificultad].

Necesitaba [lo que intentabas lograr].
```

## Proporciona contexto

No asumas que los lectores conocen tu configuración. Explica brevemente:
- Qué estabas construyendo
- Qué tecnologías estabas usando
- Qué restricciones tenías (tiempo, recursos, nivel de experiencia)

### Ejemplo

```markdown
## Contexto

**Proyecto:** Aplicación web de finanzas personales
**Stack tecnológico:** React, Node.js, PostgreSQL
**Mi experiencia:** Cómodo con lo básico de React, primera vez usando una base de datos
**Restricción:** Necesitaba desplegar gratis (presupuesto de estudiante)
```

Esto ayuda a los lectores a entender tus decisiones y si tu enfoque se adapta a su situación.

## Lo que intentaste

**Documenta tu proceso de exploración.**

Aquí es donde muchas publicaciones se quedan cortas. No solo muestres la solución final—muestra lo que intentaste primero. Esto es increíblemente valioso porque:

1. Muestra tu proceso de pensamiento
2. Ayuda a otros a evitar los mismos callejones sin salida
3. Demuestra persistencia
4. Es más realista que "lo construí perfectamente a la primera"

### Ejemplo

```markdown
## Intento 1: Estado local

Mi primer enfoque fue usar `useState` de React en el componente padre:

```javascript
function App() {
  const [cart, setCart] = useState([]);

  return (
    <div>
      <ProductList cart={cart} setCart={setCart} />
      <CartSummary cart={cart} />
      <Checkout cart={cart} setCart={setCart} />
    </div>
  );
}
```

**Qué pasó:** Esto funcionó inicialmente, pero a medida que agregué más funcionalidades, estaba pasando props a través de 3-4 niveles de componentes. El código se volvió difícil de mantener.

**Por qué falló:** El prop drilling hizo que el árbol de componentes fuera rígido. Agregar una nueva función que necesitaba acceso al carrito significaba actualizar múltiples componentes.
```

## Lo que falló (y por qué)

Sé honesto sobre los fracasos. Este es uno de los contenidos más valiosos que puedes escribir.

### Plantilla

```markdown
## Intento 2: [Nombre del enfoque]

[Breve descripción de lo que intentaste]

**Ejemplo de código:**
```[lenguaje]
[fragmento de código relevante]
```

**Qué pasó:** [Describe el resultado]

**Por qué no funcionó:** [Tu análisis de por qué este enfoque falló]

**Lo que aprendí:** [Conclusión clave de este fracaso]
```

### Ejemplo real

```markdown
## Intento 2: Context API

Cambié a React Context para evitar el prop drilling:

```javascript
const CartContext = createContext();

function CartProvider({ children }) {
  const [cart, setCart] = useState([]);

  return (
    <CartContext.Provider value={{ cart, setCart }}>
      {children}
    </CartContext.Provider>
  );
}
```

**Qué pasó:** Esto eliminó el prop drilling, pero noté problemas de rendimiento. Cada actualización del carrito re-renderizaba todos los componentes que consumían el contexto.

**Por qué no funcionó:** Context API re-renderiza todos los consumidores cuando cualquier valor cambia. Con más de 50 productos en la página, agregar un artículo al carrito causaba re-renderizados innecesarios.

**Lo que aprendí:** Context API es genial para datos que cambian poco frecuentemente (tema, autenticación), pero no es ideal para estado que se actualiza frecuentemente como un carrito de compras.
```

## Lo que funcionó

Ahora muestra tu solución. Pero no solo copies código—explica tu razonamiento.

### Estructura

1. **Por qué elegiste este enfoque**
2. **Cómo lo implementaste**
3. **Qué lo hace mejor que los intentos anteriores**
4. **Compromisos que aceptaste**

### Ejemplo

```markdown
## Solución: Redux Toolkit

Elegí Redux Toolkit porque:
- Maneja actualizaciones frecuentes de estado eficientemente
- Proporciona DevTools para depuración
- Escala bien a medida que la app crece
- Tiene excelente documentación para principiantes

**Implementación:**

```javascript
// cartSlice.js
import { createSlice } from '@reduxjs/toolkit';

const cartSlice = createSlice({
  name: 'cart',
  initialState: { items: [] },
  reducers: {
    addItem: (state, action) => {
      state.items.push(action.payload);
    },
    removeItem: (state, action) => {
      state.items = state.items.filter(item => item.id !== action.payload);
    },
  },
});

export const { addItem, removeItem } = cartSlice.actions;
export default cartSlice.reducer;
```

**Por qué esto funciona:**
- Los componentes solo se re-renderizan cuando sus datos específicos cambian
- La lógica de estado está centralizada y es testeable
- Redux DevTools me permite depurar cambios de estado fácilmente

**Compromisos:**
- Más código boilerplate que Context API
- Curva de aprendizaje más pronunciada
- Puede ser excesivo para apps muy simples

Para mi caso de uso (app en crecimiento con estado complejo), estos compromisos valieron la pena.
```

## Incluye elementos visuales

Incluso diagramas simples ayudan. No necesitas herramientas sofisticadas—incluso arte ASCII o capturas de pantalla funcionan.

### Ejemplos

**Arquitectura antes/después:**
```
ANTES (Prop Drilling):
App
├── ProductList (necesita cart, setCart)
│   └── Product (necesita cart, setCart)
│       └── AddToCartButton (necesita setCart)
└── CartSummary (necesita cart)

DESPUÉS (Redux):
App
├── ProductList
│   └── Product
│       └── AddToCartButton (useDispatch)
└── CartSummary (useSelector)
```

**Comparación de rendimiento:**
```
Context API: 50ms por actualización del carrito (50 re-renderizados de componentes)
Redux:       5ms por actualización del carrito (2 re-renderizados de componentes)
```

## Lecciones aprendidas

Termina con conclusiones concretas. ¿Qué harías diferente? ¿Qué consejo darías a alguien enfrentando un problema similar?

### Plantilla

```markdown
## Conclusiones clave

1. **[Lección 1]:** [Explicación]
2. **[Lección 2]:** [Explicación]
3. **[Lección 3]:** [Explicación]

## Si hiciera esto otra vez

- [Lo que harías diferente]
- [Lo que mantendrías igual]
- [Lo que explorarías después]
```

### Ejemplo

```markdown
## Conclusiones clave

1. **Empieza simple:** Debería haber mantenido el estado local más tiempo. Redux era excesivo hasta que tuve problemas reales de rendimiento.

2. **Mide antes de optimizar:** Asumí que Context API era lento antes de medirlo. Siempre haz profiling primero.

3. **Lee la documentación a fondo:** La documentación de Redux Toolkit tenía ejemplos que resolvían exactamente mi caso de uso. Perdí tiempo intentando resolverlo por mi cuenta primero.

4. **Los compromisos están bien:** Ninguna solución es perfecta. Entender y aceptar compromisos es parte de la ingeniería.

## Si hiciera esto otra vez

- Empezar con estado local
- Mover a Context API cuando el prop drilling se vuelva doloroso
- Solo agregar Redux cuando tenga problemas de rendimiento medidos
- Dedicar más tiempo a leer documentación antes de programar
```

## Plantilla que puedes copiar

Aquí tienes una plantilla para tu próxima publicación técnica:

```markdown
---
title: "[Título descriptivo]"
date: AAAA-MM-DD
tags: ["etiqueta1", "etiqueta2", "etiqueta3"]
summary: "Descripción en una oración de lo que cubre esta publicación."
---

## El problema

[¿Qué problema intentabas resolver?]

## Contexto

- **Proyecto:** [Qué estabas construyendo]
- **Stack tecnológico:** [Tecnologías usadas]
- **Nivel de experiencia:** [Tu experiencia con estas herramientas]
- **Restricciones:** [Tiempo, presupuesto u otras limitaciones]

## Lo que intenté

### Intento 1: [Nombre del enfoque]

[Descripción y código]

**Qué pasó:** [Resultado]
**Por qué no funcionó:** [Análisis]
**Lo que aprendí:** [Conclusión]

### Intento 2: [Otro enfoque]

[Repetir estructura]

## Lo que funcionó

[Tu solución con explicación]

**Por qué funciona:** [Beneficios]
**Compromisos:** [Desventajas que aceptaste]

## Resultados

[Métricas, resultados o impacto]

## Conclusiones clave

1. [Lección 1]
2. [Lección 2]
3. [Lección 3]

## Recursos

- [Enlace al código]
- [Documentación útil]
- [Artículos relacionados]
```

## Reflexiones finales

Las publicaciones técnicas sólidas no requieren conocimiento de nivel experto. Requieren:
- Honestidad sobre tu proceso
- Explicación clara de tu pensamiento
- Disposición para mostrar fracasos
- Ejemplos concretos y código

Tus proyectos "pequeños" y problemas "simples" son valiosos para escribir sobre ellos. Alguien más está enfrentando el mismo problema ahora mismo, y tu publicación podría ayudarle.

Empieza a escribir. Tu yo del futuro (y tus futuros lectores) te lo agradecerán.

---

**Recursos:**
- [Repositorio de ejemplo en GitHub con código de esta publicación](https://github.com/yourusername/example-repo)
- [Documentación de Redux Toolkit](https://redux-toolkit.js.org/)
- [Documentación de React Context API](https://react.dev/reference/react/useContext)
