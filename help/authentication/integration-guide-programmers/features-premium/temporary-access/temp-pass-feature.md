---
title: Función TempPass
description: Función TempPass
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# Función TempPass {#temp-pass-feature}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

TempPass es una función versátil que permite a los programadores ofrecer acceso temporal a su contenido protegido para usuarios sin credenciales de cuenta de MVPD válidas. Sirve como una herramienta eficaz para atraer a los espectadores, ya sea a través de escenarios de acceso básicos o campañas promocionales dirigidas.

TempPass es una potente solución para que los programadores:

* **Visualizadores atractivos:** Ofrezca una muestra de contenido premium para atraer nuevos suscriptores.
* **Promociones de unidad:** Ejecute campañas dirigidas para aumentar la exposición al contenido y generar lealtad a la marca.
* **Conservar el control:** Administrar períodos de acceso, aplicar límites y restablecer el acceso según sea necesario para alinearlo con los objetivos de la empresa.

La función TempPass se entrega mediante la introducción de un pseudo-MVPD (denominado además &quot;Temp Pass&quot;) dentro de la configuración del servidor de autenticación de Adobe Pass como una integración con el programador participante. La función TempPass está disponible en dos configuraciones:

* [TempPass básico](#basic-temp-pass) para acceso basado en el tiempo.
* [Pase temporal promocional](#promotional-temp-pass) para obtener acceso flexible impulsado por campañas.

>[!IMPORTANT]
>
> La función TempPass es una función premium y requiere una licencia actual de Adobe.

En la tabla siguiente se proporciona una breve comparación de las características Básicas y Promocionales de TempPass:

| **Característica** | **Paso temporal básico** | **Paso temporal promocional** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **Acceso al contenido** | <ul><li>Basado en el tiempo</li></ul> | <ul><li>Basado en el tiempo</li><li>Limitado a un número máximo de recursos</li></ul> |
| **Seguridad De Acceso Basada En** | <ul><li>ID de dispositivo</li></ul> | <ul><li>ID de dispositivo</li><li>Hash de la información de identificador de usuario proporcionada (por ejemplo, correo electrónico)</li></ul> |
| **Códigos de error mejorados** | Disponible | Disponible |
| **Función de restablecimiento de TempPass** | Disponible | Disponible |

>[!IMPORTANT]
> 
> La autenticación de Adobe Pass no incluye un mecanismo integrado para detener automáticamente el flujo en curso una vez transcurrido el tiempo asignado (X minutos). Es responsabilidad de los programadores aplicar restricciones de acceso una vez que TempPass caduque durante un flujo en curso.

Tanto si desea dar un vistazo a su biblioteca de contenido como promocionar un evento marquesina, TempPass le ofrece las herramientas para ampliar su audiencia mientras mantiene el control sobre el acceso.

## TempPass básico {#basic-temp-pass}

La función básica TempPass permite a los programadores proporcionar acceso al contenido por tiempo limitado, teniendo en cuenta varios escenarios:

* **Vistas previas cortas:** Ofrezca vistas previas breves, como un período de acceso diario de 10 minutos, para atraer a posibles suscriptores.
* **Acceso basado en eventos:** Habilite el acceso de mayor duración para eventos principales, como una sesión de 4 horas.
* **Acceso combinado:** Mezcle duraciones y combine, como un período de visualización inicial extendido seguido de vistas previas diarias más cortas en varios días.

Determinados eventos pueden requerir un acceso gratuito por fases al contenido, como un período inicial de acceso gratuito ampliado (por ejemplo, 4 horas), seguido de intervalos diarios de acceso gratuito más cortos (por ejemplo, 10 minutos cada día). Para implementar este escenario, los programadores deben coordinarse con su representante de Adobe para configurar dos MVPD TempPass adaptadas a sus necesidades.

Por ejemplo, para proporcionar una sesión inicial gratuita de 4 horas seguida de sesiones gratuitas diarias de 10 minutos, Adobe puede configurar para el programador:

* **TempPass1**: configurado con un tiempo de vida (TTL) de 4 horas para cubrir el período inicial de acceso libre.
* **TempPass2**: configurado con un tiempo de vida (TTL) de 10 minutos para los siguientes intervalos diarios de acceso libre.

Para garantizar la funcionalidad adecuada para el acceso diario, TempPass2 debe restablecerse para todos los dispositivos a las 00:00 horas cada día.

### Detalles de funciones {#basic-temp-pass-feature-details}

**Parámetros de configuración:**

* **TTL (Tiempo de vida):** Los programadores pueden especificar la duración del acceso. Este TTL basado en reloj caduca independientemente de la hora de visualización real.

**Identificación de usuario:**

La función básica TempPass utiliza el identificador del dispositivo como parámetro de identificación del usuario.

La siguiente tabla le ayuda a comprender cómo los parámetros de identificación del usuario influyen en la experiencia de prueba del usuario:

| Identificador de dispositivo | Resultado |
|-------------------|----------------|
| Nuevo | Nuevo juicio |
| Existente | Prueba existente |

**Ver cálculo de tiempo:**

El TTL representa la duración desde el tiempo de la solicitud de autorización inicial hasta el tiempo de caducidad, independientemente del tiempo real empleado en ver contenido. Cada solicitud futura compara la hora actual del servidor con la hora de caducidad almacenada para autorizar el acceso.

**Autenticación:**

No se requiere autenticación para Basic TempPass, lo que le permite continuar directamente con el paso de autorización.

**Autorización:**

Como no hay interacción con un MVPD real, el MVPD básico &quot;Temp Pass&quot; autorizará cualquier recurso dado que el TempPass es válido. En caso de que la autorización se realice correctamente, la biblioteca del verificador de tokens de medios sigue siendo aplicable para verificar el token de medios y garantizar la validación de recursos antes de iniciar la reproducción del contenido.

La decisión de autorización se basa en los parámetros de identificación del usuario y el TTL configurado. Para obtener una autorización correcta para un recurso, una solicitud válida debe cumplir las siguientes condiciones:

* **Duración sin consumir:** La hora de caducidad se calcula agregando la hora de solicitud de autorización inicial (guardada en nuestras bases de datos) al TTL configurado. La hora actual del servidor se compara con esta hora de caducidad para determinar si TempPass sigue siendo válido.

Si un usuario supera el TTL configurado, ya no podrá ver el contenido en el mismo dispositivo a menos que se restablezca su TempPass.

**Autorización previa:**

Cuando se realiza una solicitud de preautorización para una MVPD &quot;Temp Pass&quot; básica, la respuesta devuelve la lista completa de recursos de la solicitud con la autorización previa correcta. Este comportamiento refleja la lógica de autorización, ya que las condiciones de autorización se basan en límites de tiempo y no en recursos específicos. Siempre que la restricción de tiempo sea válida, se autorizarán los recursos solicitados.

**Cerrar sesión:**

No se requiere el cierre de sesión para el TempPass básico, lo que le permite cambiar directamente al paso de autenticación mediante un MVPD de usuario real.

**Datos de seguimiento y análisis:**

Durante el flujo TempPass básico, los datos de seguimiento utilizan una versión con hash del identificador del dispositivo, con el identificador de MVPD establecido en &quot;Temp Pass&quot;. Los programadores deben diferenciar las métricas de TempPass de las métricas estándar de MVPD en sus implementaciones de análisis.

## TempPass promocional {#promotional-temp-pass}

La función promocional TempPass amplía las capacidades del TempPass básico, diseñado específicamente para ejecutar campañas promocionales. Esta función permite a los programadores atraer usuarios permitiendo el acceso a un número predefinido de títulos de VOD durante un periodo de tiempo especificado después de recopilar una identificación de usuario válida, como una dirección de correo electrónico.

TempPass promocional incluye todas las funcionalidades del TempPass básico, con flexibilidad añadida para:

* Definición del número máximo de títulos de VOD accesibles durante el periodo promocional.
* Configuración del período de tiempo durante el cual el acceso promocional es válido.

Una vez que un usuario supera los límites de acceso predefinidos (número de títulos de VOD o duración), ya no podrá ver contenido en el mismo dispositivo o con el mismo identificador de usuario a menos que se restablezca su TempPass.

### Detalles de funciones {#promotional-temp-pass-feature-details}

**Parámetros de configuración:**

* **Clave de información del usuario:** La clave utilizada para comunicar el identificador proporcionado por el usuario, como una dirección de correo electrónico (por ejemplo, la clave es correo electrónico).
* **Cantidad de recursos:** Define cuántos títulos de VOD puede tener acceso un usuario.
* **TTL (Tiempo de vida):** Duración durante la cual el usuario puede consumir los recursos permitidos.

**Identificación de usuario:**

La función Promotional TempPass utiliza el hash del identificador proporcionado por el usuario sobre el identificador del dispositivo como parámetros de identificación del usuario.

>[!IMPORTANT]
>
> El programador administra la validación y el hash del identificador proporcionado por el usuario, no Adobe. Adobe no almacena ninguna información de identificación personal (PII). Como tal, el programador es responsable de generar y enviar un hash del identificador único proporcionado por el usuario al interactuar con las API de autenticación de Adobe Pass.

Adobe recomienda usar las funciones de la familia **SHA-2** o su **SHA-256**, **SHA-512** específica en los datos antes de enviarlos a Adobe. Por ejemplo, **SHA-256** sobre **&quot;user@domain.com&quot;** es **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**.

La siguiente tabla le ayuda a comprender cómo los parámetros de identificación del usuario influyen en la experiencia de prueba del usuario:

| Hash de identificador proporcionado por el usuario | Identificador de dispositivo | Resultado |
|-------------------------------|-------------------|---------------------------------------------------------|
| Nuevo | Nuevo | Nuevo juicio |
| Existente | Nuevo | Prueba existente (basada en el hash del identificador proporcionado por el usuario) |
| Nuevo | Existente | Prueba existente (basada en el identificador del dispositivo) |
| Existente | Existente | Prueba existente |

**Ver cálculo de tiempo:**

El TTL representa la duración desde el tiempo de la solicitud de autorización inicial hasta el tiempo de caducidad, independientemente del tiempo real empleado en ver contenido. Cada solicitud futura compara la hora actual del servidor con la hora de caducidad almacenada para autorizar el acceso.

**Autenticación:**

No se requiere autenticación para Promotional TempPass, lo que le permite continuar directamente con el paso de autorización.

Para admitir la implementación de la aplicación del programador, Promotional TempPass expone la siguiente información de metadatos de usuario, accesible a través de las claves correspondientes:

* **`remaining_resources`**: indica la cantidad de recursos que el usuario aún tiene derecho a consumir.
* **`used_assets`**: proporciona una lista de los recursos que el usuario ya ha consumido.
* **`expiration_date`**: muestra la fecha de caducidad del pase temporal promocional del usuario.

**Autorización:**

Como no hay interacción con un MVPD real, el MVPD Promocional &quot;Temp Pass&quot; autorizará cualquier recurso dado que el TempPass es válido. En caso de que la autorización se realice correctamente, la biblioteca del verificador de tokens de medios sigue siendo aplicable para verificar el token de medios y garantizar la validación de recursos antes de iniciar la reproducción del contenido.

La decisión de autorización se basa en los parámetros de identificación del usuario y en el número configurado de recursos y el TTL. Para obtener una autorización correcta para un recurso, una solicitud válida debe cumplir las siguientes condiciones:

* **Duración sin consumir:** La hora de caducidad se calcula agregando la hora de solicitud de autorización inicial (guardada en nuestras bases de datos) al TTL configurado. La hora actual del servidor se compara con esta hora de caducidad para determinar si TempPass sigue siendo válido.
* **Recursos no consumidos:** Se realiza un seguimiento del número de recursos consumidos (guardados en nuestras bases de datos). El número de recursos consumidos se compara con el número configurado de recursos para determinar si TempPass sigue siendo válido.

Si un usuario supera el TTL configurado o el número de recursos, ya no podrá ver el contenido en el mismo dispositivo o con el mismo identificador proporcionado por el usuario a menos que se restablezca su TempPass.

**Autorización previa:**

Cuando se realiza una solicitud de preautorización para una MVPD promocional &quot;Temp Pass&quot;, la respuesta devuelve la lista completa de recursos de la solicitud como autorizados previamente con éxito. Este comportamiento refleja la lógica de autorización, dado que las condiciones de autorización se basan en límites de tiempo y en el número total de recursos a los que se accede, en lugar de en recursos específicos. Siempre que la restricción de tiempo sea válida y no se haya superado el límite de recursos, se autorizarán los recursos solicitados.

**Cerrar sesión:**

No es necesario cerrar la sesión de Promotional TempPass, lo que le permite cambiar directamente al paso de autenticación con un MVPD de usuario real.

**Datos de seguimiento y análisis:**

Durante el flujo promocional de TempPass, los datos de seguimiento utilizan una versión con hash del identificador del dispositivo, con el identificador de MVPD establecido en &quot;Temp Pass&quot;. Los programadores deben diferenciar las métricas de TempPass de las métricas estándar de MVPD en sus implementaciones de análisis.

## Restablecer acceso a la API TempPass {#reset-tempass-api-access}

Antes de acceder a la API Reset TempPass, debe completar los pasos necesarios en el proceso de registro de cliente dinámico (DCR). Este proceso obligatorio garantiza que tenga el token de acceso necesario para interactuar con la API Reset TempPass.

Para obtener instrucciones detalladas, consulte la [Documentación general sobre el registro de clientes dinámicos](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Restablecer API de TempPass: DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

Para restablecer un TempPass específico para un dispositivo o todos los dispositivos, la autenticación de Adobe Pass proporciona a los programadores una API que funciona tanto para el TempPass básico como para el promocional.

### Solicitud {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de consulta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>El identificador único interno asociado con el proveedor de servicios durante el proceso de incorporación.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>El identificador único interno asociado con TempPass durante el proceso de incorporación.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            El identificador del dispositivo para el que es válida esta operación de restablecimiento.
            <br/><br/>
            Si no se proporciona ningún valor, la operación de restablecimiento se aplica a todos los dispositivos.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorización</td>
      <td>La generación de la carga útil del token de portador se describe en la documentación de <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Recuperar token de acceso</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

### Respuesta {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descripción</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Sin contenido</td>
      <td>
        El restablecimiento se realizó correctamente.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>No autorizado</td>
      <td>
        El token de acceso no es válido, el cliente debe obtener un nuevo token de acceso e intentarlo de nuevo. Para obtener más información, consulte la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Prohibido</td>
      <td>
        El token de acceso no es válido, el cliente necesita obtener nuevas credenciales de cliente y un nuevo token de acceso e inténtelo de nuevo. Para obtener más información, consulte la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
</table>

### Muestras {#reset-tempass-v3-reset-samples}

#### Restablecer TempPass para un dispositivo específico {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### Restablecer TempPass para todos los dispositivos {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## Restablecer API de TempPass: DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

Para restablecer una TempPass específica para una clave genérica (hash de identificador proporcionado por el usuario) o todas las claves, la autenticación de Adobe Pass proporciona a los programadores una API que funciona para la TempPass promocional.

### Solicitud {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de consulta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>El identificador único interno asociado con el proveedor de servicios durante el proceso de incorporación.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>El identificador único interno asociado con TempPass durante el proceso de incorporación.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">key</td>
      <td>
            El hash del identificador proporcionado por el usuario para el que es válida esta operación de restablecimiento.
            <br/><br/>
            Si no se proporciona ningún valor, la operación de restablecimiento se aplica a todos los usuarios.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorización</td>
      <td>La generación de la carga útil del token de portador se describe en la documentación de <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Recuperar token de acceso</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

### Respuesta {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descripción</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Sin contenido</td>
      <td>
        El restablecimiento se realizó correctamente.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>No autorizado</td>
      <td>
        El token de acceso no es válido, el cliente debe obtener un nuevo token de acceso e intentarlo de nuevo. Para obtener más información, consulte la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Prohibido</td>
      <td>
        El token de acceso no es válido, el cliente necesita obtener nuevas credenciales de cliente y un nuevo token de acceso e inténtelo de nuevo. Para obtener más información, consulte la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
</table>

### Muestras {#reset-tempass-v3-reset-generic-samples}

#### Restablecer TempPass para una clave específica {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### Restablecer TempPass para todas las claves {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## API DE REST V2 {#rest-api-v2}

El uso de la función TempPass requiere implementar actualizaciones de código para modificar la forma en que la aplicación TV Everywhere (TVE) interactúa con la autenticación de Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Para obtener una guía completa sobre estas actualizaciones y los flujos de trabajo asociados, consulte la documentación de [Flujos de acceso temporales](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).
