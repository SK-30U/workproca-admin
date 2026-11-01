# Administración Workproca — versión independiente

La app es un solo archivo (`index.html`). Los datos viven en **Firebase Firestore** (Google) y la página se publica en **GitHub Pages**. Entras con tu cuenta de Google; solo los correos que pongas en las reglas pueden ver o editar.

Qué hay en esta carpeta:

| Archivo | Para qué |
|---|---|
| `index.html` | La app completa |
| `firebase-config.js` | Las claves de tu proyecto de Firebase (las pegas tú, paso 4) |
| `firestore.rules` | Reglas de acceso: quién puede entrar (paso 3) |
| `manifest.webmanifest`, `sw.js`, `icon-*.png` | Para instalarla en el iPhone y abrirla sin señal |

Todo se hace desde el teléfono o la computadora, una sola vez. Son unos 15 minutos.

## 1. Crear el proyecto de Firebase

1. Entra a **https://console.firebase.google.com** con `fran.laguna.v@gmail.com`.
2. **Crear un proyecto** → nombre `workproca` → desactiva Google Analytics → **Crear proyecto**.

## 2. Activar la entrada con Google

1. Menú izquierdo → **Compilación → Authentication** → **Comenzar**.
2. Pestaña **Método de acceso** → **Google** → **Habilitar** → elige tu correo como correo de asistencia → **Guardar**.
3. Pestaña **Configuración** → **Dominios autorizados** → **Agregar dominio** → `sk-30u.github.io`.

## 3. Crear la base de datos y poner las reglas

1. **Compilación → Firestore Database** → **Crear base de datos**.
2. Ubicación: `southamerica-east1` (São Paulo) o la que salga por defecto → **Modo de producción** → **Crear**.
3. Pestaña **Reglas** → borra todo y pega el contenido de `firestore.rules` → **Publicar**.
   Esa lista es solo para dueños: quien esté ahí ve y cambia todo. Para que alguien registre sin ver dinero (secretaria, almacenista), no lo pongas ahí: dale acceso desde la app (ver «Personas con acceso»).

## 4. Copiar las claves a la app

1. Engranaje (arriba a la izquierda) → **Configuración del proyecto** → baja hasta **Tus apps** → icono **`</>`** (web).
2. Apodo `Workproca` → **no** marques Firebase Hosting → **Registrar app**.
3. Aparece un bloque `const firebaseConfig = { apiKey: "...", ... }`. Copia los valores a `firebase-config.js` (en GitHub: abre el archivo → lápiz → pega → **Commit changes**).
   Esas claves no son secretas: la protección son las reglas del paso 3.

## 5. Publicar en GitHub Pages

1. En el repositorio **SK-30U/workproca-admin** → **Settings** → **Pages**.
2. **Build and deployment** → Source: **Deploy from a branch** → Branch: `main`, carpeta `/ (root)` → **Save**.
3. En uno o dos minutos la app queda en **https://sk-30u.github.io/workproca-admin/**

## 6. Cargar tus datos y instalarla

1. Abre la dirección en Safari → **Entrar con Google**.
2. **Gastos → Ajustes → Restaurar desde respaldo (JSON)** → elige el archivo `respaldo-workproca-2026-10-30.json` (guárdalo antes en Archivos del iPhone). Carga trabajos, pagos, trabajadores, asistencia y movimientos tal cual estaban.
3. Botón **Compartir** de Safari → **Agregar a pantalla de inicio**. Desde ahí abre como aplicación, a pantalla completa y con caché para abrirla sin señal.

## Qué cambia respecto a la versión de Claude

- Los asistentes ("Cargar con asistente", "Registrar con asistente", "Responder al asistente") trabajan con la **API de Claude** y una clave tuya. Se consigue en la Consola de Claude (API keys) y se paga por uso con crédito prepagado, aparte del plan Max. Pégala en **Gastos → Ajustes → Asistente de Claude**, toca **Probar el asistente** y luego **Guardar ajustes**. La clave queda en tu Firestore, protegida por las mismas reglas: quien tenga acceso a la app puede usarla.
- Donde la API de Claude no tenga servicio, el asistente avisa y no responde; todo lo demás sigue funcionando.
- **Saldo de Claude IA:** la API no informa cuánto crédito queda, así que la app lo estima: en **Gastos → Ajustes → Asistente de Claude** registras el crédito comprado (total acumulado) y la app le resta el costo de cada consulta, calculado con los tokens de la respuesta a precios de lista. El saldo estimado aparece en la hoja del asistente después de cada consulta y en una tarjeta de **Resumen**; el exacto siempre está en la Consola de Claude. Al recargar crédito, actualiza el total en Ajustes.
- Todo lo demás es igual: cobranzas, nómina, cortes, recibos, gastos, balance, costos, inventario (existencias por lotes, movimientos, herramientas y equipos), resumen y respaldos.
- Desde que empieces a usar esta versión, usa solo esta. Si registras cosas en las dos, los datos se separan.
- Respaldo: **Gastos → Ajustes → Descargar respaldo completo (JSON)** de vez en cuando; con ese archivo se restaura todo en cualquier momento.

## Personas con acceso (secretaria, supervisor)

Pueden registrar asistencia, adelantos, gastos, compras que llegan, retiros, devoluciones y conteos de material, pero **nada cambia hasta que tú lo apruebes**. No ven dinero: ni montos de trabajos, ni pagos, ni tarifas, ni caja. Tu app les mantiene copias livianas (solo nombres de órdenes, trabajadores y categorías) y es lo único que leen.

1. **Una sola vez:** Firebase › Firestore Database › **Reglas** → borra todo, pega el contenido nuevo de `firestore.rules` → **Publicar**. Sin este paso la persona invitada no puede entrar. Las reglas de esta versión son distintas a las anteriores: vuelve a pegarlas aunque ya lo hayas hecho.
2. En la app: **Gastos → Ajustes → Personas con acceso** → escribe su correo de Google y su nombre → **Dar acceso**.
3. Ella abre **https://sk-30u.github.io/workproca-admin/** y entra con ese correo. Ve solo la pantalla **Registrar**, con la banda de días que faltan por registrar.
4. Lo que envíe aparece en **Resumen → Por aprobar**, agrupado por día y tipo, con totales y los posibles duplicados marcados. Tocas **Aprobar**, **Rechazar** (puedes escribir el motivo, ella lo ve) o **Aprobar todo**. Si tú cambiaste ese mismo registro después de que lo envió, la app te muestra las dos versiones antes de pisar nada.
5. Ella ve en **Lo que enviaste** qué está pendiente, aprobado o rechazado, y puede **Retirar** lo pendiente si se equivocó.
6. Para quitarle el acceso: **Gastos → Ajustes → Personas con acceso → Quitar acceso** (o **Eliminar**).

Abre la app tú primero después de actualizar: tu sesión es la que crea las copias livianas que leen las personas invitadas.

## Actualizaciones

Cuando haya una versión nueva de `index.html`, se reemplaza el archivo en el repositorio y listo: la app se actualiza sola la próxima vez que se abra con señal.
