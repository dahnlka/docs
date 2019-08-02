Для начала выполнения конкретной транзакции таблетка должна быть уверена что не осталось затрагивающих её транзакций, запланированных ранее рассматриваемой (иначе необходимо было бы выполнять вот ту раннюю транзакцию, а не рассматриваемую). Распространять эту информацию непосредственно с **[Coordinator'ов](tablet_coordinator.md)** затруднительно - т.к. не известно какие таблетки потрогали другие координаторы пришлось бы рассылать информацию о планировании на все таблетки, включая незатронутые ни одной транзакцией. Этот подход явно не оптимален, поэтому необходима точка синхронизации.

Таковой точкой синхронизации планов выполнения транзакций для группы таблеток и выступает **Mediator**. **Mediator** назначается для таблетки статически (хешированием по tabletId) и запланировав транзакцию для таблетки - **Coordinator** доставляет план до **Mediator'ов**, ответственных за затронутые таблетки. И кроме того - даже если для таблеток **Mediator'а** ничего не запланировано - всё равно доставляется план (пустой).

Уже **Mediator**, получив план со всех **Coordinator'ов** - может быть уверен, что новых транзакций не будет, объединяет планы для таблетки со всех координаторов и рассылает объединенный план на таблетку. И теперь таблетка имеет только один источник информации о плане и может выполнять транзакции сразу по получению плана. Также нет необходимости рассылать план на неактивные таблетки.

Технически **Mediator** является специальным типом таблетки, но фактически не содержит никаких данных и использует таблеточную инфраструктуру только для подъема собственного инстанса и адресации в кластере.

См. [Пайплайн выполнения транзакций](tx_processing_graph.md)