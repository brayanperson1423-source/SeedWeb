# CLAUDE.md · Brief maestro de la empresa de micro apps

> **Cómo usar este archivo:** colócalo en la raíz del proyecto con el nombre `CLAUDE.md`. Claude Code lo lee al iniciar cada sesión y lo usa como contexto permanente. Cuando una decisión cambie, actualiza este archivo; es la fuente de verdad del proyecto.
>
> **Estado:** v0.1 · Mercado: Colombia / Latinoamérica · Idioma del producto: español (Colombia) · Moneda: COP
>
> **Aviso:** las cifras de este documento son estimaciones (1 USD ≈ 4.000 COP). Verifica precios y límites vigentes de cada herramienta antes de comprometerte con ellos.

---

## 0. Instrucciones para Claude Code (leer primero)

Eres el socio técnico de un fundador que está empezando. Trabaja con estas reglas:

1. **Explica en lenguaje sencillo.** El fundador tiene poca experiencia técnica. Antes de decidir algo importante, di qué opciones hay, cuál recomiendas y por qué.
2. **Plan antes que código.** Para cualquier tarea de más de ~30 líneas, propón primero un plan corto y espera confirmación.
3. **Pasos pequeños y verificables.** Cada paso termina con instrucciones claras de cómo probarlo.
4. **Una pregunta a la vez.** Si algo es ambiguo, pregunta lo mínimo necesario (ver sección 13, decisiones abiertas).
5. **Simple antes que sofisticado.** Elige la solución más barata y mantenible que cumpla el objetivo. Nada de tecnología "porque sí".
6. **No inventes.** Si no estás seguro de una API, precio, límite o ley, dilo y pídeme que lo verifique en la documentación oficial.
7. **Nunca** guardes secretos (claves, tokens) en el código ni en el repositorio. Usa variables de entorno y un archivo `.env.example` sin valores reales.
8. **Cuida los datos personales.** Todo dato de personas se trata según la sección 9.
9. **Documenta mientras construyes.** Cada módulo nuevo actualiza el README y, si cambia una decisión, este archivo.
10. **Si hay conflicto** entre "más rápido" y "más seguro con los datos de clientes", gana lo seguro.

---

## 1. La empresa en una página

### Visión
Ser la fábrica de **micro apps** más rápida y confiable para pequeños negocios de Latinoamérica: herramientas de una sola tarea, listas en días, a un precio mensual accesible.

### Idea clave
> **Una plataforma base + "paquetes de nicho" que se personalizan en un día para cada negocio, y se cobran con mensualidad.**

No vendemos "desarrollo a la medida". Vendemos **productos reutilizables** que se sienten a la medida porque el logo, los textos, los campos, los productos y los flujos se configuran sin tocar código.

### Qué es una micro app (definición operativa)
- Hace **una tarea** bien (cotizar, agendar, controlar inventario, recibir pedidos, cobrar).
- 2 a 5 pantallas principales.
- Se usa desde el celular, como **web app instalable (PWA)**, sin pasar por tiendas de apps.
- Se entrega en **días**, no en meses.
- Tiene un resultado **medible** para el cliente (tiempo ahorrado, citas recuperadas, dinero cobrado).

### Principios del negocio
1. **Nicho primero, tecnología después.** La ventaja es conocer bien a un tipo de cliente.
2. **Reutilizar todo.** Cada proyecto nuevo debe ser más rápido y barato que el anterior.
3. **Cobrar desde el principio.** Si no pagan, el problema no era tan grave.
4. **Valor visible.** El cliente debe ver cuánto le ahorra o le recupera la app.
5. **Datos del cliente = responsabilidad nuestra.** Privacidad y seguridad no son opcionales.

---

## 2. Estado actual: el nicho aún no está definido

**Esto es una decisión abierta, no un error del documento.** Por eso la arquitectura debe ser **agnóstica al nicho**: un núcleo común y paquetes de nicho intercambiables.

### Candidatos a primer nicho (a validar con clientes reales)

| Código | Nicho | Micro app semilla | Dolor principal |
|---|---|---|---|
| A | Talleres, ferreterías, carpinterías, imprentas | Cotizador con PDF + WhatsApp | Cotizar a mano y perder ventas |
| B | Restaurantes, tiendas, distribuidores | Catálogo y pedidos por WhatsApp | Pedidos en audios, desorden |
| C | Barberías, estéticas, consultorios, gimnasios | Agenda con recordatorios | Citas perdidas |
| D | Gimnasios, colegios, arrendadores, administradores de conjuntos | Recordatorios de cobro | Cartera atrasada |
| E | Minimercados, farmacias, restaurantes | Inventario con alertas | Quedarse sin producto |
| F | Transporte, mantenimiento, obras | Checklist de inspección con fotos y PDF | Formatos en papel |

### Scorecard para elegir nicho (puntúa cada nicho de 1 a 5)

| Criterio | Peso | Qué mide |
|---|---|---|
| Intensidad del dolor | 3 | ¿Pierden plata o tiempo claramente? |
| Disposición a pagar | 3 | ¿Ya pagan por algo parecido? ¿Cuánto? |
| Acceso al cliente | 2 | ¿Conozco gente, puedo visitarlos en persona? |
| Facilidad de construcción | 2 | ¿Se resuelve con una app simple? |
| Repetibilidad | 2 | ¿Hay muchos negocios casi iguales? |
| Potencial de segunda app | 1 | ¿Puedo venderle más después? |
| Poca competencia directa | 1 | ¿Hay espacio para diferenciarse? |

**Puntaje = suma de (puntuación × peso).** El nicho elegido debe ganar con datos de conversaciones reales, no con intuición.

**Regla:** no se construye más allá del MVP genérico hasta que 3 negocios del mismo nicho digan que lo usarían y 1 pague.

---

## 3. Producto: arquitectura "núcleo + paquetes de nicho"

```
┌────────────────────────────────────────────────────────────┐
│                     PAQUETES DE NICHO                       │
│  (configuración, no código: campos, textos, plantillas)     │
│  cotizador-talleres · pedidos-restaurantes · citas-estetica │
├────────────────────────────────────────────────────────────┤
│                    MÓDULOS FUNCIONALES                      │
│  Catálogo · Clientes · Documentos/PDF · Agenda · Pedidos    │
│  Inventario · Recordatorios · Reportes                      │
├────────────────────────────────────────────────────────────┤
│                       NÚCLEO COMÚN                          │
│  Autenticación · Negocios (tenants) · Roles · Marca/tema    │
│  Datos por negocio · Auditoría · Exportación · Facturación  │
│  de la suscripción                                          │
└────────────────────────────────────────────────────────────┘
```

### Núcleo común (siempre presente)
- **Multi-negocio (multi-tenant):** cada negocio solo ve sus datos. Esto es obligatorio desde el día uno.
- **Autenticación** simple (correo + contraseña, o enlace mágico / código por WhatsApp).
- **Roles básicos:** `dueño`, `empleado`, `solo lectura`.
- **Marca configurable:** logo, colores, nombre del negocio, datos de contacto.
- **Exportación de datos** (CSV/PDF): el cliente siempre puede llevarse su información.
- **Registro de actividad** (quién hizo qué y cuándo) para soporte y confianza.

### Módulos funcionales (se activan por negocio)
| Módulo | Qué hace | Se usa en |
|---|---|---|
| Catálogo | Productos/servicios con precios y categorías | Cotizador, pedidos, inventario |
| Clientes | Contactos del negocio con historial | Todas |
| Documentos | Generación de PDF con marca del negocio | Cotizador, checklists, recibos |
| Agenda | Citas, turnos, disponibilidad | Citas |
| Pedidos | Carrito, estados, historial | Pedidos |
| Inventario | Existencias y alertas de mínimo | Inventario |
| Recordatorios | Mensajes programados (enlace `wa.me` primero; API de WhatsApp después) | Citas, cobros |
| Reportes | Resumen semanal/mensual de resultados | Todas |

### Paquete de nicho (qué contiene)
Un paquete es **solo configuración y contenido**:
- Módulos activados.
- Campos personalizados y etiquetas (ej.: "Orden de trabajo" vs. "Cotización").
- Plantillas de documentos y mensajes de WhatsApp.
- Datos de ejemplo para la demo.
- Textos de onboarding y manual de una página.

**Ejemplo de configuración (referencial, adaptar al stack):**

```yaml
niche_pack:
  id: cotizador-talleres
  name: "Cotizador para talleres y ferreterías"
  modules: [catalog, clients, documents, reports]
  labels:
    document: "Cotización"
    client: "Cliente"
    item: "Producto / servicio"
  document_template:
    sections: [header, client_data, items_table, totals, terms, signature]
    default_terms: "Cotización válida por 15 días."
  whatsapp_templates:
    send_quote: "Hola {{cliente}}, te comparto tu cotización {{numero}} por {{total}}. {{enlace}}"
  demo_data: ./demo/talleres.json
  tax:
    iva_default: 0.19
    iva_optional: true
```

> **Regla de oro:** si para crear un nuevo nicho hay que **editar código**, la arquitectura está mal. Debe bastar con un nuevo paquete.

---

## 4. MVP (alcance cerrado)

**Objetivo:** una demo funcional que se pueda mostrar en el celular de un dueño de negocio en una visita de 10 minutos.

### Dentro del MVP
- [ ] Web app instalable (PWA), pensada primero para celular.
- [ ] Login y datos separados por negocio.
- [ ] Marca configurable (logo, colores, nombre).
- [ ] Catálogo editable (crear, editar, borrar, importar desde CSV).
- [ ] Crear una cotización: cliente, ítems, cantidades, descuento, IVA opcional, total.
- [ ] Generar PDF con la marca del negocio.
- [ ] Compartir por WhatsApp con enlace `wa.me` (sin API de pago).
- [ ] Historial de cotizaciones con búsqueda y estados (borrador, enviada, aceptada, rechazada).
- [ ] Datos de ejemplo cargables con un clic para demostraciones.

### Fuera del MVP (no construir todavía)
- Pagos en línea, facturación electrónica ante la DIAN, integraciones contables.
- API oficial de WhatsApp Business (se evalúa cuando haya clientes pagando).
- Aplicación nativa en tiendas.
- Inteligencia artificial integrada (se evalúa después; ver sección 10).
- Panel de administración sofisticado para la empresa.

> **Nota fiscal:** una cotización **no es** una factura. La facturación electrónica tiene reglas propias de la DIAN; si un cliente la pide, se trata como proyecto aparte y se verifica con un contador.

### Criterios de "terminado" (Definition of Done)
Una funcionalidad está terminada solo si:
1. Funciona en celular (probado en un viewport de ~375 px) y en computador.
2. Tiene manejo de errores y estados vacíos con mensajes claros en español.
3. Respeta la separación de datos entre negocios (probado con dos negocios de prueba).
4. Tiene pruebas para la lógica crítica (cálculo de totales, permisos).
5. Está documentada en el README y se explica cómo probarla.
6. No introduce secretos ni datos reales en el repositorio.

---

## 5. Stack tecnológico (propuesta inicial, justificar antes de adoptar)

Principio: **barato, simple, con buena documentación y fácil de mantener por una sola persona.**

| Capa | Propuesta inicial | Alternativa | Criterio de decisión |
|---|---|---|---|
| Interfaz | Web app responsive + PWA | Framework de componentes a elección | Rendimiento en celulares básicos |
| Lenguaje | TypeScript | JavaScript | Menos errores a mediano plazo |
| Base de datos y login | Supabase (Postgres + autenticación + reglas por fila) | Firebase, Postgres propio | Aislamiento de datos por negocio |
| Almacenamiento de archivos | Del mismo proveedor de la base de datos | Almacenamiento compatible con S3 | Logos y PDFs |
| Generación de PDF | Librería en el cliente o en una función del servidor | Servicio externo | Calidad y costo |
| Despliegue | Vercel o Netlify (con enlace público para demos) | Cloudflare Pages | Facilidad y costo |
| Automatización/recordatorios | Tareas programadas del proveedor, o n8n | Cron propio | Simplicidad |
| Control de versiones | Git + GitHub | — | — |

**Pedido a Claude Code:** antes de crear el proyecto, confirma o corrige esta tabla con una recomendación razonada, indicando costos estimados al escalar a 10, 50 y 100 negocios y los riesgos de depender de cada proveedor.

### Restricciones técnicas
- **Aislamiento por negocio:** cada fila de datos pertenece a un negocio, y las reglas de acceso se aplican **en la base de datos**, no solo en la interfaz.
- **Offline básico (deseable):** que la app abra y muestre datos recientes sin conexión.
- **Rendimiento:** carga inicial rápida en conexiones móviles lentas.
- **Accesibilidad:** textos legibles, botones grandes, contraste suficiente.
- **Sin dependencia dura de un proveedor:** los datos deben poder exportarse y migrarse.

---

## 6. Modelo de datos (esquema conceptual inicial)

> Es una guía, no un contrato. Claude Code debe proponer el esquema final y justificarlo.

```
negocio (tenant)
  id, nombre, logo_url, colores, datos_contacto, paquete_de_nicho, plan, estado

usuario
  id, negocio_id, nombre, correo, rol, activo

cliente_del_negocio
  id, negocio_id, nombre, telefono, correo, notas, autorizacion_datos (fecha/versión)

item_catalogo
  id, negocio_id, nombre, descripcion, unidad, precio, categoria, activo

documento (cotización, pedido, etc.)
  id, negocio_id, tipo, numero, cliente_id, estado, subtotal, descuento,
  impuestos, total, notas, creado_por, creado_en

documento_item
  id, documento_id, item_id (opcional), descripcion, cantidad, precio_unitario, total

registro_actividad
  id, negocio_id, usuario_id, accion, entidad, entidad_id, fecha
```

**Reglas del modelo:**
- Todo registro lleva `negocio_id`.
- Los totales se calculan en un solo lugar (función única, probada).
- Los precios del documento se **copian** al crearlo (si cambia el catálogo, las cotizaciones viejas no cambian).
- Borrado lógico (marcar como inactivo) para no perder historial.

---

## 7. Experiencia de usuario (UX)

- **Mobile first.** El dueño del negocio usa el celular, de pie, con una mano.
- **Lenguaje del negocio**, no de software: "Cotización", "Cliente", "Enviar por WhatsApp".
- **Máximo 3 toques** para la acción principal (crear y enviar una cotización).
- **Cero manual:** si necesita explicación larga, rediseñar.
- **Estados vacíos útiles:** una pantalla sin datos explica qué hacer y ofrece un botón.
- **Confirmaciones claras** antes de borrar o enviar.
- **Tono:** cercano, respetuoso, español de Colombia; sin tecnicismos ni anglicismos innecesarios.
- **Marca del cliente primero:** el negocio debe sentir que la app es suya.

---

## 8. Modelo de negocio y precios (hipótesis a validar)

| Concepto | Rango sugerido (COP) | Aprox. USD |
|---|---|---|
| Instalación / personalización (único) | 300.000 a 800.000 | 75 a 200 |
| Mensualidad por micro app | 80.000 a 250.000 | 20 a 60 |
| Paquete de 3 micro apps | 250.000 a 600.000 / mes | 60 a 150 |
| Cambios fuera del alcance | Por horas o por paquete | — |

**Oferta de entrada:** 2 semanas de prueba gratis con datos reales del cliente, instalación descontada si firma al terminar la prueba.

**Costos variables por cliente (estimados):** hosting y base de datos, dominio, y API de IA si se usa. En total, del orden de 3 a 20 USD por cliente al mes. El costo real es el **tiempo** de venta, instalación y soporte.

**Métricas que importan:** clientes activos, ingreso mensual recurrente (MRR), costo de adquirir un cliente, tiempo de entrega por app, tasa de renovación mensual, horas de soporte por cliente.

---

## 9. Seguridad, privacidad y cumplimiento

> Esto es orientación técnica general, **no asesoría legal**. Antes de operar con datos reales de clientes, consultar a un abogado.

### Principios
- **Minimizar datos:** guardar solo lo necesario.
- **Aislamiento:** un negocio jamás ve datos de otro (reglas a nivel de base de datos + pruebas que lo demuestren).
- **Mínimo privilegio:** cada rol accede solo a lo que necesita.
- **Cifrado** en tránsito (HTTPS) y datos sensibles protegidos en reposo según el proveedor.
- **Secretos** solo en variables de entorno; nunca en el código.
- **Copias de seguridad** y capacidad de restaurar.
- **Exportación y eliminación:** el negocio puede llevarse y borrar sus datos.

### Colombia: protección de datos personales (Ley 1581 de 2012 y normas relacionadas)
- Política de tratamiento de datos clara y accesible.
- Autorización del titular para el tratamiento de sus datos, guardada con fecha y versión.
- Definir con claridad los roles: **el negocio cliente es responsable del tratamiento** de los datos de sus propios clientes, y **la empresa actúa como encargada**. Esto se refleja en un contrato de servicio / acuerdo de tratamiento de datos.
- Atender consultas, reclamos y solicitudes de supresión.
- Evaluar si aplica algún registro o requisito ante la autoridad (verificar con un abogado).

### Mensajería por WhatsApp
- Comenzar con enlaces `wa.me`, donde la persona envía el mensaje desde su propio teléfono.
- Si más adelante se usa la API oficial de WhatsApp Business: respetar las políticas de Meta, plantillas aprobadas, ventana de 24 horas y consentimiento previo. **Verificar reglas y tarifas vigentes.**

### Contratos y términos
Preparar con apoyo legal: términos del servicio, política de privacidad, acuerdo de tratamiento de datos y política de cancelación.

---

## 10. Inteligencia artificial (fase posterior, opcional)

No es parte del MVP. Se agrega **solo cuando resuelva un dolor real y medible**. Ideas candidatas:

| Idea | Valor | Riesgo |
|---|---|---|
| Crear una cotización desde un mensaje o audio de WhatsApp | Ahorra captura manual | Errores de interpretación; requiere confirmación humana |
| Importar el catálogo desde una foto o PDF de lista de precios | Acelera la instalación | Revisión obligatoria antes de guardar |
| Resumen semanal en lenguaje natural | Valor percibido alto | Bajo |
| Asistente de WhatsApp que responde preguntas básicas | Atención 24/7 | Respuestas incorrectas; límites claros |

**Reglas si se usa IA:**
- Siempre con confirmación humana antes de guardar o enviar algo.
- No enviar datos personales innecesarios al modelo.
- Registrar los casos fallidos para mejorar.
- Modelo y costos por verificar en la documentación oficial vigente.

---

## 11. Hoja de ruta por fases

| Fase | Objetivo | Entregable | Criterio para avanzar |
|---|---|---|---|
| **0 · Descubrimiento** | Elegir nicho con datos | 15 entrevistas, scorecard completo | Un nicho gana con evidencia |
| **1 · Demo** | Mostrar valor en una visita | MVP con datos de ejemplo, enlace público | 3 negocios quieren probarlo |
| **2 · Piloto** | Probar con uso real | 2 a 3 negocios usando la app 2 semanas | Resultados medidos (tiempo/dinero) |
| **3 · Primeros ingresos** | Confirmar que pagan | 1 a 3 clientes con mensualidad | Cobro recurrente funcionando |
| **4 · Plantilla** | Volver el proceso repetible | Paquete de nicho + guía de instalación en 1 día | Instalar un cliente nuevo ≤ 1 día |
| **5 · Segunda app** | Vender más al mismo cliente | Segundo módulo/paquete | Al menos 1 cliente compra la segunda |
| **6 · Escala** | 20 a 50 clientes | Automatizar altas, cobros y soporte | Soporte sostenible por persona |

---

## 12. Estructura de proyecto sugerida

> Claude Code debe ajustarla al framework elegido.

```
/
├── CLAUDE.md                  ← este archivo
├── README.md                  ← cómo instalar, correr y probar
├── .env.example               ← variables necesarias (sin valores reales)
├── docs/
│   ├── decisiones.md          ← registro de decisiones (qué, por qué, fecha)
│   ├── niche-packs.md         ← cómo crear un paquete de nicho
│   ├── onboarding-cliente.md  ← pasos para instalar un cliente en 1 día
│   └── legal/                 ← borradores de políticas y contratos (para revisión legal)
├── src/
│   ├── core/                  ← autenticación, negocios, roles, marca
│   ├── modules/               ← catálogo, clientes, documentos, agenda...
│   ├── niche-packs/           ← un paquete por nicho (solo configuración)
│   └── shared/                ← utilidades, componentes, formato COP, etc.
├── supabase/ (o equivalente)  ← migraciones y reglas de acceso
└── tests/
```

### Convenciones
- Código y nombres técnicos en inglés; **textos de interfaz y documentación en español**.
- Formato de moneda COP (`$ 1.250.000`), fechas en formato local, zona horaria `America/Bogota`.
- Commits pequeños y descriptivos.
- Una decisión importante = una entrada en `docs/decisiones.md`.

---

## 13. Decisiones abiertas (preguntar al fundador, una a la vez)

Claude Code: antes de construir, ve resolviendo estas preguntas en orden, **una por mensaje**, y registra cada respuesta en `docs/decisiones.md`.

1. ¿Cuánta experiencia técnica tiene el fundador y cuánto tiempo semanal puede dedicar?
2. ¿Qué tipo de negocios conoce o tiene acceso directo (familia, amigos, comunidad)?
3. ¿En qué ciudad empezará a visitar clientes?
4. ¿Cuál es el presupuesto mensual máximo para herramientas en los primeros 3 meses?
5. ¿Prefiere empezar por **A (cotizador)** o quiere comparar primero con otro candidato de la sección 2?
6. ¿Tiene ya un nombre de marca o dominio? ¿Quiere explorar opciones?
7. ¿Será una empresa formal desde el inicio o primero validará como independiente?
8. ¿Hay algún cliente potencial real con quien probar la primera demo?

---

## 14. Primera tarea para Claude Code

1. Lee este archivo completo.
2. Responde con un **resumen de lo que entendiste** en 10 líneas o menos.
3. Haz **la primera pregunta** de la sección 13.
4. Cuando tengas suficiente contexto, propón:
   - el stack final (justificado y con costos estimados),
   - el plan de etapas para llegar al MVP de la sección 4,
   - el esquema de datos definitivo,
   - la estructura del proyecto.
5. **No escribas código hasta que el fundador apruebe el plan.**

---

## 15. Glosario

| Término | Significado |
|---|---|
| **Micro app** | App pequeña de una sola tarea, entregada en días |
| **PWA** | Web app instalable en la pantalla de inicio, sin tienda de apps |
| **Multi-tenant** | Un mismo sistema sirve a muchos negocios con datos separados |
| **Paquete de nicho** | Configuración y contenido que adapta la plataforma a un tipo de negocio |
| **MVP** | Versión mínima que ya demuestra valor |
| **MRR** | Ingreso mensual recurrente |
| **Responsable / Encargado** | Roles de la ley de datos: quien decide sobre los datos y quien los trata por encargo |

---

*Fin del documento. Mantenerlo actualizado: es la memoria del proyecto.*
