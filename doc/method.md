### Методы

#### bot.end(reason)

Завершает соединение с сервером.
* ``причина`` - Необязательная строка, указывающая причину завершения.

#### bot.quit(reason)

Грациозно отключиться от сервера с указанной причиной (по умолчанию 'disconnect.quitting').

#### bot.tabComplete(str, [assumeCommand], [sendBlockInSight])

Эта функция возвращает `Promise` с `matches` в качестве аргумента после завершения.

Запрашивает завершение чата у сервера.
 * `str` - Строка для завершения.
 * `assumeCommand` - поле, отправляемое на сервер, по умолчанию false.
 *`sendBlockInSight` - поле, отправляемое на сервер, по умолчанию true. Установите значение false, если вам нужна более высокая производительность.

#### bot.chat(message)

Отправляет публично транслируемое сообщение в чат. При необходимости разбивает большие сообщения на несколько сообщений чата.

#### bot.whisper(username, message)

Сокращение для "/tell <имя пользователя>". Все разделенные сообщения будут переданы шепотом имени пользователя.

#### bot.chatAddPattern(pattern, chatType, description)

Исправлено, вместо этого используйте `addChatPattern`.

Добавляет regex-шаблон в соответствие чату бота. Полезно для серверов bukkit, где формат чата часто меняется.
 * `pattern` - регулярное выражение для соответствия чату
 * `chatType` - событие, которое бот выдает при совпадении шаблона. Например: "чат" или "шепот".
 * 'description' - необязательно, описывает, для чего предназначен шаблон.

#### bot.addChatPattern(name, pattern, chatPatternOptions)

** это псевдоним функции `bot.addChatPatternSet(name, [pattern], chatPatternOptions)`.

создайте событие, которое будет вызываться каждый раз, когда шаблон будет сопоставлен с сообщением,
событие будет называться `"chat:name"`, при этом name будет переданным именем
* `name` - имя, используемое для прослушивания события
* `pattern` - регулярное выражение для сопоставления с полученными сообщениями
* `chatPatternOptions` - объект
  * `repeat` - по умолчанию true, нужно ли прослушивать это событие после первого совпадения.
  * `parse` - вместо возвращения фактического сообщения, которое было сопоставлено, возвращает группы захвата из регулярного выражения
  * `deprecated` - (**нестабильный**) используется bot.chatAddPattern для сохранения совместимости, скорее всего будет удален

возвращает число, которое можно использовать с bot.removeChatPattern() для удаления только этого шаблона

#### bot.addChatPatternSet(name, patterns, chatPatternOptions)

создать событие, которое будет вызываться каждый раз, когда все шаблоны будут сопоставлены с сообщениями,
событие будет называться `"chat:name"`, при этом name будет переданным именем
* `name` - имя, используемое для прослушивания события
* `patterns` - массив регулярных выражений для сопоставления с полученными сообщениями
* ``chatPatternOptions`` - объект
  * `repeat` - по умолчанию true, нужно ли прослушивать это событие после первого совпадения.
  * `parse` - вместо возврата фактического сообщения, которое было сопоставлено, возвращает группы захвата из регулярного выражения.

возвращает число, которое можно использовать с bot.removeChatPattern() для удаления только этого набора шаблонов

#### bot.removeChatPattern(name)

удаляет паттерн(ы) чата
* `имя` : строка или число

если имя - строка, все шаблоны с таким именем будут удалены
если имя - число, то будет удален только этот шаблон.

#### bot.awaitMessage(...args)

обещание, которое разрешается, когда разрешается одно из сообщений, переданных в качестве аргумента

Пример:
```js
async function wait () {
  await bot.awaitMessage('<flatbot> hello world') // разрешает "hello world" в чате с помощью flatbot
  await bot.awaitMessage(['<flatbot> hello', '<flatbot> world']) // решает вопрос о "привет" или "мир" в чате с помощью flatbot
  await bot.awaitMessage(['<flatbot> hello', '<flatbot> world'], ['<flatbot> im', '<flatbot> batman']) // разрешает "hello" или "world" или "im" или "batman" в чате flatbot
  await bot.awaitMessage('<flatbot> hello', '<flatbot> world') //  разрешает "hello" или "world" в чате с помощью flatbot
  await bot.awaitMessage(/<flatbot> (.+)/) // разрешается при первом сообщении, соответствующем regex
}
```
#### bot.setSettings(options)

См. свойство `bot.settings`.

#### bot.loadPlugin(plugin)

Инжектирует плагин. Ничего не делает, если плагин уже загружен.

 * `plugin` - функция

 ```js
function somePlugin (bot, options) {
  function someFunction () {
    bot.chat('Yay!')
  }
  bot.myPlugin = {} // Хорошая практика для пространства имен API плагина
  bot.myPlugin.someFunction = someFunction
}
const bot = mineflayer.createBot({})
bot.loadPlugin(somePlugin)
bot.once('login', function () {
  bot.myPlugin.someFunction() // Ура!
})
```

#### bot.loadPlugins(plugins)

Инжектирует плагины, см. `bot.loadPlugin`.
 * `plugins` - массив функций

#### bot.hasPlugin(plugin)

Проверяет, загружен ли заданный плагин (или запланирован к загрузке) на этом боте.

#### bot.sleep(bedBlock)

Эта функция возвращает `Promise`, аргументом которой является `void`.

Спать в кровати. `bedBlock` должен быть экземпляром `Block`, который является кроватью.

#### bot.isABed(bedBlock)

Возвращает true, если `bedBlock` является кроватью.

#### bot.wake()

Эта функция возвращает `Promise`, аргументом которой является `void`.

Встать с кровати.

#### bot.setControlState(control, state)

Это основной метод, управляющий движениями бота. Он работает аналогично нажатию клавиш в minecraft.
Например, движение вперед с состоянием true заставит бота двигаться вперед. Forward с состоянием false заставит бота прекратить движение вперед.
Вы можете использовать bot.lookAt в сочетании с этим для управления движением. Пример jumper.js показывает, как это использовать.

 * `control` - одно из ['вперед', 'назад', 'влево', 'вправо', 'прыжок', 'спринт', 'красться'].
 * `state` - `true` или `false`.

#### bot.getControlState(control)

Возвращает true, если состояние управления переключено.

* `control` - одно из ['вперед', 'назад', 'влево', 'вправо', 'прыжок', 'спринт', 'красться'].

#### bot.clearControlStates()

Устанавливает все элементы управления в выключенное состояние.

#### bot.getExplosionDamages(entity, position, radius, [rawDamages])

Возвращает, сколько повреждений будет нанесено объекту в радиусе вокруг позиции взрыва.
Возвращает `null`, если у объекта нет брони и rawDamages не установлено в true, так как функция не может рассчитать урон с учетом брони, если брони нет.

* ``entity`` - экземпляр сущности
* `position` - экземпляр [Vec3](https://github.com/andrewrk/node-vec3)
* `radius` - радиус взрыва в виде числа.
* `rawDamages` - опционально, если true, то при расчете игнорируется броня

#### bot.lookAt(point, [force])

Эта функция возвращает `Promise`, с `void` в качестве аргумента, когда вы смотрите на `point`.

 * `point`.[Vec3](https://github.com/andrewrk/node-vec3)instance - наклоняет голову так, что она оказывается прямо напротив этой точки.
 * `force` - См. `force` в `bot.look`.

#### bot.look(yaw, pitch, [force])

Эта функция возвращает `Promise`, с `void` в качестве аргумента, вызывается, когда вы смотрите на `yaw` и `pitch`.

Устанавливает направление, в котором находится ваша голова.

 * `yaw` - количество радианов для вращения вокруг вертикальной оси, начиная с
   от направления на восток. Против часовой стрелки.
 * ``pitch`` - Количество радиан для направления вверх или вниз. 0 означает прямо вперед.
   pi / 2 означает прямо вверх. -pi / 2 означает прямо вниз.
 * `force` - Если присутствует и имеет значение true, пропускает плавный переход на стороне сервера.
   Укажите значение true, если вам нужно, чтобы сервер точно знал, куда вы
   например, при падении предметов или стрельбе стрелами. Это не
   требуется для вычислений на стороне клиента, таких как направление ходьбы.

#### bot.updateSign(block, text)

Изменяет текст на знаке.

#### bot.equip(item, destination)

Эта функция возвращает `Promise` с `void` в качестве аргумента, когда вы успешно экипировали предмет или когда вы узнали, что вам не удалось экипировать предмет.

Экипирует предмет из вашего инвентаря. Если аргумент `item` имеет значение Instance `Item`, то equip экипирует этот конкретный предмет из своего слота окна. Если аргумент `item` имеет тип `number`, equip экипирует первый найденный предмет с этим id, поиск которого осуществляется по возрастанию id слота (Hotbar перебирается последним. Слоты брони, ремесла, результата ремесла и слоты вне руки исключены).

 * `item` - экземпляр `Item` или `number` для id предмета. См. `window.items()`.
 * ``назначение''
   - ``рука`` - `null` псевдонимы для данного элемента
   - ``голова``
   - ``туловище``
   - ``ноги``
   - ``ноги``
   - ``от руки`` - когда доступно

#### bot.unequip(destination)

Эта функция возвращает `Promise` с `void` в качестве аргумента после завершения.

Удалить предмет снаряжения.

#### bot.tossStack(item)

Эта функция возвращает `Promise`, аргументом которой является `void`, когда бросок выполнен.

 * `item` - стопка предметов, которые вы хотите выбросить.
   Правда, вы не смогли завершить бросок.

#### bot.toss(itemType, metadata, count)

Эта функция возвращает `Promise`, аргументом которого является `void`, после завершения бросания.

 * `itemType` - числовой идентификатор предмета, который вы хотите выбросить
 * `metadata` - метаданные предмета, который вы хотите выбросить. Используйте `null`
   для соответствия любым метаданным
 * `count` - сколько элементов вы хотите выбросить. `null` - это псевдоним для `1`.

#### bot.dig(block, [forceLook = true], [digFace])

Эта функция возвращает `Promise`, с `void` в качестве аргумента, когда блок разрушен или вас прервали.

Начните копать в `блоке` с текущим экипированным предметом.
См. также события "diggingCompleted" и "diggingAborted".

Обратите внимание, что, начав копать в блоке, вы не можете
копать другие блоки до тех пор, пока блок не будет разрушен, или пока вы не вызовете команду
`bot.stopDigging()`.

 * `block` - блок, в котором нужно начать копать.
 * `forceLook` - (опционально) если true, посмотреть на блок и начать копать мгновенно. Если false, бот будет медленно поворачиваться к блоку для добычи. Кроме того, этому параметру можно присвоить значение 'ignore', чтобы бот вообще не двигал головой. Также этому параметру можно присвоить значение 'raycast', чтобы направить луч от головы бота туда, куда бот смотрит.
 * `digFace` - (опционально) По умолчанию 'auto' смотрит на центр блока и минирует верхнюю грань. Также может быть вектором vec3
 лица, на которое бот должен смотреть при копании блока. Например: ``vec3(0, 1, 0)`` при копании вершины. Также может быть 'raycast' raycast проверяет, есть ли лицо, видимое ботом, и минирует это лицо. Полезно для серверов с анти-читом.

Если вы вызовете bot.dig дважды до того, как закончится первый dig, вы получите фатальную ошибку 'diggingAborted'.

#### bot.stopDigging()

#### bot.digTime(block)

Сообщает, сколько времени потребуется для выкапывания блока, в миллисекундах.

#### bot.acceptResourcePack()

Принимает пакет ресурсов.

#### bot.denyResourcePack()

Отклоняет пакет ресурсов.

#### bot.placeBlock(referenceBlock, faceVector)

Эта функция возвращает `Promise`, с `void` в качестве аргумента, когда сервер подтверждает, что блок действительно был размещен.

 * `referenceBlock` - блок, рядом с которым вы хотите разместить новый блок
 * `faceVector` - одно из шести кардинальных направлений, например `new Vec3(0, 1, 0)` для верхней грани,
   указывающий, к какой грани `справочного блока` поместить блок.

Новый блок будет размещен по адресу `referenceBlock.position.plus(faceVector)`.

#### bot.placeEntity(referenceBlock, faceVector)

Эта функция возвращает `Promise`, аргументом которого является `Entity`.

 * `referenceBlock` - блок, рядом с которым вы хотите разместить сущность
 * `faceVector` - одно из шести кардинальных направлений, например `new Vec3(0, 1, 0)` для верхней грани,
   указывающее, к какой грани `справочного блока` поместить блок.

Новый блок будет размещен по адресу `referenceBlock.position.plus(faceVector)`.

#### bot.activateBlock(block, direction?: Vec3, cursorPos?: Vec3)

Эта функция возвращает `Promise`, с `void` в качестве его a
```js
{
  mode: 2,
  trackOutput: true,
  conditional: false,
  alwaysActive: true
}
```
options.mode может иметь 3 значения: 0 (ПОСЛЕДОВАТЕЛЬНОСТЬ), 1 (АВТО), 2 (РЕДСТОУН).
Все атрибуты опций по умолчанию имеют значение false, кроме mode, которое равно 2 (чтобы повторить командный блок по умолчанию в Minecraft).

#### bot.supportFeature(name)

Этот параметр можно использовать для проверки наличия определенной функции в текущей версии Minecraft. Обычно это требуется только для работы с функциями, специфичными для конкретной версии.

Список доступных функций можно найти в файле [./lib/features.json](https://github.com/PrismarineJS/mineflayer/blob/master/lib/features.json).

#### bot.waitForTicks(ticks)

Это функция, основанная на обещании, которая ждет, пока пройдет заданное количество внутриигровых тиков, прежде чем продолжить выполнение. Это полезно для быстрых таймеров, которые должны работать с определенным временем, независимо от заданной скорости тиков физики бота. Это похоже на стандартную функцию Javascript setTimeout, но выполняется специально для физического таймера бота.

### Методы инвентаризации нижнего уровня

Это методы нижнего уровня для инвентаря, они могут быть иногда полезны, но если есть возможность, предпочитайте методы инвентаря, представленные выше.

#### bot.clickWindow(slot, mouseButton, mode)

Эта функция возвращает `Promise`, аргументом которой является `void`.
  
Единственным допустимым вариантом режима на данный момент является 0. Щелчок Shift или перетаскивание мыши не реализованы.

Щелкните по текущему окну. Подробности см. на https://wiki.vg/Protocol#Click_Container.

Предпочтительнее использовать bot.simpleClick.*

#### bot.putSelectedItemRange(start, end, window, slot)

Эта функция возвращает `Promise`, аргументом которой является `void`.

Поместит элемент в `slot` в указанном диапазоне.

#### bot.putAway(slot)

Эта функция возвращает `Promise`, аргументом которой является `void`.

Поместите предмет в `slot` в инвентарь.

#### bot.closeWindow(window)

Закрыть `окно`.

#### bot.transfer(options)

Эта функция возвращает `Promise`, аргументом которой является `void`.

Передача какого-либо элемента из одного диапазона в другой. `options` - это объект, содержащий :

 * `window` : необязательно. окно, в которое будет перемещен элемент.
 * `itemType` : тип перемещаемого элемента
 * `metadata` : необязательно. метаданные перемещаемых элементов
 * `sourceStart` и `sourceEnd` : диапазон источников. `sourceEnd` необязателен и по умолчанию равен `sourceStart` + 1
 * `destStart` и `destEnd` : диапазон назначения. `destEnd` необязателен и по умолчанию будет равен `destStart` + 1
 * `count` : количество элементов для передачи. По умолчанию: `1`
 * `nbt` : nbt-данные элемента для передачи. По умолчанию: `nullish` (игнорирует nbt)

#### bot.openBlock(block, direction?: Vec3, cursorPos?: Vec3)

Открывает блок, например, сундук, возвращает обещание на открытие `окна`.

 * `block` - блок, который бот будет открывать.
 * `direction` Optional по умолчанию `new Vec3(0, 1, 0)` (вверх). Вектор, указывающий направление, в котором следует взаимодействовать с блоком контейнера. Ничего не делает, когда объект контейнера является целью.
 * `cursorPos` Необязательное значение по умолчанию `new Vec3(0.5, 0.5, 0.5)` (центр блока). Положение курсора при открытии экземпляра блока. Отправляется вместе с пакетом activate block. Ничего не делает, если объектом является контейнер.

#### bot.openEntity(entity)

Открывает сущность с инвентарем, например, жителя деревни, возвращает обещание об открытии `Window`.

 * `entity` - сущность, которую бот будет открывать.

#### bot.moveSlotItem(sourceSlot, destSlot)

Эта функция возвращает `Promise`, аргументом которой является `void`.

Перемещает элемент из `sourceSlot` в `destSlot` в текущем окне.

#### bot.updateHeldItem()

Обновить `bot.heldItem`.

#### bot.getEquipmentDestSlot(destination)

Получает идентификатор слота инвентарного оборудования для заданного имени места назначения оборудования.

Доступными пунктами назначения являются:
* голова
* торс
* ноги
* ступни
* рука
* рука

### bot.creative

Эта коллекция апов полезна в творческом режиме.
Обнаружение и смена игровых режимов здесь не реализованы,
но предполагается и часто требуется, чтобы бот находился в творческом режиме, чтобы эти функции работали.

#### bot.creative.setInventorySlot(slot, item)

Эта функция возвращает `Promise`, с `void` в качестве аргумента, который срабатывает, когда сервер устанавливает слот.

Дает боту указанный предмет в указанном слоте инвентаря.

 * ``слот`` - это координаты окна инвентаря (где 36 - первый слот квикбара и т.д.).
 * `item` - это [prismarine-item](https://github.com/PrismarineJS/prismarine-item) экземпляр, заданный с произвольными метаданными, nbtdata и т.д.
    Если `item` равно `null`, элемент в указанном слоте удаляется.
Если этот метод что-то изменит, вы можете получить уведомление через `bot.inventory.on("updateSlot")`.

#### bot.creative.clearSlot(slot)

Эта функция возвращает `Promise`, с `void` в качестве аргумента, который срабатывает, когда сервер очищает слот.

Функция устанавливает элемент в указанном слоте в null.

 * `slot` - это координаты окна инвентаря (где 36 - первый слот квикбара и т.д.).

#### bot.creative.clearInventory()

Эта функция возвращает `Promise`, с `void` в качестве аргумента, который срабатывает, когда сервер очищает слот.

#### bot.creative.flyTo(destination)

Эта функция возвращает `Promise`, с `void` в качестве аргумента, когда бот прибывает в пункт назначения.

Вызывает `startFlying()` и движется с постоянной скоростью через 3d пространство по прямой к месту назначения.
Пунктом назначения является `Vec3`, и часто координаты `x` и `z` заканчиваются на `.5`.
Эта операция не сработает, если на пути есть препятствие,
поэтому рекомендуется пролетать за один раз очень короткие расстояния.

Этот метод не пытается найти путь.
Ожидается, что реализация поиска пути будет использовать этот метод для перемещения <2 блоков за раз.

Чтобы возобновить нормальную физику, вызовите `stopFlying()`.

#### bot.creative.startFlying()

Устанавливает `bot.physics.gravity` в `0`.
Для возобновления нормальной физики вызовите `stopFlying()`.

Этот метод полезен, если вы хотите парить, копая землю под собой.
Нет необходимости вызывать эту функцию перед вызовом `flyTo()`.

Обратите внимание, что во время полета `bot.entity.velocity` не будет точной.

#### bot.creative.stopFlying()

Восстанавливает `bot.physics.gravity` до первоначального значения.