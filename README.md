### Протоколы

Протоколы в swift -- инкапсулируют сущности их реализующие.

Протоколы позволяют скрыть конкретную реализацию сущности. Сущность получившая при инициализации тип протокола или переданная в метод как переметр типа протокола может предоставлять только функциональность описанную в этом протоколе. Протоколы позволяют декларативно описывать функциональность через их название и список инструментов, которые они предоставляют. Это упрощает понимание разработчком кодовой базы системы. Объявление сущности(например сервиса) через протокол позволяет не зависет от конкретной реализации(DI из солид. «Зависимость на Абстракциях. Нет зависимости на что-то конкретное.»). На протоколах строится паттерн "Делегат". 

С помощью протоколов можно сгруппировать функционал сервиса(например) по контексту. Для сервиса, поделенного на части через протоколы легче писать моки(в моке нужно реализовывать функционал не всего класса, но только описанный в протоколе)

### DI

DI -- Framework строит зависимости между компонентами программы в момент запроса компонента из DI-контейнера. DI -- Framework обязуется что за каждой абстракцией будет стоят конкретная реализация(при этом ничего о конкретной реализации компонент, который содержит данную абстракцию, знать не должен).

###### Виды DI
**Initializer-based Dependency Injection**
by passing the dependencies as parameters to the init functions and then storing them as member variables of your classes. 

Инъекция через инициализатор не всегда самый удобный способ. Например если View-компонент создан в Storyboard или в XIB, то init-метод для него не будет вызываться в коде явным образом.

**Property-based Dependency Injection** 
Инъекция зависимостей после инициализации. В таком случае свойства должны быть не приватными

### UserDefault

**UserDefault** -- key-value хранилище. Предназначенно для сохрание начальных конфигурационных параметров приложения(поэтому так называется). В UserDefaults можно сохранять любой тип данных реализующий Codable протокол. Данные, сохраненные в UserDefaults не шифруются.     

### Keychain

**Keychain** -- key-value хранилище. Предназначен для небольших объемов данных, которые необходимо хранить в зашифрованном виде(например ключи, пароли и т.п.). Данные в keychain  сохраняются даже тогда, когда пользователь удаляет приложение. 

### GCD

**GCD** -- это библиотека от Apple, написанная на языке С и С++.  Предоставляет верхнеуровневый интерфейс для работы с многопотоностью в **OS Darwin**.

**DispatchWorkItem** -- блок кода, который можно добавить в очередь(DispatchQueue). Может быть представлен в виде сущности класса или в виде замыкания. Как сущность класса предоставляет интерфейсы управления. 

Задачи можно группировать в группы, структурированные в виде очередей -- **DispatchQueue**.

При создании очереди задается тип выполнения задач в ней( serial или concurrent ).

**Serial** выполнение гарантирует что задачи будут выполняться строго по порядку их добавления в очередь, без переключения контектса. В одном потоке.

**Concurrent** выполнение гарантирует что задачи будут начинать свое выполнение по порядку их добавления в очередь, с переключением контекста. Порядок завершения задач не определен. Возможно выполнение одной очереди в разных потоках.

###### Все системные очереди кроме Main -- concurrent

###### Системные очереди(по qos): Main; UserInteractive; UserInitiated; default(global); utility; background

В момент запуска очереди программист указывает как она будет возвращать управление текущему потоку.

**sync** -- управление текущему потоку будет возвращенно только после завершения выполнения задач в очереди. 

**async** -- управление текущему потоку возвращается немедленно после запуска очереди. 

###### Проблемы многопоточности: race condition; priority inversion; deadlock

**Thread Safe**-код может быть безопасно вызван из разных потоков. Дополнительного разграничения одновременного доступа к нему не требуется.  

**Context Switch** -- переключение контекста(но не обязательно потока)	

### Core Data Framework

**Core Data** -- это фреймворк для работы со слоем модели приложения. Является **ORM** - системой(надстройкой над более низкоуровневым языком запросов), также предоставляет интерфейс(**interface builder** ) в котором можно видеть сущности(**entity**) БД как отдельные таблицы. В **Interface builder** можно создавать новые сущности(**entity**), добавлять новые свойства(**property**) для сущностей и настраивать аттрибуты(**attributes**) для свойств. Также **interface builder** предоставляет графическое отображение графа зависимостей между(**entiry**)

> Interface builder -- это приложение встроеное в xcode, которое предоставляет графический интерфейс для работы с компонентами создаваемого приложение. Он поддерживает форматы: .storyboard ; .xib ; .xcdatamodeld.
>
> storyboard, xib -- проектирование графических элементов системы. storyboard по сравнению с xib позволяет проектировать несколько view  и viewContrillers в одном файле, в нем же строить связи между ними.
>
> xcdatamodeld -- графически описывает сущности слоя модели в приложении

Свойства(**propery**) у сущностей(**entity**) поддерживают огромное колличество форматов. В свойстве можно хранить большой бинарный файл(картинка, pdf, audio, video). Для такого свойства(**property**) нужно указать аттрибут(**attribute**) **Allow External Storage**. Тогда, если бинарник будет весить больше лимита, определенного в системе, тогда в БД будет храниться только **URI** на указанный контент. 

Для некоторых типов данных в **attributes** можно задать **validation rules**. В этом случае  **managed object context** выбросит(**throw**) ошибку  при попытке сохранения невалидных данных.

![Screen-Shot-2018-11-07-at-7.18.37-PM](http://i-swift.ir/wp-content/uploads/Screen-Shot-2018-11-07-at-7.18.37-PM.png)

Также Core Data имеет специальный тип **Transformable**, который позволяет добавить в сущность(**entity**) свойство(**property**) любого типа, который реализует **NSCoding** протокол.

Core Data способен сохранять и работать с данными нескольких форматов: SQLite; XML; BIN. Сама БД может храниться в постоянной или оперативной памяти(**in memory**). 

**In memory** -- данные в БД хранятся до тех пор, пока приложение не будет выгруженно из памяти. 

**Core Data** разделен на несколько отдельных сущностей в соответствии с принципом единственной ответственности(**single responsobility**). Данный скоп сущностей называется **Core Data Stack**.

**NSPersistentContainer** -- фасад для **Core Data Stack**. 

### Core Data Stack 

**NSManagedObjectContext** -- реализует механизм **БД - транзакций**. Любое действие с БД в Core Data происходит в определенном контексте. Для контекста можно задать поток выполнения. Только после сохранения контекста(  **open** **func** save() **throws** ) все изменения вступают в силу. Условно говоря, внутри контекста происходит работа с копией данных из БД. 	

> **БД-транзакция** -- группа последовательных операций с [базой данных](https://ru.wikipedia.org/wiki/База_данных), которая представляет собой логическую единицу работы с данными. Транзакция может быть выполнена либо целиком и успешно, соблюдая целостность данных и независимо от параллельно идущих других транзакций, либо не выполнена вообще, и тогда она не должна произвести никакого эффекта.

**NSPersistentStoreCoordinator** -- прослойка между **NSManagedObjectModel** и **NSPersistentStore**. Транслирует запросы в **SQL**(или в другой формат в зависимости от выбранного типа хранилища). Для пользователя предоставляет интерфейс, позволяющий извлекать, сохранять  данные в БД.

> Условно говоря **NSPersistentStoreCoordinator** понимает **NSManagedObjectModel**  и умеет ложить и доставать данные из **NSPersistentStore**

**NSManagedObjectModel** -- прогаммная репрезентация xcdatamodeld файла. Описывает схему entityes БД. Объеты, класса или подкласса NSManagedObjectModel являются сущностями(**entityes**) БД приложения. Может быть как написана вручную, так и автосгенерированна из .xcdatamodeld файла.

**NSPersistentStore** -- абстрагирует от реальных типов данных (XML, SQL, BIN).

**NSPersistentStore** бывает двух видов:

* Atomic (**NSAtomicStore**) - всё или ничего, то есть читает и/или пишет все данные сразу (BIN, XML)

* Incremental (**NSIncrementalStore**) - напротив позволяет сохранять/читать данные чанками (SQL)

  Можно написать свой собственный формат хранения данных. Это будет класс-наследник [**NSAtomicStore**](https://forum.swiftbook.ru/t/nspersistentstore-swift-4-0-chast-1/4391/3) или [**NSIncrementalStore**](https://forum.swiftbook.ru/t/nspersistentstore-swift-4-0-chast-2/4581). Например унаследовавшись от **NSAtomicStore** можно реализовать **JSON-формат**

--------------- --------------- --------------- --------------- --------------- --------------- --------------- ---------------



**NSFetchRequest** -- запрос данных из БД. 

*Позволяет задать:*

* Фильтрацию(**NSPredicate**)

* Сортировку(**NSSortDescription**)

* Дополнительные лимиты: 

  			1) ограничить количество извлекаемых объектов(**fetchLimit**) ;

    			2) сместить результаты выборки на заданное количество объектов(**fetchOffset**) ; 

    			3) задать лимит на колличество извлекаемых элементов в один запрос(**fetchBatchSize**)

* Тип извлекаемых объектов(**NSFetchRequestResultType**)

* Извлекать из сущности(**entity**) только конкретные свойства(**properties**) (**propertiesToFetch**) 

* **includesSubentities** -- извлечение объектов не только текущей сущности, но и сущностей-наследников(по умолчанию **true**)

``let request: NSFetchRequest<BowTie> = BowTie.fetchRequest()``

**Лимит:**

``request.fetchLimit = 12``

**Диапозон смещения**:

``request.fetchOffset = 2``

**Лимит на колличество извлекаемых элементов в один запрос:**

``request.fetchBatchZize = 20``

**Фильрация:**

``request.predicate = NSPredicate( format: "%K = %@", argumentArray: [#keyPath(BowTie.searchKey), selectedValue])`` , где ``class BowTie: NSManagedObject``

**Сортировка**:

``let nameSortDescriptor = NSSortDescriptor(key: #keyPath(BowTie.name), ascending: true) ``

``let priceSortDescriptor = NSSortDescriptor(key: #keyPath(BowTie.price), ascending: true) ``

``request.sortDescriptors = [nameSortDescriptor, priceSortDescriptor]``

``...``

``let results = try managedContext.fetch(request)``

``...``

**resultType**

``var resultType: NSFetchRequestResultType``

При извлечении данных, метод **`executeFetchRequest(_:)`** по умолчанию возвращает массив объектов класса **`NSManagedObject`** и его наследников

Изменение свойства **`resultType`** позволяет выбрать тип извлечённых объектов:

* **`NSManagedObjectResultType / ManagedObjectResultType`** — объекты класса **`NSManagedObject`** и его наследников (по умолчанию)
* **`NSManagedObjectIDResultType / ManagedObjectIDResultType`** — идентификатор объекта **`NSManagedObject`**
* **`NSDictionaryResultType / DictionaryResultType`** — словарь, где ключи — это имена атрибутов сущности
* **`NSCountResultType / CountResultType`** — вернёт один элемент массива со значением количества элементов.

**propertiesToFetch**

``request.resultType = .DictionaryResultType ``

``request.propertiesToFetch = ["name"]``

Данное свойство позволяет извлекать из сущности(**entity**) только необходимые свойства(**properties**). Обязательное условие -- **`resultType`** должен быть словарём (**`NSDictionaryResultType`** / **`DictionaryResultType`**)

---

**NSFetchResultController** 

``NSFetchedResultsController(fetchRequest: fetchRequest, managedObjectContext: context, sectionNameKeyPath: nil, cacheName: nil)``

Отслеживает изменения в переданном ему контексте, слушая нотификации 

Может дополнительно кэшировать данные

С помощью него можно отслеживать добавление новых элементов 

удобен для содержания UI -- таблиц в приложении в актуальном состоянии

Умеет уведомлять об изменениях, произошедших в контексте





----------------------



**NSEntityDescription** -- позволяет в рантайме задать тип для конкретной сущности(**entity**)

**Пример:**

``let entity = NSEntityDescription.entity(forEntityName: "Person", in: managedContext)! ``

``let person = NSManagedObject(entity: entity, insertInto: managedContext``

В примере создается entity типа "Person". Проверкой на валидность типа происходит в рантайме. В целом такая конструкция похожа на инстанции сториборда.

**NSManagedObjectModel**  тесно интегрируется с более низкоуровневым форматом хранения данных(SQL, XML, BIN). Поэтому реализация(implementation) пропертей(**property**) для класса происходит в рантайме. Такие проперти помечены аттрибутом **@NSManaged**.  

**@dynamic** аналогичен **@NSManaged**

------

### Виды связей в Core Data(**Relationships between Entities**)

* *One-To-One*

* *One-To-Many*

* *Many-To-Many*

  > * Один ко многим -- объекту А принадлежит несколько объектов Б, объекту Б принадлежит только один объект А.

В графическом редакторе(**interface builder**) каждая сущность(**entity**) имеет свое поле с описанием связей(**relationships**)

![1*KmbyBDJ7i8rJ6gSkTyheRg](https://hackernoon.com/hn-images/1*KmbyBDJ7i8rJ6gSkTyheRg.png)

**Relationship** -- имя 

**Destination** -- имя сущностни(**entity**) к которой относится конкретная связь(**relation**)

**Inverse** -- обратная зависимость. Должна ли **destination entity** иметь такую же связь. В большинстве случаев должна. Это обеспечивает консистентность данных в таблице, рекурсивное(**recursively**) удаление сущностей(**entity**). Инверсную зависимость(**inverse relationship**) имеет смысл отключить в том случае, если она начинает сильно влиять на производительность, например, если инверсная зависимость содержить очень большое число объектов(**extremely large number of objects**). 

Если нет везкой причины для обратного, связь всегда должна быть инверстной(**unless you have a specific reason not to, model the inverse**)	

Для каждой **Relationship** Xcode генерирует список методов. 

**Пример:**

Cущности(**entityes**): User; Account. Связь -- один ко многим.

```swift
extension Account {

    @objc(addUsersObject:)
    @NSManaged public func addToUsers(_ value: User)

    @objc(removeUsersObject:)
    @NSManaged public func removeFromUsers(_ value: User)

    @objc(addUsers:)
    @NSManaged public func addToUsers(_ values: NSSet)

    @objc(removeUsers:)
    @NSManaged public func removeFromUsers(_ values: NSSet)

}
```

