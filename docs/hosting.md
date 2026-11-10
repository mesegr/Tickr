# Decisión de hosting (A1)

## 1. Decisión

Se aloja Tickr en **MonsterASP.NET, plan gratuito**, con base de datos **SQL Server**.

### Por qué

- Es gratuito y no pide tarjeta de crédito.
- Incluye SQL Server.
- No obliga a recrear la base de datos cada mes (con el plan gratuito de Postgres de Render (Plan b) sí habría que hacerlo).
- El apagado por inactividad es de 3 horas, frente a 15 minutos en Render.
- Admite despliegue desde GitHub Actions con Web Deploy.

### Criterios de partida

1. 100 % gratis.
2. Accesible para cualquiera en cualquier momento, aunque la primera visita pueda tardar más por el apagado por inactividad.
3. Recuperable en un par de horas si se pierde la cuenta (ver sección 5).

## 2. Límites del plan gratuito

| Límite | Valor | Fuente | Consultado |
|---|---|---|---|
| Sitios, espacio, memoria, base de datos | 1 sitio, 5 GB, 256 MB de RAM, 1 base de datos de 1 GB | Web y términos de MonsterASP.NET (<https://www.monsterasp.net/Terms/>), lectura del autor | 8 nov 2026 |
| Dominio propio | No (subdominio del proveedor) | Ídem | 8 nov 2026 |
| Región | Solo centro de datos de la UE | Ídem | 8 nov 2026 |
| Base de datos | SQL Server | <https://www.monsterasp.net/ASP.NET-Freehosting/> | 10 nov 2026 |
| Tarjeta | No requerida | <https://help.monsterasp.net/websites/configuration/always-running> (banner informativo) | 10 nov 2026 |
| Apagado por inactividad | 3 horas sin ninguna petición; cada petición reinicia la cuenta atrás. La primera petición posterior es más lenta | <https://help.monsterasp.net/websites/configuration/always-running> (actualizada el 2 nov 2026) | 10 nov 2026 |
| AlwaysRunning (sin apagado) | No disponible en el plan gratuito | Ídem | 10 nov 2026 |
| Archivado | Webs y bases de datos gratuitas sin uso durante 6 meses se archivan. Se puede pedir su restauración por ticket | <https://help.monsterasp.net/troubleshooting/common-errors/website-archived> (actualizada el 5 oct 2026) | 10 nov 2026 |
| Qué cuenta como uso | Peticiones HTTP a la web y conexiones a la base de datos. Despliegues e inicios de sesión en el panel no cuentan | Respuesta del soporte (ver sección 4) | 10 nov 2026 |
| Copias de seguridad | No automáticas. Se pueden crear y descargar a mano un archivo BAK desde el panel |  Respuesta del soporte (ver sección 4) | 10 nov 2026 |
| Despliegue continuo | GitHub Actions con Web Deploy funciona en el plan gratuito |  Respuesta del soporte (ver sección 4) | 10 nov 2026 |
| Tareas programadas del panel | Solo en Premium; se recomienda un servicio de monitorización externo | Respuesta del soporte | 10 nov 2026 |

### Condiciones de uso (según los términos)

- El plan gratuito está pensado para desarrollo, pruebas, aprendizaje y evaluación.
- Puede cancelarse o borrarse tras un periodo de inactividad.
- Se prohíben los datos personales reales en producción.

Por eso FichaJornada usa **solo datos de demostración ficticios** y no es una herramienta para empresas reales (ver aviso del README).

## 3. Riesgos aceptados y cómo se mitigan

| Riesgo | Mitigación prevista |
|---|---|
| Arranque en frío tras 3 horas sin visitas | Aviso en el portfolio. Migraciones y datos de demo **fuera** del arranque de la app. |
| Archivado a los 6 meses sin uso | Tráfico real, o un servicio de monitorización externo que consulte un health check. |
| Sin copias automáticas | Descargar un BAK a mano de forma periódica. Los datos de demo se pueden regenerar con la semilla repetible |
| Dependencia de un proveedor pequeño | Diseño portable: configuración por variables de entorno, sin depender del disco local, proveedor de base de datos intercambiable |

## 4. Consulta al soporte

- **Ticket enviado:** [10/11/2026]
- **Respuesta recibida:** [11/11/026]
- **Resumen de la respuesta:** periodo de archivado y qué cuenta como uso; apagado tras 3 horas y arranque en frío dependiente del código de la app; copias manuales desde el panel; despliegue con GitHub Actions soportado en el plan gratuito.
- **No respondido de forma explícita:** si están permitidos los pings periódicos de comprobación. La documentación indica que un servicio de monitorización mantiene viva la web, por lo que se considera permitido de forma **implícita**, no confirmado.

El texto íntegro de la respuesta se conserva fuera del repositorio.

## 5. Recuperación (requisito: volver a levantar todo en un par de horas)

Si se pierde la cuenta, los únicos pasos manuales previstos son: crear la cuenta, crear una base de datos vacía y activar Web Deploy copiando sus valores a los secretos de GitHub. El resto (migraciones, datos de demo, despliegue) debe ser repetible y automático. Los detalles irán en el runbook del README.

## 6. Alternativas valoradas

### Plan B: Render (servicio web con Docker y Postgres gestionado)

- El servicio gratuito se duerme tras 15 minutos sin tráfico y tarda alrededor de 1 minuto en volver a arrancar.
- La base de datos Postgres gratuita caduca a los 30 días y no tiene copias, lo que obligaría a recrearla cada mes y a abandonar SQL Server.
- Una app .NET se despliega con Docker; la compatibilidad con el plan gratuito se apoya en fuentes comunitarias, **no verificada en la documentación oficial**.
- **Fuente:** documentación oficial de Render sobre planes gratuitos. **Volver a comprobarla antes de activar este plan** (URL y fecha pendientes de anotar).
- **Cuándo activarlo:** si MonsterASP.NET archiva o borra el servicio sin posibilidad de restaurarlo, si las condiciones del plan gratuito empeoran o si el arranque en frío resulta inaceptable.

### Descartadas por ahora

| Alternativa | Motivo | Nivel de verificación |
|---|---|---|
| Azure for Students / App Service gratuito | Elegibilidad dudosa (la oferta para estudiantes universitarios no está clara para un ciclo de FP) y cuotas del plan gratuito contradictorias entre fuentes | Parcial |
| Google Cloud Run | Exige cuenta de facturación (tarjeta) y puede generar cobros | Parcial |
| Oracle Cloud Always Free | Exige tarjeta, reclama las máquinas inactivas, la capacidad gratuita Arm se ha reducido y obliga a administrar el servidor | Fuentes en parte secundarias |
