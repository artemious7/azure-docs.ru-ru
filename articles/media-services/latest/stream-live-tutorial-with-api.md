---
title: Потоковая трансляция с помощью Служб мультимедиа Azure v3 и .NET Core | Документация Майкрософт
description: В этом руководстве описано, как настроить потоковую трансляцию с помощью Служб мультимедиа Azure v3 и .NET Core.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: bd149177a91bc0d5897723df2fad50fef11a37ef
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392341"
---
# <a name="stream-live-with-azure-media-services-v3-using-net-core"></a>Потоковая трансляция с помощью Служб мультимедиа Azure v3 и .NET Core

В Службах мультимедиа [События прямой трансляции](https://docs.microsoft.com/rest/api/media/liveevents) отвечают за обработку содержимого потоковой трансляции. Событие прямой трансляции предоставляет входную конечную точку (URL-адрес входа), которую вы можете передать динамическому кодировщику. Событие прямой трансляции получает от кодировщика входные потоки трансляции и предоставляет их для потоковой передачи через одну или несколько [конечных точек потоковой передачи](https://docs.microsoft.com/rest/api/media/streamingendpoints). Также события прямой трансляции предоставляют конечную точку предварительного просмотра (URL-адрес предварительного просмотра), которая позволяет просмотреть и проверить поток перед его обработкой и доставкой. В этом руководстве описано, как с помощью .NET Core создавать события прямой трансляции **сквозного** типа. 

> [!NOTE]
> Прежде чем продолжить работу, ознакомьтесь со статьей [Live streaming with Azure Media Services v3](live-streaming-overview.md) (Потоковая трансляция в Службах мультимедиа Azure v3). 

В этом руководстве описаны следующие процедуры.    

> [!div class="checklist"]
> * Создание учетной записи служб мультимедиа
> * Доступ к API Служб мультимедиа.
> * Настройка примера приложения
> * Проверка кода, который выполняет потоковую трансляцию
> * Просмотр событий с помощью [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) в http://ampdemo.azureedge.net
> * Очистка ресурсов

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

Ниже перечислены необходимые условия для выполнения действий, описанных в этом учебнике.

* Установка Visual Studio Code или Visual Studio.
* Веб-камера или другое устройстве (например, ноутбук) для потоковой трансляции события.
* Локальный динамический кодировщик, который преобразует сигналы от камеры в потоки и отправляет их службе потоковой трансляции в Службах мультимедиа. Поток должен иметь формат **RTMP** или **Smooth Streaming**.

## <a name="download-the-sample"></a>Скачивание примера приложения

Клонируйте репозиторий GitHub, содержащий пример потоковой передачи данных .NET, на компьютер с помощью следующей команды:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Пример потоковой трансляции размещен в папке [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp).

> [!IMPORTANT]
> Этот пример использует уникальный суффикс для каждого ресурса. Если вы отмените отладку или завершите приложение раньше, чем оно закончит свою работу, в вашей учетной записи сохранятся несколько событий прямой трансляции. <br/>
> Не забудьте остановить все запущенные события прямой трансляции. В противном случае за них будет **начислена плата**!

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-performs-live-streaming"></a>Проверка кода, который выполняет потоковую трансляцию

В этом разделе описаны функции, определенные в файле [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) проекта *MediaV3LiveApp*.

Этот пример создает уникальный суффикс для каждого ресурса, чтобы избежать конфликтов имен при многократном запуске примера без очистки.

> [!IMPORTANT]
> Этот пример использует уникальный суффикс для каждого ресурса. Если вы отмените отладку или завершите приложение раньше, чем оно закончит свою работу, в вашей учетной записи сохранятся несколько событий прямой трансляции. <br/>
> Не забудьте остановить все запущенные события прямой трансляции. В противном случае за них будет **начислена плата**!
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>Начало использования API Служб мультимедиа с пакетом SDK для .NET

Чтобы начать использование API Служб мультимедиа с .NET, создайте объект **AzureMediaServicesClient**. Чтобы создать объект, введите учетные данные, необходимые клиенту для подключения к Azure с помощью Azure AD. В коде, который вы клонировали в начале статьи, функция **GetCredentialsAsync** создает объект ServiceClientCredentials с использованием учетных данных, предоставленных в локальном файле конфигурации. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Создание события прямой трансляции

В этом разделе показано, как создать событие прямой трансляции **сквозного** типа (для которого параметр LiveEventEncodingType имеет значение None). Если вы хотите создать событие прямой трансляции, поддерживающее кодирование в реальном времени, задайте для LiveEventEncodingType значение Basic. 

При создании событий прямой трансляции также можно настроить следующее:

* Расположение Служб мультимедиа. 
* Протокол потоковой передачи для событий прямой трансляции (сейчас поддерживаются протоколы RTMP и Smooth Streaming).
       
    Изменить протокол во время работы события прямой трансляции или связанных с ним выходов прямой трансляции нельзя. Если вам нужны другие протоколы, создайте отдельное событие потоковой трансляции для каждого из них.  
* Ограничения по IP-адресам для приема и предварительного просмотра. Вы можете определить IP-адреса, с которых событие потоковой трансляции будет принимать видео. Можно указать отдельный допустимый IP-адрес (например, 10.0.0.1), или диапазон IP-адресов, используя IP-адрес и маску подсети CIDR (например, 10.0.0.1/22), или IP-адрес и маску подсети с точками (например, 10.0.0.1(255.255.252.0)).
    
    Если IP-адреса не указаны и правила не заданы, ни один IP-адрес не разрешен. Чтобы разрешить все IP-адреса, создайте правило и задайте адрес 0.0.0.0/0.

Для создаваемого события можно настроить автоматический запуск. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>Получение URL-адресов приема

После создания канала можно получить URL-адреса приема, которые необходимо передать динамическому кодировщику. Он использует эти адреса для передачи динамического потока на вход.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>Получение URL-адреса предварительного просмотра

Используйте конечную точку предварительного просмотра, чтобы проверить получение входных данных от кодировщика.

> [!IMPORTANT]
> Прежде, чем продолжить, убедитесь, что видео правильно передается на URL-адрес предварительного просмотра.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-liveevents-and-liveoutputs"></a>Создание событий и выходов прямой трансляции и управление ими

Настроив передачу потока данных через событие прямой трансляции, вы можете запустить событие потоковой передачи, создав ресурс, выход прямой трансляции и указатель потоковой передачи. Так вы сможете запустить архивирование потока и сделать его доступным его зрителям через конечную точку потоковой передачи. 

#### <a name="create-an-asset"></a>Создание ресурса

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

Создайте ресурс, который будет использоваться в выходе прямой трансляции.

#### <a name="create-a-liveoutput"></a>Создание выхода прямой трансляции

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaminglocator"></a>Создание указателя потоковой передачи

> [!NOTE]
> При создании учетной записи служб мультимедиа в нее добавляется конечная точка потоковой передачи **по умолчанию** в состоянии **Остановлена**. Чтобы начать потоковую передачу содержимого и воспользоваться динамической упаковкой и динамическим шифрованием, конечная точка потоковой передачи, из которой необходимо выполнять потоковую передачу содержимого, должна находиться в состоянии **Выполняется**. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Очистка ресурсов в учетной записи Служб мультимедиа

После завершения потоковой передачи мероприятия вы можете удалить выделенные ранее ресурсы с помощью описанной ниже процедуры.

* Остановите трансляцию потока из кодировщика.
* Остановите событие прямой трансляции. Плата за остановленное событие прямой трансляции не взимается. Если вам понадобится снова запустить его, вы можете воспользоваться тем же URL-адресом приема (перенастраивать кодировщик не потребуется).
* Вы можете остановить конечную точку потоковой передачи, если не хотите предоставлять доступ к архиву события прямой трансляции по требованию. Плата за остановленное событие прямой трансляции не взимается.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupAccount)]   

## <a name="watch-the-event"></a>Просмотр события

Чтобы просмотреть событие, скопируйте URL-адрес потоковой передачи, полученный при выполнении кода в разделе о [создании указателя потоковой передачи](#create-a-streaminglocator) и вставьте его в любой проигрыватель. Для проверки потока на сайте http://ampdemo.azureedge.net можно использовать [Проигрыватель мультимедиа Azure](http://amp.azure.net/libs/amp/latest/docs/index.html). 

После остановки интерактивное событие автоматически преобразуется в содержимое по требованию. Даже после остановки и удаления события пользователи смогут запрашивать потоковую передачу архивированного видеосодержимого, пока не удален соответствующий ресурс. Ресурс невозможно удалить, пока он используется каким-либо событием: сначала нужно удалить это событие. 

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вам больше не нужны какие-либо ресурсы в группе ресурсов, включая Службы мультимедиа и учетные записи хранения, созданные для этого руководства, удалите группу ресурсов, созданную ранее. Для этого можно использовать средство **CloudShell**.

В **CloudShell** выполните следующую команду:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> За запущенное событие прямой трансляции взимается плата. Учтите также, что при сбое или остановке проекта и (или) программы по любой причине, событие прямой трансляции может остаться в работающем состоянии, за что также будет взиматься плата.

## <a name="next-steps"></a>Дополнительная информация

[Потоковая передача файлов](stream-files-tutorial-with-api.md)

