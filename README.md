# Cursos V2 - PRUEBAS

Dos páginas independientes: `Admin.html` y `Tv.html`. Preparadas para GitHub Pages. La hoja privada solo almacena datos; Apps Script actúa como API. Cursos original, hoja antigua y Yodeck no se modifican.

## Acceso

Cada enlace lleva una llave larga en el fragmento, no en la petición GET. El servidor verifica su huella en cada operación. La llave de tele solo permite leer sesiones activas de hoy a +14 días; no puede escribir ni borrar. La llave de administración permite editar la hoja de pruebas. Quien tenga esa llave puede administrar: no hay verificación de identidad personal. No publicar ni reenviar el enlace de administración.

Las llaves NO están en el código ni en el repositorio. Se conservan en la sesión de la pestaña y el fragmento se limpia de la barra. Para abrir otra pestaña o navegador, usar el favorito original. Revocar y crear nuevos enlaces invalida los anteriores.

## Configuración inicial, solo propietario

En Apps Script: `Code.gs` y `Setup.html`. El despliegue de pruebas ejecuta como propietario y permite acceso externo; la API rechaza todas las operaciones sin llave válida. Las funciones internas de datos terminan en `_` y no pueden llamarse desde el cliente. La configuración de enlaces exige la cuenta del propietario.

Propiedades privadas:
- `SPREADSHEET_ID`: hoja NUEVA de pruebas, pestaña SESIONES.
- `ADMIN_EMAIL`: propietario autorizado para la configuración inicial.
- `FRONTEND_BASE_URL`: dirección de Cursos-V2 en GitHub Pages, sin barra final.

Una sola vez, el propietario abre `?view=setup` y crea sus dos enlaces. No usa esa pantalla a diario. La administración y tele se abren siempre desde los favoritos de GitHub. Las vistas de Google incluidas son auxiliares de pruebas, no el alojamiento final.

No activar GitHub Pages ni pasar a producción hasta la decisión del propietario. No colocar las llaves, IDs privados ni nombres de empleados en archivos públicos.

## Prueba local sin hoja

`npm ci` y `npm run check`. Abrir `Admin.html?demo=1` para almacenamiento local explícito. Abrir `Tv.html?demo=1` en el mismo origen para leer esas pruebas. Sin `demo=1` ni llave, la web rechaza el acceso y no cambia a almacenamiento local silenciosamente.

## Datos y comportamiento

Una fila por sesión. Columnas: ID_CURSO, ID_SESION, CATEGORIA, CURSO, FECHA, HORA_INICIO, HORA_FIN, LUGAR, CONVOCADOS, ACTIVO, CREADO_EN, ACTUALIZADO_EN, VERSION. Fechas ISO y zona Europe/Madrid. Convocados por sesión, sin fusionar cursos por título. Bloqueo y VERSION impiden pisar cambios concurrentes. Campos escapados al escribir en la hoja.

Tele: una tarjeta por página, rotación 15 s, actualización 45 s, prioridad HOY > MAÑANA > NUEVO. NUEVO dura 24 horas desde la primera aparición en esa tele; la previsualización no consume ese contador. Error de red: estado de desconexión, no agenda antigua presentada como actual.

Admin C: calendario por colores y agenda, previsualización del mismo documento de tele a 590×408, escalado proporcional. Splash del CRM Isopan con SVG original, solo durante carga y con salida también en error.

## Verificación y límites

16 tests automáticos de fechas, sellos, persistencia, llaves, revocación, permisos de tele, concurrencia y lectura ACTIVO en minúsculas. Pruebas de UI: sin llave muestra error y retira splash; llave viaja en POST y se limpia del fragmento; recarga conserva sesión de pestaña. Puente real entre dominios comprobado con rechazo de petición sin llave.

Pendiente: ciclo integrado real guardar/leer/editar/borrar con llave válida contra la hoja nueva, verificación de tele con llave, revisión en dispositivo y aprobación para activar Pages. No se ha activado producción. No hay promesa de disponibilidad ni protección contra abuso a escala; se usan cuotas gratuitas de Google y un límite básico de peticiones.
