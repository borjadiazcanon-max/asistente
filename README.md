# Asistente de ventas con IA

Un servicio que atiende a los clientes potenciales de un negocio, registra contactos y ventas,
y muestra al dueño sus resultados (ventas atribuidas y margen).

## Variables en Railway (servicio `asistente-ventas`)

| Variable | Valor |
|---|---|
| `DATABASE_URL` | `${{Postgres.DATABASE_URL}}` (referencia al servicio Postgres) |
| `ANTHROPIC_API_KEY` | tu clave de console.anthropic.com |
| `ADMIN_TOKEN` | una contraseña larga, solo tuya |

## Uso

1. **Crear un negocio** (te devuelve su `api_key`):

```bash
curl -X POST https://TU-DOMINIO/admin/business \
  -H "X-Admin-Token: TU_TOKEN" -H "Content-Type: application/json" \
  -d '{"slug":"mi-negocio","name":"Mi Negocio","info":"Servicios, precios, horarios, preguntas frecuentes..."}'
```

2. **Probar el asistente**:

```bash
curl -X POST https://TU-DOMINIO/chat \
  -H "X-Business-Key: API_KEY_DEL_NEGOCIO" -H "Content-Type: application/json" \
  -d '{"business":"mi-negocio","contact":"cliente-1","message":"Hola, ¿cuánto cuesta?"}'
```

3. **Registrar una venta** (así se calcula el margen y lo atribuido al asistente):

```bash
curl -X POST https://TU-DOMINIO/admin/sales \
  -H "X-Admin-Token: TU_TOKEN" -H "Content-Type: application/json" \
  -d '{"business":"mi-negocio","contact":"cliente-1","amount":45000,"cost":18000}'
```

4. **Ver resultados**: abre `https://TU-DOMINIO/panel`.

`contact` es el identificador del cliente en el canal (teléfono de WhatsApp, usuario de Instagram o
id de sesión web). Una venta se atribuye al asistente si ese contacto conversó con él.

## Conectar un canal

El asistente no depende del canal: WhatsApp, Instagram o una web solo deben llamar a `POST /chat`
con el mensaje y devolver la respuesta al cliente. Guarda `X-Business-Key` solo en el servidor del canal.
