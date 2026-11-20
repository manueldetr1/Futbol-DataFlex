\# Sprint 2: Navegación DrillDown en DataFlex 25.0



Documentación técnica y resolución de los ejercicios de navegación entre vistas.



\---



\## 1. `From main` → `Select - Zoom`: ¿Qué pasa con `InvokingObject`?



En el flujo estándar de navegación desde un listado (`Select`) hacia una ficha de detalle (`Zoom`) mediante \*\*`ntNavigateForward`\*\*:



\* \*\*Participantes:\*\*

&#x20; \* \*\*`InvokingView`:\*\* La vista de listado (`Select`, por ejemplo `oSelectLiga`).

&#x20; \* \*\*`InvokingObject`:\*\* El control concreto desde el que se dispara la navegación, es decir, la lista \*\*`cWebList` (`oList`)\*\*.

&#x20; \* \*\*`InvokedView`:\*\* La vista de detalle que se abre (`Zoom`, por ejemplo `oZoomLiga`).

\* \*\*Al navegar hacia adelante (`ntNavigateForward`):\*\*

&#x20; \* Al seleccionar una fila en `oList` (`InvokingObject`), se ejecuta el evento \*\*`OnGetNavigateForwardData`\*\*.

&#x20; \* Como `Select` y `Zoom` comparten el mismo \*Data Dictionary\* principal, `InvokingObject` empaqueta automáticamente el \*\*`RowId`\*\* del registro seleccionado y lo envía a la vista `Zoom` (que lo recibe en \*\*`OnNavigateForward`\*\*) para cargar esa ficha. Si se pulsa \*Nuevo\*, envía un `RowId` vacío.

\* \*\*Al volver atrás (`NavigateBack`):\*\*

&#x20; \* Al guardar cambios o retroceder desde el \*Breadcrumb\*, el `Zoom` ejecuta \*\*`OnGetNavigateBackData`\*\* devolviendo el registro actualizado.

&#x20; \* El \*\*`InvokingObject` (`oList`)\*\* recibe esos datos en \*\*`OnNavigateBack`\*\* y \*\*refresca automáticamente la fila modificada\*\* manteniendo el foco/selección sobre ella sin necesidad de recargar toda la tabla.



\---



\## 4. `Undefined` → `ZoomJugador - SelectPosicion`: ¿Hay relación entre ellas?



\* \*\*Implementación:\*\* Se añadió el botón `oVerPosicionesBtn` en `ZoomJugador.wo` configurado con `WebRegisterPath ntNavigateForwardCustom oSelectPosicion`.

\* \*\*¿Hay relación entre ellas?:\*\* \*\*No existe una relación directa padre-hijo\*\* entre las tablas `jugador` y `posicion` en la base de datos ni en los \*Data Dictionaries\* (su relación es muchos a muchos `N:M`, resuelta a través de la tabla intermedia `jugadorposicion`).

\* \*\*Consecuencia en la navegación:\*\* Al no existir relación DD directa entre ambas vistas, DataFlex clasifica la navegación como \*\*`nfUndefined`\*\* y se emplea \*\*`ntNavigateForwardCustom`\*\*, lo que abre `SelectPosicion` sin intercambio ni filtrado automático de registros entre vistas.

