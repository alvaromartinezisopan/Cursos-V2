# Cursos V2 - PRUEBAS

Código nuevo e independiente de Cursos. Sin conexión a Yodeck, en pruebas, sin conexión pública a la hoja y sin cambios en la hoja antigua.

## Estado

- Administrador nuevo: varias sesiones con convocados propios; memoria de personas, lugares y cursos; calendario mensual; vista previa; revisión antes de guardar; búsqueda, filtros y confirmación de borrado.
- Pantalla nueva: una tarjeta por página, rotación cada 15 segundos, refresco de datos cada 45 segundos, filtro desde hoy hasta +14 días incluido, zona Europe/Madrid, sellos HOY > MAÑANA > NUEVO. NUEVO dura 24 horas desde la primera aparición del ID de sesión en cada navegador.
- Error o datos sin verificar: muestra "Sin conexión", no una agenda antigua como si fuera actual.
- La hoja nueva es privada y su contenido no se incluye en el repositorio.
- Incluye adaptador de Apps Script para pruebas. El backend para GitHub Pages está pendiente de decisión e implementación.
- Código público saneado. Sin nombres de empleados, ID de hoja ni credenciales. Pages en modo local de pruebas no lee ni escribe la hoja.

## Prueba local

1. Con Node 20 o superior: `npm ci`.
2. Ejecutar `npm run check` (pruebas automáticas y compilación).
3. Abrir `Admin.html` en el navegador. Guardará únicamente en el almacenamiento local de ese navegador.
4. Abrir `Tv.html?demo=1` desde el mismo origen/navegador para probar la agenda local. El origen debe ser el mismo; un servidor local evita diferencias de almacenamiento entre archivos locales. No usar datos sensibles en un servidor público.
5. Los archivos `Admin.html` y `Tv.html` se generan a partir de las fuentes. `dist/` es otro resultado de compilación, no se versiona.

No hay librerías externas ni descargas al abrir los HTML. La dependencia esbuild solo se usa al compilar.

## Conexión futura a la hoja de PRUEBAS

La hoja nueva es **Cursos V2 - PRUEBAS**:
[Enlace privado, disponible solo para su propietario]

Este apartado documenta lo pendiente. No es una instrucción de puesta en producción. Antes de pedir una acción al propietario, confirmar y guiarle paso a paso.

- En un proyecto Apps Script nuevo, copiar `Code.gs`, `Admin.html` y `Tv.html`.
- Propiedades del script: `SPREADSHEET_ID` con el ID de la hoja nueva y `ADMIN_EMAIL` con la cuenta que la administra.
- Despliegue de PRUEBAS: ejecutar como propietario y acceso **Solo yo**. Nunca "Cualquiera" ni permiso anónimo para el administrador.
- El administrador usa `google.script.run`; no hay contraseñas compartidas ni tokens en la URL.
- La vista de tele se abre con `?view=tv`, dentro del mismo despliegue privado. La compatibilidad del inicio de sesión en Yodeck queda por estudiar, sin cambiar su configuración. No se ha autorizado ni diseñado una publicación anónima de nombres.
- Guardados protegidos con bloqueo y VERSION para no pisar una edición concurrente. Validación en cliente y servidor; sin fórmulas procedentes de campos de texto.
- Cambios y borrados reales se probarán solo en la hoja nueva con datos de prueba antes de cualquier uso real.

## Datos

Una fila por sesión. Columnas:
`ID_CURSO, ID_SESION, CATEGORIA, CURSO, FECHA, HORA_INICIO, HORA_FIN, LUGAR, CONVOCADOS, ACTIVO, CREADO_EN, ACTUALIZADO_EN, VERSION`.

FECHA es texto ISO aaaa-mm-dd. CONVOCADOS contiene una persona por línea. ACTIVO es booleano. IDs, fechas de creación/edición y VERSION los gestiona la aplicación. Los cursos con el mismo título no se fusionan. El editor abre una sesión existente; añadir más sesiones crea otras filas del mismo curso.

## Verificado

10 pruebas automáticas: fechas/horas, zona horaria y cambio de hora, filtro temporal, sellos y caducidad, primera aparición por ID, nombres completos, sesiones distintas, control de versión y fallo de almacenamiento.

Pruebas de navegador locales: guardar dos sesiones con personas diferentes, recargar y conservar, editar, cancelar borrado, buscar y filtrar, rotación HOY/MAÑANA y pantalla sin conexión. Revisado visualmente a 320, 390 y 1280 px; tele a 590×408 px.

Pendiente: prueba integrada de Apps Script y permisos, validación del tamaño real de pantalla, listas muy largas, pruebas en dispositivo, despliegue de pruebas y decisión de producción del propietario. Nada está en producción.
