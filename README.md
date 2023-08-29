# Golang project structure starter kit
Данный материал является выражением точки зрения о структуре файлов в проектах на Golang.
Предложенная структура почти не отличается от официальных, но имеет отличия.

### Зачем ?
Уникальностью данной точки зрения по структуре проекта является ее обоснованность, а именно, 
ориентированность на приложения, которые являются образующими единицами проекта.
Почему это важно? Потому что это облегчает понимание того, как устроен проект вцелом.
При проектирования проекта можно учесть фактор того, что он будет масштабироваться, и следует
продумать его разделение на независимые единицы ( __Application__ ), а эти единицы будут
состоять из пакетов ( __Package__ ) и файла инициализации в виде app.go для каждого из приложений. При этом,
каждое приложение ( __Application__) может использовать общую библиотеку ( __Library__ ) проекта,
так как это пакеты ( __Package__ ), которые могут быть переиспользованы, и не составляют основную
ценность каждого ( __Application__ ). Примером такой библиотеки может быть собственная реализация
обертки над http протоколом, чтобы не использовать фреймворки. 

### Пример использования такой структуры проекта
Примером может быть бот телеграмма. Допустим, мы хотим создать бота, который будет запоминать
важные для нас события, то есть это некий календарь, который можно обвесить всякими дополнительынми
фишками, все ограничивается нашими идеями. 
#### Допустим, бота можно разделить на функционал:
* Менеджмент телеграмма (отправка принятие сообщений с помощью запросов)
* Обаботка сообщений (бизнес лоигка регистрации событий пользователя в бд)
* Мониторинг зарегистрированных событий (Если событие произошло, то отправить его пользователю)
#### Таким образом, можно поделить проект на два приложения:
* Работа с телеграммом и бизнес логикой
* Работа по мониторингу событий
#### Зачем ? Что мы получим ?:
* Меньшую связность проекта
* Отказоустойчивость
Каждое приложение может быть запаковано в разные контейнеры. Если упадет приложений мониторинга
событий, мы все еще сможем регитсрировать новые события, бот продолжит работать.
#### Как они будут взаимодействовать ?
- Можно сделать небольшую апишку для каждого.

## Терминология
Для больше погружения в контекст проблемы, далее будут приведены термины, которые важно понимать в
контексте построения структуры проекта:
* __Applicatoin__ - исполняемая независимая единица проекта, имеющая свой исходный код и точку
входа.
* __Package__ - элемент проекта, в котором содержится код, состовляющий основную ценность приложения.
Это разные пакеты директории app. Далее это будет видно.
* __Library__ - Переиспользуемые пакеты для всех приложений, будут располагаться в internal/pkg.

## Структурирование проекта
Так как данный материал не нацелен ознакомить читателя со всеми папками, которые могут быть в проекте, 
как это было сделано в https://github.com/golang-standards/project-layout , то будут отражены только
важные папки:
* __cmd__ - содержит точки входа во все Application, которые были разработаны в рамках проекта.
* __internal__ - содержит директории pkg и app, которые нужны для расположения кода библиотек и 
исходных кодов всех приложений.
* __config__ - содержит файлы конфигурации для приложений, такие как json, yaml, xml...
* __pkg__ - содержит пакеты __Packages__ , которые подразумевают использование другими модулями, может быть это
ваши сторонние проекты. (Маловероятно, что вы захотите чем то делиться)
* __internal/app__ - Содержит исходный код всех приложений. Для примера:
```
  app/AppOne
  app/AppTwo
```
А также в самих приложения будет файлы инициализации приложений, по типу:
```
  app/AppOne/app.go
  app/AppTwo/app.go
```
* __internal/pkg__ - пакеты библиотеки, используемые между всеми приложениями.
Как и было сказано, список папок не является исчерпывающим, но является важным.

Далее речь пойдет о том, как понять, насколько детально должна быть разработана структура проекта,
так как каждый проект имеет свои нужды и объем. Таким образом, можно сказать, что структуры
проектов делятся на два типа, это __маленькие проекты__ и __большие проекты__. В репозитории
имеется две папки, которые отражают информацию об актуальности каждого типа проектов, зайдя в эти
папки, вы можете ознакомиться с данной информацией.

![image](https://github.com/wanna-beat-by-bit/golang-project-structure-starter-kit/assets/71206074/08b9b8b5-d587-43dd-823f-d5e77eaf3216)




