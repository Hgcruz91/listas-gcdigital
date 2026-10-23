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
