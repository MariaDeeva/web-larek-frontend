# Проектная работа "Веб-ларек"
Структура проекта:
- src/ — исходные файлы проекта
- src/components/ — папка с JS компонентами
- src/components/base/ — папка с базовым кодом

Важные файлы:
- src/pages/index.html — HTML-файл главной страницы
- src/types/index.ts — файл с типами
- src/index.ts — точка входа приложения
- src/styles/styles.scss — корневой файл стилей
- src/utils/constants.ts — файл с константами
- src/utils/utils.ts — файл с утилитами

## Установка и запуск
Для установки и запуска проекта необходимо выполнить команды

```
npm install
npm run start
```

или

```
yarn
yarn start
```
## Сборка

```
npm run build
```

или

```
yarn build
```

## Архитектура
В приложение взаимодействие осуществляется с помощью модели событий. Модели инициируют события, а слушатели событий в 
основном коде осуществляют передачу данных компонентам отображения. Они также осуществляют вычисления между компонентами и изменяют значения в моделях.

## Классы базового кода:

### 1. Базовый Класс `EventEmitter<T>`
Данный класс представляет собой простую реализацию паттерна наблюдатель. Он позволяет установить обработчики 
на события, снять обработчики с событий, инициировать события и выполнять другие операции связанные с событиями.
Он содержит ряд методов:
1. Метод on устанавливает обработчик на указанное событие. Принимает имя события и колбэк-функцию, которая будет 
	вызываться при наступлении этого события.
2. Метод off снимает указанный обработчик с указанного события. Приинимает имя события и колбэк-функцию, которую 
	нужно удалить.
3. Метод emit инициирует событие с указанными данными. Принимает имя события и опциональные данные, 
	которые передаются обработчикам этого события.
4. Метод onAll устанавливает обработчик на все события. Принимает колбэк-функцию, которая будет 
	вызываться при наступлении любого события.
5. Метод offAll удаляет обработчики всех событий.
6. Метод trigger создает триггер-функцию, которая генерирует указанное событие при вызове. 
	Принимает имя события и контекстные данные, которые добавляются к переданным при вызове данных события.

### 2. Базовый Класс `Component<T>`
Данный класс является абстрактным и представляет базовый класс компонента. 
Конструктор: Принимает контейнерный элемент (HTMLElement), в котором будет размещаться компонент.
Он содержит ряд методов, которые помогают в работе с DOM элементами::
1. Метод toggleClass: Позволяет переключать класс у элемента. Принимает элемент (HTMLElement), имя класса 
	(className) и необязательный аргумент force (force?: boolean), определяющий, нужно ли явно добавить или 
	удалить класс. Если указан, то класс будет добавлен, если он отсутствует, и наоборот.
2. Метод setText: Устанавливает текстовое содержимое элемента. Принимает элемент (HTMLElement) и значение (value), 
	которые будут установлены в виде текста. Если элемент существует, то его текстовое содержимое будет заменено
	на указанное значение.
3. Метод setDisabled: Изменяет состояние блокировки элемента. Принимает элемент (HTMLElement) и состояние (state), 
	где true означает блокировку элемента, а false - разблокировку. Если состояние блокировки установлено в true, 
	то элементу будет добавлен атрибут "disabled". В противном случае атрибут будет удален.
4. Метод setHidden: Скрывает элемент, устанавливая его свойство display в значение "none". 
	Принимает элемент (HTMLElement), который будет скрыт.
5. Метод setVisible: Показывает скрытый ранее элемент, удаляя его стиль display. 
	Принимает элемент (HTMLElement), который будет отображен.
6. Метод setImage: Устанавливает изображение элементу типа HTMLImageElement. Принимает элемент (HTMLImageElement), 
	ссылку на изображение (src) и необязательный аргумент alt (alt?), который задает альтернативный текст для изображения.
7. Метод render: Возвращает корневой DOM-элемент компонента, который был передан в конструкторе. 
	Опционально принимает объект данных (data), который может быть использован для обновления состояния 
	компонента до отображения. Объект данных сливается с текущим экземпляром класса с помощью Object.assign().
	Если объект данных не передан, то метод просто возвращает корневой элемент без изменений.

### 3.Базовый Класс `Model<T>`
Данный класс является абстрактным и представляет базовую модель.
Конструктор: Принимает частичные данные (data: Partial<t>) и события (events: IEvents) в качестве аргументов. 
	Метод Object.assign используется для копирования свойств из объекта данных в экземпляр модели.
Он содержит метод и функцию:
1. Метод emitChanges отправляет событие об изменении модели. Принимает строку-идентификатор события (event) и необязательный 
	объект данных (payload), который может быть передан вместе с событием. Если payload не указан, будет создан пустой объект. 
	Метод использует объект событий (events) для вызова метода emit и передачи события и данных.
2. Функция isModel которая используется как гарда для проверки модели


### 4. Базовый Класс `Api`
Данный класс является оберткой над API запросами. Класс Api также имеет свойства baseUrl и options, 
	которые хранят базовый URL и опции запроса.
Конструктор: Принимает базовый URL API и опции для fetch (baseUrl: string, options: RequestInit = {}) и  
	Если options не указаны, используется пустой объект.
Он содержит методы:	
1. ЗащизеМетод handleResponse защищенный метод, который обрабатывает ответы от сервера. Если ответ успешен метод возвращает разобранный JSON-объект с 
	данными ответа, если ответ содержит ошибку, метод выбрасывает исключение с текстом ошибки.
2. Метод get для выполнения GET-запросов. Он выполняет запрос по указанному URI и возвращает разобранный JSON-объект с данными ответа.
3. Метод post для выполнения POST-запросов. Он выполняет запрос по указанному URI с переданными данными (в виде JSON-объекта) и возвращает разобранный JSON-объект 
	с данными ответа.

## Классы модели данных:

### 1. Класс `Product`
Данный класс является описывает модель продукта и наследует функциональность от класса Model, использует интерфейс IProduct для указания типа данных, 
которые должны соответствовать модели товара. <u>(Находится в src\components\AppState.ts)</u>
Он содержит свойство: 
1. Свойство id: string - строковое значение, представляющее идентификатор продукта.
2. Свойство description: string - строковое значение, содержащее описание продукта.
3. Свойство title: string - строковое значение, содержащее заголовок продукта.
4. Свойство image: string - строковое значение, представляющее URL-адрес изображения продукта.
5. Свойство price: number | null - числовое значение, представляющее цену продукта. Может быть либо числом, либо null, если цена неизвестна или отсутствует.
6. Свойство category: string - строковое значение, представляющее категорию, к которой принадлежит продукт.


### 2. Класс `IAppState`
Данный класс является моделью приложения: хранит состояние приложения, включая каталог продуктов, корзину, заказ, данные для предварительного просмотра, ошибки формы. 
 и наследует функциональность от класса Model, использует интерфейс IAppState.
Он содержит ряд методов:
1. Конструктор принимает начальное состояние IAppState и инициализирует свойства класса catalog, basket, order, preview и formErrors соответствующими значениями.
2. Метод clearOrder используется для очистки данных о заказе.
3. Метод setCatalog используется для установки каталога продуктов.
4. Метод setPreview используется для установки предварительного просмотра продукта.
5. Метод handleBasketAction используется для обработки добавления и удаления товара в корзине.
6. Метод removeBasket используется для удаления всех товаров из корзины.
7. Метод refreshBasket используется для обновления данных о корзине.
8. Метод setOrderField используется для установки значений полей заказа.
9. Метод setContactField используется для установки значений полей контактных данных.
10. Метод validateOrder используется для валидации данных заказа.
11. Метод validateContact используется для валидации данных контактных данных.

## Классы компонентов представления

### 1. Класс `Modal<T>`
Данный класс является представлением модального окно веб-интерфейса. 
Он наследуется от базового класса Component, которому передается интерфейс IModalData в качестве типа данных для его обобщенного параметра T.
Конструктор класса инициализирует различные свойства и привязывает обработчики событий к элементам модального окна. В конструкторе используется функция ensureElement, 
	которая обеспечивает получение ссылок на элементы модального окна, используя селекторы .modal__close и .modal__content.
Он содержит ряд методов:
1. Метод content является сеттером и позволяет устанавливать содержимое модального окна, путем замены дочерних элементов новым значением.
2. Метод open отвечает за открытие модального окна, с помощью событий (через events.emit) уведомляют остальные компоненты о статусе модального окна.
3. Метод close отвечают за закрытие модального окна, с помощью событий (через events.emit) уведомляют остальные компоненты о статусе модального окна.
4. Метод render вызывает родительский метод render для отрисовки компонента на странице и затем открывает модальное окно с помощью метода open, возвращает 
	ссылку на контейнер модального окна..

### 2. Класс `Form<T>`
Данный класс используется для создания формы и управления ее состоянием. Он наследуется от базового класса Component, который использует интерфейс IFormState 
в качестве типа данных для его обобщенного параметра T.
Конструктор класса принимает container и events в качестве параметров и выполняет инициализацию элементов _submit и _errors. Он также устанавливает
	обработчики событий для ввода данных и отправки формы. При изменении значения поля ввода вызывается метод onInputChange, который генерирует событие 
	изменения данного поля. При отправке формы вызывается метод submit.
Он содержит ряд методов, обработчиков и свойст:
1. Обработчик input следит за изменениями в полях ввода формы.
2. Обработчик submit предотвращает отправку формы по умолчанию.
3. Свойство valid устанавливает disabled для кнопки отправки формы.
4. Свойство errors устанавливает текст ошибки.
5. Метод render принимает объект состояния формы и отрисовывает его. 
  
### 3. Класс `Success`
Данный класс является представлением компонента успешного завершения заказа. Он наследуется от базового класса Component и 
принимает обобщенный параметр ISuccess, который представляет тип данных для его состояния.
Конструктор класса принимает container и actions в качестве параметров и выполняет инициализацию элементов _close и __description. 
	Устанавливает обработчик события onClick на кнопку закрытия компонента, если такой событийный обработчик предоставлен в actions.
Он содержит свойств:
1. Свойство description предоставляет доступ к содержимому элемента.
  
  
### 4. Класс `ContactForm`
Данный класс является предоставлением функциональности для работы с формой контактов. Он наследуется от базового класса Form и принимает обобщенный параметр IContactForm, 
который представляет тип данных для его состояния.
Конструктор класса ContactForm  контейнер формы (container), объект событий (events) в качестве параметров и вызывает конструктор класса Form, передавая ему container и events.
Он имеет следующие методы:
2. Методы phone, email используются для получения и установки значений полей телефона и электронной почты.

### 5. Класс `OrderForm` 
Данный класс является предоставлением функциональности для работы с формой заказа. Он наследуется от базового класса Form и принимает обобщенный параметр IContactForm, который представляет тип данных для его состояния.
Конструктор класса принимает контейнер формы (container), объект событий (events) и объект actions, содержащий действия, выполняемые при нажатии на кнопку. 
	Внутри конструктора вызывается конструктор родительского класса Form, а также устанавливаются обработчики событий клика на кнопки оплаты.
Он имеет следующие методы:
1. Метод addButtonClickHandler добавляет обработчик события клика на кнопки оплаты.
2. Метод toggleButtons используется для изменения визуального состояния кнопок оплаты.
3. Метод address используются для получения и установки значений адреса в форме заказа.

### 6. Класс `BasketForm`
Данный класс используется для создания и управления формой корзины, включающей список товаров, общую стоимость и кнопку оформления заказа. Он наследуется от базового класса Component и принимает обобщенный параметр IBasketView, который представляет тип данных для его состояния.
Конструктор класса принимает контейнер формы (container) и объект событий (events), внутри вызывается конструктор родительского класса 
	Component, а также устанавливаются обработчики событий клика на кнопку оформления заказа.
Он содержит ряд методов, обработчиков и свойст:
1. Метод toggleButton ипользуется для включения и отключения кнопку оформления заказа.
2. Свойство items используется для установки списка элементов корзины.
3. Свойство total  используется для установки общей сумму заказа.

### 7. Класс `Cards`
Данный класс является наследником класса Component и предназначен для отображения карточками товаров. 
Конструктор класса принимает контейнер, в котором будет отображаться карточка, и действия (actions), 
	которые будут выполняться при клике на карточку или кнопку.
Он имеет следующие свойства и методы:
1. Метод disableButton блокирует кнопку, если цена = null.
2. Свойство id устанавливает и получает идентификатор карточки.
3. Свойство buttonText устанавливает текст кнопки.
4. Свойства title устанавливает и получает заголовок карточки.
5. Свойства price устанавливает и получает цену карточки.

### 8. Класс `Page`
Данный класс является предоставлением компонентов страницы, который отвечает за отображение и управление различными элементами страницы. Он наследуется от 
базового класса Component и принимает обобщенный параметр IPage, который представляет тип данных для его состояния. 
Конструктор класса принимает контейнер, в котором будет отображаться страница, и объект "events", содержащий события, устанавливаются элементы страницы и 
	добавляется обработчик события клика по корзине.
Он имеет следующие свойства и методы:
1. Метод ensureElement инициализирует свойства с соответствующими HTML-элементами, полученными по классам.
2. Метод handleBasketClick защищенный предназначен для обработки события клика на элемент корзины.
3. Метод counter устанавливает значение счетчика товаров в корзине.
4. Метод gallery устанавливает элементы галереи товаров.
5. Метод locked устанавливает состояние блокировки страницы.

### 9. Класс `LarekApi`
Данный класс предоставляет методы для работы с API.  Он является наследником класса Api и реализует интерфейс ILarekApi.
Конструкторе класса принимаются параметры cdn(представляет собой ссылку на CDN), baseUrl(базовый URL для запросов к API) и options(опции для запроса).
Он имеет следующие методы:
1. Метод getProductList отправляет GET-запрос на /product и возвращает массив объектов IProduct. 
2. Метод getProductItem отправляет GET-запрос на /product/${id} и возвращает объект IProduct.
3. Метод orderProducts отправляет POST-запрос на /order с объектом заказа и возвращает объект IOrderResult - результат заказа.


## Основные типы данных 
Интерфейс общего методы события\
interface IEvents {\
    on<T extends object>(event: EventName, callback: (data: T) => void): void;\
    emit<T extends object>(event: string, data?: T): void;\
    trigger<T extends object>(event: string, context?: Partial<T>): (data: T) => void;\
  };

Тип данных ответа от сервера\
type ApiListResponse<Type> = {\
    total: number, \
    items: Type[] \
};

Тип методов запросов к серверу\
type ApiPostMethods = 'POST' | 'PUT' | 'DELETE';\

Интерфейс обработчика валидности формы\
interface IFormState {\
    valid: boolean;\
    errors: string[];\
};

Интерфейс модальных данных\
interface IModalData {\
    content: HTMLElement;\
};

Интерфейс состояния приложения\
interface IAppState {\
  catalog: IProduct[];\
  basket: IProduct[];\
  preview: string | null;\
  delivery: IOrderForm | null;\
  contact: IContactForm | null;\
  order: IOrder | null;\
};

Интерфейс продукта(товара)\
`interface IProduct {\
  id: string;\
  title: string;\
  price: number | null;\
  description: string;\
  category: string;\
  image: string;\
};`

Интерфейс формы заказов\
 `interface IOrderForm {\
  payment: string;\
  address: string;\
};`

Интерфейс формы контактов\
 `interface IContactForm {\
  email: string;\
  phone: string;\
};`

Интерфейс карточки товара\
interface ICards extends IProduct{\
  index?: string;\
  buttonTitle? : string;\
};