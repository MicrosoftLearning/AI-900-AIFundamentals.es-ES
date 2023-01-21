---
lab:
  title: Exploración de la respuesta a preguntas
---

# <a name="explore-question-answering"></a>Exploración de la respuesta a preguntas

> **Nota** Para completar este laboratorio, necesitará una [suscripción de Azure](https://azure.microsoft.com/free?azure-portal=true) en la que tenga acceso de administrador.

En los escenarios de asistencia al cliente, es habitual crear un bot que pueda interpretar las preguntas más frecuentes y responder a ellas mediante una ventana de chat en el sitio web, por correo electrónico o mediante una interfaz de voz. La interfaz del bot subyacente es una base de conocimiento de preguntas y respuestas adecuadas en la que el bot puede buscar respuestas adecuadas.

## <a name="create-a-custom-question-answering-knowledge-base"></a>Creación de una knowledge base personalizada de respuesta a preguntas

La característica personalizada de respuesta a preguntas del servicio Language permite crear rápidamente una knowledge base, ya sea introduciendo pares de preguntas y respuestas, o desde un documento o página web existentes. A continuación, puede usar algunas funcionalidades integradas de procesamiento de lenguaje natural para interpretar preguntas y encontrar respuestas adecuadas.

1. Abra Azure Portal en [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e inicie sesión con su cuenta de Microsoft.

1. Haga clic en el botón **&#65291;Crear un recurso**, busque *Servicio Language* y cree un recurso de **Servicio Language** con los valores siguientes y, a continuación, haga clic en **Continue to create your resource (Continuar para crear su recurso):**   **Select Additional Features (Seleccionar características adicionales)**
    - **Default features** (Características predeterminadas): *mantenga las características predeterminadas*.
    - **Características personalizadas**: *seleccione respuesta a preguntas personalizadas*.

    ![Creación de un recurso de Language Service con la respuesta a preguntas personalizada habilitada.](media/create-a-bot/create-language-service-resource.png)

1. En la página **Crear Language**, especifique la configuración siguiente:
    - **Suscripción**: *su suscripción a Azure*.
    - **Grupo de recursos**: *seleccione un grupo de recursos existente o cree uno*.
    - **Nombre**: *un nombre único para el recurso de Language*.
    - **Plan de tarifa**: S (llamadas de 1K por minuto)
    - **Ubicación de Azure Search**: *cualquier ubicación disponible*.
    - **Plan de tarifa de Azure Search**: Gratis F (3 índices) - (*Si este plan no está disponible, seleccione Estándar S (50 índices)* )
    - **Al marcar esta casilla, certifico que he revisado y reconocido los términos del aviso de IA responsable**: *Seleccionado*.

    > **Nota** Si ya ha aprovisionado un recurso de **Azure Cognitive Search** en el plan Gratis, puede que la cuota no le permita crear otro. En ese caso, seleccione un nivel distinto de **Gratis F**.

1. Haga clic en **Revisar y crear**, y después en **Crear**. Espere a la implementación del servicio Language que admitirá la knowledge base personalizada de respuesta a preguntas.

1. En una pestaña nueva del explorador, abra el portal de Language Studio en [https://language.azure.com](https://language.azure.com?azure-portal=true) e inicie sesión con la cuenta de Microsoft asociada a su suscripción de Azure.

1. Si se le pide que elija un recurso de Language, seleccione la configuración siguiente:
    - **Directorio de Azure**: directorio de Azure que contiene la suscripción.
    - **Suscripción de Azure**: su suscripción a Azure.
    - **Language resource** (Recurso de Language): el recurso de lenguaje que ha creado antes.

1. Si ***no*** se le pide que elija un recurso de Language, puede deberse a que tiene varios recursos de Language en la suscripción, en cuyo caso:
    1. En la barra de la parte superior de la página, haga clic en el botón **Configuración (&#9881;)**.
    2. En la página **Configuración,** vea la pestaña **Recursos**.
    3. Seleccione el recurso de Language que acaba de crear y haga clic en **Switch resource** (Cambiar recurso).
    4. En la parte superior de la página, haga clic en **Language Studio** para volver a la página principal de Language Studio.

1. En la parte superior del portal de Language Studio, en el menú **Crear**, seleccione **Custom question answering** (Respuesta a preguntas personalizada).

1. En la página **Elegir idioma para *el recurso***, seleccione **Quiero seleccionar el idioma al crear un proyecto en este recurso** y haga clic en **Siguiente**.

1. En la página **Enter basic information** (Escribir información básica), escriba los detalles siguientes y haga clic en **Siguiente**:
    - **Language resource** (Recurso de Language): *elija el recurso de lenguaje*.  
    - **Azure search resource** (Recurso de Azure Search): *elija el recurso de Azure Search*.
    - **Nombre**: MargiesTravel.
    - **Descripción**: una knowledge base sencilla.
    - **Idioma de origen**: inglés.
    - **Default answer when no answer is returned** (Respuesta predeterminada cuando no se devuelve ninguna respuesta): no se encontró ninguna respuesta.

1. En la página **Revisar y finalizar**, haga clic en **Crear proyecto**.

1. Se le llevará a la página **Manage sources** (Administrar orígenes). Haga clic en **&#65291;Agregar origen** y seleccione **URL**.

1. En el cuadro **Add URLs** (Agregar direcciones URL), haga clic en **+ Agregar URL**. Escriba lo siguiente y seleccione **Agregar todo**:
    - **Nombre de la dirección URL**: MargiesKB
    - **URL**: `https://raw.githubusercontent.com/MicrosoftLearning/AI-900-AIFundamentals/main/data/qna/margies_faq.docx`
    - **Classify file structure** (Clasificar estructura de archivos): *Detectar automáticamente* 

## <a name="edit-the-knowledge-base"></a>Edición de la base de conocimiento

La base de conocimiento se basa en los detalles del documento de preguntas más frecuentes y en algunas respuestas predefinidas. Puede agregar pares personalizados de preguntas y respuestas para complementarlos.

1. Haga clic en **Edit knowledge base** (Editar knowledge base) en el panel izquierdo. Después, haga clic en **+ Add question pair** (+ Agregar par de preguntas).

1. En el cuadro **Preguntas**, escriba `Hello`y, a continuación, haga clic en **Enviar cambios**.

1. Haga clic en **+ Agregar frase alternativa** y escriba `Hi`y, a continuación, haga clic en **Enviar cambios**.

1. En el cuadro **Answer and prompts** (Respuestas y mensajes), escriba `Hello`. Mantenga el valor de **Origen**: Editorial.

1. Haga clic en **Enviar**. Después, en la parte superior de la página, haga clic en **Guardar cambios**. Es posible que tenga que cambiar el tamaño de la ventana para ver el botón.

## <a name="train-and-test-the-knowledge-base"></a>Entrenamiento y prueba de la base de conocimiento

Ahora que tiene una knowledge base, puede probarla.

1. En la parte superior de la página, haga clic en **Probar** para probar la knowledge base.

1. En el panel de prueba, en la parte inferior, escriba el mensaje *Hola*. Se debe devolver la respuesta **Hola**.

1. En el panel de prueba, en la parte inferior, escriba el mensaje *Quiero reservar un vuelo*. Se debe devolver una respuesta adecuada de las preguntas más frecuentes.

    > **Nota** La respuesta incluye una *rspuesta corta*, además de un fragmento de respuesta *más detallado*: el fragmento de respuesta muestra el texto completo en el documento de preguntas más frecuentes de la pregunta coincidente más parecida, mientras que la respuesta corta se extrae del fragmento de manera inteligente. Puede controlar si la visualización de la respuesta corta es de la respuesta; para ello, seleccione la casilla **Display short answer** (Mostrar respuesta corta) en la parte superior del panel de prueba.

1. Pruebe otra pregunta, como *¿Cómo puedo cancelar una reserva?*.

1. Cuando haya terminado de probar la base de conocimiento, haga clic en **Probar** para cerrar el panel de prueba.

## <a name="create-a-bot-for-the-knowledge-base"></a>Creación de un bot para la base de conocimiento

La base de conocimiento proporciona un servicio back-end que las aplicaciones cliente pueden usar para responder preguntas a través de algún tipo de interfaz de usuario. Normalmente, estas aplicaciones cliente son bots. Para que la base de conocimiento esté disponible para un bot, debe publicarlo como un servicio al que se pueda acceder a través de HTTP. A continuación, puede usar Azure Bot Service para crear y hospedar un bot que use la base de conocimiento para responder a las preguntas del usuario.

1. A la izquierda de la página de Language Studio, haga clic en **Deploy knowledge base** (Implementar knowledge base).

1. En la parte superior de la página, haga clic en **Implementar** y vuelva a hacer clic en **Implementar**.

1. Una vez implementado el servicio, haga clic en **Create a Bot** (Crear un bot). Se abrirá Azure Portal en una nueva pestaña del explorador para que pueda crear un bot de aplicación web en su suscripción de Azure.

1. En Azure Portal, cree un bot de aplicación web con la siguiente configuración (la mayoría de las opciones se rellenarán previamente):
    - **Identificador de bot**: *nombre único para el bot*.
    - **Suscripción**: *suscripción de Azure*
    - **Grupo de recursos**: *grupo de recursos que contiene el recurso de Language*
    - **Ubicación**: *la misma que la del servicio de lenguaje*.
    - **Plan de tarifa**: Gratis (F0).
    - **Nombre de aplicación**: *el mismo que el **Identificador de bot** con **.azurewebsites.net** agregado automáticamente*.
    - **Lenguaje del SDK**: *elija C# o Node.js*.
    - **Clave de recurso de idioma**: *se genera automáticamente. Si no la ve, debe crear un proyecto de respuesta a preguntas en Language Studio*. 
    - **Plan o ubicación de App Service**: *seleccione la flecha para crear un plan. Después, cree un nombre de plan de App Service único y elija una ubicación adecuada*.
    - **Application Insights**: desactivado.
    - **Identificador de aplicación de Microsoft y contraseña**: *cree automáticamente el identificador de aplicación y la contraseña*.

1. Espere a que se cree el bot (el icono de notificación de la parte superior derecha, que parece una campana, se moverá mientras espera). A continuación, en el mensaje que informa sobre la finalización de la implementación, haga clic en **Ir al recurso** (otra opción es hacer clic en **Grupos de recursos** en la página principal, abrir el grupo de recursos en el que creó el bot de aplicación web y hacer clic en él).

1. En el panel izquierdo del bot, busque **Configuración**, haga clic en **Probar en el Chat en web** y espere hasta que el bot muestre el mensaje **Hola y bienvenido** (puede tardar unos segundos en inicializarse).

1. Use la interfaz de chat de prueba para asegurarse de que el bot responde a las preguntas de la base de conocimiento según lo previsto. Por ejemplo, intente enviar *Necesito cancelar mi hotel*.

Experimente con el bot. Probablemente observará que el bot puede responder preguntas de las preguntas más frecuentes con bastante precisión, pero tendrá una capacidad limitada para interpretar las preguntas con las que no se ha entrenado. Siempre puede usar Language Studio para editar la knowledge base a fin de mejorarla y volver a publicarla.

## <a name="learn-more"></a>Más información

- Para obtener más información sobre el servicio de respuesta a preguntas, vea la [documentación](https://docs.microsoft.com/azure/cognitive-services/language-service/question-answering/overview).
- Para más información sobre Microsoft Bot Service, consulte la [página de Azure Bot Service](https://azure.microsoft.com/services/bot-service/).
