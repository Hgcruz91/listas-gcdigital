# Listas públicas de GC Digital

Archivos que las apps de GC Digital (Ajolote Fiscal, Costeo Artesanal y Copal) consultan
**una vez al día, cuando hay internet**. Las apps no envían nada: solo descargan este archivo.

## `revocadas.json`

Claves de licencia canceladas (por ejemplo, por un reembolso).

- Solo trae **identificadores**: los primeros 16 caracteres de la firma de cada clave.
  No hay nombres, correos ni datos de nadie, y con un identificador no se puede armar una clave.
- Está **firmado** con la llave privada de GC Digital (Ed25519). Las apps comprueban la firma
  con la llave pública que ya traen; si alguien modifica el archivo sin esa llave, lo ignoran.
- Lo publica la **consola de GC Digital** con un botón. No se edita a mano.

Formato:

```json
{
  "datos": "<JSON en base64url: {\"tipo\": \"gcdigital-revocadas\", \"emitida\": \"AAAA-MM-DDTHH:MM:SS\", \"apps\": {\"<producto>\": [\"<id>\", ...]}}>",
  "firma": "<firma Ed25519 de los bytes de datos, en base64url>"
}
```

Las apps solo aceptan una lista con `emitida` igual o más reciente que la última que guardaron.

## `novedades.json` y `avisos.json`

Avisos del SAT y cambios de ley para la sección **Novedades del SAT** de Ajolote Fiscal.

- `novedades.json` es la versión legible, **sin firmar**. Claude propone cada aviso con un Pull Request y HG lo
  revisa y lo une: eso es la aprobación.
- `avisos.json` es lo que baja Ajolote. Lo genera la **consola de GC Digital** con «Firmar y publicar»: valida
  `novedades.json`, lo firma con la llave de GC Digital (tipo `gcdigital-avisos`) y lo sube. No se edita a mano.
- Ajolote decide **en la computadora de cada quien** a quién le toca cada aviso, con sus facturas (régimen,
  proveedor de timbrado, persona física o moral). No envía nada.

Cada aviso lleva `id`, `fecha`, `urgencia` (`pronto`, `enero` o `saber`), `titulo`, `que_hacer` y `fuente`
(nombre y liga oficial https). Opcionales: `texto`, `hasta`, `franja`, `valida`, `mensaje`, `pedir` y `aplica`
(`regimenes`, `pac`, `persona`). Uno o dos avisos por semana como máximo, siempre con fuente y fecha.
