{% note warning "Ограничения по времени" %}

Провайдер {{ TF }} ограничивает время на выполнение операций с кластером {{ mrd-name }}:

* создание, в т. ч. путем восстановления из резервной копии, — 15 минут;
* изменение — 60 минут;
* удаление — 15 минут.

Операции, длящиеся дольше указанного времени, прерываются.

{% cut "Как изменить эти ограничения?" %}

Добавьте к описанию кластера блок `timeouts`, например:

```hcl
resource "yandex_mdb_redis_cluster" "<имя кластера>" {
  ...
  timeouts {
    create = "1h30m" # Полтора часа
    update = "2h"    # 2 часа
    delete = "30m"   # 30 минут
  }
}
```

{% endcut %}

{% endnote %}
