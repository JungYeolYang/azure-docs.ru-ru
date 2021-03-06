---
title: Порты, используемые службами Hadoop в HDInsight в Azure
description: Список портов, которые используются службами Hadoop, работающими в кластере HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 2d0b8aba95787f179733dd596e783f097cba4299
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64692126"
---
# <a name="ports-used-by-apache-hadoop-services-on-hdinsight"></a>Порты, используемые службами Apache Hadoop в HDInsight

В этом документе представлен список портов, которые используются службами Apache Hadoop, работающими в кластерах HDInsight под управлением Linux. Кроме того, в статье содержатся сведения о портах, которые используются для подключения к кластеру с помощью протокола SSH.

## <a name="public-ports-vs-non-public-ports"></a>Общедоступные и необщедоступные порты

Кластеры HDInsight под управлением Linux предоставляют только три общедоступных порта для трафика Интернета: 22, 23 и 443. Они используются для безопасного доступа к кластеру с помощью протокола SSH и службам, предоставляемым через защищенный протокол HTTPS.

По сути, HDInsight реализуется несколькими виртуальными машинами Azure (узлами кластера), которые работают в виртуальной сети Azure. Из виртуальной сети вы можете получить доступ к портам, недоступным из Интернета. Например, подключившись к одному из головных узлов по протоколу SSH, вы можете получить прямой доступ к службам, работающим на узлах кластера.

> [!IMPORTANT]  
> Если не указать виртуальную сеть Azure с помощью параметра конфигурации для HDInsight, она будет создана автоматически. Тем не менее к этой виртуальной сети невозможно присоединить другие компьютеры (например, другие виртуальные машины Azure или клиентский компьютер разработки).


Чтобы присоединить дополнительные компьютеры к виртуальной сети, необходимо сначала создать виртуальную сеть, а затем указать ее при создании кластера HDInsight. Дополнительные сведения см. в статье [Расширение возможностей HDInsight с помощью виртуальной сети Azure](hdinsight-extend-hadoop-virtual-network.md).

## <a name="public-ports"></a>Общедоступные порты

Все узлы в кластере HDInsight расположены в виртуальной сети Azure. Получить доступ к ним напрямую из Интернета невозможно. Общедоступный шлюз обеспечивает интернет-доступ к приведенным ниже портам. Они общие для всех типов кластеров HDInsight.

| Service | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- |
| sshd |22 |SSH |Подключает клиенты к sshd на основном головном узле. Дополнительные сведения см. в статье [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |22 |SSH |Подключает клиенты к SSHD на граничном узле. Дополнительные сведения см. в статье [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |23 |SSH |Подключает клиенты к sshd на дополнительном головном узле. Дополнительные сведения см. в статье [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Ambari |443 |HTTPS |Веб-интерфейс Ambari. Дополнительные сведения см. в статье [Управление кластерами HDInsight с помощью веб-интерфейса Ambari](hdinsight-hadoop-manage-ambari.md). |
| Ambari |443 |HTTPS |REST API Ambari. Дополнительные сведения см. в статье [Управление кластерами HDInsight с помощью REST API Ambari](hdinsight-hadoop-manage-ambari-rest-api.md). |
| WebHCat |443 |HTTPS |REST API HCatalog. Дополнительные сведения см. в статьях [Выполнение заданий Pig с помощью REST с использованием Apache Hadoop в HDInsight](hadoop/apache-hadoop-use-pig-curl.md), [Выполнение заданий Pig с помощью REST с использованием Apache Hadoop в HDInsight](hadoop/apache-hadoop-use-pig-curl.md) и [Запуск заданий MapReduce в среде Apache Hadoop, размещенной в HDInsight, с помощью REST](hadoop/apache-hadoop-use-mapreduce-curl.md). |
| HiveServer2 |443 |ODBC |Подключение к Hive с помощью ODBC. См. статью [Подключение Excel к Hadoop с помощью драйвера Microsoft Hive ODBC](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md). |
| HiveServer2 |443 |JDBC |Подключение к ApacheHive с помощью JDBC. Дополнительные сведения см. в статье [Отправка запросов в Apache Hive с помощью драйвера JDBC в HDInsight](hadoop/apache-hadoop-connect-hive-jdbc-driver.md). |

Приведенные ниже сведения доступны для определенных типов кластеров.

| Service | Порт | Protocol | Тип кластера | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |hbase |REST API HBase. Дополнительные сведения см. в статье [Начало работы с примером Apache HBase в HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md). |
| Livy |443 |HTTPS |Spark |Spark REST API. Дополнительные сведения см. в статье [Удаленная отправка заданий Spark в кластер Azure HDInsight с помощью Apache Spark REST API](spark/apache-spark-livy-rest-interface.md) |
| Сервер Thrift Spark |443 |HTTPS |Spark |Сервер Thrift Spark, который используется для отправки запросов Hive. Дополнительные сведения см. в статье [Использование клиента Apache Beeline с Apache Hive](hadoop/apache-hadoop-use-hive-beeline.md). |
| Storm |443 |HTTPS |Storm |Веб-интерфейс Storm. Дополнительные сведения см. в статье [Развертывание и администрирование топологий Apache Storm в Azure HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Authentication

Все общедоступные службы в Интернете должны проходить проверку подлинности.

| Порт | Учетные данные |
| --- | --- |
| 22 или 23 |Учетные данные пользователя SSH, указанные при создании кластера. |
| 443 |Имя для входа (по умолчанию — admin) и пароль, указанные при создании кластера. |

## <a name="non-public-ports"></a>Необщедоступные порты

> [!NOTE]  
> Некоторые службы доступны только в кластерах определенных типов. Например, служба HBase доступна только на кластерах типа HBase.

> [!IMPORTANT]  
> Некоторые службы могут работать только на одном головном узле одновременно. Если вы пытаетесь подключиться к службе на основном головном узле и получаете сообщение об ошибке, повторите попытку, используя вторичный головной узел.

### <a name="ambari"></a>Ambari

| Service | Nodes | Порт | URL-адрес | Protocol | 
| --- | --- | --- | --- | --- |
| Веб-интерфейс Ambari | Головные узлы | 8080 | / | HTTP |
| Ambari REST API | Головные узлы | 8080 | /api/v1 | HTTP |

Примеры:

* Ambari REST API: `curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>Порты HDFS

| Service | Nodes | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| Веб-интерфейс узла имен |Головные узлы |30070 |HTTPS |Пользовательский веб-интерфейс для просмотра состояния. |
| Служба метаданных на узле имен |Головные узлы |8020 |IPC |Метаданные файловой системы |
| Узел данных |Все рабочие узлы |30075 |HTTPS |Веб-интерфейс для просмотра состояния, журналов и т. д. |
| Узел данных |Все рабочие узлы |30010 |&nbsp; |Передача данных |
| Узел данных |Все рабочие узлы |30020 |IPC |Операции с метаданными |
| Дополнительный узел имен |Головные узлы |50090 |HTTP |Контрольная точка для метаданных узла имен |

### <a name="yarn-ports"></a>Порты YARN

| Service | Nodes | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| Веб-интерфейс для диспетчера Resource Manager |Головные узлы |8088 |HTTP |Веб-интерфейс для диспетчера Resource Manager |
| Веб-интерфейс для диспетчера Resource Manager |Головные узлы |8090 |HTTPS |Веб-интерфейс для диспетчера Resource Manager |
| Интерфейс администратора для Resource Manager |Головные узлы |8141 |IPC |Для отправки приложений (Hive, Hive Server, Pig и т. д.) |
| Планировщик Resource Manager |Головные узлы |8030 |HTTP |Интерфейс администратора |
| Интерфейс приложения Resource Manager |Головные узлы |8050 |HTTP |Адрес интерфейса диспетчера приложений |
| Диспетчер узлов |Все рабочие узлы |30050 |&nbsp; |Адрес диспетчера контейнеров |
| Веб-интерфейс диспетчера узлов |Все рабочие узлы |30060 |HTTP |Интерфейс Resource Manager |
| Адрес временной шкалы |Головные узлы |10200 |RPC |Служба RPC службы временной шкалы |
| Веб-интерфейс временной шкалы |Головные узлы |8181 |HTTP |Веб-интерфейс службы временной шкалы |

### <a name="hive-ports"></a>Порты Hive

| Service | Nodes | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| HiveServer2 |Головные узлы |10001 |Thrift |Служба для подключения к Hive (с помощью протокола Thrift или JDBC) |
| Метахранилище Hive |Головные узлы |9083 |Thrift |Служба для подключения к метаданным Hive (с помощью протокола Thrift или JDBC) |

### <a name="webhcat-ports"></a>Порты WebHCat

| Service | Nodes | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| Сервер WebHCat |Головные узлы |30111 |HTTP |Веб-API на базе HCatalog и других служб Hadoop |

### <a name="mapreduce-ports"></a>Порты MapReduce

| Service | Nodes | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| Журнал заданий |Головные узлы |19888 |HTTP |Веб-интерфейс журнала заданий MapReduce |
| Журнал заданий |Головные узлы |10020 |&nbsp; |Сервер журнала заданий MapReduce |
| Обработчик перемещений |&nbsp; |13562 |&nbsp; |Передача промежуточных выходных данных сопоставления в адрес запрашивающих редукторов |

### <a name="oozie"></a>Oozie,

| Service | Nodes | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| Сервер Oozie |Головные узлы |11000 |HTTP |URL-адрес службы Oozie |
| Сервер Oozie |Головные узлы |11001 |HTTP |Порт для администрирования Oozie |

### <a name="ambari-metrics"></a>Метрики Ambari

| Service | Nodes | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| Временная шкала (журнал приложения) |Головные узлы |6188 |HTTP |Веб-интерфейс службы временной шкалы |
| Временная шкала (журнал приложения) |Головные узлы |30200 |RPC |Веб-интерфейс службы временной шкалы |

### <a name="hbase-ports"></a>Порты HBase

| Service | Nodes | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| HMaster |Головные узлы |16000 |&nbsp; |&nbsp; |
| Веб-интерфейс информационного сервера HMaster |Головные узлы |16010 |HTTP |Порт для веб-интерфейса на главном узле HBase |
| Региональный сервер |Все рабочие узлы |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |Порт, используемый клиентами для подключения к ZooKeeper |

### <a name="kafka-ports"></a>Порты Kafka

| Service | Nodes | Порт | Protocol | ОПИСАНИЕ |
| --- | --- | --- | --- | --- |
| Broker |Рабочие узлы |9092 |[Сетевой протокол Kafka](https://kafka.apache.org/protocol.html) |Используется для связи с клиентами |
| &nbsp; |Узлы Zookeeper |2181 |&nbsp; |Порт, используемый клиентами для подключения к ZooKeeper |

### <a name="spark-ports"></a>Порты Spark

| Service | Nodes | Порт | Protocol | URL-адрес | ОПИСАНИЕ |
| --- | --- | --- | --- | --- | --- |
| Серверы Thrift Spark |Головные узлы |10002 |Thrift | &nbsp; | Служба для подключения к Spark SQL (с помощью протокола Thrift или JDBC) |
| Сервер Livy | Головные узлы | 8998 | HTTP | &nbsp; | Служба для запуска инструкций, заданий и приложений |
| Записная книжка Jupyter | Головные узлы | 8001 | HTTP | &nbsp; | Веб-сайт записных книжек Jupyter |

Примеры:

* Livy: `curl -u admin -G "http://10.0.0.11:8998/"`. В этом примере `10.0.0.11` — IP-адрес головного узла, на котором размещена служба Livy.
