---
title: Anuncios de productos
description: Anuncios de productos
exl-id: 3c9c66e1-d31d-4af3-8ab2-eb32492f42ca
source-git-commit: 9dcc649b4216cccc9be35cd6553308bfc345b5f4
workflow-type: tm+mt
source-wordcount: '859'
ht-degree: 1%

---

# Anuncios de productos {#product-announcements}

<a href="https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements">![Serie de seminarios web en vivo](/help/authentication/assets/rest-api-v2/live-webinar-series-banner.png)</a>

Como valioso socio de autenticación de Adobe Pass, nos gustaría invitarle a nuestro próximo seminario web en directo sobre la nueva API de REST. La nueva API se lanzó el año pasado para mejorar la experiencia del usuario final y simplificar la integración con los servicios de Adobe Pass. 

Vamos a llevar a cabo una serie de dos seminarios web para proporcionar una visión general de la nueva API, los beneficios y los casos de uso adicionales que se pueden activar migrando a la nueva API.

Regístrese para el seminario web que mejor se adapte a usted y a su equipo.

* [Seminario web del 1 al 19 de febrero de 2025](https://events.teams.microsoft.com/event/83c6ec1e-2522-4918-910f-c529b4fb0574@fa7b1b5a-7b34-4387-94ae-d2c178decee1)
* [Seminario web 2 - 5 de marzo de 2025](https://events.teams.microsoft.com/event/6f673689-e848-4c00-93d9-4f5d8f108977@fa7b1b5a-7b34-4387-94ae-d2c178decee1)

Durante la sesión, aprenderá lo siguiente:

* Información general y ventajas de la API de REST v2
* Tutorial sobre flujos básicos
* Cronología y pasos siguientes

El seminario web le resultará útil si:

* Un nuevo cliente que planea lanzar una nueva aplicación de TVE
* Clientes existentes que necesitan migrar a las nuevas API
* Socios de implementación que implementarían API para los clientes

Puede encontrar documentación técnica sobre la nueva API [aquí](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md). Le recomendamos a usted y a su equipo que rellenen cualquier pregunta que desee discutir durante la sesión [aquí](https://forms.office.com/r/sJea78tUy3).

## Fin de vida útil (EOL) {#eol}

En esta sección se describen las fechas de finalización de la compatibilidad y de la vida útil de determinadas funciones y productos de autenticación de Adobe Pass que se planea retirar del mercado.

Asegúrese de mantenerse informado sobre los plazos de clausura y de que tiene previsto llevar a cabo las acciones adecuadas que se indican a continuación.

| Anuncio | Fecha de anuncio | Fecha de fin de soporte | Fecha de finalización de la vida útil | Descripción |
|-----------------------------------------------------------------|-------------------|---------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 EOL | 11/12/2024 | N/D | 08/01/2025 | Planificamos el fin de vida útil del AccessEnabler de autenticación de Adobe Pass JavaScript SDK v3.5 el **8 de enero de 2025**. Después de esta fecha, las funciones y servicios proporcionados por AccessEnabler JavaScript SDK v3.5 dejarán de funcionar, incluida la autenticación y autorización de usuarios. |
| EOL del mecanismo de seguridad de autenticación de Adobe Pass sin DCR | 11/12/2024 | N/D | 20/01/2025 | Planificamos el fin de vida útil del mecanismo de seguridad sin DCR de autenticación de Adobe Pass el **20 de enero de 2025**. Este mecanismo se utilizó para proteger el acceso a las siguientes API de autenticación de Adobe Pass:<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md">Restablecer API de pase temporal</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md">API de degradación</a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">API de MVPD proxy</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">API de supervisión del servicio de derechos</a></li></ul>Después de esta fecha, las funciones y los servicios proporcionados por las API anteriores a los que se accede mediante este mecanismo de seguridad dejarán de funcionar.</br></br>Para garantizar el servicio continuo, deberá migrar todas sus aplicaciones para que utilicen el mecanismo [Registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md). |
| EOL de la API REST de autenticación de Adobe Pass v1 | 11/12/2024 | 31/12/2025 | Final de 2026 (TBD) | Tenemos planeado dejar de admitir la API REST de autenticación de Adobe Pass v1 el **31 de diciembre de 2025**. Después de esta fecha, ya no se proporcionarán actualizaciones ni correcciones.</br></br>Para garantizar la compatibilidad continua, deberá migrar todas sus aplicaciones a la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API de REST v2</a>.</br></br>Planificamos el fin de vida útil de la API de REST de autenticación de Adobe Pass v1 para el **final de 2026**. Después de esta fecha, las funciones y los servicios proporcionados por la API de REST v1 dejarán de funcionar, incluida la autenticación y autorización de usuarios.</br></br>Para asegurar el servicio continuo, necesitarás migrar todas tus aplicaciones a la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API REST v2</a>. |
| EOL de SDK de AccessEnabler de autenticación de Adobe Pass | 11/12/2024 | 31/05/2026 | Final de 2026 (TBD) | Tenemos planeado dejar de admitir los SDK del Habilitador de acceso a autenticación de Adobe Pass el **31 de mayo de 2026**. Después de esta fecha, ya no se proporcionarán actualizaciones ni correcciones.</br></br>Para garantizar la compatibilidad continua, deberá migrar todas sus aplicaciones a la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API de REST v2</a>.</br></br>Planificamos el fin de vida útil de los SDK del Habilitador de acceso a autenticación de Adobe Pass para el **final de 2026**. Después de esta fecha, las características y los servicios proporcionados por los SDK de AccessEnabler dejarán de funcionar, incluida la autenticación y autorización de usuarios.</br></br>Para asegurar el servicio continuo, necesitarás migrar todas tus aplicaciones a la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API REST v2</a>. |

## Versiones de productos {#product-releases}

Esta sección compila las referencias al historial de versiones y las notas de la versión correspondientes para la autenticación de Adobe Pass.

### 2024 {#product-releases-2024}

| Notas de la versión | Fechas |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de la versión de Adobe Pass Authentication 3.0.3](notes-releases/auth-rn-303.md) | 29/10/2024 - 31/10/2024 |
| [Notas de la versión de Adobe Pass Authentication 3.0](notes-releases/auth-rn-300.md) | 10/09/2024 - 12/09/2024 |
| [Notas de la versión de Adobe Pass Authentication 2.70](notes-releases/auth-rn-270.md) | 23/04/2024 - 25/04/2024 |
| [Notas de la versión de Adobe Pass Authentication 2.69](notes-releases/auth-rn-269.md) | 27/02/2024 - 29/02/2024 |
| [Notas de la versión de Adobe Pass Authentication JavaScript SDK 4.7.0](notes-releases/authn-rn-javascript-470.md) | 27/02/2024 - 29/02/2024 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.9.2](notes-releases/authn-rn-ios-tvos-392.md) | 26/03/2024 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.8.4](notes-releases/authn-rn-ios-tvos-384.md) | 26/01/2024 |

### 2023 {#product-releases-2023}

| Notas de la versión | Fechas |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de la versión de Adobe Pass Authentication 2.68](notes-releases/auth-rn-268.md) | 05/12/2023 - 07/12/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.67](notes-releases/auth-rn-267.md) | 12/09/2023 - 14/09/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.66](notes-releases/auth-rn-266.md) | 11/07/2023 - 13/07/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.65.1](notes-releases/auth-rn-2651.md) | 20/06/2023 - 22/06/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.65](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.64.1](notes-releases/auth-rn-2641.md) | 31/01/2023 - 02/02/2023 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.8.3](notes-releases/authn-rn-ios-tvos-383.md) | 16/11/2023 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.8.2](notes-releases/authn-rn-ios-tvos-382.md) | 10/02/2023 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.8.1](notes-releases/authn-rn-ios-tvos-381.md) | 26/05/2023 |
| [Notas de la versión de Adobe Pass Authentication Android SDK 3.7.3](notes-releases/authn-rn-android-373.md) | 19/09/2023 |

### 2022 {#product-releases-2022}

| Notas de la versión | Fechas |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de la versión de Adobe Pass Authentication 2.64](notes-releases/auth-rn-264.md) | 08/11/2022 - 10/11/2022 |
| [Notas de la versión de Adobe Pass Authentication 2.63](notes-releases/auth-rn-263.md) | 20/09/2022 - 22/09/2022 |
| [Notas de la versión de Adobe Pass Authentication 2.62.1](notes-releases/auth-rn-2621.md) | 02/08/2022 - 04/08/2022 |
| [Notas de la versión de Adobe Pass Authentication JavaScript SDK 4.6.0](notes-releases/authn-rn-javascript-460.md) | 20/09/2022 - 22/09/2022 |

### 2021 {#product-releases-2021}

| Notas de la versión | Fechas |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Notas de la versión de Adobe Pass Authentication JavaScript SDK 4.4.0](notes-releases/authn-rn-javascript-440.md) | 22/06/2021 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.7.0](notes-releases/authn-rn-ios-tvos-370.md) | 03/09/2021 |
