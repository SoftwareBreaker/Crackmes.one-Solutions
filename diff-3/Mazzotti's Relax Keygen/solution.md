# Crackme Info & Solution

| Property | Value | Property | Value |
| :--- | :--- | :--- | :--- |
| Author | Mazzotti | Platform | Unix/Linux etc. |
| Language | C/C++ | Difficulty | 3.0 / 5.0 |
| Upload Date | 2026-06-24 13:42 | Quality | 5.0 / 5.0 |
| Architecture | x86-64 | | |

### Description
> Try to write a keygen or just find working password for this little program. DO NOT DELETE file bits.mazz IT IS REQUIRED FOR MY KEYGEN TO WORK. Good luck!

---

## 🇷🇺 Разбор решения (Writeup)

### 1. Анализ алгоритма и ветвления
При анализе логики программы в IDA Pro был обнаружен ключевой участок проверки пользовательского ввода. В этом коде мы видим, что если мы вводим строку длиной более 32 символов, то у нас появляется 2 пути (условное ветвление в графе функций):

![Код на C](../../images/screen1.png)

### 2. Модификация бинарника (Патчинг)
Этот crackme можно легко решить патчем в функции main. Нам не обязательно подбирать сложный ключ, достаточно изменить логику условного перехода так, чтобы программа всегда выбирала правильный путь выполнения. 

Мы находим инструкцию условного перехода jz (Jump if Zero), которая отправляет нас на ветку некорректного ввода, и меняем её (например, зануляем через nop или инвертируем на jnz).

![Выделенная инструкция jz в функции main](../../images/screen2.png)

### 3. Результат выполнения
После того как мы пропатчили бинарный файл, любые проверки обходятся автоматически. Мы запускаем измененный файл, вводим символы, проверка успешно пропускает наш ввод, и программа выводит нам заветную строку.

![Консоль с успешным выводом заветной строки](../../images/screen3.png)

---

## 🇬🇧 Solution Writeup

### 1. Algorithm Analysis and Branching
While analyzing the program's logic in IDA Pro, a key user-input validation section was discovered. In this code, we can clearly see that if we enter a string longer than 32 characters, two execution paths appear (conditional branching in the function graph):
* Branch A: leads to a validation error if the conditions are not met.
* Branch B: leads to successful program execution.

![Code C](../../images/screen1.png)

### 2. Binary Modification (Patching)
This crackme can be easily solved with a simple patch inside the main function. Instead of reversing or brute-forcing a complex key, we can modify the conditional jump logic so that the program always takes the successful execution path.

We locate the conditional jump instruction jz (Jump if Zero), which redirects execution to the incorrect input branch, and modify it (for instance, by nulling it out with nop instructions or inverting it to jnz).

![Highlighted jz instruction in the main function](../../images/screen2.png)

### 3. Execution Result
Once the binary file is successfully patched, all verification checks are bypassed automatically. We run the modified executable, type in any characters, the check lets our input through seamlessly, and the program prints the desired string.

![Console demonstrating the successful output string](../../images/screen3.png)
