# Actualizaciones a documentos legales (v1)

**Status:** draft
**Fecha:** 2026-05-14
**Owner:** Daniel Rojas Vera
**Sistema afectado:** lazz.cl (sitio público) — Términos de Uso, Política de Privacidad

## Contexto

Los documentos legales publicados en lazz.cl con fecha 2026-05-13 (Versión 1.0) fueron revisados y se identificaron 14 hallazgos: 5 bloqueantes, 5 importantes y 4 menores. Esta spec consolida los cambios de texto necesarios para resolver los hallazgos relativos a redacción y consistencia legal.

Tres hallazgos viven en specs separadas y se ejecutan en paralelo:

- Cifrado real de campos PII: `student-data-encryption.md` (backend).
- Cláusulas obligatorias de Apple en EULA: `apple-eula-clauses.md` (texto legal específico de App Store).
- Verificación técnica del path de imágenes (¿realmente no se persisten?): tarea para Claude Code antes de cerrar esta spec.

## Objetivo

Producir T&C v1.1 y Política de Privacidad v1.1 que:

1. Sean coherentes con la arquitectura D2 de cifrado decidida en `student-data-encryption.md`.
2. Cumplan exigencias mínimas de Ley 21.719 y Ley 19.496.
3. No prometan features inexistentes pre-launch.
4. Cierren las inconsistencias internas detectadas en la revisión.

## No-objetivos

- Cláusulas específicas de Apple (van en `apple-eula-clauses.md`).
- Rediseño visual del sitio lazz.cl.
- Traducciones a inglés (v1 solo español Chile).
- Cookie banner, analytics, o cualquier feature web nueva.

## Decisiones del founder

Todas resueltas el 2026-05-14.

### D-1. Naturaleza legal de "Veta Studios" — ✅ Resuelta: D-1.a

Veta Studios es marca / nombre de fantasía sin personalidad jurídica independiente. Responsable legal: **Daniel Rojas Vera, persona natural**, RUT 17.889.199-5, domicilio Camino San Antonio Km 5.5, Puerto Montt. Cuando se formalice una sociedad (futuro), se re-emite v1.2 del texto.

### D-2. Plan Gratis: cap de cursos — ✅ Resuelta: variante D-2.b ajustada

Decidido: **mantener el cap pero subirlo de 1 → 3 cursos**. Backend actualizado en grader-api `b9cd5e7` (FREE_TIER_COURSE_LIMIT=3 + COURSE_PAYWALL_DETAIL). Privacy §3 dice "Gratis: 100 hojas al mes, hasta 3 cursos". Mantiene presión a upgrade sin frustrar a profes escolares que prueban con su carga real.

### D-3. Feature "compartir resultados con apoderados" — ✅ Resuelta: D-3.a

Sacar la promesa del T&C §2. La app genera reportes; el docente decide cómo compartirlos por sus medios (PDF, WhatsApp, etc.).

### D-4. Email de contacto — ✅ Resuelta: D-4.b

Dejar `contacto@vetastudios.io` (sin crear alias `@lazz.cl`). Se acepta la "inconsistencia visual" — Veta Studios es la marca/canal y vetastudios.io es el dominio propio operativo de Daniel. No vale crear infraestructura de mail nueva pre-launch.

### D-5. Plataformas — ✅ Resuelta: D-5.a

Reformular T&C §4 como "la tienda de aplicaciones desde la cual descargaste Lazz". A prueba de futuro Android.

### D-6. Verificación técnica de fotos no persistidas — ✅ Resuelta: pasa con fix

Verificación ejecutada en grader-api 2026-05-14. Resultado: nuestro código (OMR pipeline + scan endpoint) NO escribía a disco, pero Starlette por default usaba `SpooledTemporaryFile(max_size=1MB)` que hacía rollover de imágenes >1MB a `/tmp`. **Fix shipped en grader-api `b9cd5e7`**: `MultiPartParser.max_file_size = MAX_SCAN_BYTES (8MB)` → imágenes hasta 8MB permanecen en RAM siempre. Tests nuevos en `tests/test_no_disk_writes.py`:
- `test_scan_upload_does_not_rollover_to_disk` — spy sobre `SpooledTemporaryFile.rollover` con upload 4MB.
- `test_scan_upload_leaves_tmp_dir_unchanged` — snapshot de `/tmp` antes/después con upload 7.5MB (complementario al spy).

Promesa en Privacy §2/§9 ("se procesa en memoria y se descarta") ahora es **literalmente cierta** para todo upload bajo el cap.

---

## Cambios en Términos de Uso

Numeración corresponde a las secciones del documento v1.0 actual.

### T&C §1 - Aceptación

**Cambio condicionado a D-1.**

Si D-1.a (default): agregar al final del párrafo de identificación una frase aclaratoria:

> Lazz es operada por **Daniel Rojas Vera**, persona natural, RUT 17.889.199-5, domiciliado en Camino San Antonio Km 5.5, Puerto Montt, Chile, bajo la marca comercial "Veta Studios".

Si D-1.b: reescribir con razón social, RUT de sociedad, domicilio social, representante legal.

### T&C §2 - Descripción del servicio

**Cambio condicionado a D-3.**

Si D-3.a (default):

**Antes:**

> Lazz es una aplicación móvil que permite a docentes corregir pruebas de selección múltiple desde la cámara de un dispositivo móvil, generar reportes por curso y por alumno, y compartir resultados con apoderados.

**Después:**

> Lazz es una aplicación móvil que permite a docentes corregir pruebas de selección múltiple desde la cámara de un dispositivo móvil y generar reportes por curso y por alumno. Los reportes pueden ser exportados y compartidos por el docente a través de sus propios medios.

### T&C §3 - Cuenta del usuario

Sin cambios de texto. Verificación técnica adicional: si la app va a soportar login con providers de terceros (Google, Facebook), Apple exige soportar Sign in with Apple también (guideline 4.8). Si solo es email/password, no aplica. Decisión out-of-scope de esta spec.

### T&C §4 - Suscripciones y pagos

**Cambio condicionado a D-5.**

Si D-5.a (default), reemplazar referencias a "Apple App Store y Google Play" por "la tienda de aplicaciones".

Además, agregar al final de §4 un párrafo nuevo (recomendación Apple guideline 3.1.2):

> La suscripción se renueva automáticamente al final de cada periodo, a menos que la canceles al menos 24 horas antes del fin del periodo en curso. Puedes gestionar y cancelar tus suscripciones desde la configuración de tu cuenta de la tienda de aplicaciones.

### T&C §6 - Propiedad intelectual

**Antes (segundo párrafo):**

> Los datos que generas con la app (clases, claves de respuestas, calificaciones de tus alumnos) son tuyos. Lazz sólo los procesa para prestarte el servicio.

**Después:**

> Los datos que generas y administras con la app (clases, claves de respuestas, calificaciones registradas) están bajo tu control. Lazz solo los procesa para prestarte el servicio. Los datos personales de tus alumnos siguen perteneciendo a ellos o a quienes legalmente los representen; Lazz actúa como encargado de tratamiento de esos datos por cuenta tuya, en los términos del Anexo de Tratamiento de Datos descrito en la sección 12.

### T&C §12 - Anexo de Tratamiento de Datos (NUEVO)

Insertar como sección 12, antes de "Contacto" (que pasa a §13).

> ## 12. Anexo de Tratamiento de Datos
>
> Esta sección regula el tratamiento de datos personales de tus alumnos que se realiza a través de Lazz, y forma parte integrante de los Términos de Uso.
>
> **12.1 Roles.** Tú, en tu calidad de docente usuario, eres el **responsable** del tratamiento de los datos personales de tus alumnos en los términos de la Ley 21.719. Lazz actúa como **encargado** de tratamiento, procesando esos datos por cuenta tuya y bajo tus instrucciones, exclusivamente para prestar el servicio descrito en estos Términos.
>
> **12.2 Finalidades autorizadas.** Lazz tratará los datos de alumnos únicamente para: (a) ejecutar la corrección de pruebas que tú solicites; (b) almacenar los resultados para que puedas consultarlos y generar reportes; (c) aplicar las medidas de seguridad descritas en la Política de Privacidad; (d) cumplir obligaciones legales aplicables. Cualquier otra finalidad requiere tu autorización previa.
>
> **12.3 Confidencialidad.** El personal con acceso a los datos (actualmente limitado al operador del servicio) está sujeto a obligación de confidencialidad.
>
> **12.4 Medidas de seguridad.** Lazz aplica las medidas técnicas y organizativas descritas en la Política de Privacidad, incluyendo cifrado at-rest de los nombres de alumnos mediante claves derivadas por usuario.
>
> **12.5 Sub-encargados.** Lazz utiliza los proveedores listados en la sección 5 de la Política de Privacidad como sub-encargados de tratamiento. Si en el futuro se incorporan nuevos sub-encargados, te avisaremos con antelación razonable a través de la app o por email.
>
> **12.6 Asistencia.** Lazz te asistirá razonablemente para que puedas responder solicitudes de derechos (acceso, rectificación, cancelación, oposición, portabilidad, bloqueo) que tus alumnos o sus representantes te dirijan respecto de datos tratados a través de la app.
>
> **12.7 Notificación de incidentes.** En caso de un incidente de seguridad que afecte datos de tus alumnos, te avisaremos en un plazo máximo de 72 horas desde que tomemos conocimiento, conforme exige la Ley 21.719.
>
> **12.8 Devolución o eliminación.** Al terminar la relación contractual (cancelación de cuenta), eliminaremos o anonimizaremos los datos de tus alumnos en los plazos descritos en la Política de Privacidad, salvo obligación legal de retención.

Renumerar la sección "Contacto" actual de §11 a §13.

### T&C §13 - Contacto (renumerada)

**Cambio condicionado a D-4.**

Si D-4.a (default): cambiar contacto@vetastudios.io por contacto@lazz.cl.

---

## Cambios en Política de Privacidad

### Privacy §1 - Identidad del responsable

**Cambio condicionado a D-1 y D-4.** Mismo tratamiento que T&C §1 y §13.

### Privacy §2 - Qué datos recolectamos

**Cambio 2.1.** Agregar columna "Almacenamiento" a la tabla, o nota al pie en las filas relevantes:

- `Nombre completo (opcional)` del docente → "Cifrado en tránsito (TLS)"
- `Nombres y números de alumnos (opcional)` → **"Cifrados en reposo con clave derivada por usuario (AES-256-GCM + HKDF-SHA256)"**

**Cambio 2.2.** Agregar fila nueva a la tabla:

| Dato | Para qué | Cuándo |
|------|----------|--------|
| Datos técnicos del dispositivo y conexión (IP, tipo de dispositivo, sistema operativo, versión de la app) | Diagnóstico, seguridad, prevención de abuso | Automáticamente al usar la app |

**Cambio 2.3 (condicionado a D-6).** Si la verificación con Claude Code confirma que las imágenes no se persisten, mantener el párrafo "Datos que no persistimos" tal como está. Si la verificación muestra cualquier persistencia (logs, retry, S3 temporal, etc.), reescribir:

> Fotos de hojas de respuesta: la imagen se envía al servidor, donde se procesa por el motor OMR. [Describir aquí qué pasa realmente: si se guarda temporal, por cuánto tiempo, dónde, si se borra después.]

### Privacy §3 - Para qué usamos tus datos

**Cambio condicionado a D-2.**

Si D-2.a (default):

**Antes:**

> Aplicar los límites de tu plan (Gratis = 100 hojas al mes, 1 curso).

**Después:**

> Aplicar los límites de tu plan (Gratis: 100 hojas al mes).

### Privacy §4 - Base de licitud

Sin cambios al texto existente. Validar que las bases mencionadas (ejecución del contrato, consentimiento, obligación legal, interés legítimo) cubren todas las operaciones descritas en §2 y §3. Auto-review.

### Privacy §5 - Terceros

Sin cambios mientras los proveedores actuales sean Apple, Google, RevenueCat, Railway. Cuando se contrate proveedor de email transaccional (Resend, Postmark, etc.) o similar, agregar fila.

### Privacy §9 - Seguridad

Agregar bullet nuevo después de "Tokens de actualización con rotación...":

> - Los nombres de alumnos y los nombres de cursos se almacenan **cifrados en reposo** mediante AES-256-GCM, con claves de cifrado derivadas por usuario (HKDF-SHA256) a partir de una clave maestra protegida. El operador del servicio no almacena los nombres en texto plano en ningún momento.

### Privacy §10 - Menores de edad

**Antes:**

> Lazz está dirigida a docentes mayores de 18 años. No recolectamos datos de menores de edad salvo el nombre del alumno cuando el docente lo escribe. Ese nombre es responsabilidad del docente, quien actúa como tratante de datos de sus propios alumnos bajo su contexto educacional.

**Después:**

> Lazz está dirigida a docentes mayores de 18 años. No solicitamos directamente datos a menores de edad. Si el docente decide ingresar nombres de alumnos en la app, esos nombres pueden corresponder a personas menores de edad.
>
> El docente, en su calidad de educador, es el **responsable** del tratamiento de los datos personales de sus alumnos en los términos de la Ley 21.719. Lazz actúa como **encargado** de tratamiento por cuenta del docente, conforme al Anexo de Tratamiento de Datos incluido en la sección 12 de los Términos de Uso.
>
> Como medida adicional de protección, los nombres de alumnos se almacenan **cifrados en reposo** con una clave derivada por usuario, de modo que ni siquiera el personal del servicio puede leerlos sin la clave maestra correspondiente.

### Privacy §13 - Contacto

**Cambio condicionado a D-4.** Mismo tratamiento que T&C.

**Cambio adicional.** Alinear el texto sobre la Agencia con T&C §10. La Ley 21.719 entra en vigencia diciembre 2026 y la Agencia aún no opera.

**Antes:**

> Reclamos formales: Agencia de Protección de Datos Personales (Chile)

**Después:**

> Reclamos formales: Agencia de Protección de Datos Personales (Chile), cuando se encuentre en funcionamiento conforme a la Ley 21.719.

---

## Cambios transversales

### Versión y fecha

Actualizar header de ambos documentos:

- "Última actualización: [fecha de publicación]" — sustituir 13 de mayo de 2026 por la fecha real de publicación de v1.1.
- "Versión 1.1".

### Historial de versiones

Agregar al final de cada documento (antes del footer) un changelog:

> ## Historial de versiones
>
> - **v1.1 — [fecha]:** Incorporación de cláusula DPA en T&C; alineación de modelo responsable/encargado en Privacy; declaración de cifrado at-rest de datos de alumnos; ajustes menores de consistencia.
> - v1.0 — 13 de mayo de 2026: versión inicial.

---

## Tests

Esta spec no produce código. Los "tests" son revisiones de coherencia.

### Coherencia entre documentos

- Verificar que T&C §6 referencia correctamente la sección 12 (DPA).
- Verificar que Privacy §10 referencia correctamente T&C §12.
- Verificar que todos los emails de contacto coinciden (D-4 aplicado uniformemente).
- Verificar que el responsable legal nombrado coincide en T&C §1, Privacy §1 y footer (D-1 aplicado uniformemente).

### Coherencia con backend

- Privacy §9 dice "cifrado at-rest". Confirmar que `student-data-encryption.md` está en `shipped/` o coordinada para shipping simultáneo. **Esta spec no puede publicarse antes de que el cifrado esté en producción.**
- Privacy §2/§9 dicen "fotos no se persisten". Confirmar verificación de D-6.
- Privacy §5 enumera proveedores actuales. Confirmar que no hay proveedores adicionales en producción (Sentry, analytics, etc.) sin declarar.

### Coherencia con producto

- T&C §2 describe features. Confirmar que todo lo descrito existe en la app al momento de publicar.
- Privacy §3 menciona "Plan Gratis: 100 hojas al mes". Confirmar que el enforcement está implementado en backend.
- T&C §4 menciona tienda de aplicaciones. Confirmar que la integración IAP está en producción.

### Validación legal

- Revisión final por abogado chileno con expertise en Ley 21.719. **Recomendado pero opcional para v1.1.** Costo estimado: CLP 150.000-300.000 por revisión documental. Decisión del founder si lo contrata pre-launch o lo difiere a tracción.

---

## DoD manual

1. Decisiones D-1 a D-6 tomadas y registradas en el cuerpo de esta spec.
2. Verificación técnica D-6 ejecutada por Claude Code con resultado documentado.
3. Texto v1.1 de T&C generado e impreso a PDF.
4. Texto v1.1 de Privacy generado e impreso a PDF.
5. PDFs subidos a lazz.cl en URLs estables (recomendado: `/terminos` y `/privacidad`, NO con versión en URL).
6. Links cruzados funcionando: T&C → Privacy y Privacy → T&C.
7. Email de contacto verificado: enviar mensaje real, confirmar recepción.
8. App Store Connect: URLs actualizadas en metadata (Privacy Policy URL, Terms of Use URL si aplica).
9. Verificar dentro de la app que los links del paywall y onboarding apuntan a las URLs nuevas (esta verificación cae en spec de paywall, pero se valida acá end-to-end).
10. Screenshot de lazz.cl/privacidad y lazz.cl/terminos en mobile y desktop, adjuntos a esta spec.
11. Cifrado en backend (`student-data-encryption.md`) en producción **antes** de publicar el texto que lo declara.

---

## Riesgos

- **Promesa de cifrado sin cifrado real.** Si esta spec se publica antes de que `student-data-encryption.md` esté en producción, los documentos están mintiendo. Mitigado por la condición de coordinación en el DoD.
- **Promesa de no-persistencia de imágenes falsa.** Mitigado por verificación obligatoria D-6.
- **Cláusula DPA insuficiente para academias grandes.** Una academia con contrato corporativo puede exigir DPA dedicado, no anexo en T&C de consumidor. Aceptado: lo abordamos cuando aparezca el primer prospecto de academia.
- **Cambios de Ley 21.719 antes de diciembre 2026.** La Agencia aún no publica reglamentos completos. Posible que haya que ajustar el texto en v1.2 cuando salgan los reglamentos. Mitigado por el campo de "Versión" y el changelog: actualizar es barato.
- **Apple rechaza por falta de cláusulas EULA específicas.** No es riesgo de esta spec; lo cubre `apple-eula-clauses.md`. Pero hay dependencia: esta spec **no es suficiente** para submission a App Store sin la otra.

---

## Acoplamientos

- **`student-data-encryption.md`:** debe estar en `shipped/` antes de publicar esta. Las dos shippean en par.
- **`apple-eula-clauses.md`:** pendiente de crear. Necesaria para submission a App Store. Independiente del cifrado.
- **`paywall-legal-links.md`** (sugerida, no creada aún): la spec del paywall debe mostrar links visibles a Privacy y Terms antes del botón de compra, requerimiento Apple. Out-of-scope de esta spec pero parte del mismo bundle pre-launch.

---

## Próximos pasos

1. Founder responde D-1 a D-6.
2. Claude Code ejecuta verificación D-6 sobre grader-api.
3. Asistente redacta el texto final v1.1 completo de ambos documentos (T&C + Privacy) para revisión.
4. Founder revisa y aplica al sitio.
5. Cifrado en producción.
6. Publicación coordinada.
7. Spec se mueve a `shipped/` con screenshots y PDFs adjuntos.
