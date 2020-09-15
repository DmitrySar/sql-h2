# SQL
___

## Повторение предыдущей темы (*операции CRUD*)

### Удаляет таблицу, если она уже существует:
```SQL
DROP TABLE IF EXISTS TEST;
```

### Создать новую таблицу с колонками ID и NAME:
```SQL
CREATE TABLE TEST(ID INT PRIMARY KEY, NAME VARCHAR(255));
```

### Добавить новую строку
```SQL
INSERT INTO TEST VALUES(1, 'Hello');
```

### Запрос к таблице
```SQL
SELECT * FROM TEST ORDER BY ID;
```

### Изменить данные в строке
```SQL
UPDATE TEST SET NAME='Hi' WHERE ID=1;
```

### Удалить строку
```SQL
DELETE FROM TEST WHERE ID=2;
```

## Фильтрация

### Использование WHERE
```SQL
SELECT * FROM TEST WHERE ID = 1;
```

### Использование LIKE
```SQL
SELECT * FROM PERSON WHERE LASTNAME LIKE 'Ва%';
```

## Вычисления 

### Функции *COUNT, MAX, MIN, AVG ...*

### Запрос SQL, возвращающий количество дублей фамилий:
```SQL
SELECT LASTNAME , COUNT(LASTNAME) AS DOUBLES FROM PERSON GROUP BY LASTNAME HAVING DOUBLES;
```

___
 - # Задание:
   - ### Написать запрос, возвращающий  количество дублей фамилий, если совпадений больше 3. Результат отсортировать по количеству совпадений от большего к меньшему.
   ### [Вставить ссылку на форму опроса](https://....)

___
# Представление объектов в ООП на примере языка *Java*
___

### Класс Person:
```Java
// Импорт библиотек
import java.sql.Date;

// Объявление класса
public class Person {

	// Объявление полей класса
	private static int count = 0; // статическое поле для автогенерации id
	private int id;
	private String lastname;
	private String name;
	private String secondName;
	private Date birthday;
	private String gender;

	// Конструктор без id
	public Person (String lastname, String name, String secondName, Date birthday, String gender) {
		this.id = ++count;
		this.lastname = lastname;
		this.name = name;
		this.secondName = secondName;
		this.birthday = birthday;
		this.gender = gender;
	}

	// Конструктор с полем id для получения его значения из БД
	public Person (int id, String lastname, String name, String secondName, Date birthday, String gender) {
		this(lastname, name, secondName, birthday, gender);
		this.id = id;
	}


	// Getters
	public int getId() {
		return id;
	}

	public String getLastname() {
		return lastname;
	}

	public String getName() {
		return name;
	}

	public String getSecondName() {
		return secondName;
	}

	public Date getBirthday() {
		return birthday;
	}

	public String getGender() {
		return gender;
	}

	// Setters


	// Преобразование объекта в строку
	public String toString() {
		return String.format("%s %s %s %s %s %s\n", id, lastname, name, secondName, birthday, gender);
	}


 }
```
___
 - # Задание:
   - ### Написать класс Message на языке Java с полями id, title, text, date.
   ### [Вставить ссылку на форму опроса](https://....)

___

### класс *DBAccess*, для связи с БД:
```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.Date;
import java.sql.ResultSet;
import java.util.List;
import java.util.ArrayList;
import java.io.*;

class DBAccess {
	// TODO: Вынести в конфигурационный файл
	final static String driver = "org.h2.Driver";
    final static String dbSource = "jdbc:h2:h:/test";
    final static String login = "sa";
    final static String password = "";

    public List<Person> getAllPersons() {
    	// String query = "SELECT * FROM PERSON LIMIT 100, 200";
    	String query = "SELECT * FROM PERSON";
    	List<Person> persons = new ArrayList<>();
        try (Connection connection = DriverManager.getConnection(dbSource, login, password);
            Statement statement = connection.createStatement()) {
            Class.forName(driver).getDeclaredConstructor().newInstance();
            ResultSet res = statement.executeQuery(query);
            while (res.next()) {
            	persons.add(new Person(
            		res.getInt(1), 
            		res.getString(2),
            		res.getString(3),
            		res.getString(4),
            		res.getDate(5),
            		res.getString(6)));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return persons;   	
    }

}
```

### Пример работы с БД:
```Java
import java.util.*;

class TestDBConnection {
	public static void main(String[] args) {
		List<Person> persons = new DBAccess().getAllPersons();
		persons.stream()
		.skip(100)
		.limit(100)
		// .filter(p -> p.getLastname().contains("Вас"))
		.forEach(System.out::println);
	}
}
```
