---
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: 48deeec7a2c8767ab5dbb81b622e6d40483ed455
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60202827"
---
# <a name="make-a-remote-connection-to-a-kubernetes-dcos-or-docker-swarm-cluster"></a>Создание удаленного подключения к кластеру Kubernetes, DC/OS или Docker Swarm
После создания кластера службы контейнеров Azure необходимо подключиться к кластеру, чтобы развертывать рабочие нагрузки и управлять ими. В этой статье описывается подключение к главной виртуальной машине кластера с удаленного компьютера. 

Кластеры Kubernetes, DC/OS и Docker Swarm предоставляют конечные точки HTTP локально. При использовании Kubernetes к этой конечной точке предоставляется безопасный доступ через Интернет, и к ней можно обратиться, выполнив команду `kubectl` в программе командной строки на любом компьютере, подключенном к Интернету. 

Для DC/OS и Docker Swarm мы советуем создать туннель Secure Shell (SSH) от локального компьютера к системе управления кластером. После установления туннеля можно выполнять команды, которые используют конечные точки HTTP, и просматривать веб-интерфейс оркестратора (при наличии) из локальной системы. 

## <a name="prerequisites"></a>Технические условия

* Кластер Kubernetes, DC/OS или Docker Swarm, [развернутый в службе контейнеров Azure](../articles/container-service/dcos-swarm/container-service-deployment.md).
* Файл закрытого ключа RSA (SSH), соответствующий открытому ключу, добавленному в кластер во время развертывания. В этих командах предполагается, что закрытый ключ SSH хранится на вашем компьютере в папке `$HOME/.ssh/id_rsa`. Дополнительные сведения см. в статьях [Как создать и использовать пару из открытого и закрытого ключей SSH для виртуальных машин Linux в Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md) и [Использование ключей SSH с Windows в Azure](../articles/virtual-machines/linux/ssh-from-windows.md). Если SSH-подключение не работает, может потребоваться [сбросить ключи SSH](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

## <a name="connect-to-a-kubernetes-cluster"></a>Подключение к кластеру Kubernetes

Выполните следующие действия, чтобы установить и настроить `kubectl` на компьютере.

> [!NOTE] 
> В Linux или macOS может потребоваться выполнить команды из этого раздела с помощью `sudo`.
> 

### <a name="install-kubectl"></a>Установка kubectl
Один из способов установить это средство является использование `az acs kubernetes install-cli` команду Azure CLI. Чтобы выполнить эту команду, убедитесь, что вы [установлен](/cli/azure/install-az-cli2) последнюю версию Azure CLI и вы вошли в учетную запись Azure (`az login`).

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

Последнюю версию клиента `kubectl` можно также скачать со страницы [списка выпусков Kubernetes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md). Дополнительные сведения см. в разделе [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/) (Установка и настройка kubectl).

### <a name="download-cluster-credentials"></a>Скачивание учетных данных кластера
Когда средство `kubectl` будет установлено, скопируйте на локальный компьютер учетные данные кластера. Чтобы получить учетные данные, выполните команду `az acs kubernetes get-credentials`. Передайте имя группы ресурсов и имя ресурса службы контейнеров:

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

Эта команда загрузит учетные данные кластера в папку `$HOME/.kube/config`, где средство `kubectl` будет их искать.

Вы также можете с помощью `scp` безопасно скопировать файл на локальный компьютер из папки `$HOME/.kube/config` на главной виртуальной машине. Например: 

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

При работе в Windows можно использовать Bash на Ubuntu в Windows, клиент безопасного копирования файлов PuTTy или аналогичное средство.

### <a name="use-kubectl"></a>Использование kubectl

После того как вы настроите `kubectl`, проверьте подключение, просмотрев список узлов в кластере:

```bash
kubectl get nodes
```

Можно использовать другие команды `kubectl`. Например, вы можете просмотреть панель мониторинга Kubernetes. Сначала запустите прокси для сервера Kubernetes API:

```bash
kubectl proxy
```

Теперь пользовательский интерфейс Kubernetes доступен по адресу `http://localhost:8001/ui`.

Дополнительные сведения см. в статье о [быстром запуске Kubernetes](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connect-to-a-dcos-or-swarm-cluster"></a>Подключение к кластеру DC/OS или Swarm

Чтобы использовать кластеры DC/OS и Docker Swarm, развернутые в службе контейнеров Azure, создайте туннель SSH из локальной системы Linux, macOS или Windows. 

> [!NOTE]
> В статье рассматриваются инструкции по туннелированию TCP-трафика по протоколу SSH. Кроме того, можно запустить интерактивный сеанс SSH с одной из внутренних систем управления кластером, но мы не рекомендуем это делать. Работа непосредственно во внутренней системе, вы рискуете случайно изменить конфигурацию.  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a>Создание туннеля SSH в Linux или macOS
Во-первых, чтобы создать туннель SSH в любой из этих операционных систем, вам нужно узнать общедоступное DNS-имя главного сервера с балансировкой нагрузки. Выполните следующие действия.


1. На [портале Azure](https://portal.azure.com) перейдите в группу ресурсов, содержащую кластер службы контейнеров. Разверните группу ресурсов, чтобы отображались все ресурсы. 

2. Щелкните ресурс **службы контейнеров** и нажмите кнопку **Обзор**. В разделе **Основное** отобразится **полное доменное имя главного кластера**. Сохраните это имя для последующего использования. 

    ![Общедоступное DNS-имя](./media/container-service-connect/pubdns.png)

    Кроме того, в службе контейнеров можно выполнить команду `az acs show` и в выходных данных найти свойство **Master Profile:fqdn**.

3. Теперь откройте оболочку и выполните команду `ssh`, указав следующие значения: 

    **LOCAL_PORT** — TCP-порт на стороне службы, к которому подключается туннель. Для Swarm задайте порт 2375, а для DC/OS — 80. 
    **REMOTE_PORT** — порт конечной точки, к которой требуется предоставить доступ. Для Swarm используется порт 2375. Для DC/OS используется порт 80.  
    **USERNAME** — имя пользователя, указанное при развертывании кластера.  
    **DNSPREFIX** — префикс DNS, указанный при развертывании кластера.  
    **REGION** — регион, в котором расположена ваша группа ресурсов.  
    **PATH_TO_PRIVATE_KEY** (необязательно) — это путь к закрытому ключу, который соответствует открытому ключу, указанному во время создания кластера. Используйте этот параметр с флагом `-i`.

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
   > [!NOTE]
   > Для SSH-подключения используется порт 2200, а не стандартный 22. В кластере с несколькими главными виртуальными машинами этот порт используется для подключения к первой главной виртуальной машине.
   > 

   Команда завершится без выходных данных.

Примеры для кластеров DC/OS и Swarm приведены в следующих разделах.    

### <a name="dcos-tunnel"></a>Туннель DC/OS
Чтобы открыть туннель для конечных точек DC/ОС, выполните следующую команду:

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> Убедитесь, что у вас нет другого локального процесса, привязанного к порту 80. При необходимости в качестве локального порта вместо 80 можно указать, например, 8080, но в этом случае некоторые ссылки в пользовательском веб-интерфейсе могут не работать.
>

Теперь вы можете подключаться к конечным точкам DC/OS из локальной системы, используя следующие URL-адреса (при условии, что в качестве локального порта используется 80):

* DC/OS: `http://localhost:80/`
* Marathon: `http://localhost:80/marathon`
* Mesos: `http://localhost:80/mesos`

Аналогичным образом через этот туннель можно подключаться к интерфейсам REST API каждого приложения.

### <a name="swarm-tunnel"></a>Туннель Swarm
Чтобы открыть туннель для конечной точки Swarm, выполните следующую команду:

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> Убедитесь, что у вас нет другого локального процесса, привязанного к порту 2375. Например, если управляющая программа Docker выполняется локально, по умолчанию используется порт 2375. При необходимости вместо 2375 можно указать локальный порт.
>

Теперь доступ к кластеру Docker Swarm можно получить с помощью интерфейса командной строки Docker (Docker CLI) в локальной системе. Инструкции по установке Docker см. в [этом руководстве](https://docs.docker.com/engine/installation/).

Задайте для переменной среды DOCKER_HOST значение локального порта, настроенное для туннеля. 

```bash
export DOCKER_HOST=:2375
```

Выполните команды Docker, открывающие туннель к кластеру Docker Swarm. Например: 

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a>Создание туннеля SSH в Windows
Создавать туннели SSH в Windows можно несколькими способами. Если вы используете Bash на Ubuntu в Windows или аналогичное средство, для macOS и Linux необходимо выполнить инструкции по туннелированию SSH, приведенные ранее в этой статье. В качестве альтернативы для Windows в этом разделе описывается, как создать туннель с помощью PuTTY.

1. [Скачайте PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) в операционную систему Windows.

2. Запустите приложение.

3. Введите имя узла, которое состоит из имени администратора кластера и общедоступного DNS-имени первого главного сервера в кластере. **Имя узла** будет указано в этом формате: `azureuser@PublicDNSName`. В поле **Порт**введите значение 2200.

    ![Конфигурация PuTTY 1](./media/container-service-connect/putty1.png)

4. Выберите **SSH > Проверка подлинности**. Добавьте путь к файлу закрытого ключа (в формате PPK) для проверки подлинности. Чтобы создать этот файл из SSH-ключа, используемого для создания кластера, можно использовать средство [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

    ![Конфигурация PuTTY 2](./media/container-service-connect/putty2.png)

5. Выберите **SSH > Туннели** и настройте следующие порты для перенаправления:

   * **Исходный порт:** Используйте 80 для DC/OS или 2375 для Swarm.
   * **Назначение:** Используйте localhost:80 для DC/OS или localhost: 2375 для Swarm.

     В следующем примере выполняется настройка для DC/OS. Для Docker Swarm эта процедура аналогична.

     > [!NOTE]
     > При создании этого туннеля нужно, чтобы порт 80 больше нигде не использовался.
     > 

     ![Конфигурация PuTTY 3](./media/container-service-connect/putty3.png)

6. По завершении выберите **Сеанс > Сохранить**, чтобы сохранить конфигурацию подключения.

7. Чтобы подключиться к сеансу PuTTY, нажмите кнопку **Открыть**. После подключения конфигурацию порта можно просмотреть в журнале событий PuTTY.

    ![Журнал событий PuTTY](./media/container-service-connect/putty4.png)

После настройки туннеля для DC/OS связанные конечные точки будут доступны по такому адресу:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

После настройки туннеля для Docker Swarm откройте параметры Windows, чтобы указать значение `:2375` для системной переменной среды `DOCKER_HOST`. Теперь вы можете обращаться к кластеру Swarm из интерфейса командной строки Docker.

## <a name="next-steps"></a>Дальнейшие действия
Развертывание контейнеров в кластере и управление ими.

* [Работа со службой контейнеров Azure и Kubernetes](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [Работа со службой контейнеров Azure и DC/OS](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [Работа со службой контейнеров Azure и Docker Swarm](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

