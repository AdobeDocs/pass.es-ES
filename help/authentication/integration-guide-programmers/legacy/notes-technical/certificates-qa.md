---
title: Preguntas y respuestas de certificados
description: Preguntas y respuestas de certificados
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Preguntas y respuestas sobre certificados (heredados) {#certificates-q}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

</br>

**T1:** ¿Es posible registrar certificados en iOS y Android?

**A:** El certificado para iOS y Android es el mismo en la configuración actual. El certificado nativo se utiliza para ambas plataformas.

</br>

**T2:** ¿Se pueden usar los mismos certificados de iOS como certificados primarios y de copia de seguridad en los entornos Producción y Ensayo? Si no se recomienda, ¿puede ofrecer una explicación?

**A:** No sirve de nada configurar el mismo certificado como certificado principal y de copia de seguridad. Tenemos el concepto de certificados primarios y de copia de seguridad para que podamos configurar varios certificados para un programador en caso de que el certificado primario caduque o sea revocado. Tener un certificado de copia de seguridad dará tiempo a los programadores de cambiar el principal sin afectar al entorno de lanzamiento. Sin embargo, puede utilizar el mismo conjunto de certificados principales y de copia de seguridad para los perfiles de producción y ensayo.

</br>

**T3:** ¿Se necesita un nuevo certificado para las páginas web que usarán el nuevo TempPass flexible?

**A:** El certificado (y cualquier certificado) está configurado a nivel de empresa de medios y programador. FlexibleTempPass es un MVPD, no necesita configurar ningún certificado para él, por lo que si está integrando un Programador existente con TempPass flexible, se utilizará el certificado que ya está configurado a nivel de Programador / Media Company.
