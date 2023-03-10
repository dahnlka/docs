# Управление плагинами {{ OS }}

При создании кластера в {{ mos-short-name }} вы можете указать желаемый список плагинов, и они будут автоматически установлены в кластер. Полный список доступных плагинов приведен в разделе [{#T}](../concepts/plugins.md#opensearch).

## Получить список установленных плагинов {#list}

{% list tabs %}

- Консоль управления

    1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mos-name }}**.
    1. Нажмите на имя нужного кластера.

- API

    Воспользуйтесь методом API [get](../api-ref/Cluster/get.md) и передайте в запросе идентификатор кластера в параметре `clusterId`.

    Включенные плагины будут перечислены в списке `config.opensearch.plugins`.

    Идентификатор кластера можно [получить со списком кластеров в каталоге](cluster-list.md#list-clusters).

{% endlist %}

## Изменить список установленных плагинов {#update}

{% list tabs %}

- Консоль управления

    1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mos-name }}**.
    1. Выберите кластер и нажмите кнопку ![pencil](../../_assets/pencil.svg) **Изменить кластер** на панели сверху.
    1. В блоке **Базовые параметры** укажите плагины, которые должны быть установлены.
    1. Нажмите кнопку **Сохранить**.

- API

    Воспользуйтесь методом API [update](../api-ref/Cluster/update.md) и передайте в запросе:

    * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
    * Список плагинов в параметре `configSpec.opensearchSpec.plugins`. Плагины, не указанные в списке, будут выключены.

    {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}