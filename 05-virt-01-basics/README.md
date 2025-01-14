
# Домашнее задание к занятию 1.  «Введение в виртуализацию. Типы и функции гипервизоров. Обзор рынка вендоров и областей применения»


## Как сдавать задания

Обязательны к выполнению задачи без звёздочки. Их нужно выполнить, чтобы получить зачёт и диплом о профессиональной переподготовке.

Задачи со звёздочкой (*) — это дополнительные задачи и/или задачи повышенной сложности. Их выполнять не обязательно, но они помогут вам глубже понять тему.

Домашнее задание выполняйте в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Любые вопросы по решению задач задавайте в чате учебной группы.

---

## Важно

Перед отправкой работы на проверку удаляйте неиспользуемые ресурсы.
Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.

Подробные рекомендации [здесь](https://github.com/netology-code/virt-homeworks/blob/virt-11/r/README.md).

---

## Задача 1

Опишите кратко, в чём основное отличие полной (аппаратной) виртуализации, паравиртуализации и виртуализации на основе ОС.

- Полная или аппаратная виртуализация сразу может использовать аппаратные ресурсы сервера и не нуждается в операционной системе для управления виртуальными машинами.
- Паравиртуализация - программное обеспечение, использующее операционную систему для разделения ресурсов между виртуальными машинами.
- Виртуализация на основе ОС позволяет запускать изолированные "контейнеры" на хосте, которые по сути являются небольшими отдельными копиями хостовой ОС. (Наверное неправильно выразился, но в целом - на linux запускаем только такие же linux гостевые ОС). Удобно для тестирования и прочего создания множества подобных систем.


## Задача 2

Выберите один из вариантов использования организации физических серверов в зависимости от условий использования.

Организация серверов:

- физические сервера,
- паравиртуализация,
- виртуализация уровня ОС.

Условия использования:

- высоконагруженная база данных, чувствительная к отказу;
- различные web-приложения;
- Windows-системы для использования бухгалтерским отделом;
- системы, выполняющие высокопроизводительные расчёты на GPU.

Опишите, почему вы выбрали к каждому целевому использованию такую организацию.

| Условия | Организация | Почему |
| --- | --- | --- |
| Высоконагруженная база данных, чувствительная к отказу | физические сервера | Виртуализация даёт оверхед и увеличивает латентность. В высоконагруженной системе это нежелательно, а достичь отказоустойчивости можно резервированием. Постоянно нагруженной системе потребуется максимум ресурсов хоста, присутствие соседей которые могут их отобрать тоже нежелательно.  |
| Различные web-приложения | виртуализация уровня ОС | Современная практика - разворачивать часто и много веб-приложений на одном хосте, при этом нужно обеспечить их изоляцию друг от друга. Виртуализация ОС подходит лучше всего, так свернуть приложение в контейнер и развернуть из него быстрей, чем делать это с виртуальными машинами с полноценной ОС и отдельным ядром. |
| Windows системы для использования бухгалтерским отделом | паравиртуализация | Виртуализация поможет системе быть более отказоустойчивой, из предложенных вариантов для Windows возможна только паравиртуализация. |
| Системы, выполняющие высокопроизводительные расчеты на GPU | виртуализация уровня ОС | виртуализация GPU может потребоваться в проектах с машинным обучением, из предложенных видов виртуализации для GPU возможна только виртуализация средствами ОС |

## Задача 3

Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.

Сценарии:

1. 100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based-инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
2. Требуется наиболее производительное бесплатное open source-решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
3. Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows-инфраструктуры.
4. Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.
```
Подойдут Hyper-V, vSphere. Хорошо поддерживают виртуальные машины с Windows и Linux, имеют встроенные требуемые возможности (балансировка,
репликация, бэкапы) и могут работать в кластере гипервизоров, что необходимо для работы 100 виртуальных машин.
```

## Задача 4

Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.
```
Возможные проблемы и недостатки гетерогенной среды виртуализации:
- сложность администрирования;
- необходимое наличие высококвалифицированных специалистов;
- повышенный риск отказа и недоступности;
- завышенная стоимость обслуживания;

Действия для минимизации рисков и проблем:
- если гетерогенность не оправдана, то рассмотреть возможность отказа от нее;
- если она оправдана, то часть инфраструктуры можно перенести на IaaS (AWS), а саму инфраструктуру вывести в IaC (Terraform);
- максимальное автоматизировать развертывание и тестирование инфраструктуры, чтобы она была единая.

Я бы предпочел работать в единой среде. Небольшие выгоды в цене и производительности при использовании разных ведут
к большим издержкам и не всегда оправданы.

