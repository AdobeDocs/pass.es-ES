---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass Authentication
user-guide-description: Adobe Pass Authentication es una solución de asignación de derechos para TV Everywhere que proporciona un marco modular para determinar si quien solicita acceso a un recurso tiene derechos para acceder.
source-git-commit: c849882286c88d16a5652717d381700287c53277
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 2%

---


# Ayuda de autenticación de Adobe Pass {#authentication}

+ [Resumen de autenticación de Adobe Pass](home.md)
+ Conceptos de autenticación de Adobe Pass {#authentication-concepts}
   + [Documento técnico](technical-paper.md)
   + [Información general para programadores](programmer-overview.md)
   + [Información general de MVPD](mvpd-overview.md)
+ Guías de KickStart {#kickstart-guides}
   + [Guía de KickStart del programador](programmer-kickstart-guide.md)
   + [Guía de inicio de MVPD](mvpd-kickstart-guide.md)
+ Guía de integración del programador {#programmer-integration-guide}
   + [Información general sobre la guía de integración del programador](programmer-integration-guide-overview.md)
   + [Flujo de asignación de derechos del programador](entitlement-flow.md)
   + [Casos de uso del programador](programmer-use-cases.md)
   + [Pasar información del cliente (dispositivo, conexión y aplicación)](passing-client-information-device-connection-and-application.md)
   + [Mecanismo de limitación](throttling-mechanism.md)
   + API DE REST V1 {#rest-api-v1}
      + [Información general de API REST](rest-api-overview.md)
      + [Guía de la API de REST (servidor a servidor)](rest-api-cookbook-servertoserver.md)
      + [Guía de la API de REST (de cliente a servidor)](rest-api-cookbook-clienttoserver.md)
      + Referencia de API de REST {#rest-api-reference}
         + [Referencia de API de REST](rest-api-reference.md)
         + [Solicitud de código de registro](registration-code-request.md)
         + [Devolver registro de registro](return-registration-record.md)
         + [Eliminar registro](delete-registration-record.md)
         + [Proporcionar lista de MVPD](provide-mvpd-list.md)
         + [Iniciar autenticación](initiate-authentication.md)
         + [Comprobar token de autenticación](check-authentication-token.md)
         + [Recuperar token de autenticación](retrieve-authentication-token.md)
         + [Iniciar autorización](initiate-authorization.md)
         + [Recuperar token de autorización](retrieve-authorization-token.md)
         + [Obtener token de medios corto](obtain-short-media-token.md)
         + [Comprobar flujo de autenticación por aplicación web en segunda pantalla](check-authentication-flow-by-second-screen-web-app.md)
         + [Recuperar lista de recursos autorizados previamente](retrieve-list-of-preauthorized-resources.md)
         + [Recuperar lista de recursos preautorizados por aplicación web de segunda pantalla](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [Iniciar cierre de sesión](initiate-logout.md)
         + [Metadatos del usuario](user-metadata.md)
         + [Recuperar solicitud de perfil](retrieve-profilerequest.md)
         + [Intercambio de tokens](token-exchange.md)
         + [Vista previa gratuita para Pase temporal y Pase temporal promocional](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + API DE REST V2 {#rest-api-v2}
      + API {#rest-api-v2-apis}
         + [API de REST V2 - API - Información general](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
         + Configuración {#rest-api-v2-configuration-apis}
            + [Recuperar la configuración de un proveedor de servicios específico](./rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
         + Sesiones {#rest-api-v2-sessions-apis}
            + [Crear sesión de autenticación](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
            + [Reanudar sesión de autenticación](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
            + [Recuperar sesión de autenticación](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
            + [Realizar autenticación en el agente de usuario](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
         + Perfiles {#rest-api-v2-profiles-apis}
            + [Recuperación de perfiles](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
            + [Recuperar perfil para mvpd específico](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
            + [Recuperar perfil para código específico](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
         + Decisiones {#rest-api-v2-decisions-apis}
            + [Recuperar decisiones de autorización utilizando mvpd específico](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
            + [Recuperar decisiones de preautorización utilizando mvpd específico](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
         + Cerrar sesión {#rest-api-v2-logout-apis}
            + [Iniciar el cierre de sesión de un mvpd específico](./rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
         + Inicio de sesión único de socio {#rest-api-v2-partner-single-sign-on-apis}
            + [Recuperar solicitud de autenticación de socio](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
            + [Recuperar perfil mediante la respuesta de autenticación del socio](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
      + Flujos {#rest-api-v2-flows}
         + [API de REST V2: Flujos: información general](./rest-api-v2/flows/rest-api-v2-flows-overview.md)
         + Flujos de acceso básicos {#rest-api-v2-basic-access-flows}
            + [Flujo de perfiles básicos realizado dentro de la aplicación principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
            + [Flujo de perfiles básicos realizado en la aplicación secundaria](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
            + [Flujo de autenticación básico realizado en la aplicación principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
            + [Flujo de autenticación básico realizado en la aplicación secundaria](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
            + [Flujo de autorización básico realizado en la aplicación principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
            + [Flujo de preautorización básico realizado en la aplicación principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
            + [Flujo de cierre de sesión básico realizado en la aplicación principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
         + Flujos de acceso degradados {#rest-api-v2-degraded-access-flows}
            + [Flujos de acceso degradados](rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
         + Flujos de acceso temporales {#rest-api-v2-temporary-access-flows}
            + [Flujos de acceso temporales](rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
         + Flujos de acceso de inicio de sesión único {#rest-api-v2-single-sign-on-access-flows}
            + [Inicio de sesión único con flujos de socios](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
            + [Inicio de sesión único con flujos de identidad de plataforma](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
            + [Inicio de sesión único mediante flujos de token de servicio](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
            + [Flujo de cierre de sesión único](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
      + Apéndice {#rest-api-v2-appendix}
         + Encabezados {#rest-api-v2-appendix-headers}
            + [Encabezado: AD-Service-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
            + [Encabezado: Adobe-Subject-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
            + [Encabezado: AP-Device-Identifier](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
            + [Encabezado: AP-Partner-Framework-Status](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
            + [Encabezado: AP-TempPass-Identity](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
            + [Encabezado: X-Device-Info](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
   + SDK de AccessEnabler {#accessenabler-sdk}
      + SDK para JavaScript {#javascriptsdk}
         + [Información general del SDK para JavaScript](javascript-sdk-overview.md)
         + [Guía del SDK para JavaScript](javascript-sdk-cookbook.md)
         + [Referencia de API de SDK para JavaScript](javascript-sdk-api-reference.md)
         + Directrices {#js-sdk-guidelines}
            + [Inicio y cierre de sesión sin actualización](refreshless-login-and-logout.md)
         + API de JavaScript {#js-api}
            + [Preautorizar](js-preauthorize.md)
      + SDK {#ios-sdk} de iOS/tvOS
         + [Información general del SDK para iOS/tvOS](iostvos-sdk-overview.md)
         + [Guía del SDK para iOS/tvOS](iostvos-sdk-cookbook.md)
         + [Referencia de API de SDK para iOS/tvOS](iostvos-sdk-api-reference.md)
         + Directrices {#ios-tvos-sdk-guidelines}
            + [Registro de aplicaciones de iOS/tvOS](iostvos-application-registration.md)
            + Directrices de migración {#migration-guidelines}
               + [Guía de migración de iOS/tvOS v3.x](iostvos-v3x-migration-guide.md)
            + [Comprobaciones de integridad del almacenamiento de iOS/tvOS](iostvos-sdk-storage-integrity-checks.md)
         + API de iOS/tvOS {#ios-tvos-api}
            + [Autorizar Previamente](preauthorize.md)
      + SDK para Android {#androidsdk}
         + [Información general del SDK para Android](android-sdk-overview.md)
         + [Guía del SDK para Android](android-sdk-cookbook.md)
         + [Referencia de API de SDK para Android](android-sdk-api-reference.md)
         + Directrices {#androidguidelines}
            + [Registro de aplicaciones Android](android-application-registration.md)
            + [SDK de Android con registro de cliente dinámico](android-sdk-with-dynamic-client-registration.md)
         + API de Android {#androidapi}
            + [Preautorizar](preauthorize-android.md)
      + SDK {#fireossdk} de Amazon FireOS
         + [Amazon FireOS SSO - Guía de inicio del programador](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [Amazon FireOS SSO con la API de cliente Cookbook](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Información general técnica de Amazon FireOS](amazon-fireos-technical-overview.md)
         + [Guía de integración de Amazon FireOS](amazon-fireos-integration-cookbook.md)
         + [Referencia de la API de Amazon FireOS](amazon-fireos-native-client-api-reference.md)
         + [Registro de aplicaciones de Amazon FireOS](amazon-fireos-application-registration.md)
         + [FireOS SDK con registro de cliente dinámico](fireos-sdk-with-dynamic-client-registration.md)
   + SSO de plataforma {#platform-sso}
      + Apple SSO {#apple-sso}
         + [Información general sobre Apple SSO](apple-sso-overview.md)
         + [Guía de Apple SSO (API de REST)](apple-sso-cookbook-rest-api.md)
         + [Guía de Apple SSO (SDK de iOS/tvOS)](apple-sso-cookbook-iostvos-sdk.md)
      + SSO de Roku {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + Metadatos de contenido {#content-metadata}
      + [Identificar recurso protegido](identify-protected-resources.md)
   + Integración del servidor de contenido {#content-serv-int}
      + [Cómo integrar el Media Token Verifier](media-token-verifier-int.md)
   + Apéndices {#appendices}
      + [Sugerencias de depuración](appendix-b-debugging-tips.md)
+ Guía de integración de MVPD {#mvpd-int-guide}
   + [Funciones de integración](mvpd-integr-features.md)
   + [Autenticación](authn-usecase.md)
   + [Autenticación mediante el protocolo OAuth 2.0](authn-oauth2-protocol.md)
   + [Autorización](authz-usecase.md)
   + [Autorización de verificación previa](mvpd-preflight-authz.md)
   + [Cierre de sesión de MVPD](usecase-mvpd-logout.md)
   + [Intercambio de metadatos de contenido](mvpd-content-metadata-exchange.md)
   + [Intercambio de metadatos de usuario](mvpd-user-metadata-exchng.md)
   + [Servicio web de MVPD proxy](proxy-mvpd-webserv.md)
   + [Integración de SAML de MVPD proxy](proxy-mvpd-saml-int.md)
   + [Ámbito del proveedor de servicios](serv-provider-scoping.md)
   + [Direcciones IP permitidas de MVPD](mvpd-listing-ip-addres.md)
+ Funciones de autenticación de Adobe Pass {#auth-features}
   + Integración de Adobe Analytics {#analytics-int}
      + [Integración de datos del lado del servidor de Adobe Pass Authentication en Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [Uso del ID de Experience Cloud en la autenticación de Adobe Pass](exp-cloud-id-authn.md)
   + Supervisión del servicio de derechos {#entitlement-service-monitoring}
      + [Resumen de monitorización del servicio de derechos](entitlement-service-monitoring-overview.md)
      + [API de monitorización del servicio de derechos](entitlement-service-monitoring-api.md)
      + [Métricas del lado del servidor](understanding-serverside-metrics.md)
   + Paso temporal {#temp-pass}
      + [Pase temporal](temp-pass.md)
      + [Pase temporal promocional](promotional-temp-pass.md)
      + [Restablecer pase temporal](reset-temp-pass.md)
   + Inicio de sesión único {#sso}
      + [Compatibilidad con inicio de sesión único](sso-support.md)
      + [SSO mediante autenticación pasiva](sso-passive-authn.md)
   + Autenticación basada en inicio {#home-based-auth}
      + [Autenticación basada en el hogar para TV en todas partes](home-based-authn-tve.md)
      + [Estado de HBA para MVPD](hba-status-mvpds.md)
   + Metadatos de usuario {#user-metadat}
      + [Metadatos del usuario](user-metadata-feature.md)
   + [Autorización de verificación previa](preflight-authz.md)
   + Informe de errores {#error-reportn}
      + [Informes de errores](error-reporting.md)
      + [Códigos de error mejorados](enhanced-error-codes.md)
   + Registro de cliente {#client-regn}
      + [Registro dinámico de clientes](dynamic-client-registration.md)
      + [API de registro de cliente dinámico](dynamic-client-registration-api.md)
      + [Dynamic Client Registration Management](dynamic-client-registration-management.md)
   + Servicio de degradación {#degrn-service}
      + [Resumen de API de degradación](degradation-api-overview.md)
   + Preparación para la privacidad {#privacy-readiness}
      + [Información general de soporte de privacidad](privacy-supp-overview.md)
      + [Cómo realizar una solicitud de privacidad](make-privacy-req.md)
+ Sugerencias y solución de problemas {#tips-troubleshoot}
   + [Permitir MVPD en el cuadro de diálogo de selección](allow-mvpd-selectn-dialog.md)
   + [Evitar que las MVPD aparezcan en el cuadro de diálogo de selección](prevent-mvpd-selectn-dialog.md)
+ Compatibilidad {#support}
   + [Procedimientos de escalación](escalation-procedures.md)
   + [Monitorización de Adobe Pass Adobe PayTV Pass](monitoring-adobe-pay-tv-pass.md)
   + [Requisitos mínimos del sistema](minimum-system-requirements.md)
+ Notas de la versión {#release-notes}
   + [Notas de la versión de Adobe Pass Authentication 2.70](auth-rn-270.md)
   + [Notas de la versión de Adobe Pass Authentication 2.69](auth-rn-269.md)
   + [Notas de la versión de Adobe Pass Authentication 2.68](auth-rn-268.md)
   + [Notas de la versión de Adobe Pass Authentication 2.67](auth-rn-267.md)
   + [Notas de la versión de Adobe Pass Authentication 2.66](auth-rn-266.md)
   + [Notas de la versión de autenticación de Adobe Pass 2.65.1](auth-rn-2651.md)
   + [Notas de la versión de Adobe Pass Authentication 2.65](auth-rn-265.md)
   + [Notas de la versión de autenticación de Adobe Pass 2.64.1](auth-rn-2641.md)
   + [Notas de la versión de Adobe Pass Authentication 2.64](auth-rn-264.md)
   + [Notas de la versión de Adobe Pass Authentication 2.63](auth-rn-263.md)
   + [Notas de la versión de autenticación de Adobe Pass 2.62.1](auth-rn-2621.md)
   + Notas de la versión de JavaScript SDK {#release-notes-javascript}
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.6.0](authn-rn-javascript-460.md)
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.4.0](authn-rn-javascript-440.md)
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.2.0](authn-rn-javascript-420.md)
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.1.1](authn-rn-javascript-411.md)
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.1.0](authn-rn-javascript-410.md)
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.0.0](authn-rn-javascript-400.md)
      + [Notas de la versión de Adobe Pass Authentication JavaScript 3.5.0](authn-rn-javascript-350.md)
   + Notas de la versión de iOS/tvOS SDK {#release-notes-ios}
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.9.2](authn-rn-ios-tvos-392.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.8.4](authn-rn-ios-tvos-384.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.8.3](authn-rn-ios-tvos-383.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.8.2](authn-rn-ios-tvos-382.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.8.1](authn-rn-ios-tvos-381.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.7.0](authn-rn-ios-tvos-370.md)
   + Notas de la versión de Android SDK {#release-notes-android}
      + [Notas de la versión de Adobe Pass Authentication Android 3.7.3](authn-rn-android-373.md)
+ Notas técnicas {#tech-notes}
   + SDK de autenticación de Adobe Pass {#primetime-authentication-sdks}
      + [Preguntas y respuestas de certificados](certificates-qa.md)
      + SDK para JavaScript {#javascript}
         + [Evaluación de la prevención del seguimiento: Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [Evaluación de la prevención de seguimiento: Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [Actualizaciones de cookies: indicadores SameSite y Secure](cookies-updates-samesite-and-secure-flags.md)
      + SDK para Android {#android}
         + [Inicio de sesión único (SSO) del SDK de Android con acceso habilitado en aplicaciones de Android 10](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Autenticación de Adobe Pass y el nuevo modelo de permisos de Android 6 &quot;Marshmallow&quot;](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + SDK {#iostvos} de iOS/tvOS
         + [Compatibilidad con WKWebView en el SDK 3.1+ de iOS](wkwebview-support-on-ios-sdk-31.md)
         + [Compatibilidad con SFSafariViewController en el SDK 3.2+ de iOS](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO en iOS al utilizar el Habilitador de acceso a autenticación de Adobe Pass](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [Error de autenticación de iOS: no se encuentra adobepass.ios.app](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Restablecer pase temporal en iOS](reset-temp-pass-on-ios.md)
         + [Depuración del SDK de AccessEnabler para iOS/tvOS mediante los registros de aplicación de la consola](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Ruta de actualización de AccessEnabler iOS/tvOS 3.7.0](accessenabler-iostvos-370-upgrade-path.md)
   + Pasar entornos de autenticación {#primetime-authentication-environments}
      + [Explicación de los entornos de Adobe](understanding-the-adobe-environments.md)
      + [Configurar el entorno y realizar pruebas en la calidad previa](setting-up-your-environment-and-testing-in-prequal.md)
      + [Prueba de los flujos de autenticación y autorización mediante el sitio de prueba de la API de Adobe](test-authn-authz-flows-using-adobes-api-test-site.md)
   + API sin cliente {#clientless-api}
      + [Implementación de API sin cliente: códigos de error/mensajes con motivo/causa probable](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Flujo de API sin cliente en ausencia de ID de dispositivo](clientless-api-flow-in-the-absence-of-device-id.md)
      + [Sin cliente: evite utilizar &#39;&amp;&#39;reg_code en la solicitud /authentication](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Habilitar los servicios de autorización de Adobe Pass para un programador en Xbox 360 y XboxOne sin cliente](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Métricas y tipo de dispositivo sin cliente](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + Experiencia del usuario {#user-exp}
      + [Migración de la página de inicio de sesión de MVPD de iFrame a elemento emergente](migr-mvpd-login-iframe-popup.md)
      + [Función de comprobación preliminar: cómo habilitar, solucionar o determinar el problema](preflight-feature.md)
   + Herramientas y utilidades {#tools-and-utilities}
      + [Uso del proxy Charles](using-charles-proxy.md)
   + Conceptos {#concepts}
      + [Explicación de los ID de usuario](understanding-user-ids.md)
+ [Guía del usuario del Tablero de TVE](tve-dashboard-user-guide.md)
+ Nueva guía de usuario del panel de TVE {#user-guide}
   + [Resumen del panel de TVE](/help/authentication/tve-dashboard-overview.md)
   + [Entornos](/help/authentication/tve-dashboard-environments.md)
   + [Revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md)
   + [Tablero](/help/authentication/tve-dashboard-home.md)
   + [Canales](/help/authentication/tve-dashboard-channels.md)
   + [Programadores](/help/authentication/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/tve-dashboard-mvpds.md)
   + [Integraciones](/help/authentication/tve-dashboard-integrations.md)
   + [Informes](/help/authentication/tve-dashboard-reports.md)
   + [Registro de cambios](/help/authentication/tve-dashboard-changes-log.md)
+ [Glosario](glossary.md)

