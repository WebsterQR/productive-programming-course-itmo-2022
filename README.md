# Краткий конспект лекций по продуктивному программированию на КТ ИТМО (y2019)
## Code Review (лекция 3)
**Changed List** - набор изменений переводящихся из состояния разработки в стабильное состояние. Синоним pull request-а

Code review - одна из преград на CL в стабильное состояние. Или немного преземленнее: одна из преград на пути pull request-а к слитию в основную ветку.

![image](https://user-images.githubusercontent.com/57497898/210035931-2770e4e5-081f-4584-8fe7-1fcac7caad60.png)

По-хорошему, code review должен быть последним этапом защиты кодовой базы от плохого кода (после unit тестов, проверок на кодстайл и тд), так как
1. занимает время другого разработчика
2. именно человек должен принять финальное решение по слитию кода, а не unit тесты или что-то еще проверяемое программами

---
Есть большая разница между **Code Quality (Качество кода)** и **Code Style (Стиль кода)**

Code Quality - это то насколько код хорош, во всевозможных смыслах

Code Style ~ четкие правила для синтаксиса языка (напр сколько аргументов у функции может быть максимум, использовать ли camelCase или snake_case в названиях переменных). Чаще различается у разных компаний.

Табличка различий Code Quality и Code Style
![image](https://user-images.githubusercontent.com/57497898/210037453-1dbd156a-d797-4320-96b2-8ce046596c87.png)

---
Зачем нужен Code Review?

1. Качество кода влечет качество продукта. Во первых это дешевый способ отловить потенциальные баги, во вторых это хорошая инвестиция в будущее качество проекта.
2. Feedback loop -> профессиональный рост
3. Reviewer в свою очередь может узнавать что-то новое из проверяемого кода

Золотые правила Code Review
1. Если PR улучшает кодовую базу, то этого достаточно чтобы его принять
2. Правила Review кода это хорошо, но если продукт требует срочных критических правок, то ими можно пренебречь 

На что обращать внимание при Code Review
1. Какой смысл у этого PR? (Должен ли он вообще быть?)
2. Делает ли он то что нужно?
3. Можно ли потом будет эти изменения исправить? (откатить, изменить)

Сложность изменения/отката PR расчитывается через формулу:

- $SE = \int P\ dt\ dh$

*Software Engeneering = интеграл программирования по времени и по людям*

----
### На что обратить внимание при проверке PR?
#### 1. Должен ли PR существовать?
- критерий: хорошее описание. Описание к PR не должно быть сильно кратким, типа "fix bug". Так же не надо сразу вдаваться в детали, сначала напишите summary
- PR должен делать одну конкретную вещь, исключая рефакторинги
- PR должен быть небольшого размера
- автор PR должен иметь права делать такие изменения

#### 2. Проверка кода на функцональность
- в команде есть договоренность что нужно сделать этот код (например документ или обсуждение на созвоне)
- выполняет ли код свои функции
  - функционал покрыт тестами
  - правильно ли написаны бд транзакции
  - правильно ли работает параллельный код
- ревьюер - последняя стадия защиты

#### 3. Сложность и структура классов
- ООП должно удовлетворять паттерну SOLID - это очень облегчает понимание кода
- не пропускайте неоправданно сложные конструкции (строчка слишком сложная, функция слишком сложная, класс слишком сложный)
- вы можете упростить что-то в PR. Например вынести общий код в отдельный метод, чтобы упростить как и свой новый код, так и старый

Не будьте злыми, не надо усложнять код. Людям которые считают свой сложный код достаточно легким, напоминайте что их код будут поддерживать другие люди. Да и сам автор через 6 месяцев не вспомнит контекст и будет долго разбираться в своем же коде.

Еще один случай: люди усложняют код, чтобы он работал быстрее
- просите автора доказать что код ускорит свое выполнение
- нужно ли это ускорение? Например: пусть у нас в 2 раза ускорится выполнение логики программы, но 99% времени все еще будет занимать запрос к базе, тогда это усложнение кода не нужно

Правило по которому по которому можно заниматься сложными оптимизациями: 
- сначала мы пишем код, до состояния когда он работает корректно
- затем оптимизируем узкие места (bottlenecks) если нужно

![image](https://user-images.githubusercontent.com/57497898/212164722-fb7376f5-0ca2-4909-a879-8e6289c73551.png)

Как писать сложный код?
- максимально инкапсулируем его и создаем простое апи, чтобы разработчик мог не долго задумываться и использовать ваше решение как черный ящик
- максимально протестируйте ваш сложный функционал

#### 4. API
