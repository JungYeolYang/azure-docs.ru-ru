---
title: Руководство по созданию кластера Azure Red Hat OpenShift | Документация Майкрософт
description: Узнайте, как создать кластер Microsoft Azure Red Hat OpenShift с помощью Azure CLI.
services: container-service
author: TylerMSFT
ms.author: twhitney
manager: jeconnoc
ms.topic: tutorial
ms.service: openshift
ms.date: 05/08/2019
ms.openlocfilehash: baada8a5238725456ca4a2ec7e8257c229066115
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2019
ms.locfileid: "65466179"
---
# <a name="tutorial-create-an-azure-red-hat-openshift-cluster"></a>Руководство по Создание кластера Azure Red Hat OpenShift

Это руководство представляет первую часть цикла. Из него вы узнаете, как создать кластер Microsoft Azure Red Hat OpenShift с помощью Azure CLI, выполнить его масштабирование, а затем удалить, чтобы очистить ресурсы.

В первой части цикла вы узнаете, как выполнять такие задачи:

> [!div class="checklist"]
> * Создание кластера Azure Red Hat OpenShift

Из этого цикла руководств вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> * Создание кластера Azure Red Hat OpenShift
> * [Масштабирование кластера Azure Red Hat OpenShift](tutorial-scale-cluster.md)
> * [Удаление кластера Azure Red Hat OpenShift](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Предварительные требования

Перед началом работы с этим руководством выполните следующие действия:

Убедитесь в том, что вы [настроили среду разработки](howto-setup-environment.md), в том числе:
- установили последнюю версию интерфейса командной строки (версию 2.0.64 или более позднюю версию);
- создали клиент;
- создали объект приложения Azure;
- создали пользователя Active Directory для входа в приложения, работающие в кластере.

## <a name="step-1-sign-in-to-azure"></a>Шаг 1. Вход в Azure

Если интерфейс Azure CLI запущен локально, откройте оболочку Bash и выполните команду `az login`, чтобы войти в Azure.

```bash
az login
```

 Если у вас есть доступ к нескольким подпискам, выполните команду `az account set -s {subscription ID}`, заменив `{subscription ID}` необходимой подпиской.

## <a name="step-2-create-an-azure-red-hat-openshift-cluster"></a>Шаг 2. Создание кластера Azure Red Hat OpenShift

В окне командной строки Bash задайте следующие переменные:

> [!IMPORTANT]
> Имя кластера нужно указать прописными буквами, иначе создание кластера завершится сбоем.

```bash
CLUSTER_NAME=<cluster name in lowercase>
```

 Используйте то же имя для кластера, которое вы выбрали на шаге 6 процедуры [создания регистрации приложения](howto-aad-app-configuration.md#create-a-new-app-registration).

```bash
LOCATION=<location>
```

Выберите расположение для создания кластера. Список регионов Azure, в которых поддерживается OpenShift в Azure, см. в списке [поддерживаемых регионов](supported-resources.md#azure-regions). Например, `LOCATION=eastus`.

Параметру `FQDN` присвойте полное доменное имя кластера. Это имя составляется из имени кластера, расположения и `.cloudapp.azure.com` в конце. Оно будет совпадать с URL-адресом входа, который вы создали на шаге 6 процедуры [создания регистрации приложения](howto-aad-app-configuration.md#create-a-new-app-registration). Например:   

```bash
FQDN=$CLUSTER_NAME.$LOCATION.cloudapp.azure.com
```

Параметру `APPID` присвойте значение, которое вы сохранили на шаге 9 процедуры [создания регистрации приложения](howto-aad-app-configuration.md#create-a-new-app-registration).  

```bash
APPID=<app ID value>
```

Параметру `SECRET` присвойте значение, которое вы сохранили на шаге 6 процедуры [создания секрета клиента](howto-aad-app-configuration.md#create-a-client-secret).  

```bash
SECRET=<secret value>
```

Параметру `TENANT` присвойте значение идентификатора клиента, которое вы сохранили на шаге 7 процедуры [создание клиента](howto-create-tenant.md#create-a-new-azure-ad-tenant).  

```bash
TENANT=<tenant ID>
```

Создайте группу ресурсов для кластера. Выполните следующую команду из оболочки Bash, в которой вы ранее определили переменные:

```bash
az group create --name $CLUSTER_NAME --location $LOCATION
```

### <a name="optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network"></a>Необязательно: подключение виртуальной сети кластера к существующей виртуальной сети

Если вам не нужно подключать виртуальную сеть создаваемого кластера к существующей виртуальной сети с помощью пиринга, пропустите этот шаг.

Для начала получите идентификатор существующей виртуальной сети. Этот идентификатор имеет такой формат: `/subscriptions/{subscription id}/resourceGroups/{resource group of VNET}/providers/Microsoft.Network/virtualNetworks/{VNET name}`.

Если вы не знаете имя сети или группу ресурсов, к которой относится существующая виртуальная сеть, перейдите на [колонку виртуальной сети](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Network%2FvirtualNetworks) и щелкните нужную виртуальную сеть. Откроется страница виртуальной сети, на которой указаны имя сети и группа ресурсов, к которой она принадлежит.

Определите переменную VNET_ID с помощью следующей команды CLI в оболочке BASH:

```bash
VNET_ID=$(az network vnet show -n {VNET name} -g {VNET resource group} --query id -o tsv)
```

Например: `VNET_ID=$(az network vnet show -n MyVirtualNetwork -g MyResourceGroup --query id -o tsv`

### <a name="create-the-cluster"></a>Создание кластера

Теперь можно создать новый кластер.

 Если вы не будете подключать виртуальную сеть кластера к существующей виртуальной сети, исключите последний параметр `--vnet-peer-id $VNET_ID` из следующего примера.

```bash
az openshift create --resource-group $CLUSTER_NAME --name $CLUSTER_NAME -l $LOCATION --fqdn $FQDN --aad-client-app-id $APPID --aad-client-app-secret $SECRET --aad-tenant-id $TENANT --vnet-peer-id $VNET_ID
```

Через несколько минут команда `az openshift create` завершит выполнение и возвратит документ JSON с информацией о кластере.

> [!NOTE]
> Если вы получите ошибку с сообщением о том, что имя узла недоступно, это может означать, что указанное имя кластера уже существует. Попробуйте удалить исходную регистрацию приложения и повторить все действия из процедуры создания регистрации приложения (howto-aad-app-configuration.md#create-a-new-app-registration) (пропустите последний шаг создания нового пользователя, так как пользователь уже создан), указав другое имя кластера.

## <a name="step-3-sign-in-to-the-openshift-console"></a>Шаг 3. Войдите в консоль OpenShift

Теперь вы готовы войти в консоль OpenShift для нового кластера. [Веб-консоль OpenShift](https://docs.openshift.com/aro/architecture/infrastructure_components/web_console.html) позволяет визуализировать, просматривать содержимое проектов OpenShift и управлять им.

Войдите с учетными данными [нового пользователя AAD](howto-aad-app-configuration.md#create-a-new-active-directory-user), которого вы создали для тестирования. Для этого вам потребуется новый экземпляр браузера, в кэше которого нет удостоверения, с которым вы обычно входите на портал Azure.

1. Откройте окно *инкогнито* (Chrome) или *InPrivate* (Microsoft Edge).
2. Откройте URL-адрес входа, который вы создали на шаге 6 процедуры [создания регистрации приложения](howto-aad-app-configuration.md#create-a-new-app-registration). Например https://constoso.eastus.cloudapp.azure.com.

> [!NOTE]
> Консоль OpenShift использует самозаверяющий сертификат.
> Когда в браузере появится предупреждение о "недоверенном" сертификате, подтвердите этот сертификат и закройте предупреждение.

Войдите с именем пользователя и паролем, которые вы создали в процедуре [создания нового пользователя Active Directory](howto-aad-app-configuration.md#create-a-new-active-directory-user). Когда откроется диалоговое окно **Запрошенные разрешения**, выберите **Согласие от имени вашей организации** и щелкните **Принять**.

Итак, вы вошли в консоль кластера.

![Снимок экрана с консолью кластера OpenShift](./media/aro-console.png)

 Узнайте больше [об использовании консоли OpenShift](https://docs.openshift.com/aro/getting_started/developers_console.html) для создания и сборки образов, ознакомившись с [документацией по Red Hat OpenShift](https://docs.openshift.com/aro/welcome/index.html).

## <a name="step-4-install-the-openshift-cli"></a>Шаг 4. Установка интерфейса командной строки OpenShift

[Интерфейс командной строки OpenShift](https://docs.openshift.com/aro/cli_reference/get_started_cli.html) (или *OC Tools*) предоставляет команды для управления приложениями и служебные программы низкого уровня для взаимодействия с разными компонентами кластера OpenShift.

В консоли OpenShift щелкните вопросительный знак в правом верхнем углу, рядом с именем текущего пользователя, и выберите **Command Line Tools** (Средства командной строки).  Перейдите по ссылке **Latest Release** (Последний выпуск), чтобы скачать и установить поддерживаемые версии интерфейса командной строки OC для Linux, MacOS или Windows.

> [!NOTE]
> Если вы не видите в правом верхнем углу значка с вопросительным знаком, выберите *Service Catalog* (Каталог служб) или *Application Console* (Консоль приложения) в раскрывающемся списке слева вверху.
>
> Кроме того, вы можете использовать прямую ссылку [на скачивание CLI OC](https://www.okd.io/download.html).

Страница **Command Line Tools** (Средства командной строки) содержит команду формата `oc login https://<your cluster name>.<azure region>.cloudapp.azure.com --token=<token value>`.  Нажмите кнопку *Копировать в буфер обмена*, чтобы скопировать эту команду.  В окне терминала [задайте переменную path](https://docs.okd.io/latest/cli_reference/get_started_cli.html#installing-the-cli), чтобы она включала локальный каталог установки средств OC. Затем войдите в кластер с помощью команды CLI OC, которую скопировали ранее.

Если описанные выше шаги не позволили получить значение токена, получите это значение с помощью `https://<your cluster name>.<azure region>.cloudapp.azure.com/oauth/token/request`.

## <a name="next-steps"></a>Дополнительная информация

В этой части руководства вы узнали, как выполнить следующие действия:

> [!div class="checklist"]
> * Создание кластера Azure Red Hat OpenShift

Перейдите к следующему руководству:
> [!div class="nextstepaction"]
> [Масштабирование кластера Azure Red Hat OpenShift](tutorial-scale-cluster.md)