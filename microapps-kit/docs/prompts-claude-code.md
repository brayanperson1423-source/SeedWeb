# Secuencia de prompts para Claude Code

Cómo usar este archivo:

1. Abre Claude Code dentro de la carpeta del proyecto (así lee `CLAUDE.md` automáticamente).
2. Pega **un prompt a la vez**, en orden.
3. Antes de pasar al siguiente, comprueba el **criterio de salida** del actual.
4. Si algo sale mal, usa los **prompts de control** del final.
5. Lo que esté entre `[corchetes]` lo reemplazas tú.

> Regla de oro: no avances si el paso anterior no funciona. Un error pequeño que se arrastra se vuelve un problema grande.

---

## FASE A · Orientación y decisiones

### Prompt 0 · Orientación

```
Lee CLAUDE.md completo y los archivos de la carpeta docs/.
Luego responde con:
1) un resumen de lo que entendiste del proyecto en máximo 10 líneas,
2) lo que NO está definido todavía,
3) la primera pregunta de la sección 13 de CLAUDE.md.
No escribas código ni crees archivos todavía.
```

**Criterio de salida:** el resumen coincide con lo que quieres y te hizo una sola pregunta.

### Prompt 1 · Entrevista de decisiones

```
Vamos a resolver las decisiones abiertas de la sección 13 de CLAUDE.md,
una pregunta por mensaje. Después de cada respuesta mía, registra la decisión
en docs/decisiones.md con la plantilla del archivo y hazme la siguiente pregunta.
Cuando terminemos, muéstrame un resumen de todo lo decidido.
```

**Criterio de salida:** `docs/decisiones.md` tiene las 8 decisiones registradas (o marcadas como "pendiente" si aún no las sabes).

### Prompt 2 · Elegir nicho con datos (opcional si ya lo sabes)

```
Ayúdame a elegir nicho usando el scorecard de la sección 2 de CLAUDE.md.
Hazme las preguntas necesarias sobre cada candidato (una por vez), calcula
el puntaje y dime cuál recomiendas y por qué. Aclara qué datos debería
confirmar hablando con negocios reales antes de decidir.
Guarda el resultado en docs/decisiones.md.
```

**Criterio de salida:** hay un nicho recomendado **o** una lista clara de qué validar en la calle antes de elegir.

---

## FASE B · Plan y arquitectura

### Prompt 3 · Stack final

```
Con base en lo decidido y en la sección 5 de CLAUDE.md, confirma o corrige
el stack tecnológico. Para cada capa dame: opción recomendada, alternativa,
costo estimado mensual con 1, 10, 50 y 100 negocios, y riesgo de depender
del proveedor. Explícalo en lenguaje sencillo y dime qué verificar en la
documentación oficial (precios y límites). No instales nada todavía.
```

**Criterio de salida:** entiendes cada elección y aprobaste el stack. Se registra en `docs/decisiones.md`.

### Prompt 4 · Plan de etapas del MVP

```
Propón un plan de etapas para llegar al MVP de la sección 4 de CLAUDE.md.
Cada etapa debe tener: objetivo, tareas, cómo probarla y tiempo estimado
para una persona con [X horas semanales]. Ordena las etapas para que tras
la primera ya haya algo que pueda enseñar en el celular.
Espera mi aprobación antes de escribir código.
```

**Criterio de salida:** plan aprobado. Guárdalo en `docs/plan-mvp.md`.

### Prompt 5 · Esquema de datos

```
Diseña el esquema de datos definitivo a partir de la sección 6 de CLAUDE.md.
Incluye las reglas de acceso para que cada negocio solo vea sus datos.
Explícame cada tabla en una línea y dime qué podría cambiar más adelante.
Todavía no apliques nada: primero lo reviso.
```

**Criterio de salida:** entiendes el esquema y lo apruebas.

---

## FASE C · Construcción del MVP

### Prompt 6 · Inicializar el proyecto

```
Inicializa el proyecto con el stack aprobado, respetando la estructura de
la sección 12 de CLAUDE.md. Incluye: configuración base, lectura de
variables de entorno desde .env (usando .env.example), un comando para
correr en local y uno para correr las pruebas.
Al terminar dime exactamente qué comandos ejecutar para ver la app
funcionando y actualiza el README.
```

**Criterio de salida:** la app abre en el navegador local y las pruebas corren (aunque sean pocas).

### Prompt 7 · Núcleo: negocios, usuarios y acceso

```
Implementa el núcleo: registro e inicio de sesión, creación de negocio,
roles (dueño, empleado, solo lectura) y aislamiento de datos por negocio
aplicado en la base de datos. Crea pruebas que demuestren que un negocio
NO puede leer ni modificar datos de otro. Muéstrame cómo probarlo a mano.
```

**Criterio de salida:** con dos negocios de prueba, ninguno ve los datos del otro.

### Prompt 8 · Marca configurable

```
Implementa la marca por negocio: nombre, logo, colores y datos de contacto,
editables desde la app y aplicados en toda la interfaz sin tocar código.
Diseño pensado primero para celular (~375 px).
```

**Criterio de salida:** cambias logo y colores y toda la app lo refleja.

### Prompt 9 · Catálogo

```
Implementa el catálogo de productos/servicios: crear, editar, desactivar,
buscar e importar desde CSV (con vista previa y validación de errores
antes de guardar). Estados vacíos útiles y mensajes en español de Colombia.
Precios en COP con formato local.
```

**Criterio de salida:** importas un CSV de prueba y el catálogo se ve bien en celular.

### Prompt 10 · Cotización y totales

```
Implementa la creación de cotizaciones: elegir o crear cliente, agregar
ítems con cantidades, descuento, IVA opcional y total. La lógica de totales
debe estar en UNA sola función con pruebas (casos: sin descuento, con
descuento, con IVA, cantidades decimales, redondeo en COP).
Los precios se copian al documento al crearlo. Meta: crear una cotización
en máximo 3 toques desde la pantalla principal.
```

**Criterio de salida:** las pruebas de totales pasan y puedes crear una cotización desde el celular.

### Prompt 11 · PDF

```
Genera el PDF de la cotización con la marca del negocio (logo, colores,
datos de contacto), tabla de ítems, totales y condiciones editables.
Que se vea profesional y legible al imprimir y en pantalla de celular.
```

**Criterio de salida:** el PDF se descarga y se ve bien.

### Prompt 12 · Compartir por WhatsApp

```
Agrega el botón "Enviar por WhatsApp" usando enlaces wa.me (sin API de pago)
con un mensaje configurable que incluya nombre del cliente, número y total,
más un enlace al PDF. Verifica cómo se formatea el número colombiano
(indicativo 57) y maneja el caso de teléfonos mal escritos.
```

**Criterio de salida:** el botón abre WhatsApp con el mensaje correcto.

### Prompt 13 · Historial y estados

```
Implementa el historial de cotizaciones con búsqueda por cliente o número,
filtro por estado (borrador, enviada, aceptada, rechazada) y la posibilidad
de duplicar una cotización. Agrega un resumen simple (cantidad y valor del mes).
```

**Criterio de salida:** encuentras y duplicas cotizaciones fácilmente.

### Prompt 14 · Datos de ejemplo para demo

```
Crea un botón (solo para el dueño) que cargue datos de ejemplo realistas
del nicho elegido: catálogo, clientes y algunas cotizaciones, y otro que
los borre. Deben ser claramente ficticios. Esto es para mostrar la demo
en visitas de 10 minutos.
```

**Criterio de salida:** con un clic tienes una demo lista para mostrar.

### Prompt 15 · Web app instalable (PWA)

```
Convierte la app en una PWA instalable: manifest, íconos con la marca,
pantalla completa y carga rápida. Si es viable de forma simple, que abra
sin conexión mostrando los datos recientes. Explícame cómo probar la
instalación en Android y en iPhone.
```

**Criterio de salida:** se instala en la pantalla de inicio del celular.

---

## FASE D · Calidad, seguridad y publicación

### Prompt 16 · Revisión de seguridad y privacidad

```
Haz una revisión de seguridad y privacidad de todo el proyecto según la
sección 9 de CLAUDE.md. Revisa: secretos en el código, reglas de acceso,
validación de entradas, exposición de datos, registro de actividad,
exportación y borrado de datos del negocio. Dame una lista de hallazgos
ordenada por gravedad y propón las correcciones. No cambies nada sin
mi aprobación.
```

**Criterio de salida:** hallazgos graves corregidos y probados.

### Prompt 17 · Pruebas y checklist

```
Revisa docs/checklist-lanzamiento.md y verifica uno por uno cuáles puntos
ya se cumplen. Para los que no, crea las tareas o pruebas faltantes.
Muéstrame el resultado en una tabla: punto, estado, qué falta.
```

**Criterio de salida:** los puntos de "Producto" y "Seguridad" del checklist están en verde.

### Prompt 18 · Publicar la demo con un enlace

```
Prepara el despliegue de una versión de demostración con enlace público.
Dame los pasos exactos, qué variables de entorno configurar y qué costos o
límites del plan gratuito debo verificar. Asegúrate de que la demo use solo
datos de ejemplo, nunca datos reales.
```

**Criterio de salida:** abres el enlace desde tu celular y funciona.

---

## FASE E · Paquetes de nicho y primer cliente

### Prompt 19 · Primer paquete de nicho

```
Convierte la configuración actual del nicho elegido en un paquete de nicho
siguiendo docs/niche-packs.md: módulos, etiquetas, plantillas de documento
y de WhatsApp, y datos de ejemplo. Demuestra que crear un segundo paquete
de prueba NO requiere editar código, solo configuración.
```

**Criterio de salida:** un segundo paquete de prueba funciona solo con configuración.

### Prompt 20 · Onboarding de un cliente

```
Automatiza lo posible de docs/onboarding-cliente.md: un asistente o script
para crear un negocio nuevo, activar su paquete, subir logo e importar
catálogo. Meta: instalar un cliente nuevo en menos de 1 día. Dime qué pasos
siguen siendo manuales.
```

**Criterio de salida:** instalas un cliente de prueba de principio a fin siguiendo la lista.

### Prompt 21 · Material de venta

```
Con la demo ya funcionando, prepara: un guion de demostración de 10 minutos,
una página de una sola hoja para el cliente (qué hace, cuánto cuesta, cómo
empieza) y un manual de uso de una página. Lenguaje simple, sin tecnicismos,
español de Colombia.
```

**Criterio de salida:** puedes hacer la demo de 10 minutos sin improvisar.

---

## PROMPTS DE CONTROL (úsalos cuando haga falta)

### Retomar una sesión

```
Lee CLAUDE.md, docs/decisiones.md y docs/plan-mvp.md. Dime en qué etapa
estamos, qué está terminado, qué sigue y si hay algo bloqueado.
```

### Cuando algo falla

```
Esto falló: [pega el error o describe qué pasó].
Antes de cambiar nada, explícame en lenguaje sencillo qué crees que causó
el problema, qué evidencia tienes y qué opciones hay para arreglarlo.
```

### Cuando no entiendo algo

```
Explícame [tema] como si no tuviera experiencia técnica, con un ejemplo
de la vida real y con las ventajas y desventajas de cada opción.
```

### Antes de aprobar un cambio grande

```
Antes de hacer este cambio, dime: qué archivos toca, qué podría romperse,
cómo lo probamos y cómo lo revertimos si sale mal.
```

### Cierre de sesión

```
Resume lo que hicimos hoy, actualiza docs/decisiones.md y el README con
cualquier cambio, y dime cuál es el siguiente prompt de docs/prompts-claude-code.md
que debería usar en la próxima sesión.
```

### Revisar costos

```
Estima el costo mensual actual del proyecto y proyecta el costo con
10, 50 y 100 negocios, indicando qué supuestos usaste y qué precios
debo verificar en la documentación oficial de cada proveedor.
```

---

## Notas finales

- Los precios, límites y pasos de instalación de cualquier herramienta **cambian**: verifícalos en su documentación oficial.
- Todo lo legal (política de datos, términos, contratos) debe revisarlo un abogado antes de usarse con clientes reales.
- Si el nicho cambia, vuelve al Prompt 2 y actualiza `CLAUDE.md`.
