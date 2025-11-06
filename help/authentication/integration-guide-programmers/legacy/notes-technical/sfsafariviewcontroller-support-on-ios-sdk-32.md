---
title: Compatibilidad con SFSafariViewController en iOS SDK 3.2+
description: Compatibilidad con SFSafariViewController en iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# (Heredado) Compatibilidad con SFSafariViewController en iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

</br>


**Debido a requisitos de seguridad, algunas páginas de inicio de sesión de MVPD DEBEN presentarse en un SFSafariViewController, en lugar de vistas web.**

Algunos MVPD requieren que sus páginas de inicio de sesión se presenten en un control de explorador seguro como SFSafariViewController. Están bloqueando activamente las visualizaciones web, por lo que para poder autenticarnos con ellas debemos usar SVC.

## Compatibilidad {#compatiblity}

A partir de iOS SDK versión 3.1, AccessEnabler SDK muestra automáticamente la página de inicio de sesión de MVPD específico en un SFSafariViewController, en función de la configuración del servidor.

La versión 3.1 de SDK presenta automáticamente SFSafariViewController desde el controlador de vista raíz de la aplicación. Aunque esto simplifica la administración de páginas de inicio de sesión para los implementadores, hay casos en los que no es posible presentar el SFSafariViewController desde el controlador de vista raíz, debido a la implementación específica de la aplicación (como un controlador modal ya visible).

Para estos casos, la versión 3.2 introduce la capacidad para que el programador administre manualmente el SVC.

## Administración manual de SVC {#manual-svc-management}

Para administrar manualmente SVC, el implementador debe realizar los siguientes pasos:


1. llame a **setOptions([&quot;handleSVC&quot;:true])** después de la inicialización de AccessEnabler (asegúrese de que esta llamada se realice antes de que comience la autenticación). Esto habilitará la administración &quot;manual&quot; del SVC, el SDK no presentará automáticamente el SVC, sino que, cuando sea necesario, lo hará     llamar a **navegar(toUrl:*{url}* useSVC:true)**.

1. implementar la llamada de retorno opcional **`navigateToUrl:useSVC:`** dentro de la implementación debe crear una instancia de servicio mediante la instancia de SFSafariViewController mediante la dirección url proporcionada y presentarla en la pantalla:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Notas:***

   - *Puede personalizar SFSafariViewController como desee. Por ejemplo, en iOS 11+, puede cambiar la etiqueta &quot;Listo&quot; a &quot;Cancelar&quot;.*
   - *para poder descartar el servicio, necesita una referencia a él, no lo cree en el ámbito de **navegarToUrl:useSVC***
   - *usar su propio controlador de vista para &quot;myController&quot;*


1. En la implementación delegada de la aplicación de **application(\_app: UIApplication, abra la URL: URL, opciones: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**, agregue código para cerrar el servicio. Ya debería tener código allí que llame a **accessEnabler.handleExternalURL()**. Justo debajo añada:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   De nuevo, svc es una referencia al SFSafariViewController que creó en el paso 2.


1. Implemente **safariViewControllerDidFinish(\_ controller: SFSafariViewController)** desde **SFSafariViewControllerDelegate** para detectar cuándo el usuario canceló el servicio con el botón &quot;Listo&quot;. En esta función, para informar a SDK de que la autenticación se ha cancelado, debe llamar a:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
