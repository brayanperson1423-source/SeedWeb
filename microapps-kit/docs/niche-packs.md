# Cómo crear un paquete de nicho

Un paquete de nicho **solo contiene configuración y contenido**, nunca lógica nueva.
Si necesitas editar código para crear un nicho, la arquitectura necesita ajustarse.

## Contenido de un paquete

- Módulos activados (catálogo, clientes, documentos, agenda, pedidos, inventario...)
- Etiquetas (cómo llama este nicho a cada cosa)
- Plantillas de documento (PDF)
- Plantillas de mensajes de WhatsApp
- Datos de ejemplo para la demo
- Textos de onboarding

## Ejemplo (referencial)

```yaml
niche_pack:
  id: cotizador-talleres
  name: "Cotizador para talleres y ferreterías"
  modules: [catalog, clients, documents, reports]
  labels:
    document: "Cotización"
    client: "Cliente"
    item: "Producto / servicio"
  whatsapp_templates:
    send_quote: "Hola {{cliente}}, te comparto tu cotización {{numero}} por {{total}}. {{enlace}}"
  demo_data: ./demo/talleres.json
```

## Pasos para crear uno nuevo

1. Copiar un paquete existente como base.
2. Cambiar id, nombre, etiquetas y módulos.
3. Escribir las plantillas de mensajes.
4. Preparar datos de ejemplo realistas del nicho.
5. Probar la demo completa en celular.
6. Registrar el nuevo paquete en `docs/decisiones.md`.
