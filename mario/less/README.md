# Mario

{% video https://youtu.be/SQjeP62j9F0 %}

{% next %}

## World 1-1

В кінці рівня World 1-1 гри Super Mario Brothers компанії Nintendo, Маріо має піднятись на напівпіраміду з блоків перед тим, як стрибнути до флагштока (якщо він хоче максимізувати набрані очки). Знизу ви можете побачити скріншот з гри.

![screenshot of Mario jumping up a right-aligned pyramid](pyramid.png)

Давайте відтворимо цю піраміду на С, у текстовому вигляді за допомогою хешів (#), які будуть замість цеглин. Кожен хеш трохи вищий, ніж ширший, то ж і сама піраміда стає вищою та вужчою.

```
       #
      ##
     ###
    ####
   #####
  ######
 #######
########
```

Наша програма буде називатись mario. Давайте дамо користувачеві можливість вирішити самостійно, наскільки високою має бути піраміда, спочатку запитавши в користувача ціле число від 1 до 8 включно.

Ось, як має працювати програма, якщо користувач ввів число 8:

```
$ ./mario
Height: 8
       #
      ##
     ###
    ####
   #####
  ######
 #######
########
```

Ось, як має працювати програма, якщо користувач ввів число 4:

```
$ ./mario
Height: 4
   #
  ##
 ###
####
```

Ось, як має працювати програма, якщо користувач ввів число 2:

```
$ ./mario
Height: 2
 #
##
```

Ось, як має працювати програма, якщо користувач ввів число 1:

```
$ ./mario
Height: 1
#
```

Якщо користувач не ввів додатне ціле число від 1 до 8 включно, програма має продовжувати запитувати число, поки користувач його не введе:

```
$ ./mario
Height: -1
Height: 0
Height: 42
Height: 50
Height: 4
   #
  ##
 ###
####
```

{% spoiler "РОЗВ’ЯЗОК КОМАНДИ КУРСУ" %}

Для перегляду розв’язків цієї задачі від команди курсу, виконайте:

```
./mario
```

ось тут [this sandbox](http://bit.ly/2VAClIi).

{% endspoiler %}

Як розпочати? Давайте розглянемо задачу покроково.

{% next %}

## Pseudocode

First, write in `pseudocode.txt` at right some pseudocode that implements this program, even if not (yet!) sure how to write it in code. There's no one right way to write pseudocode, but short English sentences suffice. Recall how we wrote pseudocode for [finding Mike Smith](https://cdn.cs50.net/2018/fall/lectures/0/lecture0.pdf). Odds are your pseudocode will use (or imply using!) one or more functions, conditions, Boolean expressions, loops, and/or variables.

{% spoiler %}

There's more than one way to do this, so here's just one!

1. Prompt user for height
1. If height is less than 1 or greater than 8 (or not an integer at all), go back one step
1. Iterate from 1 through height:
    1. On iteration *i*, print *i* hashes and then a newline

It's okay to edit your own after seeing this pseudocode here, but don't simply copy/paste ours into your own!

{% endspoiler %}

{% next %}

## Запит вхідних даних від користувача

Яким би не був ваш псевдокод, давайте почнемо з коду на С, який просить (одноразово чи повторно) користувача ввести дані.

Змініть `mario.c` справа таким чином, аби у коді був запит до користувача про висоту піраміди, збережіть введені дані у змінній, ще раз запитайте користувача, а потім знову за потреби, якщо він вводить не цілі додатні числа від 1 до 8 включно. Потім просто виведіть значення змінної, чим ви підтвердите (для себе), що ви насправді зберегли введені користувачем дані, як наведено нижче.

```
$ ./mario
Height: -1
Height: 0
Height: 42
Height: 50
Height: 4
Stored: 4
```

{% spoiler "Підказки" %}

* Пам’ятайте, що можете скомпілювати вашу програму за допомогою make.
* Пам’ятайте, що ви можете виводити int за допомогою printf скориставшись %i.
* Пам’ятайте, що ви можете одержати ціле число від користувача за допомогою get_int.
* Пам’ятайте, що get_int оголошується у cs50.h.
* Пам’ятайте, що ми запитували у користувача додатне ціле число під час лекції в positive.c (потрібен акаунт у GitHub) [`positive.c`](https://sandbox.cs50.io/b56865fd-c861-425f-aad7-4adcf6831139).

{% endspoiler %}

## Побудова зворотної піраміди

Ваша програма (сподіваємось!) приймає вхідні дані, як потрібно, настав час наступного кроку.

Виявляється, трохи простіше будувати піраміду, вирівняну за лівим краєм, ніж за правим, як ось тут:

```
#
##
###
####
#####
######
#######
########
```

Тож давайте таку й побудуємо для початку, а потім, коли все вже буде працювати, ще й вирівняємо її справа!

Змініть `mario.c` справа таким чином, щоб програма не просто виводила дані користувача, а побудувала піраміду із вирівнювання зліва такої висоти.

{% spoiler "Підказки" %}

Не забудьте, що хеш – це просто символ, тому ви можете вивести його за допомогою printf, як і будь-який інший.
Як у Scratch є блок Repeat, у С є цикл for за допомогою якого можна повторювати певну кількість разів. Можливо, на кожній ітерації i ви зможете виводити стільки ж хешів?
Ви можете “вкладати” цикли один в один, ітеруючі через одну змінну (наприклад, i) у “зовнішньому” циклі і через іншу (наприклад, j) у “внутрішньому”. Наприклад, ось так ви зможете вивести квадрат із сторонами n. Звісно, це не той квадрат, який вам самим треба вивести!

    ```
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            printf("#");
        }
        printf("\n");
    }
    ```

{% endspoiler %}

{% next %}

## Right-Aligning with Dots

Let's now right-align that pyramid by pushing its hashes to the right by prefixing them with dots (i.e., periods), a la the below.

```
.......#
......##
.....###
....####
...#####
..######
.#######
########
```

Modify `mario.c` in such a way that it does exactly that!

{% spoiler "Hint" %}

Notice how the number of dots needed on each line is the "opposite" of the number of that line's hashes. For a pyramid of height 8, like the above, the first line has but 1 hash and thus 7 dots. The bottom line, meanwhile, has 8 hashes and thus 0 dots. Via what formula (or arithmetic, really) could you print that many dots?

{% endspoiler %}

### How to Test Your Code

Does your code work as prescribed when you input

* `-1` (or other negative numbers)?
* `0`?
* `1` through `8`?
* `9` or other positive numbers?
* letters or words?
* no input at all, when you only hit Enter?

{% next %}

## Removing the Dots

All that remains now is a finishing flourish! Modify `mario.c` in such a way that it prints spaces instead of those dots!

{% spoiler "Hint" %}

A space is just a press of your space bar, just as a period is just a press of its key! Just remember that `printf` requires that you surround both with double quotes!

{% endspoiler %}

{% next %}

## How to Submit

Execute the below, logging in with your GitHub username and password when prompted. For security, you'll see asterisks (`*`) instead of the actual characters in your password.

```
submit50 cs50/2018/fall/mario/less
```
