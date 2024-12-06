---
title: Función de comprobación preliminar, Cómo habilitar, solucionar problemas o determinar el problema
description: Función de comprobación preliminar, Cómo habilitar, solucionar problemas o determinar el problema
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Función de comprobación preliminar: Cómo habilitar, solucionar problemas o determinar el problema {#preflight-feature}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Se ha producido un cambio en la forma en que la autenticación de Adobe Pass calcula los recursos preautorizados. La API de preautorización tiene una nueva implementación. Esta implementación reemplaza la solución antigua, que consiste en realizar varias llamadas de autorización únicamente.
La interfaz externa para la API de preautorización no cambia y no se requieren actualizaciones en la aplicación del programador.

Existen tres formas de calcular los recursos de comprobaciones:

* **Método de bifurcación y unión a MVPD**: esto implica que el Adobe realice varias llamadas de autorización a MVPD (aunque el cliente tendrá que realizar una llamada de comprobación preliminar).
* **Línea de canales**: la MVPD expone la línea de canales para el usuario que ha iniciado sesión en la respuesta de autenticación SAML y el Adobe devuelve los recursos autorizados basados en eso. La respuesta authN de SAML en el rastreador de SAML debe exponer esa lista.
* **Autorización multicanal**: la autenticación de cliente y Adobe realizan una sola llamada a la MVPD para un conjunto de recursos.

Independientemente de la MVPD, la aplicación cliente realizará una sola llamada al extremo de comprobación preliminar (API checkPreauthorizedResources), pasando un conjunto de resourceIDs. En función de una de las formas anteriores admitidas por MVPD, Adobe devolverá los ID de recurso preautorizados.

Si la comprobación preliminar se basa en el método fork &amp; join, el backend de autenticación de Adobe Pass comprueba un valor establecido para el &quot;máximo de llamadas de preautorización&quot; en su configuración. Se configura mediante el Adobe.

El valor predeterminado para la configuración &quot;máximo de llamadas de preautorización&quot; es &quot;5&quot;, lo que significa que al máximo solo se pueden enviar 5 recursos en Comprobación preliminar para las MVPD de ramificación y unión. Si se pasan más de cinco recursos, se producirá una excepción y se devolverá una lista nula. Este es el comportamiento esperado. Podemos configurarlo con cualquier valor si la MVPD no admite la alineación de canales o la autorización de varios canales, pero solo después de consultarlos como varias llamadas de autorización de ramificación y unión aumentarán sus tiempos de carga.

Por lo tanto, estos son aspectos que hay que buscar al habilitar/solucionar problemas de comprobación preliminar para una MVPD:

* El método que admite la MVPD (ramificación y unión, alineación de canales o multicanal).
* Si solo se admite fork &amp; join, es necesario preguntar al programador cuántos resourceID enviará en la llamada de comprobación preliminar.
* Es necesario consultar la MVPD y conocer el impacto de realizar un número &quot;n&quot; de llamadas de autorización de ramificación y unión. Después, el valor debe configurarse en config si es mayor que 5.

**Limitación**

Tenga en cuenta que no obtenemos ningún resourceID de la llamada de comprobación preliminar para algunas MVPD como AT&amp;T y TWC si alguno de los resourceID es un ID falso o un ID no reconocido en la lista de resourceID que envían en la llamada de comprobación preliminar, aunque también tengamos recursos válidos y autorizados en esa lista.
