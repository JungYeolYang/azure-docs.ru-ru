---
title: Учебник по масштабированию приложений в Службе приложений Azure с помощью Ansible | Документация Майкрософт
description: Узнайте, как увеличить масштаб приложения в Службе приложений Azure.
keywords: ansible, azure, devops, bash, playbook, Azure App Service, Web App, scale, Java
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: d63708cd87afa426f2712da6d0fcb11c84590798
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230948"
---
# <a name="tutorial-scale-apps-in-azure-app-service-using-ansible"></a>Руководство по масштабированию приложений в Службе приложений Azure с помощью Ansible

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-app-service.md](../../includes/open-source-devops-intro-app-service.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * получение сведений о существующем плане Службы приложений;
> * Увеличение масштаба плана службы приложений до уровня S2 с тремя рабочими ролями

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
- **Приложение в Службе приложений Azure**. Если у вас нет приложения в Службе приложений Azure, [настройте приложение в Службе приложений Azure с помощью Ansible](ansible-create-configure-azure-web-apps.md).

## <a name="scale-up-an-app"></a>Увеличение масштаба приложения

Существуют два рабочих процесса масштабирования: *увеличение масштаба* и *расширение*.

**Увеличение масштаба**. Увеличить масштаб означает задействовать дополнительные ресурсы. К ним относятся ЦП, память, дисковое пространство, виртуальные машины и многое другое. Чтобы увеличить масштаб приложения, следует изменить ценовую категорию плана службы приложений, к которому относится это приложение. 
**Расширение**. Расширить означает увеличить количество экземпляров виртуальных машин, на которых работает приложение. В зависимости от ценовой категории плана службы приложений, вы можете расширить приложение максимум на 20 экземпляров. [Автомасштабирование](/azure/azure-monitor/platform/autoscale-get-started) позволяет масштабировать число экземпляров автоматически на основе предварительно определенных правил и расписаний.

Код из сборника схем в этом разделе определяет следующие операции:

* получение сведений о существующем плане Службы приложений;
* обновление плана Службы приложений до уровня S2 с тремя рабочими ролями;

Сохраните следующий сборник схем как `webapp_scaleup.yml`:

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_name: myAppServicePlan
    location: eastus

  tasks:
  - name: Get facts of existing App service plan
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku

  - name: Scale up the App service plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      is_linux: true
      sku: S2
      number_of_workers: 3
      
  - name: Get facts
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku
```

Запустите сборник схем с помощью команды `ansible-playbook`.

```bash
ansible-playbook webapp_scaleup.yml
```

После запуска сборника схем отобразятся результаты, аналогичные приведенным ниже.

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Get facts of existing App service plan] 
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] 
changed: [localhost]

TASK [Get facts] 
ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP 
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"] 
> [Документация по Ansible в Azure](/azure/ansible/)