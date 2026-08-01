---
title: "Cómo escribir sobre un proyecto (aunque sea pequeño)"
date: 2024-03-15
draft: true
tags: ["carrera", "portafolio", "escritura"]
summary: "Una guía práctica para documentar tus proyectos de manera que demuestre habilidades y atraiga oportunidades, sin importar lo simple que sea el proyecto."
---

## El problema de la inseguridad

He hablado con docenas de estudiantes que dicen: "No tengo nada que valga la pena escribir."

Han construido:
- Una aplicación de lista de tareas
- Un panel meteorológico
- Un juego simple
- Una visualización de datos
- Un bot de Discord

Pero no escriben sobre ellos porque "todos han hecho eso" o "es muy básico."

La verdad es: La complejidad del proyecto no importa. Lo que importa es cómo hablas de él.

Un proyecto "simple" bien documentado supera a un proyecto "complejo" sin documentar cada vez.

## ¿Por qué escribir sobre proyectos pequeños?

### 1. Demuestra habilidades de comunicación
Escribir claramente sobre trabajo técnico es una habilidad fundamental. La mayoría de los desarrolladores pueden programar. Menos pueden explicar su código.

### 2. Muestra tu proceso
Los reclutadores quieren ver cómo piensas, no solo qué construiste. Un proyecto simple con razonamiento claro es más impresionante que un proyecto complejo sin explicación.

### 3. Construye tu portafolio
Necesitas contenido. Tres proyectos pequeños bien documentados superan a cero proyectos grandes documentados.

### 4. Te ayuda a aprender
Escribir te obliga a entender qué hiciste y por qué. Es una herramienta de aprendizaje, no solo una vitrina.

### 5. SEO y descubrimiento
Cuando los reclutadores te buscan en Google, encuentran tu trabajo. Cuando otros estudiantes buscan soluciones, encuentran tus publicaciones. Ambos son valiosos.

## El framework: Antes/Después

La forma más simple de enmarcar cualquier proyecto es: "Antes de esto, X era un problema. Después de esto, X está resuelto."

### Ejemplo: App de lista de tareas

**Enfoque débil:**
> "Construí una app de lista de tareas con React."

**Enfoque fuerte:**
> "Estaba usando notas en papel y perdiendo el seguimiento de tareas. Construí una app web para organizar mis tareas por prioridad y fecha de vencimiento. Ahora tengo un solo lugar para gestionar todo, y aprendí React en el proceso."

¿Ves la diferencia? La segunda versión:
- Explica la motivación (problema real)
- Describe la solución (funcionalidades específicas)
- Muestra el resultado (problema resuelto + habilidad aprendida)

## Plantilla de documentación de proyectos

Aquí tienes una estructura que funciona para cualquier proyecto, grande o pequeño:

### 1. Descripción general (2-3 oraciones)

¿Qué es? ¿Qué hace? ¿Por qué lo construiste?

```markdown
## Descripción general

Una aplicación web de lista de tareas que me ayuda a organizar tareas por prioridad y fecha de vencimiento. Construida para resolver mi propio problema de perder el seguimiento de tareas y para aprender los fundamentos de React.

**Demo en vivo:** [enlace]
**GitHub:** [enlace]
```

### 2. El problema (1-2 párrafos)

¿Qué problema estabas resolviendo? Sé específico. Los problemas personales son problemas válidos.

```markdown
## El problema

Estaba usando una combinación de notas en papel, recordatorios del teléfono y archivos de texto aleatorios para rastrear tareas, fechas límite de proyectos y tareas personales. Olvidaba revisar mis notas, perdía fechas límite y perdía tiempo buscando dónde anoté algo.

Necesitaba un solo lugar para ver todas mis tareas, organizadas por lo urgente y lo próximo. También quería aprender React, así que construir algo que realmente usaría parecía el proyecto perfecto.
```

**Por qué funciona:**
- Detalles específicos (notas en papel, archivos de texto)
- Consecuencias reales (fechas límite perdidas)
- Motivación clara (aprender React + resolver problema)

### 3. Stack tecnológico

Lista lo que usaste y brevemente por qué.

```markdown
## Stack tecnológico

- **Frontend:** React - quería aprender arquitectura basada en componentes
- **Estilos:** CSS Modules - estilos con alcance sin dependencias extra
- **Almacenamiento:** LocalStorage - persistencia simple sin backend
- **Despliegue:** Netlify - hosting gratuito con despliegue fácil
```

No solo listes tecnologías. Agrega una oración explicando tu elección.

### 4. Características principales

¿Qué puede hacer? Usa viñetas para claridad.

```markdown
## Características principales

- ✅ Agregar, editar y eliminar tareas
- 🎯 Establecer niveles de prioridad (alta, media, baja)
- 📅 Agregar fechas de vencimiento con indicadores visuales
- 🔍 Filtrar tareas por prioridad o estado de completado
- 💾 Guardado automático en almacenamiento del navegador
- 📱 Diseño responsivo para uso móvil
```

### 5. Lo que aprendí

Esta es la sección más importante. ¿Qué habilidades desarrollaste?

```markdown
## Lo que aprendí

### Fundamentos de React
Este fue mi primer proyecto en React. Aprendí:
- Composición de componentes y props
- Gestión de estado con `useState`
- Efectos secundarios con `useEffect`
- Renderizado condicional
- Manejo de eventos

### Persistencia de datos
Aprendí sobre almacenamiento del navegador:
- API de LocalStorage
- Serialización JSON
- Manejo de límites de almacenamiento
- Migración de datos cuando la estructura cambia

### CSS y diseño responsivo
- CSS Grid para diseño
- Media queries para responsividad móvil
- Variables CSS para temas
- Enfoque de diseño mobile-first

### Despliegue
- Compilar para producción
- Variables de entorno
- Despliegue continuo con Git
- Configuración de dominio personalizado
```

**Por qué funciona:**
- Habilidades específicas, no afirmaciones vagas
- Muestra profundidad de aprendizaje
- Demuestra que entiendes lo que hiciste

### 6. Desafíos y soluciones

¿Qué salió mal? ¿Cómo lo arreglaste? Esto muestra resolución de problemas.

```markdown
## Desafíos

### Desafío 1: Complejidad en la gestión de estado

**Problema:** A medida que agregué funcionalidades, gestionar el estado se volvió desordenado. Tenía estado separado para tareas, filtros y elementos de UI, y no se sincronizaban correctamente.

**Solución:** Reestructuré mi estado para usar una única fuente de verdad y derivar valores de ella. En lugar de almacenar tareas filtradas en el estado, las calculaba al renderizar.

```javascript
// Antes: Múltiples variables de estado
const [tasks, setTasks] = useState([]);
const [filteredTasks, setFilteredTasks] = useState([]);
const [filter, setFilter] = useState('all');

// Después: Única fuente de verdad
const [tasks, setTasks] = useState([]);
const [filter, setFilter] = useState('all');

// Calcular tareas filtradas
const filteredTasks = tasks.filter(task => {
  if (filter === 'completed') return task.completed;
  if (filter === 'active') return !task.completed;
  return true;
});
```

**Lo que aprendí:** Mantén el estado mínimo y deriva valores cuando sea posible. Esto reduce errores y hace el código más fácil de razonar.

### Desafío 2: Diseño móvil

**Problema:** El diseño de escritorio se rompía en móvil. Los botones eran muy pequeños y la lista de tareas era difícil de leer.

**Solución:** Adopté un enfoque mobile-first, diseñando primero para pantallas pequeñas y luego agregando complejidad para pantallas más grandes.

```css
/* Mobile first */
.task-item {
  padding: 1rem;
  font-size: 1rem;
}

/* Mejora para escritorio */
@media (min-width: 768px) {
  .task-item {
    padding: 0.75rem;
    font-size: 0.9rem;
  }
}
```

**Lo que aprendí:** El diseño mobile-first es más fácil que intentar hacer un diseño de escritorio responsivo. Empieza pequeño y agrega complejidad.
```

### 7. Resultados e impacto

¿Qué pasó después de construirlo? Incluso impactos pequeños cuentan.

```markdown
## Resultados

- 🎯 Lo he usado diariamente durante 3 meses
- ✅ Rastreé más de 200 tareas sin perder una fecha límite
- 👥 Lo compartí con 3 compañeros que ahora lo usan
- 📈 Reduje el tiempo organizando tareas en ~30 minutos/semana
- 💡 Recibí retroalimentación que llevó a 5 mejoras de funcionalidades
```

**Por qué funciona:**
- Números concretos (200+ tareas, 3 meses)
- Uso real (uso diario)
- Validación externa (otros lo usan)
- Impacto medible (30 minutos ahorrados)

### 8. Mejoras futuras

¿Qué agregarías después? Muestra que estás pensando a futuro.

```markdown
## Mejoras futuras

- **Integración con backend:** Mover de LocalStorage a una base de datos para sincronización entre dispositivos
- **Colaboración:** Permitir compartir tareas con otros
- **Tareas recurrentes:** Soporte para tareas diarias/semanales repetitivas
- **Categorías:** Organizar tareas por proyecto o área de vida
- **Analíticas:** Visualizar tendencias de productividad a lo largo del tiempo
```

### 9. Conclusiones

Termina con 3-5 lecciones concretas.

```markdown
## Conclusiones clave

1. **Construye para ti mismo:** Los proyectos que realmente usarás son más motivantes y llevan a mejores resultados.

2. **Empieza simple:** Quería agregar 20 funcionalidades inicialmente. Empezar con la funcionalidad principal e iterar fue mucho mejor.

3. **Obtén retroalimentación temprano:** Mostrarlo a amigos después de la semana 1 llevó a mejoras que no habría pensado.

4. **La documentación importa:** Escribir esta publicación me ayudó a entender lo que realmente aprendí.

5. **Los proyectos pequeños son válidos:** Esta "simple" app de tareas me enseñó más de lo que seguir un tutorial lo habría hecho.
```

## Ejemplo real: Reenmarcando un proyecto "simple"

Tomemos un proyecto básico y mostremos cómo presentarlo bien.

### Proyecto: Panel meteorológico

**Versión débil:**
> "Hice una app del clima usando una API del clima. Muestra la temperatura y las condiciones."

**Versión fuerte:**

---

## Panel meteorológico

Un panel meteorológico limpio y rápido que muestra las condiciones actuales y un pronóstico de 5 días para cualquier ciudad.

**Demo en vivo:** [weather.yoursite.com](https://weather.yoursite.com)
**GitHub:** [github.com/you/weather-dashboard](https://github.com/you/weather-dashboard)

### El problema

Estaba cansado de abrir weather.com y lidiar con anuncios, carga lenta e interfaces desordenadas. Quería una forma rápida y limpia de verificar el clima, especialmente al planificar mi viaje en bicicleta al campus.

### Qué lo hace diferente

- **Rápido:** Carga en menos de 1 segundo
- **Limpio:** Sin anuncios, sin desorden, solo datos meteorológicos
- **Útil:** Muestra temperatura "se siente como" y velocidad del viento (importante para andar en bici)
- **Persistente:** Recuerda tu última ciudad buscada

### Stack tecnológico

- **Frontend:** JavaScript vanilla (quería practicar fundamentos sin frameworks)
- **API:** OpenWeatherMap API
- **Estilos:** CSS Grid y propiedades personalizadas
- **Despliegue:** GitHub Pages

### Lo que aprendí

**Integración con API:**
- Hacer peticiones HTTP con `fetch()`
- Manejar errores de API y límites de uso
- Parsear respuestas JSON
- Asegurar claves API (variables de entorno)

**JavaScript asíncrono:**
- Promesas y async/await
- Manejo de errores con try/catch
- Estados de carga y retroalimentación al usuario

**UI/UX:**
- Pantallas skeleton mientras carga
- Mensajes de error que ayudan a los usuarios
- Diseño responsivo para móvil
- Accesibilidad (navegación por teclado, etiquetas ARIA)

### Desafíos

**Desafío: Seguridad de la clave API**

Inicialmente codifiqué mi clave API en el archivo JavaScript. Un compañero señaló que esto era visible en las herramientas de desarrollo del navegador.

**Solución:** Moví la clave API a un proxy backend (una función simple de Netlify) que hace la llamada real a la API. El frontend llama a mi proxy, que agrega la clave API del lado del servidor.

**Lo que aprendí:** Nunca expongas claves API en código frontend. Siempre usa un proxy backend o variables de entorno.

### Resultados

- ⚡ Carga 5x más rápido que weather.com
- 🚴 Lo reviso cada mañana antes de ir en bici a clase
- 👥 10 amigos lo agregaron a favoritos y lo usan regularmente
- 📱 Funciona perfectamente en móvil
- ⭐ 15 estrellas en GitHub al compartirlo en Reddit

### Planes futuros

- Agregar alertas meteorológicas
- Soporte para múltiples ciudades guardadas
- Datos meteorológicos históricos
- Sugerencias de actividades basadas en el clima ("¡Buen día para andar en bici!")

### Conclusiones

1. **Resuelve tus propios problemas:** Lo uso diariamente porque resuelve un problema real que tenía.
2. **Simple puede ser mejor:** Una herramienta enfocada que hace una cosa bien supera a una herramienta compleja que hace todo mal.
3. **La seguridad importa:** Aprender sobre seguridad de claves API temprano me salvó de un error costoso.
4. **Comparte tu trabajo:** Publicar en Reddit llevó a gran retroalimentación y usuarios.

---

¿Ves cuánto más convincente es eso? Mismo proyecto, mejor presentación.

## Errores comunes a evitar

### 1. Disculparte por la simplicidad

**No digas:**
- "Este es solo un proyecto simple..."
- "Sé que esto no es muy impresionante..."
- "Todos han hecho esto antes..."

**En su lugar:**
- Enfócate en lo que aprendiste
- Enfatiza el problema que resolviste
- Muestra tu enfoque o ideas únicos

### 2. Solo mostrar código

Código sin contexto es difícil de evaluar. Siempre explica:
- Por qué lo escribiste de esta manera
- Qué problema resuelve
- Qué alternativas consideraste

### 3. Saltarte el "por qué"

No solo digas qué construiste. Explica:
- Por qué lo construiste
- Por qué elegiste estas tecnologías
- Por qué tomaste decisiones específicas

### 4. Sin elementos visuales

Agrega capturas de pantalla, diagramas o GIFs. La prueba visual hace tu proyecto más tangible.

### 5. Ignorar los fracasos

Los fracasos son oportunidades de aprendizaje. Comparte lo que no funcionó y lo que aprendiste de ello.

## Enlazando a GitHub

Tu publicación del proyecto debe enlazar a tu repositorio en GitHub. Asegúrate de que tu repositorio tenga:

### Secciones esenciales del README

```markdown
# Nombre del Proyecto

Breve descripción

## Características

- Característica 1
- Característica 2

## Instalación

```bash
git clone https://github.com/you/project
cd project
npm install
npm start
```

## Uso

[Cómo usarlo]

## Tecnologías

- Tecnología 1
- Tecnología 2

## Lo que aprendí

[Breve resumen]

## Licencia

MIT
```

### Buenos mensajes de commit

Muestra tu proceso de desarrollo con commits claros:

```
✅ Bien:
- "Add task filtering by priority"
- "Fix mobile layout overflow issue"
- "Implement LocalStorage persistence"

❌ Mal:
- "Update"
- "Fix stuff"
- "Changes"
```

## Pasos de acción

¿Listo para documentar tu proyecto? Esto es lo que debes hacer:

1. **Elige un proyecto** (cualquier proyecto, aunque sea "simple")
2. **Responde estas preguntas:**
   - ¿Qué problema resolvió?
   - ¿Qué aprendiste?
   - ¿Qué desafíos enfrentaste?
   - ¿Qué harías diferente?
3. **Escribe un borrador** usando la plantilla de arriba
4. **Agrega elementos visuales** (capturas de pantalla, diagramas, fragmentos de código)
5. **Enlaza a tu código** (repositorio en GitHub con buen README)
6. **Publica y comparte**

## Reflexiones finales

Tus proyectos "simples" son más valiosos de lo que piensas. Demuestran:
- Que puedes terminar cosas
- Que puedes aprender de forma independiente
- Que puedes comunicar conceptos técnicos
- Que resuelves problemas reales

Deja de esperar el proyecto "perfecto" sobre el cual escribir. Documenta lo que has construido, comparte lo que has aprendido y ayuda a otros que están un paso detrás de ti.

El mejor momento para empezar fue ayer. El segundo mejor momento es ahora.

---

**Recursos:**
- [Repositorio de plantillas con ejemplos](https://github.com/yourusername/project-template)
- [Mi portafolio de proyectos](https://yoursite.com/projects)
- [Más consejos de escritura](https://yoursite.com/posts/writing-tips)

**¿Preguntas?** Contáctame en [Twitter](https://twitter.com/you) o [correo electrónico](mailto:your.email@example.com). ¡Estaré encantado de revisar tus escritos sobre proyectos!
