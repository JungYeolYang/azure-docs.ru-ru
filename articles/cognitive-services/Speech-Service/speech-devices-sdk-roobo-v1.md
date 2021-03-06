---
title: Аудио разработки пакета SDK для устройств Roobo Smart речи Kit v1 - службы распознавания речи
titleSuffix: Azure Cognitive Services
description: Предварительные требования и инструкции по началу работы с Speech SDK устройств версии 1 пакета средств разработки Roobo смарт-аудио.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 7c1a13a44d9db8ed029ce798f0bb34944a1a65a7
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409044"
---
# <a name="device-roobo-smart-audio-dev-kit"></a>Устройство: Пакет средств разработки аудио Roobo Smart

В этой статье предоставляет сведений об устройстве для Roobo пакета средств разработки смарт-аудио.

## <a name="set-up-the-development-kit"></a>Настройка набора средств разработки

1. Комплект средств разработки содержит два разъема micro USB. Левый разъем предназначен для питания комплекта средств разработки и обозначен надписью "Power" (Питание) на рисунке ниже. Правый разъем предназначен для управления и обозначен на рисунке надписью "Debug" (Отладка).

    ![Подключение комплекта разработки](media/speech-devices-sdk/qsg-1.png)

1. С помощью кабеля micro USB подключите разъем питания комплекта средств разработки к компьютеру или адаптеру питания, чтобы обеспечить питание комплекта средств разработки. Под верхней панелью должен засветиться зеленый индикатор питания.

1. Для управления в пакет средств разработки, подключите порт отладки на компьютере с помощью второй micro USB-кабеля. Важно использовать кабель высокого качества для обеспечения надежной связи.

1. Сориентируйте комплект разработки для получения кольцевой или линейной конфигурации.

    |Конфигурация набора средств разработки|Ориентация|
    |-----------------------------|------------|
    |Кольцевая|Вертикальное положение, микрофоны направлены к потолку|
    |Линейный|На боку, микрофоны направлены к вам (показано на изображении ниже)|

    ![Расположение комплекта разработки с линейной решеткой](media/speech-devices-sdk/qsg-2.png)

1. Установите сертификаты и устанавливать разрешения для звукового устройства. Введите следующие команды в окне командной строки:

   ```powershell
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/
   adb shell
   cd /data/
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Эти команды используют средство Android Debug Bridge (`adb.exe`), которое является частью установки Android Studio. Это средство находится в каталоге C:\Users\[имя пользователя]\AppData\Local\Android\Sdk\platform-tools. Вы можете добавить этот каталог в путь, чтобы было удобнее вызывать `adb`. В противном случае нужно указывать полный путь к установке adb.exe в каждой команде, вызывающей `adb`.
    >
    > Если появится сообщение об ошибке `no devices/emulators found` проверьте на USB-кабель подключен и кабель высокого качества. Можно использовать команду `adb devices`, чтобы проверить, может ли компьютер взаимодействовать с комплектом средств разработки, так как она вернет список устройств.
    >
    > [!TIP]
    > Отключите звук микрофона и динамик вашего компьютера, чтобы обеспечить работу с микрофонами комплекта разработки. В этом случае вы случайно не запустите на устройстве аудио с компьютера.

1. Если требуется подключить динамик к комплекту разработки, это можно сделать с помощью звукового линейного выхода. Следует выбрать динамика хорошего качества с 3,5 мм аналоговый разъем.

    ![Звук в Vysor](media/speech-devices-sdk/qsg-14.png)

## <a name="development-information"></a>Сведения о разработке

Дополнительные сведения о разработке см. в разделе [руководство по разработке Roobo](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

## <a name="audio"></a>Аудио

Roobo предоставляет средство, позволяющее перехватывать все аудио для устройства флэш-памяти. Это может помочь в устранении проблем с аудио. Для каждой конфигурации набора разработки предусмотрена отдельная версия инструмента. На [сайта Roobo](https://ddk.roobo.com/), выберите устройство, а затем выберите **Roobo средства** ссылку в нижней части страницы.

## <a name="next-steps"></a>Дальнейшие действия

* [Запуск Android примера приложения](speech-devices-sdk-android-quickstart.md)
