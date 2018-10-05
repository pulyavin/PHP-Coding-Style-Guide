# PHP Coding Style Guide

Ключевые слова «НЕОБХОДИМО»/«ДОЛЖНО» («MUST»), «НЕДОПУСТИМО»/«НЕ ДОЛЖНО» («MUST NOT»), «ТРЕБУЕТСЯ»
(«REQUIRED»), «НУЖНО» («SHALL»), «НЕ ПОЗВОЛЯЕТСЯ» («SHALL NOT»), «СЛЕДУЕТ»
(«SHOULD»), «НЕ СЛЕДУЕТ» («SHOULD NOT»), «РЕКОМЕНДУЕТСЯ» («RECOMMENDED»),
«ВОЗМОЖНО» («MAY») и «НЕОБЯЗАТЕЛЬНО» («OPTIONAL»)
в этом документе должны расцениваться так, как описано в [RFC 2119](http://rfc2.ru/2119.rfc)

## Содержание
- [Общие требования](#general)
- [Именование сущностей](#naming)
- [Основные требования к оформлению](#visual)
    - [Оформление файлов](#file)
    - [Оформление классов](#class)
    - [Оформление методов](#methods)
    - [Оформление управляющих структур](#structures)
    - [Работа с массивами](#arrays)
    - [Работа с исключениями](#exceptions)
- [Требования к документированию кода](#doc)


## Общие требования <a name="general"></a>
- Ключевые слова PHP ДОЛЖНЫ быть написаны в нижнем регистре

Неправильный вариант:
```php
Do {
} WHILE(true);
```

Правильный вариант:
```php
do {
} while(true);
```

- Константы PHP true, false и null ДОЛЖНЫ быть написаны в нижнем регистре

Неправильный вариант:
```php
$isBool = True;
$nullable = NULL;
```

Правильный вариант:
```php
$isBool = true;
$nullable = null;
```

- Отступы НЕОБХОДИМО оформлять в виде пробелов, а не табуляции. Один отступ — 4 пробела для PHP-кода и 2 символа для YAML-кода.
- Длину строки РЕКОМЕНДУЕТСЯ придерживать в 80 символов или менее. Более длинные строки СЛЕДУЕТ разбивать на несколько отдельных строк, длина каждой из которых не превышала бы 80 символов.
- В конце непустых строк НЕ ДОЛЖНО быть пробелов.
- Блоки кода НЕОБХОДИМО разделять пустыми строками для повышения удобочитаемости

Неправильный вариант:
```php
$counter = 0;
$length = 4;
for ($i = 0; $i < $length; $i++) {
    $counter++;
}
if ($counter > 0) {
    return true;
}
```

Правильный вариант:
```php
$counter = 0;
$length = 4;

for ($i = 0; $i < $length; $i++) {
    $counter++;
}

if ($counter > 0) {
    return true;
}
```

- НЕДОПУСТИМО использовать операторы `else`, `elseif`, `break` после условий `if` и `case` которые возвращают значение либо выбрасывают исключение

Неправильный вариант:
```php
$a = true;

if ($a == true) {
    throw new \Exception('Illegal data');
}
else {
    return false;
}
```

Правильный вариант:
```php
$a = true;

if ($a == true) {
    throw new \Exception('Illegal data');
}

return false;
```

- Точку с запятой НЕ СЛЕДУЕТ переносить на новую строку

Неправильный вариант:
```php
$this->setFormatter()
    ->setDelimeter()
;
```

Правильный вариант:
```php
$this->setFormatter()
    ->setDelimeter();
```

- Название класса НЕДОПУСТИМО задавать строкой, НЕОБХОДИМО использовать конструкцию `::class`

Неправильный вариант:
```php
$className = '\Name\Space\ClassName';
```

Правильный вариант:
```php
use \Name\Space\ClassName;

$className = ClassName::class;
```

- Для конкатенации строк НЕОБХОДИМО использовать функцию [sprintf](http://php.net/manual/en/function.sprintf.php)

Неправильный вариант:
```php
$type = 'error';
$message = 'Some problems occured';

$exception = 'Exception ('.$type.'): ' . $message;
```

Правильный вариант:
```php
$type = 'error';
$message = 'Some problems occured';

$exception = sprintf('Exception (%s): %s', $type, $message);
```

- НЕОБХОДИМО добавлять пробелы вокруг любого бинарного оператора (`==`, `&&`, ...)

Неправильный вариант:
```php
if ($a==1&&$b>3) {
    //
}
```

Правильный вариант:
```php
if ($a == 1 && $b > 3) {
    //
}
```

- Унарные операторы (`!`, `--`, ...) НЕ ДОЛЖНЫ быть отделены от переменной пробелом

Неправильный вариант:
```php
$a = true;
$b = ! $a;
```

Правильный вариант:
```php
$a = true;
$b = !$a;
```

## Именование сущностей <a name="naming"></a>
-  Для строковой переменной НУЖНО использовать существительное

Пример:
```php
$serverMessage = 'hello server';
$componentHtml = '<h1>Hello</h1>';
$login = 'username';
$password = 'qwerty';
```
- Имена всех сущностей (классов, методов, переменных) ДОЛЖНЫ быть на английском языке

Неправильный вариант:
```php
$moiTovar;
$cena;
$ssilka;
```

Правильный вариант:
```php
$myGood;
$price;
$link;
```

- Имена сущностей НЕДОПУСТИМО сокращать

Неправильный вариант:
```php
$usrCntr;
$err;
$msg;
```

Правильный вариант:
```php
$userCounter;
$error;
$message;
```

- В именах сущностей НЕ СЛЕДУЕТ употреблять аббревиатуры.
- Переменные, функции и методы ДОЛЖНЫ именоваться с использованием семантики `«camelCase»`

Неправильный вариант:
```php
function get_email_by_id(int $user_id)
{
    //
}
```

Правильный вариант:
```php
function getEmailById(int $userId)
{
    //
}
```

- В именах параметров и опциях (внутри массивов, конфигурациях и т.п.) ДОЛЖНА использоваться семантика `«snake_case»`

Пример (YAML-файл):
```yaml
- name: create folder
  path_file:
    folder_path: /var/www
    mode: 0775
    main_owner: deployer
    main_group: deployer
```

Пример (PHP-массив):
```php
$users = [
    [
        'id' => 1,
        'last_name' => 'John',
        'first_name' => 'Smith',
    ],
];
```

- В именах переменных РЕКОМЕНДУЕТСЯ использовать спецификаторы 

```
- количество (count)
- код чего-либо (code)
- размер (size, length)
- номер (number)
- индекс (index)
```

Пример:
```php
$keyCode = 15;
$numberFromEnd = 101;
$maxWindowSize = 900;
$minCharacterLength = 10;
$newMessageCount = 3;
```

- Имена булевых переменных ДОЛЖНЫ содержать в себе булевые спецификаторы (булевый спецификатор — это вопрос с ответом да или нет)

```
это (is)
содержит (has/contain)
может (can)
должен (should)
возможность (able)
```

```php
$isPopupOpen = true;
$hasUpperLetters = true;
$containObject = false;
$shouldUpdate = false;
$disabled = true;
```

- РЕКОМЕНДУЕТСЯ начинать имена методов с глаголов, например: `find`, `get`, `set` и т.д. Названия методов ДОЛЖНЫ отражать свое поведение. Имя метода не должно иметь более 1 глагола. Методы, содержащие более одного глагола часто являются code smell и признаком нарушения Single Responsibility Principle

Правильный вариант:
```php
public function getBooksOfTheSameAuthor(Book $book)
{
    //
}

public function sendEmailNotification()
{
    //
}
```

Неправильный вариант:
```php
public function activateUserAndSendWelcomeEmail(User $user)
{
    //
}
```

Вышеприведённый метод должен быть отрефакторен в 2 метода 2 разных классов: `$userRepository->activateUser(User $user)` и `$emailNotificationService->sendWelcomeEmail(User $user)`

- В именах парных сущностей РЕКОМЕНДУЕТСЯ использовать симметричные пары

Общие:
```
min/max
up/down
old/new
begin/end
first/last
next/previous
```

Для функций:
```
open/close
get/set
add/remove
create/destroy
start/stop
insert/delete
increment/decrement
show/hide
suspend/resume
```

- Имена методов, возвращающих логическое значение, РЕКОМЕНДУЕТСЯ начинать c глаголов: `is`, `has`, `in` и т.п.

Пример
```php
public function isActive(Book $book): bool
{
    //
}

public function inArray(int $itemId): bool
{
    //
}
```

- Массивы это существительные во множественном числе. Переменные, указывающие на массивы ДОЛЖНЫ иметь окончание на `-s` и `-es`

Неправильный вариант:
```php
$user = [
    [
        'name' => 'Peter',
    ],
    [
        'name' => 'John',
    ],
];
$letter = [
    'A',
    'B',
    'C',
];
$code = [
    21,
    37,
];
```

Правильный вариант:
```php
$users = [
    [
        'name' => 'Peter',
    ],
    [
        'name' => 'John',
    ],
];
$letters = [
    'A',
    'B',
    'C',
];
$codes = [
    21,
    37,
];
```

- Классы ДОЛЖНЫ именоваться с использованием `«CapitalizedCamelCase»` (каждое слово начинается с большой буквы, между словами нет разделителей)

Пример;
```php
class UserRepository
{
    //
}
```

- Объекты классов ДОЛЖНЫ именоваться с использованием `«concreteCamelCase»` (со строчной буквы)

Пример:
```php
$userRepository = new UserRepository();
```

- Имена методов ДОЛЖНЫ именоваться с использованием `«camelCase»` (первое слово пишется в нижнем регистре, далее каждое слово начинается с большой буквы, а между словами нет разделителей).

- НЕ СЛЕДУЕТ использовать числа в именах переменных, свойств и методов.
- РЕКОМЕНДУЕТСЯ придерживаться практики, что чем длинее название переменной, тем шире скоуп её видимости

Пример:
```php
class UserRepository
{
    /**
     * Returns some data
     *
     * @param int $id
     *
     * @return array
     */
    public function getData(int $id): array
    {
        $returned = []; // виден только в методе, описание в одно слово
        
        for ($i = 0; $i < $id; $i++) { // переменная $i временная и её скоуп ограничен только циклом
            $returned[] = $this->userService->get($i); // $this->userService необходим для всего класса, поэтому даём полное название, на, например $this->us
        }
        
        return $returned;    
    }
}
```


## Основные требования к оформлению <a name="visual"></a>

### Оформление файлов <a name="file"></a>
- Файлы НЕОБХОДИМО представлять только в кодировке UTF-8 без BOM-байта.
- В файлах НЕОБХОДИМО использовать исключительно теги `<?php` и `<?=`.
- Закрывающий тег `?>` ДОЛЖЕН отсутствовать в файлах, содержащих только PHP-код.
- После определения пространства имён (`namespace`) и после блока импорта пространств имён (`use`) ДОЛЖНА быть одна пустая строка

Пример:
```php
<?php

namespace App\Services;

use App\Repositories\UserRepository;
use App\Repositories\BookRepository;

class OrderService
{
    /**
     * @var UserRepository
     */
    private $userRepository;

    /**
     * @var BookRepository
     */
    private $bookRepository;

    /**
     * OrderServices constructor.
     *
     * @param UserRepository $userRepository
     * @param BookRepository $bookRepository
     */
    public function __construct(UserRepository $userRepository, BookRepository $bookRepository)
    {
        $this->userRepository = $userRepository;
        $this->bookRepository = $bookRepository;
    }
}
```
- В блоке `use` НЕ ДОЛЖНО быть классов, интерфейсов, трейтов, констант, которые не используются в текущем файле.
- В одном PHP-файле НЕ ПОЗВОЛЯЕТСЯ объявление более одного класса (трейта, интерфейса, абстрактного класса).
- Неиспользуемые переменные СЛЕДУЕТ убирать из кода.


### Оформление классов <a name="class"></a>

- Открывающая фигурная скобка `{` в определении класса ДОЛЖНА располагаться на новой строке, а закрывающая фигурная скобка ДОЛЖНА располагаться на следующей строке после тела класса (используется `Allman style braces`)

Неправильный вариант:
```php
class Order {
    //
}
```

Правильный вариант:
```php
class Order
{
    //
}
```

- Абстрактные классы ДОЛЖНЫ иметь префикс `Abstract`

Пример:
```php
class AbstractValidator
{
    //
}
```

- Имена интерфейсов ДОЛЖНЫ оканчиваться словом `Interface`

Пример:
```php
class ValidatorInterface
{
    //
}
```

- Имена трейтов ДОЛЖНЫ оканчиваться словом `Trait`

Пример:
```php
class ValidationTrait
{
    //
}
```

- Имена исключений ДОЛЖНЫ оканчиваться словом `Exception`

Пример:
```php
class ValidatorException
{
    //
}
```

- Константы классов ДОЛЖНЫ быть объявлены в верхнем регистре с использованием символа подчёркивания в качестве разделителя слов

Пример:
```php
class VendorLibrary
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

- Ключевые слова `extends` и `implements` ДОЛЖНЫ находиться на той же строке, на которой находится имя класса

Неправильный вариант:
```php
class Order
    extends
        AbstractOrder
    implements
        \Serializable
{
    //
}
```

Правильный вариант:
```php
class Order extends AbstractOrder implements \Serializable
{
    //
}
```

- Список реализуемых интерфейсов МОЖЕТ быть разделён на несколько строк, каждая из которых дополнена слева одним отступом (четырьмя пробелами). В таком случае первый элемент списка интерфейсов ДОЛЖЕН начинаться с новой строки, и в каждой строке ДОЛЖЕН быть указан только один интерфейс

Пример:
```php
class Order extends AbstractOrder implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    //
}
```

- Конструктор класса служит только для инициализации объекта и НЕ ДОЛЖЕН содержать бизнес логику

Неправильный вариант:
```php
class OrderDto
{
    /**
     * Id of order
     * 
     * @var integer
     */
    private $id;

    /**
     * Title of order
     *
     * @var string
     */
    private $title;

    /**
     * OrderDto constructor.
     *
     * @param int $id
     * @param string $title
     */
    public function __construct(int $id, string $title)
    {
        if (empty($title)) {
            throw new \InvalidArgumentException('Title can not be empty');
        }
        
        $this->id = $id;
        $this->title = str_replace($title, ' ', '');
    }
}
```

Правильный вариант:
```php
class OrderDto
{
    /**
     * Id of order
     * 
     * @var integer
     */
    private $id;

    /**
     * Title of order
     *
     * @var string
     */
    private $title;

    /**
     * OrderDto constructor.
     *
     * @param int $id
     * @param string $title
     */
    public function __construct(int $id, string $title)
    {
        $this->id = $id;
        $this->title = $title;
    }
}
```

- Внедрение зависимостей ДОЛЖНО осуществляться через конструктор класса. В случае, если зависимость контекстно используется только в 1 методе, то использовать `method attribute injection`

Пример:
```php
class PaymentService
{
    public function __construct(
        OrderRepository $orderRepository,
        ResponseBuilder $responseBuilder
    )
    {
        //
    }
    
    public function pay(int $orderId, User $user)
    {
        //
    }
}
```

- В конструкторе класса НЕ РЕКОМЕНДУЕТСЯ использовать более 5 зависимостей

Неправильный вариант:
```php
class Order
{
    public function __construct(
        UserRepository $userRepository,
        OrderRepository $orderRepository,
        BookRepository $bookRepository,
        AddressRepository $addressRepository,
        BookTransformer $bookTransformer,
        ResponseBuilder $responseBuilder
    )
    {
        //
    }
}
```

Класс с таким количеством зависимостей, скорее всего нарушает SRP. В этом случае, стоит часть логики и зависимостей вынести в отдельный класс и уже этот класс инжектить в текущий.

- Свойства класса ДОЛЖНЫ объявляться раньше методов.
- Список свойств класса, указывающих на зависимости этого класса (dependencies) ДОЛЖЕН идти выше, чем список свойств самого класса (properties)

Неправильный вариант:
```php
class Order
{
    /**
     * @var BookRepository $bookRepository
     */
    protected $bookRepository;

    /**
     * Count of books
     *
     * @var integer $attemptCount
     */
    protected $count;

    /**
     * @var UserRepository $userRepository
     */
    protected $userRepository;

    /**
     * Price of book
     *
     * @var float $price
     */
    protected $price;
}
```

Правильный вариант:
```php
class Order
{
    /**
     * @var BookRepository $bookRepository
     */
    protected $bookRepository;
    
    /**
     * @var UserRepository $userRepository
     */
    protected $userRepository;

    /**
     * Count of books
     *
     * @var integer $attemptCount
     */
    protected $count;

    /**
     * Price of book
     *
     * @var float $price
     */
    protected $price;
}
```

- В классе публичные (`public`) методы ДОЛЖНЫ объявляться первыми, защищенные (`protected`) вторыми и приватные (`private`) третьими. Исключением из правила являются конструкторы и методы `setUp` и `tearDown`, которые должны всегда объявляться первыми, в целях улучшения читаемости кода.
- НЕДОПУСТИМО дублировать название объекта в названии свойства или метода

Неправильный вариант:
```php
class User
{
    /**
     * Name of user
     *
     * @var string
     */
    private $userName;

    /**
     * Name getter
     *
     * @return string
     */
    public function getUserName(): string
    {
        return $this->userName;
    }
}
```

Правильный вариант:
```php
class User
{
    /**
     * Name of user
     *
     * @var string
     */
    private $name;

    /**
     * Name getter
     *
     * @return string
     */
    public function getName(): string
    {
        return $this->name;
    }
}
```

- НЕДОПУСТИМО использование публичных свойств классов. Для работы со свойствами класса вы ДОЛЖНЫ использовать геттеры и сеттеры

Неправильный вариант:
```php
class Order
{
    /**
     * @var integer
     */
    public $id;
}
```

Правильный вариант:
```php
class Order
{
    /**
     * @var integer
     */
    private $id;

    /**
     * Order id setter
     *
     * @param int $id
     *
     * @return Order
     */
    public function setId(int $id): Order
    {
        $this->id = $id;

        return $this;
    }

    /**
     * Order id getter
     *
     * @return int
     */
    public function getId(): int
    {
        return $this->id;
    }
}
```

- В классах НЕ РЕКОМЕНДУЕТСЯ использовать магические методы.
- Методы ДОЛЖНЫ быть отделены друг от друга и от полей классов 1 пустой строкой.


### Оформление методов <a name="methods"></a>
- После имени метода НЕ ДОЛЖНО быть пробела. Открывающая фигурная скобка `{` ДОЛЖНА находиться на отдельной строке, а закрывающая фигурная скобка `}` ДОЛЖНА находиться на следующей за телом метода строке. НЕ ДОЛЖНО быть пробелов после открывающей `(` и перед закрывающей круглыми скобками `)` в определении метода. В списке аргументов НЕ ДОЛЖНО быть пробела перед запятыми, но ДОЛЖЕН быть пробел после каждой запятой

Неправильный вариант:
```php
class User
{
    /**
     * Name of user
     *
     * @var string
     */
    private $name;

    /**
     * Sets name of user
     * 
     * @param string $name
     *
     * @return User
     */
    public function setName ( string $name ): User {
        $this->name = $name;

        return $this;
    }
}
```

Правильный вариант:
```php
class User
{
    /**
     * Name of user
     *
     * @var string
     */
    private $name;

    /**
     * Sets name of user
     * 
     * @param string $name
     *
     * @return User
     */
    public function setName(string $name): User
    {
        $this->name = $name;

        return $this;
    }
}
```

- Аргументы со значениями по умолчанию ДОЛЖНЫ располагаться в конце списка (после аргументов без значений по умолчанию)

Пример:
```php
namespace Vendor\Package;
 
class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        //
    }
}
```

- Список аргументов МОЖЕТ быть разделён на несколько строк. В этом случае первый элемент списка аргументов ДОЛЖЕН начинаться с новой строки, и в каждой строке ДОЛЖЕН быть указан только один аргумент, дополненный слева одним отступом (четырьмя пробелами). Открывающая круглая скобка `(` остаётся на строке с именем метода. Закрывающая круглая скобка `)` и открывающая фигурная скобка `{` ДОЛЖНЫ располагаться вместе на своей отдельной строке, а между ними должен быть один пробел

Пример:
```php
namespace Vendor\Package;
 
class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // тело метода
    }
}
```

- Список аргументов метода и функции ДОЛЖЕН придерживаться следующей последовательности: сначала идут аргументы в виде скалярных примитивов, потом массивы и уже потом зависимости в виде классов (интерфейсов)

Неправильный вариант:
```php
class PaymentOrder
{
    /**
     * //
     */
    public function __construct(
        array $orderItems,
        PaymentService $paymentService,
        int $oderId,
        int $userId,
        OrderRepository $orderRepository
    ) {
        //
    }
}
```

Правильный вариант:
```php
class PaymentOrder
{
    /**
     * //
     */
    public function __construct(
        int $oderId,
        int $userId,
        array $orderItems,
        PaymentService $paymentService,
        OrderRepository $orderRepository
    ) {
        //
    }
}
```

- Все методы сеттеры ДОЛЖНЫ возращать `$this` (fluent interface)

Пример:
```php
class Order
{
    /**
     * @var integer
     */
    private $id;
    
    /**
     * @var string
     */
    private $title;

    /**
     * Order id setter
     *
     * @param int $id
     *
     * @return Order
     */
    public function setId(int $id): Order
    {
        $this->id = $id;

        return $this;
    }

    /**
     * Order title setter
     *
     * @param string $title
     *
     * @return Order
     */
    public function setTitle(string $title): Order
    {
        $this->title = $title;

        return $this;
    }
}
```

- При объединении методов в цепочку СЛЕДУЕТ каждый метод переносить на новую строку с отступом (четыре пробела).
Метод отдающий fluent interface ДОЛЖЕН вызываться на первой строке.

Неправильный вариант:
```php
$message = $this
    ->getMessageBuilder()
    ->setReceiver('user@localhost.lo')
    ->setSubject('subject')
    ->setMessage('message body')
    ->build();
```

Правильный вариант:
```php
$message = $this->getMessageBuilder()
    ->setReceiver('user@localhost.lo')
    ->setSubject('subject')
    ->setMessage('message body')
    ->build();
```

- Оператору `return` ДОЛЖНА предшествовать пустая линия, за исключением случая когда `return` единственная строка внутри управляющей структуры (например if, function)

Пример 1:
```php
class Order
{
    /**
     * @var integer
     */
    private $id;
    
    /**
     * Order id getter
     *
     * @return int
     */
    public function getId(): int
    {
        return $this->id;
    }
}
```

Пример 2:
```php
class Order
{
   /**
    * @var integer
    */
   private $id;
   
   /**
    * Order id getter
    *
    * @return int
    */
   public function getId(): int
   {
       if (empty($this->id)) {
           return 0;
       }
       
       return $this->id;
   }
}
```

- НЕОБХОДИМО использовать `return null;`, когда функция должна возвращать значение `null`, и НЕОБХОДИМО использовать `return;` в случаях когда функция возвращает пустое значение (void).

- Код методов ДОЛЖЕН иметь не более 1 уровня вложенности (за исключением некоторых особых ситуаций). Else блоки и лишние уровни вложенности РЕКОМЕНДУЕТСЯ убирать при помощи [return early](https://arne-mertz.de/2016/12/early-return/) и [extract method](https://refactoring.com/catalog/extractMethod.html)

Пример:
```php
function sendEmailNotificationIfRequired(Transaction $transaction)
{
	if ($transaction->type() == 'purchase') {
    	// 1
    	if ($transaction->completed()) {
			// 2
            if ($this->timeService->hours() > 19) {
            	// 3
				return false
            } else {
				// 4
	            return $this->notificationService->sendTransactionNotification($transaction);
            }
        } else {
			// 2
        	return false;
        }
    } else {
		// 1
    	return false;
    }
}
```

Отрефакторенный пример:
```php
function sendEmailNotificationIfRequired(Transaction $transaction)
{
	if ($transaction->type() != 'purchase') { // 1
	    return false;
	}

    if (!$transaction->completed()) { // 2
        return false;
    }

    if ($this->timeService->hours() > 19) { // 3
        return false;
    }

    return $this->notificationService->sendTransactionNotification($transaction); // 4
}
```

- В коде вызова функций и методов НЕ ДОЛЖНО быть пробела между именем функции или метода и открывающей круглой скобкой. НЕ ДОЛЖНО быть пробела после открывающей круглой скобки. НЕ ДОЛЖНО быть пробела перед закрывающей круглой скобкой. В списке аргументов НЕ ДОЛЖНО быть пробелов перед запятыми, но ДОЛЖЕН быть пробел после каждой запятой

Пример:
```php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

- Список аргументов МОЖЕТ быть разделён на несколько строк, каждая из которых дополнена слева одним отступом (четырьмя пробелами). В таком случае первый элемент списка аргументов ДОЛЖЕН начинаться с новой строки, и в каждой строке ДОЛЖЕН быть указан только один аргумент

Пример:
```php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

- Для каждого аргумента метода и функции РЕКОМЕНДУЕТСЯ использовать `Type Hinting`. В качестве зависимостей РЕКОМЕНДУЕТСЯ использовать контракты (интерфейсы сущности) вместо `Direct Type Hinting` (упоминания конкретного класса). `Direct Type Hinting` используется при невозможности использовать контракт.



### Оформление управляющих структур <a name="structures"></a>

- В управляющих конструкциях после ключевого слова и перед открывающей круглой скобкой `(` ДОЛЖЕН располагаться один пробел. Открывающая фигурная скобка `{` в управляющих конструкциях ДОЛЖНА располагаться на той же строке, что и сама конструкция, а закрывающая фигурная скобка `}` ДОЛЖНА располагаться на следующей строке после тела конструкции. После открывающей круглой скобки `(` и перед закрывающей круглой скобкой `)` в управляющих конструкциях НЕ ДОЛЖНО быть пробела. НЕОБХОДИМО добавлять пробел после каждой запятой разделителя. Между закрывающей круглой скобкой `)` и открывающей фигурной скобкой `{` ДОЛЖЕН быть один пробел. Тело конструкции ДОЛЖНО быть дополнено одним отступом (четырьмя пробелами)

Пример:
```php
while (true) {
    //
}
```

- Тело каждой управляющей конструкции ДОЛЖНО быть заключено в фигурные скобки. Это позволяет стандартизировать внешний вид управляющих конструкций с снизить риск возникновения ошибок при добавлении новых строк в тело конструкции

Неправильный вариант:
```php
if (true)
    return true;
```

Правильный вариант:
```php
if (true) {
    return true;
}
```

- Ключевое слово `elseif` СЛЕДУЕТ использовать вместо отдельного сочетания `else` и `if`. Так конструкция будет представлять собой одно слово.
- Слова `else` и `elseif` ДОЛЖНЫ располагаться на той же строке, что и закрывающая фигурная скобка предшествующего тела конструкции

```php
if ($expr1) {
    //
} elseif ($expr2) {
    //
} else {
    //
}
```

- Конструкция `switch` ДОЛЖНА выглядеть следующим образом

```php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```

- Конструкция `while` ДОЛЖНА выглядеть следующим образом

```php
while ($expr) {
    // тело конструкции
}
```

- Конструкция `do while` ДОЛЖНА выглядеть следующим образом

```php
do {
    // тело конструкции
} while ($expr);
```

- Конструкция `for` ДОЛЖНА выглядеть следующим образом

````php
for ($i = 0; $i < 10; $i++) {
    // тело for
}
````

- Конструкция `foreach` ДОЛЖНА выглядеть следующим образом

```php
foreach ($iterable as $key => $value) {
    // тело foreach
}
```

- Блоки конструкции `try catch` ДОЛЖНЫ выглядеть следующим образом

```php
try {
    // тело try
} catch (FirstExceptionType $e) {
    // тело catch
} catch (OtherExceptionType $e) {
    // тело catch
}
```


### Работа с исключениями <a name="exceptions"></a>
- Выбрасывание исключений класса `\Exception` НЕДОПУСТИМО. Для всех выбрасываемых исключений ДОЛЖНЫ создаваться пользовательские исключения, наследующие класс `\Exception`. Выбрасываться ДОЛЖНЫ либо пользовательские исключения, наследуемые от `\Exception`, либо исключения, наследуемые от пользовательских исключений.
- Вы ДОЛЖНЫ обрабатывать исключения, которые кидает вызываемый класс в своём коде. Если вы не можете обработать исключение нижнего слоя, вам НЕОБХОДИМО обернуть это исключение в исключение своего слоя и выбросить его выше. Обработка исключений нижнего слоя ДОЛЖНА представлять собой набор блоков `catch` в каждом из которых обрабатываются различные исключения вызывающего класса. Последним блоком `catch` ДОЛЖЕН быть блок с обработкой `\Throwable` и/или `\Exception`

Пример:
```php
namespace App\Order\Exception;

class OrderException extends \Exception
{
    //
}
```

```php
namespace App\Order\Exception;

class OrderNotFoundException extends OrderException
{
    //
}
```

```php
namespace App\Order\Exception;

class OrderAlreadyPayed extends OrderException
{
    //
}
```

```php
namespace App\Service;

use App\Order\Exception\OrderException;
use App\Order\Exception\OrderNotFoundException;
use App\Order\Exception\OrderAlreadyPayed;
use App\Order\Repository\ModelNotFoundException;
use App\Service\PaymentService;
use App\Repository\OrderRepository;

class PaymentOrder
{
    /**
     * @var PaymentService
     */
    private $paymentService;
    
    /**
     * @var OrderRepository
     */
    private $orderRepository;

    /**
     * PaymentOrder constructor.
     * 
     * @param PaymentService $paymentService
     * @param OrderRepository $orderRepository
     */
    public function __construct(PaymentService $paymentService, OrderRepository $orderRepository)
    {
        $this->paymentService = $paymentService;
        $this->orderRepository = $orderRepository;
    }

    /**
     * @param int $id
     * 
     * @return bool
     * 
     * @throws OrderAlreadyPayed
     * @throws OrderException
     * @throws OrderNotFoundException
     */
    public function payment(int $id): bool
    {
        try {
            $order = $this->orderRepository->get($id);
        } catch (ModelNotFoundException $e) {
            throw new OrderNotFoundException(sprintf('Order id %s not found', $id));
        } catch (\Throwable $e) {
            throw new OrderException(sprintf('An error occurred for order id %s', $id));
        }

        if ($order->isPayed()) {
            throw new OrderAlreadyPayed(sprintf('An order id %s already payed', $id));
        }

        return $this->paymentService->pay($id);
    }
}
```

- Конструктор класса НЕ ДОЛЖЕН кидать исключения.


### Работа с массивами <a name="arrays"></a>
- Массивы ДОЛЖНЫ определяться при помощи конструкции `[]`, использование конструкции `array()` НЕДОПУСТИМО.
- Во всех уместных случаях вместо `foreach` РЕКОМЕНДУЕТСЯ использовать соответствующие методы обработки массивов

Пример 1:
```php
$identifiers = [];
foreach ($books as $book) {
	$identifiers[] = $book->id;
}
```

Отрефакторенный пример 1:
```php
$bookMapper = function($book)
{
	return $book->id;
}

$identifiers = array_map($bookMapper, $books);
```

Пример 2:
```php
$activeBooks = [];
foreach($books as $book) {
	if ($book->active)	{
		$activeBooks[] = $book;
	}
}
```

Отрефакторенный пример 2:
```php
$bookFilter = function(Book $book)
{
    return $book->active;
}

$activeBooks  = array_filter($books, $bookFilter);
```

- При инициализации массива, каждый элемент массива ДОЛЖЕН быть на новой строке. НЕОБХОДИМО добавлять запятую в конце каждой строки многострочного массива (даже после последней)

Неправильный вариант:
```php
$books = [
    'Harry Potter and the Cursed Child'
];

$items = ['Fridge', 'Toaster'];
```

Правильный вариант:
```php
$books = [
    'Harry Potter and the Cursed Child',
];

$items = [
    'Fridge',
    'Toaster',
];
```

## Требования к документированию кода <a name="doc"></a>
- Документацию РЕКОМЕНДУЕТСЯ писать на английском языке.
- В файлах проекта НЕ ДОЛЖНО быть комментариев, которые автоматически сгенерировала IDE (например, `Created by ...`)
- Для `Type Hinting` в `PHPDocs` и приведения типов ДОЛЖНЫ использоваться следующие нотации: `bool` (вместо `boolean` и `Boolean`), `int` (вместо `integer`) и `float` (вместо `double` и `real`). Данные требования необходимы для совместимости с нотациями использующимися в PHP7.
- Типы свойств класса, получаемые магическими методами ДОЛЖНЫ быть обозначены при помощи `PHPDoc` блока `@property`

Пример:
```php
/**
 * Class Order
 *
 * @property string $title
 * @property integer $id
 */
class Order
{
    //
}
```

- Типы методов класса, получаемые магическими методами ДОЛЖНЫ быть обозначены при помощи `PHPDoc` блока `@method`

Пример:
```php
/**
 * Class Order
 *
 * @method setTitle(string $title)
 * @method string getTitle()
 */
class Order
{
    //
}
```

- Для комментариев НЕ СЛЕДУЕТ использовать решетку

Неправильный вариант:
```php
# somme comment
```

Правильный вариант:
```php
// somme comment
```

- У каждого свойства класса ДОЛЖНА присутствовать документация содержащая краткое описание свойства и его тип

Пример:
```php
class Order
{
    /**
     * Order id
     * 
     * @var int
     */
    private $id;

    /**
     * Order title
     * 
     * @var string
     */
    private $title;
}
```

- У каждого метода класса ДОЛЖНА присутствовать документация содержащая краткое описание метода, список принимаемых аргументов, список выбрасываемых исключений и тип возвращаемого значения

Пример:
```php
class Order
{
    /**
     * Order id setter
     * 
     * @param int $id
     * 
     * @return Order
     * 
     * @throws \InvalidArgumentException
     */
    public function setId(int $id): Order
    {
        if ($id > 10) {
            throw new \InvalidArgumentException('Order is does not correct');
        }
        
        $this->id = $id;
        
        return $this;
    }
}
```

- Блоки описания к свойству или методу, описания типа переменной `@var`, списка принимаемых агрументов метода `@param`, возвращаемого значения `@return`, списка выбрасываемых исключений `@throws` ДОЛЖНЫ быть отделены друг от друга пустой строкой

Неправильный вариант:
```php
class Order
{
    /**
     * Current balance
     * @var float
     */
    private $balance;

    /**
     * Sends money between addresses
     * @param string $from
     * @param string $to
     * @param float $amount
     * @return bool
     * @throws \InvalidArgumentException
     * @throws InvalidAmountException
     */
    public function sendMoney(string $from, string $to, float $amount): bool
    {
        if (empty($from)) {
            throw new \InvalidArgumentException('From address can not be empty');
        }

        if ($amount < 0) {
            throw new InvalidAmountException('An amount must be greater than zero');
        }
        
        return true;
    }
}
```

Правильный вариант:
```php
class Order
{
    /**
     * Current balance
     * 
     * @var float
     */
    private $balance;

    /**
     * Sends money between addresses
     * 
     * @param string $from
     * @param string $to
     * @param float $amount
     * 
     * @return bool
     * 
     * @throws \InvalidArgumentException
     * @throws InvalidAmountException
     */
    public function sendMoney(string $from, string $to, float $amount): bool
    {
        if (empty($from)) {
            throw new \InvalidArgumentException('From address can not be empty');
        }

        if ($amount < 0) {
            throw new InvalidAmountException('An amount must be greater than zero');
        }
        
        return true;
    }
}
```

- Все итераторы, не поддерживающие `Type Hinting`, ДОЛЖНЫ использовать inline-PHPDoc директиву `@var` для обозначения типа локальной переменной

Пример:
```php
foreach ($books as $book) {
	/* @var Book $book */
	$book->save();
}
```
