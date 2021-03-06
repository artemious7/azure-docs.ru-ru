---
title: Подключение хранилища файлов Azure из виртуальной машины Windows в Azure | Документация Майкрософт
description: Сведения о хранении файлов в облаке с помощью хранилища файлов Azure и подключение общих облачных файловых ресурсов из виртуальной машины Azure.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.openlocfilehash: 8d537bdc882487784baef9f693e4677c76d3bd8d
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2018
ms.locfileid: "27577557"
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Использование общих файловых ресурсов Azure с виртуальными машинами Windows 

Вы можете использовать общие файловые ресурсы Azure для хранения файлов с вашей виртуальной машины и для доступа к ним. Например, можно сохранить скрипт или файл конфигурации приложения, который требуется использовать на всех виртуальных машинах. В этой статье рассматривается создание и подключение общего ресурса Azure, а также отправка и скачивание файлов.

## <a name="connect-to-a-file-share-from-a-vm"></a>Подключение к общему файловому ресурсу с виртуальной машины

В этом разделе предполагается, что у вас уже есть общий файловый ресурс, к которому нужно подключиться. Если вам нужно создать его, см. сведения в разделе этой статьи о [создании файлового ресурса](#create-a-file-share).

1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню слева выберите **Учетные записи хранения**.
3. Выберите учетную запись хранения.
4. На странице **Обзор** в разделе **Служба** выберите **Файлы**.
5. Выберите файловый ресурс или щелкните **+ Файловый ресурс**, чтобы создать файловый ресурс для использования.
6. Щелкните **Подключить**, чтобы открыть страницу с синтаксисом командной строки для подключения общего файлового ресурса из Windows или Linux.
7. В разделе **Буква диска** выберите букву для идентификации диска.
8. Выберите синтаксис для использования и нажмите кнопку "Копировать" справа, чтобы скопировать его в буфер обмена. Вставьте данные в расположение, где к ним можно легко получить доступ. 
8. Подключитесь к виртуальной машине и откройте окно командной строки.
9. Вставьте отредактированный синтаксис соединения и нажмите кнопку **ВВОД**.
10. После установки соединения появится сообщение **Команда выполнена успешно**.
11. Проверьте подключение. Для этого введите букву диска, чтобы перейти к нему, а затем введите **dir**, чтобы просмотреть содержимое общего файлового ресурса.



## <a name="create-a-file-share"></a>Создайте общую папку 
1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню слева выберите **Учетные записи хранения**.
3. Выберите учетную запись хранения.
4. На странице **Обзор** в разделе **Служба** выберите **Файлы**.
5. На странице службы файлов щелкните **+ Файловый ресурс**.
6. Введите имя общего файлового ресурса. В его имени можно использовать строчные буквы, цифры и отдельные дефисы. Имя не может начинаться с дефиса, а также нельзя использовать несколько дефисов подряд. 
7. Укажите ограничение на размер файла (до 5120 ГБ).
8. Нажмите кнопку **ОК**, чтобы создать файловый ресурс.
   
## <a name="upload-files"></a>Отправка файлов
1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню слева выберите **Учетные записи хранения**.
3. Выберите учетную запись хранения.
4. На странице **Обзор** в разделе **Служба** выберите **Файлы**.
5. Выберите общий файловый ресурс.
6. Щелкните **Отправить**, чтобы открыть страницу **Отправить файлы**.
7. Щелкните значок папки, чтобы найти файл для передачи в локальной файловой системе.   
8. Щелкните **Отправить** для отправки файла в общий файловый ресурс.

## <a name="download-files"></a>Скачивание файлов
1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню слева выберите **Учетные записи хранения**.
3. Выберите учетную запись хранения.
4. На странице **Обзор** в разделе **Служба** выберите **Файлы**.
5. Выберите общий файловый ресурс.
6. Щелкните файл правой кнопкой мыши и выберите **Загрузить**, чтобы скачать его на локальный компьютер.
   

## <a name="next-steps"></a>Дополнительная информация

Вы также можете создавать файловые ресурсы и управлять ими с помощью PowerShell. Дополнительные сведения см. в статье [Develop for Azure File storage with .NET](../../storage/files/storage-dotnet-how-to-use-files.md) (Разработка для хранилища файлов Azure с помощью .NET).
