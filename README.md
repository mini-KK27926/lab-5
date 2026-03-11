# Лабораторная работа №5 

### Задание 1. Шаблоны
В класс Дробь, добавить интерфейс на два метода: получение вещественного значения, установка числителя и установка знаменателя.
Сгенерировать такую версию дроби, которая будет кэшировать вычисление вещественного значения.
Если раннее в вашем варианте не было Дроби, то создайте сущность Дробь со следующими особенностями:
- Имеет числитель: целое число
- Имеет знаменатель: целое число
- Дробь может быть создана с указанием числителя и знаменателя
- Может вернуть строковое представление вида “числитель/знаменатель”
- Необходимо корректно обрабатывать отрицательные значения. Учтите, что знаменатель не может быть отрицательным.
- Переопределите метод сравнения объектов по состоянию таким образом, чтобы две дроби считались одинаковыми тогда, когда у них одинаковые значения числителя и знаменателя.

### Решение
- Класс `Fraction` представляет дробь с числителем и знаменателем.
- Методы:
  - `getDecimalValue()` — возвращает вещественное значение дроби.
  - `toString()` — возвращает строковое представление вида "числитель/знаменатель".
- Проверка отрицательных числителей и знаменателей.
```java

public interface FractionInterface {
    double getDecimalValue();
    void setNumerator(int numerator);
    void setDenominator(int denominator);
}

public class Fraction implements FractionInterface {
    private int numerator;
    private int denominator;
    private Double cachedValue = null;

    public Fraction(int numerator, int denominator) {
        if (denominator <= 0) throw new IllegalArgumentException("Знаменатель > 0");
        this.numerator = numerator;
        this.denominator = denominator;
    }

    @Override
    public double getDecimalValue() {
        if (cachedValue == null) {
            cachedValue = (double) numerator / denominator;
        }
        return cachedValue;
    }

    @Override
    public void setNumerator(int numerator) {
        this.numerator = numerator;
        cachedValue = null;
    }

    @Override
    public void setDenominator(int denominator) {
        if (denominator <= 0) throw new IllegalArgumentException("Знаменатель > 0");
        this.denominator = denominator;
        cachedValue = null;
    }

    @Override
    public String toString() {
        return numerator + "/" + denominator;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Fraction)) return false;
        Fraction f = (Fraction) obj;
        return this.numerator * f.denominator == this.denominator * f.numerator;
    }
}

```
---

### Задание 2. Структурные шаблоны
1 Количество мяуканий.
Необходимо воспользоваться классом Кот и методом принимающим всех мяукающих из задачи 2.5.4.
Необходимо таким образом передать кота в указанный метод, что бы после окончания его работы узнать сколько раз мяукал кот за время его работы. На рисунке показан пример работы. Перед вызовом метода создаем кота, отправляем ссылку на кота в метод, после окончания его работы выводим количество мяуканий на экран. Кота изменять нельзя.

Если раннее в вашем варианте не было Кота, то создайте:
1. сущность Кот, которая описывается следующим образом:
- Имеет Имя (строка)
- Для создания необходимо указать имя кота.
- Может быть приведен к текстовой форме вида: “кот: Имя”
- Может помяукать, что приводит к выводу на экран следующего текста: “Имя: мяу!”, вызвать мяуканье можно без параметров.
2. интерфейс Мяуканье: разработайте метод, который принимает набор объектов способных мяукать и вызывает мяукание у каждого объекта. Мяукающие объекты должны иметь метод со следующей сигнатурой:public void meow();

### Решение
- Интерфейс `Meowable` для объектов, умеющих мяукать.
- Класс `Cat`:
  - Свойство `name`.
  - Метод `meow()` выводит в консоль "Имя: мяу!".
  - Метод `getMeowCount()` возвращает количество мяуканий.
- Класс `MeowProcessor`:
  - Метод `meowAll(Meowable... cats)` вызывает `meow()` у всех переданных объектов.
 ```java
public interface Meowable {
    void meow();
}

public class MeowProcessor {
    public static void meowAll(Meowable... cats) {
        for (Meowable c : cats) {
            c.meow();
        }
    }
}
public class Cat implements Meowable {
    private String name;
    private int meowCount = 0;

    public Cat(String name) {
        this.name = name;
    }

    @Override
    public void meow() {
        meowCount++;
        System.out.println(name + ": мяу!");
    }

    public int getMeowCount() {
        return meowCount;
    }

    @Override
    public String toString() {
        return "Кот: " + name;
    }
}
```
---

### Задание 4. Мап (логины учеников)
1 На вход программы подаются фамилии и имена учеников. Известно, что общее количество учеников не превышает 100. В первой строке вводится количество учеников, принимавших участие в соревнованиях, N. Далее следуют N строк, имеющих следующий формат:  
<Фамилия><Имя>  
Здесь <Фамилия> – строка, состоящая не более чем из 20 символов; <Имя>– строка, состоящая не более чем из 15 символов. При этом <Фамилия> и <Имя> разделены одним пробелом.  

Требуется написать программу, которая формирует и печатает уникальный логин для каждого ученика по следующему правилу: если фамилия встречается первый раз, то логин – это данная фамилия, если фамилия встречается второй раз, то логин – это фамилия, в конец которой приписывается число 2 и т.д.

### Решение
- Класс `LoginGenerator`:
  - Метод `generateLogins(List<String> students)` создает логины по фамилии.
  - Для повторяющихся фамилий добавляется порядковый номер.
- Используется `Map` для учета количества встреч фамилий.
```java
public class LoginGenerator {
    public static void generateLogins(List<String> names) {
        Map<String, Integer> count = new HashMap<>();
        for (String fullName : names) {
            String surname = fullName.split(" ")[0];
            count.put(surname, count.getOrDefault(surname, 0) + 1);
            if (count.get(surname) == 1) {
                System.out.println(surname);
            } else {
                System.out.println(surname + count.get(surname));
            }
        }
    }
}

```

---

### Задание 6. Очередь
Составить программу, которая удаляет из списка L все элементы E, если такие есть.
### Решение
- Класс `QueueProcessor`:
  - Метод `printReverse(Queue<T> queue)` выводит элементы в обратном порядке.
- Используется `Stack` или `LinkedList` для реверса очереди.
```java
public class QueueProcessor {
    public static <T> void printReverse(Queue<T> queue) {
        List<T> list = new ArrayList<>(queue);
        Collections.reverse(list);
        System.out.println(list);
    }
}

```
---

### Задание 7. Стрим
1 Дан набор объектов типа Point, необходимо взять все Point в разных координатах, убрать с одинаковыми X,Y, отсортировать по X, отрицательные Y сделать положительными и собрать это все в ломаную (объект типа Polyline).

### Решение
- Класс `Point` с координатами X и Y.
- Класс `Polyline` хранит массив точек.
- Класс `StreamProcessor`:
  - Метод `processPoints(List<Point> points)` обрабатывает точки, формирует уникальные точки и сортирует по X.
  - Отрицательные Y преобразуются в положительные.
```java
public class Point {
    int x, y;
    public Point(int x, int y) { this.x=x; this.y=y; }
    @Override
    public String toString() { return "{" + x + ";" + y + "}"; }
}
public class Polyline {
  List<Point> points;
  public Polyline(List<Point> points) { this.points = points; }
  @Override
  public String toString() { return "Линия " + points; }
}
public class StreamProcessor {
  public static Polyline processPoints(List<Point> points) {
      List<Point> processed = points.stream()
              .distinct()
              .sorted(Comparator.comparingInt(p -> p.x))
              .map(p -> new Point(p.x, Math.abs(p.y)))
              .collect(Collectors.toList());
      return new Polyline(processed);
  }
}
```


