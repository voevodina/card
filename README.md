# Отчёт о тестировании IntelliJ IDEA на ОС Windows 10

## Краткое описание

В рамках тестирования было проверено:

* Работа IntelliJ IDEA согласно [руководству использования](https://github.com/netology-code/javaqa-homeworks/tree/master/intro) п.2

Дата начала 25.03.2020 - Дата окончания 25.03.2020 было проведено функциональное тестирование.

На тестирование затрачено: 20 минут

В результате тестирования выявлен следующий баг:
* [Банковская карта American Express с 15-значным наименованием отдает FAIL](https://github.com/voevodina/card/issues/1)

## Описание процесса тестирования

В процессе тестирования использовались следующие артефакты*:
* [Инструкция по установке IntelliJ IDEA](https://github.com/netology-code/javaqa-homeworks/blob/master/intro/idea.md)
* [Руководство использования IntelliJ IDEA](https://github.com/netology-code/javaqa-homeworks/tree/master/intro)

В качестве тестовых данных использовались данные из [Руководства использования IntelliJ IDEA](https://github.com/netology-code/javaqa-homeworks/tree/master/intro):

* Установочный файл **jetbrains-toolbox-1.16.6319.exe** с сайта [www.jetbrains.com](https://www.jetbrains.com/toolbox/app/)
* Номера банковских карт Visa, Mastercard, American Express с ресурса [www.getcreditcardnumbers.com](https://www.getcreditcardnumbers.com/) 
* [Код](https://github.com/netology-code/javaqa-homeworks/tree/master/intro)

```
public class Main {
  public static void main(String[] args) {
    // TODO: подставлять номер карты нужно сюда между двойными кавычками, без пробелов
    String number = "5351719427810741";
    System.out.println(String.format("Result is %s", isValidCardNumber(number) ? "OK" : "FAIL"));
  }

  public static boolean isValidCardNumber(String number) {
    if (number == null) {
      return false;
    }

    if (number.length() != 16) {
      return false;
    }

    long result = 0;
    for (int i = 0; i < number.length(); i++) {
      int digit;
      try {
        digit = Integer.parseInt(number.charAt(i) + "");
      } catch (NumberFormatException e) {
        return false;
      }

      if (i % 2 == 0) {
        digit *= 2;
        if (digit > 9) {
          digit -= 9;
        }
      }
      result += digit;
    }

    return (result != 0) && (result % 10 == 0);
  }
}
```

>Шаг 1. Запустила IntelliJ IDEA.

>Шаг 2. Выбрала "Create New Project"

>Шаг 3. Выбрала в Project SDK Java 11

>Шаг 4. Нажала "Next"

>Шаг 5. Добавила в наименованию проекта 1. Нажала Finish

>Шаг 6. В панельке проекта (Alt + 1) раскрыла структуру каталогов и на каталоге src щёлкнула правой кнопкой мыши и выбрала "New" -> "Java Class"

>Шаг 7. Заменила содержимое открывшегося файла кодом 

```
public class Main {
  public static void main(String[] args) {
    // TODO: подставлять номер карты нужно сюда между двойными кавычками, без пробелов
    String number = "5351719427810741";
    System.out.println(String.format("Result is %s", isValidCardNumber(number) ? "OK" : "FAIL"));
  }

  public static boolean isValidCardNumber(String number) {
    if (number == null) {
      return false;
    }

    if (number.length() != 16) {
      return false;
    }

    long result = 0;
    for (int i = 0; i < number.length(); i++) {
      int digit;
      try {
        digit = Integer.parseInt(number.charAt(i) + "");
      } catch (NumberFormatException e) {
        return false;
      }

      if (i % 2 == 0) {
        digit *= 2;
        if (digit > 9) {
          digit -= 9;
        }
      }
      result += digit;
    }

    return (result != 0) && (result % 10 == 0);
  }
}
```

>Шаг 8. Нажала на зелёную стрелку на первой строке, выбрала "Run 'Main.main()'":

**Ожидаемый результат**:

Номера банковских карт при проверке отдают ОК

![run](https://github.com/netology-code/javaqa-homeworks/blob/master/intro/pic/toolbox-step21.png)

**Фактический результат**:

Банковские карты, номера которых менее 16 цифр, отдают FAIL

![img](https://github.com/voevodina/images/blob/master/Card_3.png?raw=true)

Тестирование производилось в следующем окружении:

* ОС Windows 10 Pro, 64bit
* версия Java 11
