Сама по себе роль не дает права выполнять какие-либо операции и используется только в сочетании с другими ролями.


Как сочетать роль с другими ролями зависит от того, входит облако в организацию или нет.

{% list tabs %}

- В организации

    {% include [roles-cloud-member-organization](./roles-cloud-member-organization.md) %}

- Без организации

    Роль необходима для доступа к ресурсам в облаке всем, кроме [владельцев облака](../resource-manager/concepts/resources-hierarchy.md#owner) и [сервисных аккаунтов](../iam/concepts/users/service-accounts.md).

    Без этой роли у пользователя не будут работать никакие другие роли.

    Роль назначается автоматически при добавлении в облако без организации нового пользователя.

{% endlist %}

