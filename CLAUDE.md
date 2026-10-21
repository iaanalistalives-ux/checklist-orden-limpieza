# CLAUDE.md — Check List de Orden y Limpieza (checklist-orden-limpieza)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
Check list diario de orden y limpieza por fábrica (Chorrillos y Barranco). **Es una de las pantallas que se usan por link directo, fuera del portal**, sin usuario ni clave del portal; por eso no lleva el aviso de portal ni el bloque `lives-portal-obligatorio`.
- `index.html` — **formulario de celular** en 3 pasos:
  - 1. Datos: fecha (por defecto hoy; no acepta fechas futuras; avisa si es fin de semana), fábrica y auditor (se elige de una lista).
  - 2. Secciones colapsables con ítems numerados, cada uno OK / X / N/A (tocar de nuevo quita la marca). Con X pide "¿Qué se encontró?". "Marcar toda la sección OK". Barra de progreso; Enviar queda desactivado hasta evaluar todos los ítems.
  - 3. Check list enviado, con código y resumen.
  - Un check list vigente por día y fábrica: si ya existe, ofrece corregirlo (reemplaza al anterior).
  - Sin señal el envío queda en cola en el celular y se reenvía con "Reintentar"; un borrador se autoguarda con cada marca. Enviar dos veces no duplica (cada envío lleva un id).
- `dashboard.html` — **tablero semanal en vivo**, solo lectura (se refresca cada 45 s): filtros de fábrica y semana; KPIs de cumplimiento, no conformidades, días registrados, marcas pendientes e ítems; hoja por fábrica (ítems × días con ✓ ✗ N/A y "!" = día pasado sin registro, que no cuenta como incumplimiento sino como falta de auditoría); tabla de no conformidades con su observación. "Estadísticas acumuladas": semanal, mensual y anual (el comparativo anual va en líneas, una por año). Descarga a PDF (elige semanas; imprime en A4 horizontal) y Excel LIVES.

## Cómo se calcula
- % de cumplimiento = OK ÷ (OK + X) × 100. Los N/A no entran al denominador. Los promedios acumulados son ponderados (suma de OK ÷ suma de evaluados).
- No hay meta definida por LIVES.

## Base de datos (Supabase lives-datos)
- Solo 4 funciones: `ol_inicio` (catálogo de ítems y lista de auditores), `ol_dia` (si ya hay un check list de ese día y fábrica), `ol_guardar` (un check list completo con todas sus respuestas) y `ol_datos` (tablero; devuelve solo lo que cambió desde la última lectura).
- Tablas: `ol_catalogo`, `ol_auditores`, `ol_checklists`, `ol_marcas` (una por ítem y día), `ol_bitacora`. Al 21/10/2026: 111 check lists y 1.899 marcas.
- No usa Storage ni fotos.
- Validación en la base: el auditor tiene que ser de la lista, la fábrica y los ítems tienen que existir en el catálogo, y la fecha no puede ser futura.

## Quién la usa
Entra por link directo, así que no queda en el registro de entradas del portal. Lo llenan los supervisores de cada fábrica.

## Quién puede registrar
- Cualquiera con el link puede abrir el formulario y elegir un auditor de la lista; la base valida lo que llega. Excepción acordada, igual que Solicitud de Movilidad y Entregas del chofer.
- A Sistemas: es una pantalla sin usuario; si pasa al sistema de Sistemas, decidir cómo se identifica el auditor.

## Pendientes vistos al escribir esto (21/10/2026)
- `index.html` trae en el código, como respaldo, una lista con nombres de 2 auditores (dato personal en un repo público); conviene quitarla y que la lista salga solo de la base.
- El dashboard tiene preparada una "copia estática" para revisión externa (variables `DATOS_EMBEBIDOS`, hoy vacías); la herramienta que la rellena no está en este repo.
- No se pudo confirmar desde el código propio qué datos entrega el dashboard al bloque de Excel.
