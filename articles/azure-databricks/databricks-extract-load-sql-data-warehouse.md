---
title: Руководство. Выполнение операций извлечения, преобразования и загрузки с помощью Azure Databricks
description: Узнайте, как извлечь данные из Data Lake Storage 2-го поколения в Azure Databricks, преобразовать их, а затем загрузить в Хранилище данных SQL Azure.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.date: 05/07/2019
ms.openlocfilehash: e2110378d16ff5826b8ded4620276b784ef1d68e
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65203349"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Руководство по Извлечение, преобразование и загрузка данных с помощью Azure Databricks

В этом руководстве рассматривается выполнение операций извлечения, преобразования и загрузки данных с помощью Azure Databricks. Мы извлечем данные из Azure Data Lake Storage 2-го поколения в Azure Databricks, выполним преобразование данных в Azure Databricks, а затем загрузим преобразованные данные в Хранилище данных SQL Azure.

Для действий, описанных в этом руководстве, используется соединитель хранилища данных SQL для Azure Databricks, позволяющий передавать данные в Azure Databricks. Этот соединитель, в свою очередь, использует хранилище BLOB-объектов Azure как временное хранилище для данных, передаваемых между кластером Azure Databricks и хранилищем данных SQL Azure.

На следующем рисунке показан поток в приложении.

![Azure Databricks с Data Lake Store и хранилищем данных SQL](./media/databricks-extract-load-sql-data-warehouse/databricks-extract-transform-load-sql-datawarehouse.png "Azure Databricks с Data Lake Store и хранилищем данных SQL")

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * Создайте службу Azure Databricks.
> * создание кластера Spark в Azure Databricks;
> * Создание файловой системы в учетной записи Data Lake Storage 2-го поколения.
> * Отправка примера данных в учетную запись Azure Data Lake Storage 2-го поколения.
> * Создание субъекта-службы.
> * Извлечение данных из учетной записи Azure Data Lake Storage 2-го поколения.
> * преобразование данных в Azure Databricks;
> * Загрузка данных в хранилище данных SQL Azure.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

> [!Note]
> Инструкции из этого руководство нельзя выполнять с **бесплатной пробной версией подписки Azure**.
> Чтобы использовать бесплатную учетную запись для создания кластера Azure Databricks, перед созданием кластера перейдите в свой профиль и измените свою подписку на **оплату по мере использования**. Дополнительные сведения см. на странице [создания бесплатной учетной записи Azure](https://azure.microsoft.com/free/).
     
## <a name="prerequisites"></a>Предварительные требования

Прежде чем начать работу с этим руководством, выполните следующие задачи:

* Создайте хранилище данных SQL Azure, создайте правило брандмауэра на уровне сервера и подключитесь к серверу с правами администратора. См. [Краткое руководство. Создание хранилища данных SQL Azure на портале Azure и отправка запросов к этому хранилищу данных](../sql-data-warehouse/create-data-warehouse-portal.md).

* Создайте главный ключ базы данных для хранилища данных SQL Azure. Ознакомьтесь со статьей о [создании главного ключа базы данных](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key).

* Создайте учетную запись хранилища BLOB-объектов Azure, содержащую контейнер. Также получите ключ для доступа к учетной записи хранения. См. [Краткое руководство. Передача, скачивание и составление списка больших двоичных объектов с помощью портала Azure](../storage/blobs/storage-quickstart-blobs-portal.md).

* Создайте учетную запись хранения Azure Data Lake Storage 2-го поколения. См. [Краткое руководство. Создание поддерживаемой учетной записи хранения Azure Data Lake Storage 2-го поколения](../storage/blobs/data-lake-storage-quickstart-create-account.md).

*  Создание субъекта-службы. Дополнительные сведения см. в статье [Azure Создание приложения Azure Active Directory и субъект-службы с доступом к ресурсам с помощью портала](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

   Существует несколько конкретных действий, которые необходимо выполнить при изучении этой статьи.

   * При выполнении действий, описанных в разделе [Назначение приложению роли](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) этой статьи, не забудьте назначить субъекту-службе роль **участника данных BLOB-объектов хранилища**.

     > [!IMPORTANT]
     > Убедитесь в том, что роль назначается в учетной записи хранения Data Lake Storage 2-го поколения. Можно назначить роль родительской группе ресурсов или подписке, но вы будете получать ошибки, связанные с разрешениями, пока роль не будет назначена учетной записи хранения.

   * При выполнении действий, описанных в разделе [Получение значений для входа](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) этой статьи, вставьте идентификатор клиента, код приложения и значения ключа аутентификации в текстовый файл. Они вам скоро понадобятся.

* Войдите на [портале Azure](https://portal.azure.com/).

## <a name="gather-the-information-that-you-need"></a>Сбор необходимых сведений

Обязательно выполните предварительные требования данного руководства.

   Прежде чем начать, вам понадобится собрать такую информацию:

   :heavy_check_mark:  Имя базы данных, имя сервера базы данных, имя пользователя и пароль Хранилища данных SQL Azure.

   :heavy_check_mark:  Ключ доступа к учетной записи хранения больших двоичных объектов.

   :heavy_check_mark:  Имя учетной записи хранения Data Lake Storage 2-го поколения.

   :heavy_check_mark:  Идентификатор клиента подписки.

   :heavy_check_mark:  Идентификатор приложения, зарегистрированного в Azure Active Directory (Azure AD).

   :heavy_check_mark:  Ключ проверки подлинности для приложения, зарегистрированного в Azure AD.

## <a name="create-an-azure-databricks-service"></a>Создание службы Azure Databricks.

В этом разделе вы создадите службу Azure Databricks с помощью портала Azure.

1. На портале Azure выберите **Создать ресурс** > **Analytics** > **Azure Databricks**.

    ![Databricks на портале Azure](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png "Databricks on Azure portal")

2. В разделе **Служба Azure Databricks** укажите следующие значения, чтобы создать службу Databricks.

    |Свойство  |ОПИСАНИЕ  |
    |---------|---------|
    |**Имя рабочей области**     | Укажите имя рабочей области Databricks.        |
    |**Подписка**     | Выберите подписку Azure в раскрывающемся списке.        |
    |**Группа ресурсов**     | Укажите, следует ли создать новую группу ресурсов или использовать имеющуюся. Группа ресурсов — это контейнер, содержащий связанные ресурсы для решения Azure. Дополнительные сведения см. в [обзоре группы ресурсов Azure](../azure-resource-manager/resource-group-overview.md). |
    |**Местоположение.**     | Выберите **Западная часть США 2**.  Другие доступные регионы см. в статье о [доступности служб Azure по регионам](https://azure.microsoft.com/regions/services/).      |
    |**Ценовая категория**     |  Выберите **Стандартная**.     |

3. Установите флажок **Закрепить на панели мониторинга** и щелкните **Создать**.

4. Создание учетной записи займет несколько минут. Во время создания учетной записи на портале с правой стороны отображается плитка **Submitting deployment for Azure Databricks** (Идет отправка развертывания для Databricks). Чтобы отслеживать состояние операции, просмотрите индикатор выполнения вверху.

    ![Плитка развертывания Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-deployment-tile.png "Databricks deployment tile")

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Создание кластера Spark в Azure Databricks.

1. На портале Azure перейдите к созданной службе Databricks, а затем выберите **Launch Workspace** (Запустить рабочую область).

2. Вы будете перенаправлены на портал Azure Databricks. На портале выберите **Кластер**.

    ![Databricks в Azure](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Databricks on Azure")

3. На странице **создания кластера** укажите значения для создания кластера.

    ![Создание кластера Databricks Spark в Azure](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

4. Заполните значения в следующих полях, и примите значения по умолчанию для других полей.

    * Введите имя кластера.

    * В рамках этой статьи создайте кластер со средой выполнения **5.1**.

    * Убедитесь, что установлен флажок **Terminate after \_\_ minutes of inactivity** (Завершить через \_\_ минут бездействия). Укажите длительность (в минутах) для завершения работы кластера, если тот не используется.

    * Выберите **Create cluster** (Создать кластер). Когда кластер будет выполняться, можно присоединить к нему записные книжки и запустить на нем задания Spark.

## <a name="create-a-file-system-in-the-azure-data-lake-storage-gen2-account"></a>Создание файловой системы в учетной записи Azure Data Lake Storage 2-го поколения

В этом разделе показано создание записной книжки в рабочей области Azure Databricks и выполнение фрагментов кода для настройки учетной записи хранения.

1. На [портале Azure](https://portal.azure.com) перейдите к созданной службе Azure Databricks, а затем выберите **Launch Workspace** (Запустить рабочую область).

2. В левой области выберите **Рабочая область**. В раскрывающемся списке **Рабочая область** выберите **Создать** > **Notebook** (Записная книжка).

    ![Создание записной книжки в Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "Создание записной книжки в Databricks")

3. В диалоговом окне **создания записной книжки** введите имя записной книжки. Выберите **Scala** в качестве языка, а затем выберите созданный ранее кластер Spark.

    ![Предоставление сведений для записной книжки в Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "Предоставление сведений для записной книжки в Databricks")

4. Нажмите кнопку **Создать**.

5. Скопируйте следующий блок кода и вставьте его в первую ячейку.

   ```scala
   spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", "<application-id>")
   spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", "<authentication-key>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   dbutils.fs.ls("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")
   ```

6. В этом блоке кода замените значения заполнителя `application-id`, `authentication-id`, `tenant-id` и `storage-account-name` значениями, полученными в ходе выполнения предварительных условий этого руководства. Значение заполнителя `file-system-name` замените на имя файловой системы.

   * `application-id` и `authentication-id` заменяются значениями из приложения, которое вы зарегистрировали в Azure AD при создании субъекта-службы.

   * Значение `tenant-id` берется из подписки.

   * `storage-account-name` — это имя учетной записи хранения Azure Data Lake Storage 2-го поколения.

7. Нажмите клавиши **SHIFT + ВВОД**, чтобы запустить код в этом блоке.

## <a name="ingest-sample-data-into-the-azure-data-lake-storage-gen2-account"></a>Прием примера данных в учетную запись Azure Data Lake Storage 2-го поколения

Прежде чем приступить к работе с этим разделом, выполните следующие предварительные требования.

Введите следующий код в ячейку записной книжки:

    %sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json

В ячейке нажмите клавиши **SHIFT+ВВОД**, чтобы выполнить код.

Теперь в новую ячейку под этой введите следующий код и замените значения, которые отображаются в скобках, использованными ранее.

    dbutils.fs.cp("file:///tmp/small_radio_json.json", "abfss://<file-system>@<account-name>.dfs.core.windows.net/")

В ячейке нажмите клавиши **SHIFT+ВВОД**, чтобы выполнить код.

## <a name="extract-data-from-the-azure-data-lake-storage-gen2-account"></a>Извлечение данных из учетной записи Azure Data Lake Storage 2-го поколения

1. Теперь можно загрузить пример JSON-файла в виде кадра данных в Azure Databricks. Вставьте следующий код в новую ячейку. Замените значения заменителей, показанные в скобках, собственными.

   ```scala
   val df = spark.read.json("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/small_radio_json.json")
   ```

   * Замените значение заполнителя `file-system-name` именем, которое вы дали своей файловой системе в Обозревателе службы хранилища.

   * Замените заполнитель `storage-account-name` именем вашей учетной записи хранения.

2. Нажмите клавиши **SHIFT + ВВОД**, чтобы запустить код в этом блоке.

3. Чтобы просмотреть содержимое кадра данных, выполните следующий код:

    ```scala
    df.show()
    ```
   Должен отобразиться результат, аналогичный приведенному ниже фрагменту кода.

   ```bash
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   |               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   | El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
   | Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
   | Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
   ...
   ...
   ```

   В результате были извлечены данные из Azure Data Lake Storage Gen2 в Azure Databricks.

## <a name="transform-data-in-azure-databricks"></a>Преобразование данных в Azure Databricks.

Необработанный файл с примером данных **small_radio_json.json** содержит сведения о слушателях радиостанции и имеет множество столбцов. В этом разделе мы преобразуем данные, чтобы извлечь только определенные столбцы из набора данных.

1. Сначала извлеките только столбцы **firstName**, **lastName**, **gender**, **location** и **level** из созданного кадра данных.

   ```scala
   val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
   specificColumnsDf.show()
   ```

   Выходные данные должны выглядеть так, как показано в следующем фрагменте кода:

   ```bash
   +---------+----------+------+--------------------+-----+
   |firstname|  lastname|gender|            location|level|
   +---------+----------+------+--------------------+-----+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   +---------+----------+------+--------------------+-----+
   ```

2. Эти данные можно еще преобразовывать, переименовав столбец **level** на **subscription_type**.

   ```scala
   val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
   renamedColumnsDF.show()
   ```

   Выходные данные должны выглядеть так, как показано в следующем фрагменте кода.

   ```bash
   +---------+----------+------+--------------------+-----------------+
   |firstname|  lastname|gender|            location|subscription_type|
   +---------+----------+------+--------------------+-----------------+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   +---------+----------+------+--------------------+-----------------+
   ```

## <a name="load-data-into-azure-sql-data-warehouse"></a>Загрузка данных в хранилище данных Azure SQL

В этом разделе преобразованные данные отправляются в хранилище данных SQL Azure. С помощью соединителя Хранилища данных SQL Azure для Azure Databricks можно напрямую отправить кадр данных в виде таблицы в хранилище данных SQL.

Как упоминалось ранее, соединитель Хранилища данных SQL использует хранилище BLOB-объектов Azure в качестве временного хранилища для передачи данных между Azure Databricks и Хранилищем данных SQL Azure. Таким образом сначала нужно предоставить конфигурацию для подключения к учетной записи хранения. Вы уже должны были создать учетную запись, выполняя предварительные требования для этой статьи.

1. Предоставьте конфигурацию для получения доступа к учетной записи хранения Azure из Azure Databricks.

   ```scala
   val blobStorage = "<blob-storage-account-name>.blob.core.windows.net"
   val blobContainer = "<blob-container-name>"
   val blobAccessKey =  "<access-key>"
   ```

2. Укажите временную папку, которая будет использоваться при перемещении данных между Azure Databricks и Хранилищем данных SQL Azure.

   ```scala
   val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"
   ```

3. Запустите следующий фрагмент кода, чтобы сохранить ключ доступа к хранилищу BLOB-объектов Azure в конфигурации. Благодаря этому действию вам не придется хранить ключ доступа в записной книжке в виде обычного текста.

   ```scala
   val acntInfo = "fs.azure.account.key."+ blobStorage
   sc.hadoopConfiguration.set(acntInfo, blobAccessKey)
   ```

4. Укажите значения для подключения к экземпляру хранилища данных SQL Azure. Вы должны были создать хранилище данных SQL, выполняя требования для этой статьи.

   ```scala
   //SQL Data Warehouse related settings
   val dwDatabase = "<database-name>"
   val dwServer = "<database-server-name>"
   val dwUser = "<user-name>"
   val dwPass = "<password>"
   val dwJdbcPort =  "1433"
   val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
   val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
   val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
   ```

5. Запустите следующий фрагмент кода, чтобы загрузить преобразованный кадр данных **renamedColumnsDF** в качестве таблицы в хранилище данных SQL. Этот фрагмент кода создает таблицу с именем **SampleTable** в базе данных SQL.

   ```scala
   spark.conf.set(
       "spark.sql.parquet.writeLegacyFormat",
       "true")

   renamedColumnsDF.write
       .format("com.databricks.spark.sqldw")
       .option("url", sqlDwUrlSmall) 
       .option("dbtable", "SampleTable")
       .option( "forward_spark_azure_storage_credentials","True")
       .option("tempdir", tempDir)
       .mode("overwrite")
       .save()
   ```

   > [!NOTE]
   > В этом примере используется флаг `forward_spark_azure_storage_credentials`, который вызывает Хранилище данных SQL для доступа к данным из хранилища BLOB-объектов с помощью ключа доступа. Это единственный поддерживаемый способ проверки подлинности.
   >
   > Если для хранилища BLOB-объектов Azure нельзя выбирать виртуальные сети, Хранилище данных SQL запросит [Управляемое удостоверение службы, а не ключи доступа](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage). В таком случае вы получите ошибку с сообщением о том, что у вызывающей стороны нет прав на выполнение этой операции.

6. Подключитесь к базе данных SQL и убедитесь, что вы видите базу данных **SampleTable**.

   ![Проверка примера таблицы](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "Проверка примера таблицы")

7. Выполните запрос SELECT, чтобы проверить содержимое таблицы. В таблице должны быть те же данные, что и в кадре данных **renamedColumnsDF**.

    ![Проверка содержимого примера таблицы](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "Проверка содержимого примера таблицы")

## <a name="clean-up-resources"></a>Очистка ресурсов

После выполнения заданий из этого руководства вы можете завершить работу кластера. В рабочей области Azure Databricks в области слева выберите **Кластеры**. Для завершения работы кластера в разделе **Действия** наведите указатель мыши на многоточие (...) и выберите значок **Завершить**.

![Завершение работы кластера Databricks](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "Stop a Databricks cluster")

Если не завершить работу кластера вручную, она завершится автоматически, если во время создания кластера вы установили флажок **Terminate after \_\_ minutes of inactivity** (Завершить через \_\_ минут бездействия). В этом случае работа кластера автоматически завершается, если он был неактивным в течение определенного времени.

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * Создание службы Azure Databricks.
> * Создание кластера Spark в Azure Databricks.
> * Создание записной книжки в Azure Databricks.
> * Извлечение данных из учетной записи Data Lake Storage 2-го поколения.
> * Преобразование данных в Azure Databricks.
> * Загрузка данных в хранилище данных Azure SQL

Перейдите к следующему руководству, чтобы узнать о потоковой передачи данных в реальном времени в Azure Databricks с помощью Центров событий Azure.

> [!div class="nextstepaction"]
>[Потоковая передача данных в Azure Databricks при помощи службы "Центры событий"](databricks-stream-from-eventhubs.md)
