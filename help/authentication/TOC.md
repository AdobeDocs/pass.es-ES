---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass Authentication
user-guide-description: Adobe Pass Authentication es una solución de asignación de derechos para TV Everywhere que proporciona un marco modular para determinar si quien solicita acceso a un recurso tiene derechos para acceder.
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 3%

---


# Ayuda de autenticación de Adobe Pass {#authentication}

+ [Adobe Pass Authentication](home.md)
+ KickStart {#kickstart}
   + [Documento técnico](kickstart/technical-paper.md)
   + [Información general del programador](kickstart/programmer-overview.md)
   + [Información general de MVPD](kickstart/mvpd-overview.md)
   + [Guía de KickStart del programador](kickstart/programmer-kickstart-guide.md)
   + [Guía de inicio de MVPD](kickstart/mvpd-kickstart-guide.md)
   + [Procedimientos de escalación](notes-technical/escalation-procedures.md)
   + [Glosario](kickstart/glossary.md)
+ Guía De Integración Para Programadores {#integration-guide-programmers}
   + API de REST {#rest-apis}
      + DCR API REST {#rest-api-dcr}
         + [Información general sobre el registro dinámico de clientes](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + API {#rest-api-dcr-apis}
            + [Recuperar credenciales de cliente](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [Recuperar token de acceso](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + Flujos {#rest-api-dcr-flows}
            + [Flujo de registro de cliente dinámico](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + API DE REST V2 {#rest-api-v2}
         + [Información general de la API REST 2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [Glosario de la API de REST 2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [Preguntas frecuentes sobre la API de REST V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + API {#rest-api-v2-apis}
            + [Información general sobre las API de REST V2](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + Configuración {#rest-api-v2-configuration-apis}
               + [Recuperar la configuración de un proveedor de servicios específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + Sesiones {#rest-api-v2-sessions-apis}
               + [Crear sesión de autenticación](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [Reanudar sesión de autenticación](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [Recuperar sesión de autenticación](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [Realizar autenticación en el agente de usuario](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + Perfiles {#rest-api-v2-profiles-apis}
               + [Recuperación de perfiles](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [Recuperar perfil para mvpd específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [Recuperar perfil para código específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + Decisiones {#rest-api-v2-decisions-apis}
               + [Recuperar decisiones de autorización utilizando mvpd específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [Recuperar decisiones de preautorización utilizando mvpd específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + Cerrar sesión {#rest-api-v2-logout-apis}
               + [Iniciar el cierre de sesión de un mvpd específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + Inicio de sesión único de socio {#rest-api-v2-partner-single-sign-on-apis}
               + [Recuperar solicitud de autenticación de socio](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [Recuperar perfil mediante la respuesta de autenticación del socio](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + Flujos {#rest-api-v2-flows}
            + [Información general sobre flujos de la API REST V2](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + Flujos de acceso básicos {#rest-api-v2-basic-access-flows}
               + [Flujo de perfiles básicos realizado dentro de la aplicación principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [Flujo de perfiles básicos realizado en la aplicación secundaria](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [Flujo de autenticación básico realizado en la aplicación principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [Flujo de autenticación básico realizado en la aplicación secundaria](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [Flujo de autorización básico realizado en la aplicación principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [Flujo de preautorización básico realizado en la aplicación principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [Flujo de cierre de sesión básico realizado en la aplicación principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + Flujos de acceso degradados {#rest-api-v2-degraded-access-flows}
               + [Flujos de acceso degradados](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + Flujos de acceso temporales {#rest-api-v2-temporary-access-flows}
               + [Flujos de acceso temporales](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + Flujos de acceso de inicio de sesión único {#rest-api-v2-single-sign-on-access-flows}
               + [Inicio de sesión único con flujos de socios](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [Inicio de sesión único con flujos de identidad de plataforma](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [Inicio de sesión único mediante flujos de token de servicio](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [Flujo de cierre de sesión único](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + Libros de cocina {#rest-api-v2-cookbooks}
            + [Guía de API de REST V2 (de cliente a servidor)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [Guía de API de REST V2 (servidor a servidor)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + Apéndice {#rest-api-v2-appendix}
            + Encabezados {#rest-api-v2-appendix-headers}
               + [Encabezado: autorización](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [Encabezado: AP-Device-Identifier](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [Encabezado: X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [Encabezado: AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [Encabezado: Adobe-Subject-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [Encabezado: AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [Encabezado: AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + Características estándar {#standard-features}
      + Derechos {#entitlements}
         + [Identificar recurso protegido](integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)
         + [Autorización de verificación previa](integration-guide-programmers/features-standard/entitlements/preflight-authz.md)
         + [Cómo integrar el Media Token Verifier](integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)
         + [Metadatos del usuario](integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md)
         + [Certificado de metadatos de usuario para cifrado](integration-guide-programmers/features-standard/entitlements/user-metadata-certificate.md)
      + Informes de errores {#error-reporting}
         + [Códigos de error mejorados](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
         + [Informes de errores](integration-guide-programmers/features-standard/error-reporting/error-reporting.md)
      + Acceso de inicio de sesión único {#sso-access}
         + Inicio de sesión único de socio {#partner-sso}
            + Inicio de sesión único de Apple {#apple-sso}
               + [Información general sobre Apple SSO](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Guía de Apple SSO (API REST V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
               + [Guía de Apple SSO (API de REST V1)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v1.md)
               + [Guía de Apple SSO (SDK de iOS/tvOS)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-iostvos-sdk.md)
         + Inicio de sesión único de Platform {#platform-sso}
            + Inicio de sesión único de Amazon {#amazon-sso}
               + [Guía de Amazon SSO (API REST V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
               + [Guía de Amazon SSO (API de REST V1)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
            + Inicio de sesión único de Roku {#roku-sso}
               + [Información general de Roku SSO](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
         + [Compatibilidad con inicio de sesión único](integration-guide-programmers/features-standard/sso-access/sso-support.md)
         + [SSO mediante autenticación pasiva](integration-guide-programmers/features-standard/sso-access/sso-passive-authn.md)
      + Acceso a autenticación basada en inicio {#hba-access}
         + [Autenticación basada en el hogar para TV en todas partes](integration-guide-programmers/features-standard/hba-access/home-based-authn-tve.md)
         + [Estado de HBA para MVPD](integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)
      + Compatibilidad con privacidad {#privacy-support}
         + [Información general de soporte de privacidad](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [Cómo realizar una solicitud de privacidad](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + Características Premium {#features-premium}
      + Acceso temporal {#temporary-access}
         + [Pase temporal](integration-guide-programmers/features-premium/temporary-access/temp-pass.md)
         + [Pase temporal promocional](integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
         + [Restablecer pase temporal](integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md)
      + Acceso degradado {#degraded-access}
         + [Resumen de API de degradación](integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)
      + ESM {#esm}
         + [Resumen de monitorización del servicio de derechos](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [API de monitorización del servicio de derechos](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [Métricas del lado del servidor](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + Analytics {#analytics}
         + [Integración de datos del lado del servidor de Adobe Pass Authentication en Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Uso del ID de Experience Cloud en la autenticación de Adobe Pass](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + Heredado {#legacy}
      + API de REST (heredada) 1 {#rest-api-v1}
         + [Información general de la API REST 1](integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
         + [Referencia de la API de REST 1](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + API (heredadas) {#rest-api-v1-apis}
            + [Solicitud de código de registro](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [Devolver registro de registro](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [Eliminar registro](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [Proporcionar lista de MVPD](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [Iniciar autenticación](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [Comprobar token de autenticación](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [Recuperar token de autenticación](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [Iniciar autorización](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [Recuperar token de autorización](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [Obtener token de medios corto](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [Comprobar flujo de autenticación por aplicación web en segunda pantalla](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [Recuperar lista de recursos autorizados previamente](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [Recuperar lista de recursos preautorizados por aplicación web de segunda pantalla](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [Iniciar cierre de sesión](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [Metadatos del usuario](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [Recuperar solicitud de perfil](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [Intercambio de tokens](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [Vista previa gratuita para Pase temporal y Pase temporal promocional](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + Libros de cocina (heredados) {#rest-api-v1-cookbooks}
            + [Guía de API de REST V1 (de cliente a servidor)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [Guía de API de REST V1 (servidor a servidor)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + SDK (heredados) {#sdks}
         + SDK de JavaScript (heredado) {#javascript-sdk}
            + [Información general del SDK para JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [Guía del SDK para JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [Referencia de API de SDK para JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [Autorización previa de API de SDK para JavaScript](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + Directrices (heredadas) {#javascript-sdk-guidelines}
               + [Inicio y cierre de sesión sin actualización](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + (Heredado) SDK de iOS/tvOS {#ios-tvos-sdk}
            + [Información general del SDK para iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [Guía del SDK para iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [Referencia de la API del SDK para iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [Autorización previa de API de SDK para iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + Directrices (heredadas) {#ios-tvos-sdk-guidelines}
               + [Registro de aplicaciones de iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [Guía de migración de iOS/tvOS v3.x](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [Comprobaciones de integridad del almacenamiento de iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + SDK de Android (heredado) {#android-sdk}
            + [Información general del SDK para Android](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [Guía del SDK para Android](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [Referencia de API de SDK para Android](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [Autorización previa de API de SDK para Android](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + Directrices (heredadas) {#android-sdk-guidelines}
               + [Registro de aplicaciones Android](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [SDK de Android con registro de cliente dinámico](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + (Heredado) SDK de FireOS {#fireos-sdk}
            + [Información general técnica de Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [Guía de integración de Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [Referencia de la API de Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [Registro de aplicaciones de Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [FireOS SDK con registro de cliente dinámico](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [Amazon FireOS SSO - Guía de inicio del programador](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
   + [Información general sobre la guía de integración del programador](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [Mecanismo de limitación](integration-guide-programmers/throttling-mechanism.md)
   + [Requisitos mínimos del sistema](integration-guide-programmers/minimum-system-requirements.md)
   + [Flujo de derechos del programador](integration-guide-programmers/entitlement-flow.md)
   + [Casos de uso del programador](integration-guide-programmers/programmer-use-cases.md)
   + [Pasar información del cliente (dispositivo, conexión y aplicación)](integration-guide-programmers/passing-client-information-device-connection-and-application.md)
+ Guía de integración para MVPD {#integration-guide-mvpds}
   + [Funciones de integración](integration-guide-mvpds/mvpd-integr-features.md)
   + [Autenticación](integration-guide-mvpds/authn-usecase.md)
   + [Autenticación mediante el protocolo OAuth 2.0](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [Autorización](integration-guide-mvpds/authz-usecase.md)
   + [Autorización de verificación previa](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [Cierre de sesión de MVPD](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [Intercambio de metadatos de contenido](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [Intercambio de metadatos de usuario](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [Servicio web de MVPD proxy](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [Integración de SAML de MVPD proxy](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [Ámbito del proveedor de servicios](integration-guide-mvpds/serv-provider-scoping.md)
   + [Direcciones IP permitidas de MVPD](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ Guía del usuario para el tablero de TVE {#user-guide-tve-dashboard}
   + [Resumen del panel de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [Entornos](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [Revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Tablero](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [Canales](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [Programadores](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [Integraciones](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [Informes](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [Registro de cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
   + [Guía del usuario del Tablero de TVE](user-guide-tve-dashboard/tve-dashboard-user-guide.md)
+ Notas de la versión {#release-notes}
   + {#release-notes-2024} de 2024
      + [Notas de la versión de Adobe Pass Authentication 3.0.3](notes-releases/auth-rn-303.md)
      + [Notas de la versión de Adobe Pass Authentication 3.0](notes-releases/auth-rn-300.md)
      + [Notas de la versión de Adobe Pass Authentication 2.70](notes-releases/auth-rn-270.md)
      + [Notas de la versión de Adobe Pass Authentication 2.69](notes-releases/auth-rn-269.md)
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.7.0](notes-releases/authn-rn-javascript-470.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.9.2](notes-releases/authn-rn-ios-tvos-392.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.8.4](notes-releases/authn-rn-ios-tvos-384.md)
   + {#release-notes-2023} de 2023
      + [Notas de la versión de Adobe Pass Authentication 2.68](notes-releases/auth-rn-268.md)
      + [Notas de la versión de Adobe Pass Authentication 2.67](notes-releases/auth-rn-267.md)
      + [Notas de la versión de Adobe Pass Authentication 2.66](notes-releases/auth-rn-266.md)
      + [Notas de la versión de autenticación de Adobe Pass 2.65.1](notes-releases/auth-rn-2651.md)
      + [Notas de la versión de Adobe Pass Authentication 2.65](notes-releases/auth-rn-265.md)
      + [Notas de la versión de autenticación de Adobe Pass 2.64.1](notes-releases/auth-rn-2641.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.8.3](notes-releases/authn-rn-ios-tvos-383.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.8.2](notes-releases/authn-rn-ios-tvos-382.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.8.1](notes-releases/authn-rn-ios-tvos-381.md)
      + [Notas de la versión de Adobe Pass Authentication Android 3.7.3](notes-releases/authn-rn-android-373.md)
   + {#release-notes-2022} de 2022
      + [Notas de la versión de Adobe Pass Authentication 2.64](notes-releases/auth-rn-264.md)
      + [Notas de la versión de Adobe Pass Authentication 2.63](notes-releases/auth-rn-263.md)
      + [Notas de la versión de autenticación de Adobe Pass 2.62.1](notes-releases/auth-rn-2621.md)
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.6.0](notes-releases/authn-rn-javascript-460.md)
   + {#release-notes-2021} de 2021
      + [Notas de la versión de Adobe Pass Authentication JavaScript 4.4.0](notes-releases/authn-rn-javascript-440.md)
      + [Notas de la versión de Adobe Pass Authentication iOS/tvOS 3.7.0](notes-releases/authn-rn-ios-tvos-370.md)
+ Notas técnicas {#tech-notes}
   + Entornos {#tech-notes-environments}
      + [Explicación de los entornos de Adobe](notes-technical/understanding-the-adobe-environments.md)
      + [Configurar el entorno y realizar pruebas en la calidad previa](notes-technical/setting-up-your-environment-and-testing-in-prequal.md)
      + [Prueba de los flujos de autenticación y autorización mediante el sitio de prueba de la API de Adobe](notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + Experiencia del usuario {#tech-notes-user-experience}
      + [Migración de la página de inicio de sesión de MVPD de iFrame a elemento emergente](notes-technical/migr-mvpd-login-iframe-popup.md)
      + [Función de comprobación preliminar: cómo habilitar, solucionar o determinar el problema](notes-technical/preflight-feature.md)
      + [Permitir MVPD en el cuadro de diálogo de selección](notes-technical/allow-mvpd-selectn-dialog.md)
      + [Evitar que las MVPD aparezcan en el cuadro de diálogo de selección](notes-technical/prevent-mvpd-selectn-dialog.md)
   + API DE REST V1 {#tech-notes-rest-api-v1}
      + [Implementación de API sin cliente: códigos de error/mensajes con motivo/causa probable](notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Flujo de API sin cliente en ausencia de ID de dispositivo](notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
      + [Sin cliente: evite utilizar &#39;&amp;&#39;reg_code en la solicitud /authentication](notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Habilitar los servicios de autorización de Adobe Pass para un programador en Xbox 360 y XboxOne sin cliente](notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Métricas y tipo de dispositivo sin cliente](notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + SDK {#tech-notes-sdks}
      + [Preguntas y respuestas de certificados](notes-technical/certificates-qa.md)
      + [Explicación de los ID de usuario](notes-technical/understanding-user-ids.md)
      + SDK para JavaScript {#tech-notes-javascript-sdk}
         + [Evaluación de la prevención del seguimiento: Apple Safari](notes-technical/tracking-prevention-assessment-apple-safari.md)
         + [Evaluación de la prevención de seguimiento: Google Chrome](notes-technical/tracking-prevention-assessment-google-chrome.md)
         + [Actualizaciones de cookies: indicadores SameSite y Secure](notes-technical/cookies-updates-samesite-and-secure-flags.md)
         + [Sugerencias de depuración](notes-technical/appendix-b-debugging-tips.md)
      + SDK para Android {#tech-notes-android-sdk}
         + [Inicio de sesión único (SSO) del SDK de Android con acceso habilitado en aplicaciones de Android 10](notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Autenticación de Adobe Pass y el nuevo modelo de permisos de Android 6 &quot;Marshmallow&quot;](notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + SDK {#tech-notes-ios-tvos-sdk} de iOS/tvOS
         + [Compatibilidad con WKWebView en el SDK 3.1+ de iOS](notes-technical/wkwebview-support-on-ios-sdk-31.md)
         + [Compatibilidad con SFSafariViewController en el SDK 3.2+ de iOS](notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO en iOS al utilizar el Habilitador de acceso a autenticación de Adobe Pass](notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [Error de autenticación de iOS: no se encuentra adobepass.ios.app](notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Restablecer pase temporal en iOS](notes-technical/reset-temp-pass-on-ios.md)
         + [Depuración del SDK de AccessEnabler para iOS/tvOS mediante los registros de aplicación de la consola](notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Ruta de actualización de AccessEnabler iOS/tvOS 3.7.0](notes-technical/accessenabler-iostvos-370-upgrade-path.md)
   + Solución de problemas {#tech-notes-troubleshooting}
      + [Uso del proxy Charles](notes-technical/using-charles-proxy.md)
      + [Monitorización de Adobe Pass Adobe PayTV Pass](notes-technical/monitoring-adobe-pay-tv-pass.md)
