---
title: Что такое Gen1 хранилища Озера данных Azure? | Документация Майкрософт
description: Общие сведения о поколение 1 хранилища Озера данных (ранее известный как Azure Data Lake Store) и значение, он предоставляет для других хранилищ данных
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: twooley
ms.openlocfilehash: 518c129aedf3161ab761d09139e0c4d988dd2cbc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "60885562"
---
# <a name="what-is-azure-data-lake-storage-gen1"></a>Что такое Gen1 хранилища Озера данных Azure?

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Storage 1-го поколения — это крупномасштабный репозиторий корпоративного уровня для рабочих нагрузок анализа больших данных. Azure Data Lake позволяет сохранять данные с любым размером, типом и скоростью приема в одном месте для эксплуатационной и исследовательской аналитики.

Data Lake Storage 1-го поколения доступно из Hadoop (имеется в кластере HDInsight) с помощью интерфейсов REST API, совместимых с WebHDFS. Он предназначен для анализа сохраненных данных и является адаптированы к различным сценариям анализа данных. Поколение 1 хранилища Озера данных включает все возможности корпоративного уровня: безопасности, управляемости, масштабируемости, надежности и доступности.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

## <a name="key-capabilities"></a>Ключевые возможности

Ниже перечислены некоторые основные возможности Data Lake Storage 1-го поколения.

### <a name="built-for-hadoop"></a>Поддержка Hadoop

Поколение 1 хранилища Озера данных имеет файловую систему Apache Hadoop, совместимый с Hadoop распределенной файловой системы (HDFS), который работает с экосистемой Hadoop. Существующие приложения и службы HDInsight, использующие API-интерфейс WebHDFS, могут легко интегрироваться с Data Lake Storage 1-го поколения. Data Lake Storage 1-го поколения также предоставляет для приложений интерфейс REST, совместимый с WebHDFS.

Можно легко анализировать данные, хранящиеся в поколение 1 хранилища Озера данных с помощью аналитических платформ Hadoop, таких как MapReduce или Hive. Можно подготовить кластеры Azure HDInsight и настроить их для прямого доступа к данных, хранящихся в поколение 1 хранилища Озера данных.

### <a name="unlimited-storage-petabyte-files"></a>Неограниченное пространство хранения, файлы петабайтного размера

Поколение 1 хранилища Озера данных обеспечивает неограниченный объем хранилища и может хранить широкий набор данных для анализа. Он не накладывает никаких ограничений на размер учетной записи, размер файла или объем данных, которые могут храниться в озере данных. Отдельные файлы находится в диапазоне от килобайт до петабайт в размере. Надежность хранения данных обеспечивается созданием нескольких копий. Нет ограничений на продолжительность времени, для которого данные могут храниться в озере данных.

### <a name="performance-tuned-for-big-data-analytics"></a>Настройки производительности для анализа больших данных

Поколение 1 хранилища Озера данных предназначено для работы в крупномасштабных аналитических системах, которые требуется высокая пропускная способность для запроса и анализа больших объемов данных. В озере данных фрагменты файлов распределяются по нескольким отдельным серверам хранилища. Это повышает пропускную способность при параллельном чтении файла для проведения анализа данных.

### <a name="enterprise-ready-highly-available-and-secure"></a>Готовность для предприятий: Высокая доступность и надежность

Data Lake Storage 1-го поколения обладает доступностью и надежностью, соответствующими отраслевым стандартам. Надежность хранения данных обеспечивается созданием избыточных копий для защиты от любых непредвиденных сбоев.

Data Lake Storage 1-го поколения также обеспечивает безопасность корпоративного уровня для сохраненных данных. Дополнительные сведения см. в статье о [защите данных в Data Lake Storage 1-го поколения](#DataLakeStoreSecurity).

### <a name="all-data"></a>Все данные

Поколение 1 хранилища Озера данных можно хранить все данные в его собственном формате без необходимости предварительного преобразования. Data Lake Storage 1-го поколения не требует определять схему перед загрузкой данных. Интерпретацию данных и определение схемы осуществляет конкретная аналитическая платформа во время анализа. Возможность хранения файлов произвольных форматов и размера позволяет Gen1 хранилища Озера данных для обработки структурированных, полуструктурированных и неструктурированных данных.

В Data Lake Storage 1-го поколения хранятся контейнеры для данных — папки и файлы. Работать с хранимыми данными, используя пакеты SDK, портал Azure и Azure Powershell. Если поместить данные в хранилище, используя эти интерфейсы и соответствующие контейнеры, можно хранить любые типы данных. Data Lake Storage 1-го поколения обрабатывает сохраняемые данные без учета их типа.

## <a name="DataLakeStoreSecurity"></a>Защита данных

Списки (ACL) для управления доступом управления Gen1 хранилища Data Lake использует Azure Active Directory (Azure AD) для проверки подлинности и доступа к данным.

| Компонент | Описание |
| --- | --- |
| Authentication |Поколение 1 хранилища Озера данных интегрируется с Azure AD для управления удостоверениями и доступом для всех данных, хранящихся в поколение 1 хранилища Озера данных. Благодаря интеграции преимущества Gen1 хранилища Озера данных из всех Azure AD компонентов, таких как многофакторная проверка подлинности, условный доступ, управление доступом на основе ролей, отслеживание использования приложений, мониторинг безопасности и предупреждения и так далее. Data Lake Storage 1-го поколения поддерживает протокол OAuth 2.0 для аутентификации в интерфейсе REST. См. в разделе [Gen1 хранилища Озера данных проверки подлинности](data-lakes-store-authentication-using-azure-active-directory.md).|
| Контроль доступа |Data Lake Storage 1-го поколения обеспечивает контроль доступа за счет поддержки разрешений POSIX, предоставляемых протоколом WebHDFS. Можно включить списки управления доступом в корневой папке, вложенных папок и отдельных файлов. Дополнительные сведения о работе списки управления доступом в контексте Gen1 хранилища Озера данных см. в разделе [управление доступом в поколение 1 хранилища Озера данных](data-lake-store-access-control.md). |
| Шифрование |Поколение 1 хранилища Озера данных можно также включить шифрование для данных, хранящихся в учетной записи. Параметры шифрования можно задать во время создания учетной записи Data Lake Storage 1-го поколения. Шифрование данных можно как включить, так и отключить. Дополнительные сведения см. в статье [Шифрование данных в Data Lake Storage 1-го поколения](data-lake-store-encryption.md). Инструкции о том, как отправлять конфигурации, связанные с шифрованием, см. в разделе [приступить к работе с Gen1 хранилища Озера данных с помощью портала Azure](data-lake-store-get-started-portal.md). |

Инструкции по защите данных в Azure Data Lake Storage 1-го поколения см. в [этой статье](data-lake-store-secure-data.md).

## <a name="application-compatibility"></a>Совместимость приложений

Data Lake Storage 1-го поколения совместимо с большинством компонентов с открытым исходным кодом в экосистеме Hadoop. Он также интегрируется с другими службами Azure. Чтобы узнать больше об использовании Gen1 хранилища Озера данных с открытым исходным кодом компонентов и других служб Azure, воспользуйтесь следующими ссылками:

- Ознакомьтесь со [списком приложений с открытым кодом, совместимых с Data Lake Storage 1-го поколения](data-lake-store-compatible-oss-other-applications.md).
- См. в разделе [интеграция с другими службами Azure](data-lake-store-integrate-with-other-services.md) понять, как использовать Gen1 хранилища Озера данных с другими службами Azure для реализации более широкого круга сценариев.
- См. статью о [сценариях работы с Data Lake Storage 1-го поколения](data-lake-store-data-scenarios.md), включая прием, обработку, загрузку и визуализацию данных.

## <a name="data-lake-storage-gen1-file-system"></a>Файловая система Gen1 хранилища Озера данных

Поколение 1 хранилища Озера данных может осуществляться через файловую систему AzureDataLakeFilesystem (adl: / /) в средах Hadoop (имеется в кластере HDInsight). Приложения и службы, использующие adl: / / можно воспользоваться преимуществами дальнейшей оптимизации производительности, которые в настоящее время недоступны в WebHDFS. В результате Gen1 хранилища Озера данных дает возможность либо внесенные использование оптимальной производительности в рекомендуемых параметров с помощью adl: / / или сохранить существующий код, продолжая использовать API-Интерфейс WebHDFS напрямую. Azure HDInsight использует все возможности AzureDataLakeFilesystem для обеспечения максимальной производительности в Data Lake Storage 1-го поколения.

Для доступа к данным в Data Lake Storage 1-го поколения можно использовать `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Дополнительные сведения о том, как получить доступ к данным в поколение 1 хранилища Озера данных см. в разделе [Просмотр свойств хранимых данных](data-lake-store-get-started-portal.md#properties).

## <a name="next-steps"></a>Дальнейшие действия

- [Начало работы с Gen1 хранилища Озера данных с помощью портала Azure](data-lake-store-get-started-portal.md)
- [Начало работы с Gen1 хранилища Озера данных с помощью пакета SDK для .NET](data-lake-store-get-started-net-sdk.md)
- [Создание кластеров HDInsight, использующих Data Lake Store, с помощью портала Azure](data-lake-store-hdinsight-hadoop-use-portal.md)