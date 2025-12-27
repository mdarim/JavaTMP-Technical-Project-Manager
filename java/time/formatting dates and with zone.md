Sure! Here are **Java code examples** showing how to format dates and times using `java.time` with **custom formats** and **time-zones** (using `ZoneId`), which is the modern and recommended API for date/time in Java. ([Oracle Docs][1])

---

## ✅ **1. Formatting `ZonedDateTime` with a Custom Pattern & ZoneId**

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

public class ZonedDateTimeFormatExample {
    public static void main(String[] args) {
        // Get current datetime in a specific time zone
        ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("Europe/Paris"));

        // Create a formatter with a custom pattern
        DateTimeFormatter formatter =
            DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss z");

        // Format the ZonedDateTime
        String formatted = zdt.format(formatter);

        System.out.println("Formatted ZonedDateTime: " + formatted);
    }
}
```

➡️ This prints something like:

```
Formatted ZonedDateTime: 2025-12-26 20:17:34 CET
```

The pattern includes:

* `yyyy` – 4-digit year
* `MM` – month
* `dd` – day
* `HH:mm:ss` – hour/minute/second
* `z` – short time-zone name (like CET) ([Oracle Docs][1])

---

## ✅ **2. Formatting `ZonedDateTime` with Full Time-Zone Info**

You can also include the full zone ID (e.g., `Europe/Paris`) in the formatted string:

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

public class FullZoneFormat {
    public static void main(String[] args) {
        ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("Asia/Tokyo"));

        DateTimeFormatter formatter = 
            DateTimeFormatter.ofPattern("uuuu-MM-dd'T'HH:mm:ss VV");

        System.out.println("Tokyo Time: " + zdt.format(formatter));
    }
}
```

Here:

* `VV` prints the full **ZoneId** (e.g., `Asia/Tokyo`)
* `T` is just a literal character (commonly used in ISO formats)
  ➡️ Example output: `2025-12-26T23:34:56 Asia/Tokyo` ([Oracle Docs][1])

---

## ✅ **3. Using Predefined Formatters**

Java has built-in ISO standards you can use without defining a custom pattern:

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

public class IsoFormatExample {
    public static void main(String[] args) {
        ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("America/New_York"));

        // ISO_DATE_TIME includes offset but not the zone name
        String iso = zdt.format(DateTimeFormatter.ISO_DATE_TIME);

        // ISO_ZONED_DATE_TIME includes offset + zone ID
        String isoZoned = zdt.format(DateTimeFormatter.ISO_ZONED_DATE_TIME);

        System.out.println("ISO_DATE_TIME: " + iso);
        System.out.println("ISO_ZONED_DATE_TIME: " + isoZoned);
    }
}
```

Output examples:

```
ISO_DATE_TIME: 2025-12-26T15:45:12-05:00
ISO_ZONED_DATE_TIME: 2025-12-26T15:45:12-05:00[America/New_York]
```

👉 Predefined constants like `ISO_DATE_TIME` and `ISO_ZONED_DATE_TIME` are very useful for standard formats. ([Oracle Docs][1])

---

## 🧠 Notes

* `ZonedDateTime` has both time-zone **offset** and full **ZoneId** info. You can tailor what gets printed by using pattern letters such as `Z`, `z`, or `VV`. ([Oracle Docs][1])
* Patterns are case-sensitive — e.g., `HH` is hours in 24-hour format, `hh` is 12-hour. ([Oracle Docs][1])

---

Let me know if you want **parsing examples** (converting a string back to a date/time object), or custom locale formatting!

[1]: https://docs.oracle.com/en/java/javase/23/docs/api/java.base/java/time/format/DateTimeFormatter.html?utm_source=chatgpt.com "DateTimeFormatter (Java SE 23 & JDK 23)"

Here are **Java code examples** showing how to **parse date/time strings** into Java **`java.time`** objects using `DateTimeFormatter`, including examples with **time zones (`ZoneId`)**. Java’s `DateTimeFormatter` is a thread-safe, immutable formatter used to parse and format dates and times. ([Oracle Docs][1])

---

## 🔹 1. **Parsing a Simple Date String to `LocalDate`**

```java
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;

public class ParseLocalDateExample {
    public static void main(String[] args) {
        String dateStr = "28-08-2025";
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd-MM-yyyy");

        try {
            LocalDate date = LocalDate.parse(dateStr, formatter);
            System.out.println("Parsed LocalDate: " + date);
        } catch (DateTimeParseException e) {
            System.out.println("Failed to parse date: " + e.getMessage());
        }
    }
}
```

This will output something like:

```
Parsed LocalDate: 2025-08-28
```

> The parse method expects the string to match the pattern exactly (e.g., `"dd-MM-yyyy"`). ([Java67][2])

---

## 🔹 2. **Parsing a Date-Time String to `LocalDateTime`**

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class ParseLocalDateTime {
    public static void main(String[] args) {
        String dateTimeStr = "08/06/2024 14:30:45";
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm:ss");

        LocalDateTime dateTime = LocalDateTime.parse(dateTimeStr, formatter);
        System.out.println("Parsed LocalDateTime: " + dateTime);
    }
}
```

This produces:

```
Parsed LocalDateTime: 2024-06-08T14:30:45
```

Use `HH` for 24-hour time. ([Java67][3])

---

## 🔹 3. **Parsing a Zoned Date-Time String to `ZonedDateTime`**

If your input string *contains time-zone information*, you can parse it to a `ZonedDateTime`.

### ✅ Using the default ISO formatter

```java
import java.time.ZonedDateTime;

public class ParseZonedDateTimeIso {
    public static void main(String[] args) {
        String input = "2018-12-26T20:28:33.213+05:30[Asia/Calcutta]";
        ZonedDateTime zdt = ZonedDateTime.parse(input);
        System.out.println("Parsed ZonedDateTime: " + zdt);
    }
}
```

Java will use the *ISO-8601 with zone* format by default. ([GeeksforGeeks][4])

---

## 🔹 4. **Parsing a Custom Pattern with Time-Zone**

If your input date has a *non-ISO* format, you can define a custom formatter:

```java
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;

public class ParseCustomZoned {
    public static void main(String[] args) {
        String input = "08/06/2024 14:30:45 UTC";
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm:ss z");

        try {
            ZonedDateTime zonedDateTime = ZonedDateTime.parse(input, formatter);
            System.out.println("Parsed ZonedDateTime: " + zonedDateTime);
        } catch (DateTimeParseException e) {
            System.out.println("Failed to parse zoned date-time: " + e.getMessage());
        }
    }
}
```

This approach works when the input includes a textual time-zone like `"UTC"` or `"PST"`. ([Java67][3])

---

## 🧠 **Notes**

* If the input string **doesn’t contain zone info**, Java will throw a `DateTimeParseException` when parsing into `ZonedDateTime`. Either include valid zone data or parse into `LocalDateTime` first and then attach a time zone later (using `.atZone(ZoneId.of(...))`). ([baeldung.com][5])
* Default parse methods (like `LocalDate.parse(str)` or `LocalDateTime.parse(str)`) use *standard ISO formats* unless you supply a custom `DateTimeFormatter`. ([Oracle Docs][1])

---

If you want, I can also show **how to combine parsing and formatting** (e.g., parse a string and then reformat it to a different pattern or locale). Just ask!

[1]: https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/time/format/DateTimeFormatter.html?utm_source=chatgpt.com "DateTimeFormatter (Java SE 11 & JDK 11 )"
[2]: https://www.java67.com/2019/01/10-examples-of-format-and-parse-dates-in-java.html?utm_source=chatgpt.com "10 Examples to DateTimeFormatter in Java 8 to Parse, Format LocalDate and LocalTime | Java67"
[3]: https://www.java67.com/2024/06/how-to-convert-string-to-localdate.html?utm_source=chatgpt.com "How to Convert String to LocalDate, LocalTime, LocalDateTime and ZonedDateTime in Java? Example Tutorial | Java67"
[4]: https://www.geeksforgeeks.org/java/zoneddatetime-parse-method-in-java-with-examples/?utm_source=chatgpt.com "ZonedDateTime parse() method in Java with Examples - GeeksforGeeks"
[5]: https://www.baeldung.com/java-format-zoned-datetime-string?utm_source=chatgpt.com "Format ZonedDateTime to String | Baeldung"
