---
title: Anuncios de productos
description: Anuncios de productos
exl-id: 3c9c66e1-d31d-4af3-8ab2-eb32492f42ca
source-git-commit: 170d49b06e4ac8b31a840ee1bc5fac114bb3aa0b
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 21%

---

# Anuncios de productos {#product-announcements}

## Fin de vida útil (EOL) {#eol}

En esta sección se describen las fechas de finalización de la compatibilidad y de la vida útil de determinadas funciones y productos de autenticación de Adobe Pass que se planea retirar del mercado.

Asegúrese de mantenerse informado sobre los plazos de clausura y de que tiene previsto llevar a cabo las acciones adecuadas que se indican a continuación.

| Anuncio | Fecha de anuncio | Fecha de fin de soporte | Fecha de finalización de la vida útil |   | Descripción |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|---------------------|---------------------------------------------|:--|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| EOL de URL del tablero de TVE de autenticación de Adobe Pass <ul><li><a href="https://console.auth.adobe.com">console.auth.adobe.com</a></li><li><a href="https://console.auth-staging.adobe.com">console.auth-staging.adobe.com</a></li><li><a href="https://console-prequal.auth.adobe.com">console-prequal.auth.adobe.com</a></li><li><a href="https://console-prequal.auth-staging.adobe.com">console-prequal.auth-staging.adobe.com</a></li></ul> | 10/02/2024 | 01/01/2025 | 03/12/2025 |   | Planificamos el fin de vida útil del tablero de TVE de autenticación de Adobe Pass alojado en [console.auth.adobe.com](https://console.auth.adobe.com), [console.auth-staging.adobe.com](https://console.auth-staging.adobe.com), [console-prequal.auth.adobe.com](https://console-prequal.auth.adobe.com) y [console-prequal.auth-staging.adobe.com](https://console-prequal.auth-staging.adobe.com) el **12 de marzo de 2025** .</br></br>Después de esta fecha, ya no estará disponible el acceso a las URL mencionadas y se le redirigirá al nuevo [Tablero de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) para seguir accediendo a tus integraciones.</br></br>Para asegurar el servicio continuo, necesitarás acceder al nuevo [Tablero de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) alojado en [experience.adobe.com/pass/authentication](https://experience.adobe.com/pass/authentication). |
| Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 EOL | 12/11/2024 | N/D | <span style="color: red;">01/08/2025</span> |   | Planificamos el fin de vida útil del AccessEnabler de autenticación de Adobe Pass JavaScript SDK v3.5 el **8 de enero de 2025**. Después de esta fecha, las funciones y servicios proporcionados por AccessEnabler JavaScript SDK v3.5 dejarán de funcionar, incluida la autenticación y autorización de usuarios. |
| EOL del mecanismo de seguridad de autenticación de Adobe Pass sin DCR | 12/11/2024 | N/D | <span style="color: red;">01/20/2025</span> |   | Planificamos el fin de vida útil del mecanismo de seguridad sin DCR de autenticación de Adobe Pass el **20 de enero de 2025**. Este mecanismo se utilizó para proteger el acceso a las siguientes API de autenticación de Adobe Pass:<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md">Restablecer API de pase temporal</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md">API de degradación</a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">API de MVPD proxy</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">API de supervisión del servicio de derechos</a></li></ul>Después de esta fecha, las funciones y los servicios proporcionados por las API anteriores a los que se accede mediante este mecanismo de seguridad dejarán de funcionar.</br></br>Para garantizar el servicio continuo, deberá migrar todas sus aplicaciones para que utilicen el mecanismo [Registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md). |
| EOL de la API REST de autenticación de Adobe Pass v1 | 12/11/2024 | 12/31/2025 | Final de 2026 (TBD) |   | Tenemos planeado dejar de admitir la API REST de autenticación de Adobe Pass v1 el **31 de diciembre de 2025**. Después de esta fecha, ya no se proporcionarán actualizaciones ni correcciones.</br></br>Para garantizar soporte continuo, necesitarás migrar todas tus aplicaciones a la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API de REST v2</a>.</br></br>Planificamos el fin de la vida útil de la API de REST de autenticación de Adobe Pass v1 para **finales de 2026**. Después de esta fecha, las funciones y los servicios proporcionados por la API de REST v1 dejarán de funcionar, incluida la autenticación y autorización de usuarios.</br></br>Para asegurar el servicio continuo, necesitarás migrar todas tus aplicaciones a la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API REST v2</a>. |
| EOL de SDK de AccessEnabler de autenticación de Adobe Pass | 12/11/2024 | 05/31/2026 | Final de 2026 (TBD) |   | Tenemos planeado dejar de admitir los SDK del Habilitador de acceso a autenticación de Adobe Pass el **31 de mayo de 2026**. Después de esta fecha, ya no se proporcionarán actualizaciones ni correcciones.</br></br>Para garantizar la asistencia continua, deberá migrar todas sus aplicaciones a la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API de REST v2</a>.</br></br>Planificamos el fin de la vida útil de los SDK del habilitador de acceso a autenticación de Adobe Pass para el **final de 2026**. Después de esta fecha, las características y los servicios proporcionados por los SDK de AccessEnabler dejarán de funcionar, incluida la autenticación y autorización de usuarios.</br></br>Para asegurar el servicio continuo, necesitarás migrar todas tus aplicaciones a la <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">API REST v2</a>. |

## Versiones de productos {#product-releases}

Esta sección compila las referencias al historial de versiones y las notas de la versión correspondientes para la autenticación de Adobe Pass.

### 2026 {#product-releases-2026}

| Notas de la versión | Fechas |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de la versión de Adobe Pass Authentication 3.7.0](notes-releases/auth-rn-370.md) | 05/12/2026 - 05/14/2026 |
| [Notas de la versión de Adobe Pass Authentication 3.6.0](notes-releases/auth-rn-360.md) | 04/07/2026 - 04/09/2026 |

### 2025 {#product-releases-2025}

| Notas de la versión | Fechas |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de la versión de Adobe Pass Authentication 3.5.0](notes-releases/auth-rn-350.md) | 12/09/2025 - 12/11/2025 |
| [Notas de la versión de Adobe Pass Authentication Android 3.8.0](notes-releases/authn-rn-android-380.md) | 09/18/2025 |
| [Notas de la versión de Adobe Pass Authentication 3.4.0](notes-releases/auth-rn-340.md) | 09/16/2025 - 09/18/2025 |
| [Notas de la versión de Adobe Pass Authentication 3.3.0](notes-releases/auth-rn-330.md) | 07/22/2025 - 07/24/2025 |
| [Notas de la versión de Adobe Pass Authentication 3.2.0](notes-releases/auth-rn-320.md) | 06/10/2025 - 06/12/2025 |
| [Notas de la versión de Adobe Pass Authentication 3.1.0](notes-releases/auth-rn-310.md) | 02/25/2025 - 02/27/2025 |
| [Notas de la versión de Adobe Pass Authentication JavaScript SDK 4.7.1](notes-releases/authn-rn-javascript-471.md) | 02/25/2025 - 02/27/2025 |

### 2024 {#product-releases-2024}

| Notas de la versión | Fechas |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de la versión de Adobe Pass Authentication 3.0.3](notes-releases/auth-rn-303.md) | 10/29/2024 - 10/31/2024 |
| [Notas de la versión de Adobe Pass Authentication 3.0](notes-releases/auth-rn-300.md) | 09/10/2024 - 09/12/2024 |
| [Notas de la versión de Adobe Pass Authentication 2.70](notes-releases/auth-rn-270.md) | 04/23/2024 - 04/25/2024 |
| [Notas de la versión de Adobe Pass Authentication 2.69](notes-releases/auth-rn-269.md) | 02/27/2024 - 02/29/2024 |
| [Notas de la versión de Adobe Pass Authentication JavaScript SDK 4.7.0](notes-releases/authn-rn-javascript-470.md) | 02/27/2024 - 02/29/2024 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.9.2](notes-releases/authn-rn-ios-tvos-392.md) | 03/26/2024 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.8.4](notes-releases/authn-rn-ios-tvos-384.md) | 01/26/2024 |

### 2023 {#product-releases-2023}

| Notas de la versión | Fechas |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de la versión de Adobe Pass Authentication 2.68](notes-releases/auth-rn-268.md) | 12/05/2023 - 12/07/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.67](notes-releases/auth-rn-267.md) | 09/12/2023 - 09/14/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.66](notes-releases/auth-rn-266.md) | 07/11/2023 - 07/13/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.65.1](notes-releases/auth-rn-2651.md) | 06/20/2023 - 06/22/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.65](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| [Notas de la versión de Adobe Pass Authentication 2.64.1](notes-releases/auth-rn-2641.md) | 01/31/2023 - 02/02/2023 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.8.3](notes-releases/authn-rn-ios-tvos-383.md) | 11/16/2023 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.8.2](notes-releases/authn-rn-ios-tvos-382.md) | 02/10/2023 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.8.1](notes-releases/authn-rn-ios-tvos-381.md) | 26/05/2023 |
| [Notas de la versión de Adobe Pass Authentication Android SDK 3.7.3](notes-releases/authn-rn-android-373.md) | 09/19/2023 |

### 2022 {#product-releases-2022}

| Notas de la versión | Fechas |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de la versión de Adobe Pass Authentication 2.64](notes-releases/auth-rn-264.md) | 11/08/2022 - 11/10/2022 |
| [Notas de la versión de Adobe Pass Authentication 2.63](notes-releases/auth-rn-263.md) | 09/20/2022 - 09/22/2022 |
| [Notas de la versión de Adobe Pass Authentication 2.62.1](notes-releases/auth-rn-2621.md) | 08/02/2022 - 08/04/2022 |
| [Notas de la versión de Adobe Pass Authentication JavaScript SDK 4.6.0](notes-releases/authn-rn-javascript-460.md) | 09/20/2022 - 09/22/2022 |

### 2021 {#product-releases-2021}

| Notas de la versión | Fechas |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Notas de la versión de Adobe Pass Authentication JavaScript SDK 4.4.0](notes-releases/authn-rn-javascript-440.md) | 06/22/2021 |
| [Notas de la versión de Adobe Pass Authentication iOS / tvOS SDK 3.7.0](notes-releases/authn-rn-ios-tvos-370.md) | 09/03/2021 |
