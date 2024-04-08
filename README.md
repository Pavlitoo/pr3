### Використання командного рядка Java для обчислення кількість повних тетрад у двійковому поданні заданого

Даний Java-застосунок дозволяє обчислювати кількість повних тетрад у двійковому поданні заданого
десяткового числа, та тестувати різноманітні функції через командний рядок.

## CalculationResult.java

```java
package pr2;

import java.io.Serializable;

public class CalculationResult implements Serializable {
    private int parameter1;
    private int parameter2;
    private int result;

    public CalculationResult(int parameter1, int parameter2, int result) {
        this.parameter1 = parameter1;
        this.parameter2 = parameter2;
        this.result = result;
    }

    // Геттеры и сеттеры для всех полей

    public int getResult() {
        return result;
    }

    public int getParameter1() {
        return parameter1;
    }

    public int getParameter2() {
        return parameter2;
    }

    @Override
    public String toString() {
        return "CalculationResult{" +
                "parameter1=" + parameter1 +
                ", parameter2=" + parameter2 +
                ", result=" + result +
                '}';
    }
}
```

## ResultDisplay.java

```java
package pr2;

public interface ResultDisplay {
    void displayResult(CalculationResult result);
}
```

## ResultDisplayFactory.java

```java
package pr2;

public interface ResultDisplayFactory {
    ResultDisplay createResultDisplay();
}
```

## SerializationDemo.java

```java
package pr2;

import java.io.*;

public class SerializationDemo {
    public static void main(String[] args) {
        CalculationResult result = new CalculationResult(10, 20, 200);

        // Сохранение объекта в файл
        try (ObjectOutputStream outputStream = new ObjectOutputStream(new FileOutputStream("calculation_result.ser"))) {
            outputStream.writeObject(result);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Восстановление объекта из файла
        try (ObjectInputStream inputStream = new ObjectInputStream(new FileInputStream("calculation_result.ser"))) {
            CalculationResult restoredResult = (CalculationResult) inputStream.readObject();
            System.out.println("Restored result: " + restoredResult);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

## TextResultDisplay.java

```java
package pr2;

public class TextResultDisplay implements ResultDisplay {
    @Override
    public void displayResult(CalculationResult result) {
        System.out.println("Parameter 1: " + result.getParameter1());
        System.out.println("Parameter 2: " + result.getParameter2());
        System.out.println("Result: " + result.getResult());
    }
}
```


## TextResultDisplayFactory.java

```java
package pr2;

public class TextResultDisplayFactory implements ResultDisplayFactory {
    @Override
    public ResultDisplay createResultDisplay() {
        return new TextResultDisplay();
    }
}
```

## CalculationTest.java

```java
package Test;

import pr2.CalculationResult;
import pr2.ResultDisplay;
import pr2.ResultDisplayFactory;
import pr2.TextResultDisplayFactory;

import java.io.*;

public class CalculationTest {
    public static void main(String[] args) {
        // Создаем объект CalculationResult с заданными параметрами
        CalculationResult result = new CalculationResult(5, 10, 50);

        // Тестирование результатов вычислений
        if (result.getResult() != 50) {
            throw new AssertionError("Calculation result is incorrect");
        } else {
            System.out.println("Calculation result is correct: " + result.getResult());
        }

        // Тестирование сериализации/десериализации
        try {
            // Сериализация объекта result
            ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
            ObjectOutputStream objectOutputStream = new ObjectOutputStream(outputStream);
            objectOutputStream.writeObject(result);

            // Десериализация объекта из сериализованных данных
            ByteArrayInputStream inputStream = new ByteArrayInputStream(outputStream.toByteArray());
            ObjectInputStream objectInputStream = new ObjectInputStream(inputStream);
            CalculationResult deserializedResult = (CalculationResult) objectInputStream.readObject();

            // Проверка десериализованного объекта
            if (deserializedResult.getParameter1() != 5 || deserializedResult.getParameter2() != 10 || deserializedResult.getResult() != 50) {
                throw new AssertionError("Deserialization failed: unexpected parameters or result");
            } else {
                System.out.println("Deserialization successful:");
                System.out.println("Parameter1: " + deserializedResult.getParameter1());
                System.out.println("Parameter2: " + deserializedResult.getParameter2());
                System.out.println("Result: " + deserializedResult.getResult());
            }
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }

        // Создание и использование фабрики для отображения результатфов
        ResultDisplayFactory factory = new TextResultDisplayFactory();
        ResultDisplay resultDisplay = factory.createResultDisplay();
        resultDisplay.displayResult(result);
    }
}
```

### Ось результат ↓

![Результат](/screenshot/pr3.jpg)




