# Vigenère

{% video https://youtu.be/qmgr9yJFdXQ %}

{% next %}

## Ooh, la la!

Шифр Віженера є покращенням відносно шифру Цезаря. Шифрування у ньому відбувається шляхом використання _послідовності ключів_ (або _лючового слова_).

Тобто, якщо _p_ – це звичайний текст і _k_ – ключове слово (як, наприклад, алфавітний рядок: де А (або а) представляє 0, B (або b) представляє 1, C (або c) представляє 2, Z представляє с 25), то кожна літера _c<sub>i</sub>_, в зашифрованому тексті _c_ обраховується як:

c<sub>i</sub> = (p<sub>i</sub> + k<sub>j</sub>) % 26

Зауважте, що шифр використовує _k<sub>j</sub>_ проти _k_. І пам'ятайте, що, якщо довжина _k_ менша за довжину _p_, то літери в _k_ мають бути циклічно використані стільки раз, скільки потрібно для шифрування _p_.

Інакше кажучи, якби сам Віженер хотів конфіденційно сказати комусь HELLO із використанням ключового слова, скажемо, ABC, він би зашифрував H за допомогою ключа 0 (тобто A), Е – за допомогою ключа 1 (тобто B), а першу букву L – за допомогою ключа 2 (тобто С). На цьому етапі в нього б закінчились букви у ключовому слові, і тому він знову використав би частину ключового слова, щоб зашифрувати другу L ключем 0 (тобто A), а букву O – ключем 1 (тобто B). Отже, він написав би HELLO як HFNLP, як на рисунку нижче:

| plaintext    | H | E | L | L | O |
|--------------|---|---|---|---|---|
| + key        | A | B | C | A | B |
| (shift value)| 0 | 1 | 2 | 0 | 1 |
| = ciphertext | H | F | N | L | P |

Давайте напишемо програму `vigenere`, яка дозволяє шифрувати повідомлення за допомогою коду Віженера. Під час запуску програми користувачем, він має обрати за допомогою аргументу командного рядка, яким буде ключ у секретному повідомленні, яке він введе під час роботи програми.

Ось кілька прикладів того, як може працювати програма:

```
$ ./vigenere bacon
plaintext:  Meet me at the park at eleven am
ciphertext: Negh zf av huf pcfx bt gzrwep oz
```

або коли користувач вводить ключове слово, але воно не повністю складається з літер:

```
$ ./vigenere 13
Usage: ./vigenere keyword
```

Або коли користувач не вводить ключове слово:

```
$ ./vigenere
Usage: ./vigenere keyword
```

Або коли користувач вводить забагато ключових слів:

```
$ ./vigenere bacon and eggs
Usage: ./vigenere keyword
```

{% spoiler "Спробуйте" %}

Для того, щоб спробувати варіанти розв’язку цієї задачі командою курсу, виконайте%

```
./vigenere keyword
```

Поставте валідний літерний рядок на місце `ключового слова` ось[тут](http://bit.ly/2Qocfog).

{% endspoiler %}

З чого почати? Давайте почнемо з чогось знайомого!

{% next %}

## Déjà vu

Як ви вже, мабуть, зрозуміли, основна ідея цього виду шифрування дуже подібна до ідеї, яка лежить в основі шифрування Цезаря. Отже, наш код у шифруванні Цезаря – це гарний початок, тож можете спокійно спробувати замінити весь вміст `vigenere.c` вашим розв’язком для `caesar.c`. 

Єдина відмінність між шифруваннями Цезаря та Віженера – ключ для шифрування Віженера складається з літер, а не з чисел. Тому треба впевнитись, що користувач насправді введе ключове слово! Змініть перевірку, яку ви імплементували для шифрування Цезаря так, щоб впевнитись, що кожен символ ключового слова літерний, а не цифровий. Якщо якийсь елемент не літерний, програма має виводити `Usage: ./vigenere keyword` та повертати ненульове значення, як ми робили раніше. Якщо всі символи літерні, після перевірки ви маєте вивести `Success`, а потім вийти за допомогою `return 0;` одразу ж (на разі), оскільки наш шифруючий код ще не зовсім готовий до роботи, тож наша програма не буде його виконувати.

Приклад поведінки:

```
$ ./vigenere alpha
Success
```

або

```
$ ./vigenere 123
Usage: ./vigenere keyword
```

{% spoiler "Підказка" %}

* Пам’ятайте, що заголовковий файл `string.h` містить корисні функції, які працюють з рядками. Перегляньте меню [CCS50 Reference](https://reference.cs50.net/) аби довідатись про деякі з них!
* Пам’ятайте, що ми можемо використовувати цикл для ітерування через кожен символ рядка, якщо ми знаємо його довжину.
* Пам’ятайте, що заголовковий файл `ctype.h` містить корисні функції, які розповідають нам різні речі про символи. Перегляньте меню [CS50 Reference](https://reference.cs50.net/) аби довідатись про деякі з них!

* Recall that the `string.h` header file contains a number of useful functions that work with strings. See [CS50 Reference](https://reference.cs50.net/)'s menu for some!
* Recall that we can use a loop to iterate over each character of a string if we know its length.
* Recall that the `ctype.h` header file contains a number of useful functions that tell us things about characters. See [CS50 Reference](https://reference.cs50.net/)'s menu for some!

{% endspoiler %}

{% next %}

## Одержання значення зсуву

Let's for now assume that the user is providing single-character keywords. Can we convert that character into the correct shift value? Let's do so by writing a _function_.

Near the top of your file, below the `#include` lines, let's _declare_ the _prototype_ for a new function whose purpose is to do just that. It will take a single character as input, and it will output the shift value for that character.

```
int shift(char c);
```

Now we've declared a function called `shift` that takes a single character (`c`) as input, and will output an integer.

Now, down below the closing curly brace of `main`, let's give ourselves a place to _define_ (i.e., implement) this new function.

```
int shift(char c)
{
   // TODO
}
```

In place of that `TODO` is where we'll do the work of converting that character to its positional integer value (so, again, `A` or `a` would be 0, `B` or `b` would be 1, `Z` or `z` would be 25, etc.)

To test this out, delete the line where you printed `"Success"` (but leave the `return 0;` for now), and in place of the just-deleted line, add the below lines to test whether your code works.

```
int key = shift(argv[1][0]);
printf("%i\n", key);
```

Your program should print a 0 if run with the keyword `A` or `a`. Try running the program with other capital and lowercase letters as the keyword. Is the behavior what you expect?

{% spoiler "Hints" %}

* Functions have inputs and outputs.
* When we *declare* a function, we need to provide its return type, name, and an argument list, each of which also has a type.
* When we *use* or *call* a function, we just plug in appropriate values in the argument list, and assign the output of the function to a variable that corresponds to the function's return type.
* If `argv[1]` is a string, then `argv[1][0]` is just the first character of that string.
* Recall that the `ctype.h` header file contains a number of useful functions that tell us things about characters.
* The ASCII value of `A` is 65. The ASCII value of `a` is 97.
* The ASCII value of `B` is 66. The ASCII value of `b` is 98. See a potential pattern emerging?

{% endspoiler %}

{% next %}

## One-character keywords

Time to get back to using that enciphering code you wrote before! You may have noticed that if your keyword _k_ consists of exactly one letter (say, `H` or `h`), Vigenère's cipher effectively becomes a Caesar cipher (of, in this example, 7). Let's for now indeed assume the user's keyword will just be a single letter. Use your newly-written `shift` function to calculate the shift value for the letter they provided, assign the return value of that function to an integer variable `key`, and use `key` exactly as you did in Caesar's cipher! It should suffice, in fact, to simply delete the recently-added `printf` and the `return 0;` line now, letting the program finally proceed to your previously-written Caesar cipher code!

```
$ ./vigenere A
plaintext:  hello
ciphertext: hello
```

or

```
$ ./vigenere b
plaintext:  HELLO
ciphertext: IFMMP
```

or

```
$ ./vigenere C
plaintext:  HeLlO
ciphertext: JgNnQ
```

{% spoiler "Hints" %}

If some of your variables in your Caesar solution don't match what they've been called so far in this lab, just edit the names of things so they do match!

{% endspoiler %}

{% next %}

## Final Steps

Now it's your turn to take things across the finish line by implementing the remaining functionality in `vigenere.c`. Remember that the user's keyword will probably consist of multiple letters, so you may need to calculate a new shift value for each letter of the plaintext; you may then want to move your `shift` function into your loop somehow.

Remember also that every time you encipher a character, you need to move to the next letter of _k_, the keyword (and wrap around to the beginning of the keyword if you exhaust all of its characters). But if you don't encipher a character (e.g., a space or a punctuation mark), don't advance to the next character of _k_!

And as before, be sure to preserve case, but do so only based on the case of the original message. Whether or not a letter in the keyword is capitalized should have no bearing on whether a letter in the ciphertext is!

{% spoiler "Hints" %}

* You'll probably need one counter, `i` for iterating over the plaintext and one counter, `j` for iterating over the keyword.
* You'll probably find it easiest to control the keyword counter yourself, rather than relying on the `for` loop you're using to iterate over the plaintext!
* If the length of the keyword is, say, 4 characters, then the last character of that keyword can be found at `keyword[3]`. Then, for the next character you encipher, you'll want to use `keyword[0]`.

{% endspoiler %}

{% next %}

## How to Submit

Execute the below, logging in with your GitHub username and password when prompted. For security, you'll see asterisks (`*`) instead of the actual characters in your password.

```
submit50 cs50/2018/fall/vigenere
```
