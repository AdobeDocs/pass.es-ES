---
title: Lista de comprobación de API de REST V2
description: Lista de comprobación de API de REST V2
source-git-commit: f0001d86f595040f4be74f357c95bd2919dadf15
workflow-type: tm+mt
source-wordcount: '2535'
ht-degree: 0%

---

# Lista de comprobación de API de REST V2 {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Este documento agrega en un solo lugar los requisitos obligatorios y las prácticas recomendadas para los programadores que implementan aplicaciones cliente que consumen autenticación de Adobe Pass [API de REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

El seguimiento de este documento debe considerarse parte de los criterios de aceptación al implementar la API de REST V2 y debe utilizarse como lista de comprobación para garantizar que se hayan realizado todos los pasos necesarios para obtener una integración exitosa.

## Requisitos obligatorios {#mandatory-requirements}

### 1. Fase de registro {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Ámbito de aplicación registrada</i></td>
      <td>Utilice una aplicación registrada con el ámbito de API de REST v2.</td>
      <td>Riesgos que activan respuestas de error "no autorizadas" HTTP 401, sobrecarga de recursos del sistema y aumento de la latencia.</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>Almacenamiento en caché de credenciales de cliente</i></td>
      <td>Almacene las credenciales del cliente en un almacenamiento persistente y vuelva a utilizarlas para cada solicitud de token de acceso.</td>
      <td>Corre el riesgo de perder la autenticación cuando se regeneran las credenciales del cliente.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Almacenamiento en caché de tokens de acceso</i></td>
      <td>Almacene los tokens de acceso en un almacenamiento persistente y vuelva a utilizarlos hasta que caduquen; no solicite un nuevo token para cada llamada a la API de REST v2.</td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".</td>
   </tr>
</table>

### 2. Fase de configuración {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recuperación de configuración</i></td>
      <td>Recupere la respuesta de configuración solo cuando sea necesario solicitar al usuario que seleccione su MVPD (proveedor de TV) antes de la fase de autenticación.<br/><br/>No es necesario recuperar la respuesta de configuración cuando:<ul><li>El usuario ya se ha autenticado.</li><li>Se ofrece acceso temporal al usuario.</li><li>La autenticación del usuario ha caducado, pero se puede pedir al usuario que confirme que sigue siendo suscriptor del MVPD seleccionado anteriormente.</li></ul></td>
      <td>Riesgos de sobrecarga de recursos del sistema y aumento de la latencia.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Almacenamiento en caché de selección de proveedor de TV</i></td>
      <td>Almacene la selección del proveedor de TV de pago (MVPD) del usuario en un almacenamiento persistente para utilizarlo en todas las fases posteriores:<ul><li>En el almacén de respuestas de configuración, el usuario seleccionó "id" de MVPD.</li><li>En el almacén de respuestas de configuración, el usuario seleccionó MVPD "displayName".</li><li>En el almacén de respuestas de configuración, el usuario seleccionó MVPD "logoUrl".</li></ul></td>
      <td>Riesgos de sobrecarga de recursos del sistema y aumento de la latencia.</td>
   </tr>
</table>

### 3. Fase de autenticación {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Iniciando mecanismo de sondeo</i></td>
      <td>Inicie el mecanismo de sondeo en las siguientes condiciones:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Autenticación realizada en la aplicación principal (pantalla)</a></b><ul><li>La aplicación principal (streaming) debe comenzar a sondear cuando el usuario llega a la página de destino final, después de que el componente del explorador cargue la URL especificada para el parámetro "redirectUrl" en la solicitud de extremo de sesiones.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Autenticación realizada en una aplicación secundaria (pantalla)</a></b><ul><li>La aplicación principal (streaming) debe iniciar el sondeo en cuanto el usuario inicie el proceso de autenticación, justo después de recibir la respuesta de extremo de sesiones y mostrar el código de autenticación al usuario.</li></ul></td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Detención del mecanismo de sondeo</i></td>
      <td>Detener el mecanismo de sondeo en las siguientes condiciones:<br/><br/><b>Autenticación correcta</b><ul><li>La información de perfil del usuario se ha recuperado correctamente, lo que confirma su estado de autenticación y, por lo tanto, ya no es necesario realizar encuestas.</li></ul><br/><b>Sesión de autenticación y caducidad del código</b><ul><li>La sesión de autenticación y el código caducan, el usuario debe reiniciar el proceso de autenticación y el sondeo con el código de autenticación anterior debe detenerse inmediatamente.</li></ul><br/><b>Nuevo código de autenticación generado</b><ul><li>Si el usuario solicita un nuevo código de autenticación, la sesión existente se invalida y el sondeo que utiliza el código de autenticación anterior debe detenerse inmediatamente.</li></ul></td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configuración del mecanismo de sondeo</i></td>
      <td>Configure la frecuencia del mecanismo de sondeo en las siguientes condiciones:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Autenticación realizada en la aplicación principal (pantalla)</a></b><ul><li>La aplicación principal (streaming) debe sondear cada 3-5 segundos.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Autenticación realizada en una aplicación secundaria (pantalla)</a></b><ul><li>La aplicación principal (streaming) debe sondear cada 3-5 segundos.</li></ul></td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Almacenamiento en caché de perfiles</i></td>
      <td>Almacene partes de la información de perfil del usuario en un almacenamiento persistente para mejorar el rendimiento y minimizar las llamadas innecesarias a la API de REST v2.<br/><br/>El almacenamiento en caché debe centrarse en los siguientes campos de respuesta de perfiles:<br/><br/><b>mvpd</b><ul><li>La aplicación cliente puede utilizar esto para realizar un seguimiento del proveedor de TV seleccionado del usuario y seguir utilizándolo durante las fases de preautorización o autorización.</li><li>Cuando caduca el perfil de usuario actual, la aplicación cliente puede utilizar la selección de MVPD que se recuerda y solo pedirle que confirme.</li></ul><br/><b>atributos</b><ul><li>Se utiliza para personalizar la experiencia del usuario en función de diferentes claves de metadatos de usuario (por ejemplo, zip, maxRating, etc.).</li><li>Los metadatos del usuario están disponibles una vez finalizado el flujo de autenticación, por lo que la aplicación cliente no necesita consultar un extremo independiente para recuperar la información de metadatos del usuario, ya que ya se incluye en la información del perfil.</li><li>Algunos atributos de metadatos se pueden actualizar durante la fase de autorización, según el MVPD (por ejemplo, Charter) y el atributo de metadatos específico (por ejemplo, householdID). Como resultado, es posible que la aplicación cliente necesite volver a consultar las API de perfiles después de la autorización para recuperar los metadatos de usuario más recientes.</li></ul></td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".</td>
   </tr>
</table>

### 4. (Opcional) Fase de preautorización {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recuperación de decisiones de preautorización</i></td>
      <td>Utilice decisiones de autorización previa para el filtrado de contenido, nunca para las decisiones de reproducción.</td>
      <td>Corre el riesgo de incumplir los acuerdos contractuales entre el programador, las MVPD y Adobe.<br/><br/>Riesgos que se saltan nuestros sistemas de monitoreo y alerta.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Reintento de recuperación de decisiones de preautorización</i></td>
      <td>Gestione correctamente los <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">códigos de error mejorados</a> y utilice el campo de acción para determinar los pasos de corrección necesarios.<br/><br/>Solo un número limitado de códigos de error mejorados justifica un reintento, mientras que la mayoría requiere resoluciones alternativas, como se especifica en el campo de acción.<br/><br/>Asegúrese de que cualquier mecanismo de reintentos implementado para recuperar decisiones de preautorización no resulte en un bucle interminable y de que limite los reintentos a un número razonable (es decir, 2-3).</td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Almacenamiento en caché de decisiones de preautorización</i></td>
      <td>Almacene en caché las decisiones correctas de permisos en la memoria para mejorar el rendimiento y minimizar las llamadas innecesarias a la API de REST v2, ya que las actualizaciones de suscripción mientras la aplicación se está ejecutando son poco frecuentes.</td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".</td>
   </tr>
</table>

### 5. Fase de autorización {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recuperación de decisiones de autorización</i></td>
      <td>Obtener decisiones de autorización antes de la reproducción, independientemente de si existe una decisión de preautorización.<br/><br/>Permita que los flujos continúen sin interrupciones aunque el token de medios caduque durante la reproducción y solicite una nueva decisión de autorización que contenga un token de medios (nuevo) cuando el usuario realice su siguiente solicitud de reproducción, independientemente de si es para el mismo recurso o para uno diferente.<br/><br/>Las transmisiones en vivo que se ejecuten por períodos prolongados de tiempo pueden optar por solicitar una nueva decisión de autorización después de las operaciones de vídeo como pausar contenido, iniciar pausas comerciales o modificar configuraciones de nivel de recursos cuando el MRSS experimenta cambios.</td>
      <td>Corre el riesgo de incumplir los acuerdos contractuales entre el programador, las MVPD y Adobe.<br/><br/>Riesgos que se saltan nuestros sistemas de monitoreo y alerta.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Reintento de recuperación de decisiones de autorización</i></td>
      <td>Gestione correctamente los <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">códigos de error mejorados</a> y utilice el campo de acción para determinar los pasos de corrección necesarios.<br/><br/>Solo un número limitado de códigos de error mejorados justifica un reintento, mientras que la mayoría requiere resoluciones alternativas, como se especifica en el campo de acción.<br/><br/>Asegúrese de que cualquier mecanismo de reintentos implementado para recuperar decisiones de autorización no resulte en un bucle interminable y de que limite los reintentos a un número razonable (es decir, 2-3).</td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".</td>
   </tr>
</table>

### 6. Fase de cierre {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Compatibilidad con cierre de sesión</i></td>
      <td>Implemente la API de cierre de sesión para permitir que los usuarios cierren sesión manualmente, finalizando su perfil autenticado y siguiendo el nombre de acción de la API de REST v2 especificado para cada perfil eliminado:<ul><li>Para las MVPD que admiten un extremo de cierre de sesión, la aplicación cliente requiere navegar a la "url" proporcionada en un user-agent.</li><li>Para perfiles de tipo "appleSSO", la aplicación cliente requiere guiar al usuario para que también cierre la sesión desde el nivel de socio (configuración del sistema de Apple).</li></ul></td>
      <td>Corre el riesgo de que la aplicación cliente no funcione correctamente debido a la falta de compatibilidad en la aplicación cliente.</td>
   </tr>
</table>

### 7. Parámetros y encabezados {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Enviar encabezado de autorización</i></td>
      <td>Envíe el encabezado <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">Authorization</a> para cada solicitud de API de REST v2.</td>
      <td>Riesgos que activan respuestas de error "no autorizadas" HTTP 401, sobrecarga de recursos del sistema y aumento de la latencia.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Enviar encabezado de identificador de dispositivo AP</i></td>
      <td>Envíe el encabezado <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> para cada solicitud de API de REST v2.<br/><br/>Incluso cuando la solicitud se origina desde un servidor en nombre de un dispositivo, el valor del encabezado AP-Device-Identifier debe reflejar el identificador real del dispositivo de flujo continuo.</td>
      <td>Riesgos que activan respuestas de error HTTP 400 "Solicitud incorrecta", sobrecargan recursos del sistema y aumentan la latencia.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Enviar encabezado X-Device-Info</i></td>
      <td>Envíe el encabezado <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> para cada solicitud de API de REST v2.<br/><br/>Incluso cuando la solicitud se origina desde un servidor en nombre de un dispositivo, el valor del encabezado X-Device-Info debe reflejar la información real del dispositivo de flujo continuo.</td>
      <td>Los riesgos se clasifican como procedentes de una plataforma desconocida y se tratan como inseguros, y se someten a reglas más restrictivas, como TTL de autenticación más cortos.<br/><br/>Además, algunos campos, como el dispositivo de transmisión connectionIp y connectionPort, son obligatorios para características como la autenticación de base de inicio de Spectrum.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Identificador de dispositivo estable</i></td>
      <td>Calcule y almacene un identificador de dispositivo estable que no cambie al actualizar o reiniciar el encabezado <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.<br/><br/>En plataformas sin identificador de hardware, genere un identificador único a partir de los atributos de la aplicación y manténgalo.</td>
      <td>Corre el riesgo de perder la autenticación cuando cambia el identificador del dispositivo.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Seguir referencias de API</i></td>
      <td>Asegúrese de que solo envía los parámetros y encabezados esperados de la API de REST v2.</td>
      <td>Riesgos que activan respuestas de error HTTP 400 "Solicitud incorrecta", sobrecargan recursos del sistema y aumentan la latencia.</td>
   </tr>
</table>

### 8. Gestión de errores {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Compatibilidad mejorada con gestión de código de error</i></td>
      <td>Gestione correctamente los <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">códigos de error mejorados</a> y utilice el campo de acción para determinar los pasos de corrección necesarios.<br/><br/>Solo un número limitado de códigos de error mejorados justifica un reintento, mientras que la mayoría requiere resoluciones alternativas, como se especifica en el campo de acción.<br/><br/>Los códigos de error más mejorados enumerados en la documentación de <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2">Códigos de error mejorados - API REST V2</a> se pueden evitar por completo si se administran correctamente durante la fase de desarrollo antes de iniciar la aplicación.</td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".<br/><br/>Corre el riesgo de que la aplicación cliente no funcione correctamente debido a la falta de administración de los códigos de error mejorados, lo que provoca mensajes de error poco claros, instrucciones incorrectas del usuario o un comportamiento de reserva incorrecto.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Compatibilidad con gestión de errores HTTP</i></td>
      <td>Diferenciar entre la gestión de respuestas de error HTTP (por ejemplo, 400, 401, 403, 404, 405, 500) y respuestas de éxito (por ejemplo, 200, 201) que contienen cargas de códigos de error mejorados, como se ha indicado anteriormente.<br/><br/>Solo un número limitado de códigos de error HTTP justifica un reintento, mientras que la mayoría requiere resoluciones alternativas.<br/><br/>La mayoría de las respuestas de error HTTP se pueden evitar por completo si se administran correctamente durante la fase de desarrollo antes de iniciar la aplicación.</td>
      <td>Se corre el riesgo de sobrecargar recursos del sistema, aumentar la latencia y activar potencialmente respuestas de error HTTP 429 "Demasiadas solicitudes".<br/><br/>Corre el riesgo de que la aplicación cliente no funcione correctamente debido a la falta de administración de los códigos de error mejorados, lo que provoca mensajes de error poco claros, instrucciones incorrectas del usuario o un comportamiento de reserva incorrecto.</td>
   </tr>
</table>

### 9. Pruebas {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Pruebas del ciclo vital</i></td>
      <td>Desarrolle y pruebe la aplicación con los entornos oficiales de no producción de autenticación de Adobe Pass:<ul><li>Prequal-Production</li><li>Release-Staging</li></ul><br/>Realice un control de calidad exhaustivo (QA) en estos entornos antes de iniciar Release-Production.<br/><br/>Las aplicaciones cliente no deben continuar con Release-Production sin completar primero la validación de extremo a extremo en entornos que no sean de producción.</td>
      <td>Se arriesga a lanzarse con defectos críticos y graves.<br/><br/>La falta de una ruta de depuración corta y eficaz puede impedir que el equipo de ingeniería y soporte técnico de Adobe intervenga con rapidez.</td>
   </tr>
</table>

## Prácticas recomendadas {#recommended-practices}

### 1. Fase de registro {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Prácticas</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validación de tokens de acceso</i></td>
      <td>Compruebe de forma proactiva la validez del token de acceso y actualícelo cuando caduque.<br/><br/>Asegúrese de que cualquier mecanismo de reintento para controlar los errores "no autorizados" de HTTP 401 actualice primero el token de acceso antes de volver a intentar la solicitud original.</td>
      <td>Riesgos que activan respuestas de error "no autorizadas" HTTP 401, sobrecarga de recursos del sistema y aumento de la latencia.</td>
   </tr>
</table>

### 2. Fase de configuración {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Prácticas</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Almacenamiento en caché de configuración</i></td>
      <td>Almacene la respuesta de configuración en la memoria o en el almacenamiento persistente durante un corto periodo de tiempo (por ejemplo, de 3 a 5 minutos) para mejorar el rendimiento y minimizar las llamadas innecesarias a la API de REST v2.</td>
      <td>Riesgos de sobrecarga de recursos del sistema y aumento de la latencia.</td>
   </tr>
</table>

### 3. Fase de autenticación {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Prácticas</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validación del código de autenticación (autenticación en la segunda pantalla)</i></td>
      <td>Valide el código de autenticación enviado a través de los datos proporcionados por el usuario en la segunda aplicación (pantalla) secundaria antes de llamar a la API /api/v2/authenticate con las siguientes condiciones:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">Autenticación realizada en la aplicación secundaria (pantalla) con mvpd preseleccionado</a></b><ul><li>Use <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">Reanudar sesión de autenticación</a> - POST /api/v2/{serviceProvider}/session/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">Autenticación realizada en una aplicación secundaria (pantalla) sin mvpd preseleccionado</a></b><ul><li>Use <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">Recuperar sesión de autenticación</a> - GET /api/v2/{serviceProvider}/session/{code}</li></ul><br/>La aplicación cliente recibiría un error si el código de autenticación proporcionado se escribía incorrectamente o en caso de que la sesión de autenticación caducara.</td>
      <td>Arriesga varias respuestas de error y problemas de flujo de trabajo durante la autenticación.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Compatibilidad con varios perfiles</i></td>
      <td>Asegúrese de que la aplicación cliente pueda gestionar varios perfiles pidiéndole al usuario que seleccione un perfil o aplicando lógica personalizada, como seleccionar automáticamente el perfil con el periodo de validez más largo.</td>
      <td>Corre el riesgo de que la aplicación cliente no funcione correctamente debido a la falta de compatibilidad en la aplicación cliente.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>(Opcional) Compatibilidad con flujos no básicos</i></td>
      <td>Admitir flujos no básicos en caso de que el negocio de las aplicaciones cliente requiera lo siguiente:<ul><li>Flujos de acceso degradados (función Premium)</li><li>Flujos de acceso temporales (función Premium)</li><li>Flujos de acceso de inicio de sesión único (función estándar)</li></ul></td>
      <td>Corre el riesgo de crear una experiencia de usuario menos que ideal.</td>
   </tr>
</table>

### 4. (Opcional) Fase de preautorización {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Prácticas</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Experiencia del usuario</i></td>
      <td>Muestre comentarios claros del usuario si se rechaza una decisión de preautorización, utilizando la mensajería proporcionada por MVPD o Adobe a través de códigos de error mejorados.</td>
      <td>Corre el riesgo de crear una experiencia de usuario menos que ideal.</td>
   </tr>
</table>

### 5. Fase de autorización {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Prácticas</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validación de tokens de medios</i></td>
      <td>Valide tokens de medios usando la biblioteca <a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">Verificador de token de medios</a>.</td>
      <td>Riesgos de esquemas de fraude como la extracción de flujos.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Experiencia del usuario</i></td>
      <td>Muestre comentarios claros del usuario si se rechaza una decisión de autorización, utilizando la mensajería proporcionada por MVPD o Adobe a través de códigos de error mejorados.</td>
      <td>Corre el riesgo de crear una experiencia de usuario menos que ideal.</td>
   </tr>
</table>

### 6. Fase de cierre {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Prácticas</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Experiencia del usuario</i></td>
      <td>Evite llamar automáticamente (mediante programación) a la API de cierre de sesión en situaciones como la denegación de autorización o preautorización, ya que la API de cierre de sesión solo debe invocarse en respuesta a una solicitud directa del usuario.</td>
      <td>Se corre el riesgo de confundir al usuario de que la autenticación esté fallando.</td>
   </tr>
</table>

### 7. Parámetros y encabezados {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Prácticas</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Código de reutilización</i></td>
      <td>Reutilice el código de la API de REST v1 para calcular el identificador del dispositivo y la información del dispositivo con ajustes menores, pero asegúrese de que solo envía los parámetros y encabezados esperados de la API de REST v2.<br/><br/>Vuelva a utilizar el código de la API de REST v1 para llamar a la API de DCR y recuperar un token de acceso.</td>
      <td>-</td>
   </tr>
</table>

### 8. Pruebas {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Prácticas</th>
      <th style="background-color: #EFF2F7;">Riesgos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Cobertura de prueba</i></td>
      <td>Asegúrese de que los siguientes flujos básicos se prueben en todos los dispositivos y plataformas:<br/><br/><b>Flujos de autenticación</b><ul><li>Escenario de autenticación de la aplicación principal (pantalla)</li><li>Escenario de autenticación de la aplicación secundaria (pantalla)</li></ul><br/><b>(Opcional) Flujos de preautorización</b><ul><li>Escenario de decisiones de permiso de prueba</li><li>Escenario de decisiones de denegación de prueba</li></ul><br/><b>Flujos de autorización</b><ul><li>Escenario de decisiones de permiso de prueba</li><li>Escenario de decisiones de denegación de prueba</li></ul><br/><b>Flujos de cierre de sesión</b><br/><br/>Además, pruebe otros flujos de acceso, si corresponde:<br/><br/><ul><li>Flujos de acceso degradados (función Premium)</li><li>Flujos de acceso temporales (función Premium)</li><li>Flujos de acceso de inicio de sesión único (función estándar)</li></ul><br/>Incluya las principales integraciones de MVPD (que abarcan los proveedores más utilizados).</td>
      <td>Se arriesga a fallos imprevistos en la producción, especialmente en plataformas que se prueban con menor frecuencia o en flujos excepcionales como escenarios negativos.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Herramientas de prueba</i></td>
      <td>Usar el sitio web <a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a>.</td>
      <td>-</td>
   </tr>
</table>

## Resumen {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Fase</th>
      <th style="background-color: #EFF2F7;">Obligatorio</th>
      <th style="background-color: #EFF2F7;">(Muy) Recomendado</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Registro</i></td>
      <td>Credenciales de cliente de caché<br/><br/>Tokens de acceso de caché</td>
      <td>Validación y actualización de tokens de acceso</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configuración</i></td>
      <td>Minimizar recuperaciones de respuestas de configuración</td>
      <td>Respuesta de configuración de caché</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autenticación</i></td>
      <td>Ajuste fino del mecanismo de sondeo<br/><br/>Partes de caché de perfiles</td>
      <td>Admitir varios perfiles<br/><br/>Admitir característica de degradación (si es necesario para la empresa)<br/><br/>Admitir característica TempPass (si es necesario para la empresa)<br/><br/>Admitir característica de inicio de sesión único (si es necesario para la empresa)</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Preautorización</i></td>
      <td>Caché que permite decisiones de preautorización<br/><br/>Reintentar el ajuste del mecanismo</td>
      <td>Mejorar la experiencia del usuario mediante el uso de códigos de error para las decisiones de preautorización denegadas</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autorización</i></td>
      <td>Recuperar decisión de autorización cuando el usuario solicita la reproducción<br/><br/>Reintentar ajuste del mecanismo</td>
      <td>Mejore la experiencia del usuario utilizando códigos de error para las decisiones de autorización denegadas<br/><br/>Validación de token de medios</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Cerrar sesión</i></td>
      <td>Implementar la API de cierre de sesión para permitir que los usuarios cierren sesión manualmente</td>
      <td>Evite llamar automáticamente a la API de cierre de sesión</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Obligatorio</th>
      <th style="background-color: #EFF2F7;">(Muy) Recomendado</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Parámetros y encabezados</i></td>
      <td>Seguir especificaciones obligatorias de encabezados</td>
      <td>Reutilizar código de la API de REST v1</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Control de errores</i></td>
      <td>Implementar la administración de errores mejorada<br/><br/>Implementar la administración de errores HTTP</td>
      <td>-</td>
   </tr>
</table>
