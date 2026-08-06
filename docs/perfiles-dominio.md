# §6 · APÉNDICE — PERFILES DE DOMINIO

Categorías de referencia para rellenar §3 más rápido. **Adáptalas, no las copies tal cual.**

### (a) Datos sensibles regulados (salud, financieros, biométricos)
- P0: el dato regulado + su vínculo con la identidad.
- Consentimiento explícito, retención definida y derecho de borrado.
- Prohibido cualquier log con el dato o con identificadores.
- Requiere revisión legal antes de operar con usuarios reales.

### (b) Datos de ubicación de personas
- P0: coordenadas + identidad + marca de tiempo (juntos revelan rutinas).
- Guardar con precisión mínima necesaria; agregar o difuminar cuando se muestre en público.
- Quitar EXIF de las imágenes.
- Evaluar si alguien corre riesgo al ser identificado como origen de un dato.

### (d) Datos financieros y tributarios de un negocio

- P0: ingresos, gastos, deudas y situación tributaria **vinculados a un contribuyente identificable**.
  Filtrados revelan la salud real de un negocio a su competencia, a un acreedor o a un extorsionador.
- **Nunca replicar un trámite oficial.** Si la app calcula o presenta algo que legalmente debe
  presentarse ante la autoridad tributaria, el aviso va **dentro del flujo**: la app es una ayuda, no
  el canal oficial (§3 pregunta 5).
- **Trazabilidad e inmutabilidad:** los registros contables se **corrigen con un asiento nuevo**,
  nunca editando el anterior. Un histórico editable no sirve como prueba.
- **Exactitud aritmética verificada por test.** Un error de redondeo en un impuesto no es un defecto
  cosmético: es una declaración incorrecta. Los importes se manejan en enteros o decimales exactos,
  **jamás en coma flotante**.
- Retención acorde al plazo legal de conservación de documentos tributarios del país.
- Prohibido cualquier log con importes, identificadores tributarios o nombres de clientes.
- Requiere revisión contable y legal antes de operar con usuarios reales.

### (c) Sin datos sensibles
- P0 se reduce a secretos, autenticación e integridad de los datos.
- El núcleo (§2), la Fase 0 (§4) y las trampas (§5) **siguen siendo obligatorios**: no dependen del
  tipo de dato.

---

_Plantilla Enterprise reutilizable. Al instanciarla: rellena §1 y §3, ejecuta §4, borra §0._
