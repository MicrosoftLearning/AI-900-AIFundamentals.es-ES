---
lab:
  title: Exploración del reconocimiento facial
---

# Exploración del reconocimiento facial

> **Nota** Para completar este laboratorio, necesitará una [suscripción de Azure](https://azure.microsoft.com/free?azure-portal=true) en la que tenga acceso de administrador.

Las soluciones de Computer Vision a menudo requieren una solución de inteligencia artificial (IA) para poder detectar caras humanas. Por ejemplo, supongamos que la empresa minorista Northwind Traders quiere localizar a los clientes en una tienda para ayudarles mejor. Una manera de lograrlo es determinar si hay caras en las imágenes y, si es así, identificar las coordenadas del rectángulo delimitador alrededor de ellas.

Para probar las funcionalidades del servicio Face, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell. Los mismos principios y funcionalidad se aplican en soluciones reales, como sitios web o aplicaciones de teléfono.

## Creación de un recurso de *Cognitive Services*

Puede usar el servicio Face creando un recurso de **Face** o un recurso de **Cognitive Services**.

Si aún no lo ha hecho, cree un recurso de **Cognitive Services** en la suscripción de Azure.

1. En otra pestaña del explorador, abra Azure Portal en [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e inicie sesión con su cuenta Microsoft.

1. Haga clic en el botón **&amp;#65291;Crear un recurso**, busque *Cognitive Services* y cree un recurso de **Cognitive Services** con la siguiente configuración:
    - **Suscripción**: *su suscripción a Azure*.
    - **Grupo de recursos**: *cree o seleccione un grupo de recursos con un nombre único*.
    - **Región**: *elija cualquier región disponible*.
    - **Nombre**: *escriba un nombre único*.
    - **Plan de tarifa**: estándar S0
    - **Al marcar esta casilla, confirmo que he leído y comprendido todos los términos que aparecen a continuación**: Seleccionado.

1. Revise y cree el recurso y espere a que finalice la implementación. A continuación, vaya al recurso implementado.

1. Vea la página **Claves y punto de conexión** del recurso de Cognitive Services. Necesitará el punto de conexión y las claves para conectarse desde las aplicaciones cliente.

## Ejecución de Cloud Shell

Para probar las funcionalidades del servicio Face, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell en Azure. 

1. En Azure Portal, seleccione el botón **[>_]** (*Cloud Shell*) situado en la parte superior de la página, a la derecha del cuadro de búsqueda. Se abre un panel de Cloud Shell en la parte inferior del portal. 

    ![Inicio de Cloud Shell haciendo clic en el icono situado a la derecha del cuadro de búsqueda superior](media/create-face-solutions/powershell-portal-guide-1.png)

1. La primera vez que abra Cloud Shell, es posible que se le pida que elija el tipo de shell que desea usar (*Bash* o *PowerShell*). Seleccione **PowerShell**. Si no ve esta opción, omita el paso.  

1. Si se le pide que cree almacenamiento para Cloud Shell, asegúrese de que se haya especificado su suscripción y seleccione **Crear almacenamiento**. A continuación, espere un minuto más o menos a que se cree el almacenamiento.

    ![Para crear el almacenamiento, haga clic en Confirmar.](media/create-face-solutions/powershell-portal-guide-2.png)       

1. Asegúrese de que el tipo de shell indicado en la parte superior izquierda del panel de Cloud Shell se cambia a *PowerShell*. Si es *Bash*, cambie a *PowerShell* mediante el menú desplegable.

    ![Cómo encontrar el menú desplegable de la izquierda para cambiar a PowerShell](media/create-face-solutions/powershell-portal-guide-3.png) 

1. Espere a que se inicie PowerShell. Debería ver la siguiente pantalla en Azure Portal:  

    ![Espere a que se inicie PowerShell.](media/create-face-solutions/powershell-prompt.png)

## Configuración y ejecución de una aplicación cliente

Ahora que tiene un modelo personalizado, puede ejecutar una sencilla aplicación cliente que use el servicio Face.

1. En el shell de comandos, escriba el siguiente comando para descargar la aplicación de ejemplo y guárdela en una carpeta llamada ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    > **Consejo** Si ya usó este comando en otro laboratorio para clonar el repositorio *ai-900*, puede omitir este paso.

1. Los archivos se descargan en una carpeta llamada **ai-900**. Ahora queremos ver todos los archivos del almacenamiento de Cloud Shell y trabajar con ellos. Escriba el siguiente comando en el shell:

     ```PowerShell
    code .
    ```

    Observe cómo se abre un editor como el de la imagen siguiente: 

    ![Editor de código.](media/create-face-solutions/powershell-portal-guide-4.png) 

1. En el panel **Archivos** de la izquierda, expanda **ai-900** y seleccione **find-faces.ps1**. Este archivo contiene código que usa el servicio Face para detectar y analizar caras en una imagen, como se muestra aquí:

    ![Editor que contiene código para detectar caras en una imagen](media/create-face-solutions/find-faces-code.png)

1. No se preocupe demasiado por los detalles del código, lo importante es que necesita la dirección URL del punto de conexión y cualquiera de las claves para el recurso Cognitive Services. Cópielos desde la página **Claves y puntos de conexión** del recurso en Azure Portal y péguelos en el editor de código, pero reemplace los valores de marcador de posición **YOUR_KEY** y **YOUR_ENDPOINT**, respectivamente.

    > **Consejo** Es posible que tenga que usar la barra de separación para ajustar el área de pantalla mientras trabaja con los paneles **Claves y punto de conexión** y **Editor**.

    Después de pegar los valores de clave y punto de conexión, las dos primeras líneas de código deben tener un aspecto similar al siguiente:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. En la parte superior derecha del panel del editor, use el botón **...** para abrir el menú y seleccione **Guardar** para guardar los cambios. A continuación, vuelva a abrir el menú y seleccione **Cerrar editor**.

    La aplicación cliente de ejemplo usará el servicio Face para analizar la siguiente imagen, capturada por una cámara en la tienda de Northwind Traders:

    ![Imagen de un elemento primario que usa la cámara de un teléfono móvil para capturar una imagen de un elemento secundario en una tienda.](media/create-face-solutions/store-camera-1.jpg)

1. En el panel de PowerShell, escriba los siguientes comandos para ejecutar el código:

    ```PowerShell
    cd ai-900
    ./find-faces.ps1 store-camera-1.jpg
    ```

1. Revise la información devuelta, que incluye la ubicación de la cara en la imagen. La ubicación de una cara se indica mediante las coordenadas de la parte superior izquierda, y el ancho y el alto de un *rectángulo delimitador*, como se muestra aquí:

    ![Imagen de una persona con la cara identificada](media/create-face-solutions/store-camera-1-face.jpg)

    >**Nota** Las funcionalidades del servicio Face que devuelven características de identificación personal están restringidas. Para obtener información detallada, vea https://azure.microsoft.com/blog/responsible-ai-investments-and-safeguards-for-facial-recognition/.

1. Ahora vamos a probar otra imagen:

    ![Imagen de una persona con una cesta de la compra](media/create-face-solutions/store-camera-2.jpg)

    Para analizar la segunda imagen, escriba el siguiente comando:

    ```PowerShell
    ./find-faces.ps1 store-camera-2.jpg
    ```

1. Revise los resultados del análisis de la cara de la segunda imagen.

1. Vamos a probar con otra más:

    ![Imagen de una persona con un carro de la compra](media/create-face-solutions/store-camera-3.jpg)

    Para analizar la tercera imagen, escriba el siguiente comando:

    ```PowerShell
    ./find-faces.ps1 store-camera-3.jpg
    ```

1. Revise los resultados del análisis de la cara de la tercera imagen.

## Más información

Esta sencilla aplicación muestra solo algunas de las funcionalidades del servicio Face. Para más información sobre lo que puede hacer con este servicio, consulte la [página de Face API](https://azure.microsoft.com/services/cognitive-services/face/).
