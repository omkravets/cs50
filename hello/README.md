# Hello

{% video https://youtu.be/SmwyU3Sz8Ig %}

{% next %}

## Перегляд файлів

Привіт, світ! Справа, у текстовому редакторі CS50 Lab ми напишемо першу програму на С у файлі під назвою hello.c. 

Натисніть на іконку теки, ви побачите, що hello.c – це єдиний файл, який у вас зараз є. Якщо ви звернули увагу на /root/sandbox під іконкою, це означає, що файл hello.c знаходиться у теці (теки ще називають директоріями) під назвою sandbox, а вона знаходиться у іншій теці під назвою root. Натисніть ще раз на іконку теки, аби сховати все це.

Далі у вікні терміналу справа одразу після знаку долара ($), який також відомий як запрошення командного рядка, наберіть точнісенько наступне (малими літерами), потім натисніть Enter:


```
ls
```

Чи маєте ви бачити лише hello.c? Так, це тому, що ви щойно вивели список файлів у цій теці,скориставшись інтерфейсом командного рядка за допомогою виключно клавіатури, а не графічного користувацького інтерфейсу GUI, представленого іконкою теки. Якщо точно, ви виконали (тобто запустили) команду ls, це скорочено «list» або «список» (ця команда використовується настільки часто, що її автори назвали просто ls, аби заощадити час під час набору). Логічно?

Надалі для виконання (запуску) команд друкуйте їх у вікні терміналу та натискайте Enter. Команди чутливі до регістру, тому уважно поставтесь до введення – не друкуйте великими літерами, коли маєте на увазі маленькі, та навпаки.

{% next %}

## Компілювання програм

Перед тим, як виконувати програму, розміщену справа, згадайте, що ми маємо її скомпілювати компілятором (тобто clang), який перекладає її з вихідного коду на машинний код (тобто нулі та одиниці). Виконайте команду нижче, аби скомпілювати програму:

```
clang hello.c
```

А потім викличіть ще раз:

```
ls
```

Цього разу ви маєте побачити не тільки hello.c а й a.out у списку. (Те ж саме ви можете побачити графічно, якщо клікнете на іконку теки ще раз.) Все тому, що clang переклав вихідний код у hello.c на машинний код у a.out, що означає «вивід асемблера», але поговоримо про це іншим разом.

Запустіть програму за допомогою команди нижче.


```
./a.out
```

Hello, world, насправді!

{% next %}

## Називання програм

a.out не дуже привітна для користувача назва програми. Скомпілюємо hello.c ще раз, але цього разу збережемо машинний код до файлу із назвою hello. Виконайте команду нижче.

```
clang -o hello hello.c
```

Зверніть увагу, у команді не має бути зайвих пробілів! А потім ще раз виконайте:

```
ls
```

Тепер у вас буде не тільки hello.c (та a.out з попередньої компіляції) а й hello також. Це тому що -o – це аргумент командного рядка, іноді його називають прапорець або ключ, він повідомляє clang вивести (output, тобто o) файл з назвою hello. Виконайте команду нижче, щоб переглянути, як працює програма з новою назвою.

```
./hello
```

І знову привіт!

{% next %}

## Давайте спростимо

Згадайте, що ми можемо автоматизувати процес виклику clang, дозволивши make самостійно з усім розібратись, а значить зберегти нам трохи часу. Виконайте команду нижче, аби скомпілювати програму останній раз.

```
make hello
```

YВи маєте побачити, що make викликає clang з іще більшою кількістю аргументів командного рядка. Але про це також іншим разом!

Тепер запустіть саму програму останній раз за допомогою команди нижче.

```
./hello
```

Фух!

{% next %}

## Отримання даних від користувача

Можемо сказати, що незалежно від того, як саме ви компілювали або запускали програму, вона все одно виведе тільки hello, world. Давайте трохи персоналізуємо, як було під час лекції.

Змініть програму так, щоб вона спочатку запитувала у користувача його ім’я, а потім виводила hello, такий-то, де такий-то - ім’я користувача.

Впевніться, що ви скомпілювали програму за допомогою:

```
make hello
```

Та впевніться, що запустили програму й кілька разів перевірили її з різними вхідними даними за допомогою:

```
./hello
```

### Розв'язок команди курсу

Аби переглянути варіант реалізації від команди курсу для цієї задачі, виконайте:

<pre>
./hello
</pre>

ось тут [this sandbox](http://bit.ly/2Qp0a2g).

### Підказки

#### Не пам’ятаєте, як запитати у користувача його чи її ім’я?

Згадайте, що можна скористатись get_string ось так, зберігши значення, яке повернеться, у змінній з назвою name типу string..

```c
string name = get_string("What is your name?\n");
```

#### Не пам’ятаєте, як форматувати рядки?

Не пам’ятаєте, як об’єднувати (тобто конкатенувати) ім’я користувача з привітанням? Згадайте, що можна використати printf не тільки для виведення на екран, а й для форматування рядків, саме так, як показано нижче, де у name збережено string.

```c
printf("hello, %s\n", name);
```

#### Використання неоголошеного ідентифікатора?

Бачите наступне, скоріше за все, над усіма іншими помилками?

```
error: use of undeclared identifier 'string'; did you mean 'stdin'?
```

Пригадайте, коли використовуємо get_string, потрібно підключити cs50.h (у якій функцію get_string оголошено) нагорі файлу, як ось тут:

```c
#include <cs50.h>
```
