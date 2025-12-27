Here’s a **practical Java example** showing how to **parse a date/time string and then re-format it** into a different pattern or with a different time-zone using the modern `java.time` API (`DateTimeFormatter`, `LocalDate`, `ZonedDateTime`, etc.). This builds on the fact that `DateTimeFormatter` supports both parsing and formatting based on patterns. ([Oracle Docs][1])

---

## ✅ **Example 1 — Parse & Reformat a Date String**

This example parses a simple date string (e.g., `"2025-01-15"`) and then formats it in a different style:

```java
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public class ParseAndReformatExample {
    public static void main(String[] args) {
        // Input string
        String inputDate = "2025-01-15";

        // Parse with input formatter
        DateTimeFormatter inputFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
        LocalDate date = LocalDate.parse(inputDate, inputFormatter);

        // Format with another formatter
        DateTimeFormatter outputFormatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");
        String formatted = date.format(outputFormatter);

        System.out.println("Reformatted date: " + formatted);
    }
}
```

**Output:**

```
Reformatted date: 15/01/2025
```

✔ First `LocalDate.parse()` creates a `LocalDate` from the string.
✔ Then `format()` with a new pattern outputs the date in a different style. ([php.cn][2])

---

## ✅ **Example 2 — Parse with Time & Time-Zone, Then Re-Format**

This example parses a date-time string that includes time-zone information, and then formats it in a target pattern and time zone:

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

public class ParseAndReformatZone {
    public static void main(String[] args) {
        // Example ISO string with offset and zone ID
        String input = "2025-12-26T15:30:00+01:00[Europe/London]";

        // Parse using built-in ISO_ZONED_DATE_TIME
        ZonedDateTime zonedDateTime = ZonedDateTime.parse(input, DateTimeFormatter.ISO_ZONED_DATE_TIME);

        // Reformat with a custom pattern in the same zone
        DateTimeFormatter outputFmt = DateTimeFormatter.ofPattern("dd MMM yyyy HH:mm z");
        String formatted = zonedDateTime.format(outputFmt);

        System.out.println("Formatted (same zone): " + formatted);

        // Convert to another zone
        ZonedDateTime nyTime = zonedDateTime.withZoneSameInstant(ZoneId.of("America/New_York"));
        System.out.println("Formatted (New York): " + nyTime.format(outputFmt));
    }
}
```

✔ `ZonedDateTime.parse(...)` reads the string including the zone.
✔ Using a custom pattern like `"dd MMM yyyy HH:mm z"` prints day, abbreviated month, year, 24-hour time, and short zone name.
✔ You can convert the same moment to a **different time zone** using `withZoneSameInstant(...)`. ([Oracle Docs][1])

---

## 🧠 Notes on Patterns

* The letters in the patterns define specific fields:

    * `dd` → day of month
    * `MM` → numeric month
    * `MMM` → short month name (Jan, Feb, …)
    * `yyyy` → year
    * `HH` → 24-hour hour
    * `mm` → minute
    * `z` → short time-zone name (e.g., CET) ([Oracle Docs][1])

* The same formatter can be reused for **parsing or formatting** because `DateTimeFormatter` objects are immutable and thread-safe. ([Oracle Docs][3])

* If the input string contains zone/offset info, use appropriate formatters like `ISO_ZONED_DATE_TIME` or patterns including `z`/`VV`. ([Oracle Docs][1])

---

## 📌 Summary of the Steps

1. **Create an input formatter** matching the input string.
2. **Parse the string** into a date/time object (`LocalDate`, `LocalDateTime`, or `ZonedDateTime`).
3. **Create an output formatter** with the desired pattern.
4. **Format** the parsed object to the target string.

This pattern makes it easy to convert date/time strings from one format (and zone) into another. ([Oracle Docs][3])

---

If you want an example with **locales** (e.g., formatting month names in Arabic or French), just let me know!

[1]: https://docs.oracle.com/en/java/javase/23/docs/api/java.base/java/time/format/DateTimeFormatter.html?utm_source=chatgpt.com "DateTimeFormatter (Java SE 23 & JDK 23)"
[2]: https://m.php.cn/en/faq/1796860006.html?utm_source=chatgpt.com "How to format a date in Java?-javaTutorial-php.cn"
[3]: https://docs.oracle.com/javase/tutorial/datetime/iso/format.html?utm_source=chatgpt.com "Parsing and Formatting (The Java™ Tutorials > Date Time > Standard Calendar)"

Here’s a **practical Java example** showing how to **parse a date/time string and then re-format it** into a different pattern or with a different time-zone using the modern `java.time` API (`DateTimeFormatter`, `LocalDate`, `ZonedDateTime`, etc.). This builds on the fact that `DateTimeFormatter` supports both parsing and formatting based on patterns. ([Oracle Docs][1])

---

## ✅ **Example 1 — Parse & Reformat a Date String**

This example parses a simple date string (e.g., `"2025-01-15"`) and then formats it in a different style:

```java
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public class ParseAndReformatExample {
    public static void main(String[] args) {
        // Input string
        String inputDate = "2025-01-15";

        // Parse with input formatter
        DateTimeFormatter inputFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
        LocalDate date = LocalDate.parse(inputDate, inputFormatter);

        // Format with another formatter
        DateTimeFormatter outputFormatter = DateTimeFormatter.ofPattern("dd/MM/yyyy");
        String formatted = date.format(outputFormatter);

        System.out.println("Reformatted date: " + formatted);
    }
}
```

**Output:**

```
Reformatted date: 15/01/2025
```

✔ First `LocalDate.parse()` creates a `LocalDate` from the string.
✔ Then `format()` with a new pattern outputs the date in a different style. ([php.cn][2])

---

## ✅ **Example 2 — Parse with Time & Time-Zone, Then Re-Format**

This example parses a date-time string that includes time-zone information, and then formats it in a target pattern and time zone:

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

public class ParseAndReformatZone {
    public static void main(String[] args) {
        // Example ISO string with offset and zone ID
        String input = "2025-12-26T15:30:00+01:00[Europe/London]";

        // Parse using built-in ISO_ZONED_DATE_TIME
        ZonedDateTime zonedDateTime = ZonedDateTime.parse(input, DateTimeFormatter.ISO_ZONED_DATE_TIME);

        // Reformat with a custom pattern in the same zone
        DateTimeFormatter outputFmt = DateTimeFormatter.ofPattern("dd MMM yyyy HH:mm z");
        String formatted = zonedDateTime.format(outputFmt);

        System.out.println("Formatted (same zone): " + formatted);

        // Convert to another zone
        ZonedDateTime nyTime = zonedDateTime.withZoneSameInstant(ZoneId.of("America/New_York"));
        System.out.println("Formatted (New York): " + nyTime.format(outputFmt));
    }
}
```

✔ `ZonedDateTime.parse(...)` reads the string including the zone.
✔ Using a custom pattern like `"dd MMM yyyy HH:mm z"` prints day, abbreviated month, year, 24-hour time, and short zone name.
✔ You can convert the same moment to a **different time zone** using `withZoneSameInstant(...)`. ([Oracle Docs][1])

---

## 🧠 Notes on Patterns

* The letters in the patterns define specific fields:

    * `dd` → day of month
    * `MM` → numeric month
    * `MMM` → short month name (Jan, Feb, …)
    * `yyyy` → year
    * `HH` → 24-hour hour
    * `mm` → minute
    * `z` → short time-zone name (e.g., CET) ([Oracle Docs][1])

* The same formatter can be reused for **parsing or formatting** because `DateTimeFormatter` objects are immutable and thread-safe. ([Oracle Docs][3])

* If the input string contains zone/offset info, use appropriate formatters like `ISO_ZONED_DATE_TIME` or patterns including `z`/`VV`. ([Oracle Docs][1])

---

## 📌 Summary of the Steps

1. **Create an input formatter** matching the input string.
2. **Parse the string** into a date/time object (`LocalDate`, `LocalDateTime`, or `ZonedDateTime`).
3. **Create an output formatter** with the desired pattern.
4. **Format** the parsed object to the target string.

This pattern makes it easy to convert date/time strings from one format (and zone) into another. ([Oracle Docs][3])

---

If you want an example with **locales** (e.g., formatting month names in Arabic or French), just let me know!

[1]: https://docs.oracle.com/en/java/javase/23/docs/api/java.base/java/time/format/DateTimeFormatter.html?utm_source=chatgpt.com "DateTimeFormatter (Java SE 23 & JDK 23)"
[2]: https://m.php.cn/en/faq/1796860006.html?utm_source=chatgpt.com "How to format a date in Java?-javaTutorial-php.cn"
[3]: https://docs.oracle.com/javase/tutorial/datetime/iso/format.html?utm_source=chatgpt.com "Parsing and Formatting (The Java™ Tutorials > Date Time > Standard Calendar)"

Here’s how you can **parse a date/time string and then re-format it using the **Arabic locale** (`Locale("ar", ...)`) in Java’s modern `java.time` API — including examples that will show **localized month names and localized formatting**. ([Stack Overflow][1])

---

## ✅ **1. Re-formatting with Arabic Locale (Localized Text)**

If you want the **month names and day names in Arabic**, you can use a localized formatter via `DateTimeFormatter.ofLocalizedDateTime(...)` with `.withLocale(...)`:

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.Locale;

public class ArabicLocaleFormatting {
    public static void main(String[] args) {
        // Example ZonedDateTime
        ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("Asia/Riyadh"));

        // Create a localized formatter for full date & time in Arabic
        Locale arabicLocale = new Locale("ar", "SA"); // Arabic / Saudi Arabia
        DateTimeFormatter formatter = 
            DateTimeFormatter.ofLocalizedDateTime(FormatStyle.FULL, FormatStyle.SHORT)
                             .withLocale(arabicLocale);

        // Format with Arabic locale
        String formatted = zdt.format(formatter);
        System.out.println("Localized in Arabic: " + formatted);
    }
}
```

✔ This uses a **localized style** (`FormatStyle.FULL` etc.) which prints month/day names in Arabic.
✔ Locale is built manually as `new Locale("ar", "SA")` since Java doesn’t have a built-in constant for `Locale` Arabic. ([Stack Overflow][1])

---

## ✅ **2. Parsing & Reformatting with Arabic Locale and Custom Pattern**

You can also parse a string first, then format it with an Arabic locale and pattern:

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Locale;

public class ParseAndFormatArabic {
    public static void main(String[] args) {
        // Input date-time string
        String input = "2025-12-26 14:30:00";

        // Parser using ISO-like pattern
        DateTimeFormatter parser = 
            DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        LocalDateTime dt = LocalDateTime.parse(input, parser);

        // Arabic locale formatter with custom pattern
        Locale arabicLocale = new Locale("ar", "SA");
        DateTimeFormatter formatter = 
            DateTimeFormatter.ofPattern("dd MMMM uuuu HH:mm", arabicLocale);

        // Print formatted (month name in Arabic)
        System.out.println("Formatted (Arabic): " + dt.format(formatter));
    }
}
```

Output example might look like:

```
Formatted (Arabic): 26 ديسمبر 2025 14:30
```

✔ Here `MMMM` produces the **full month name** (like “ديسمبر” for December) in Arabic. ([Oracle Docs][2])

---

## 🧠 Notes on Numerals (Western vs Eastern Arabic)

By default, `java.time.format.DateTimeFormatter` will format **numbers** (like days, years) using Western numerals (e.g., `26`, `2025`).
If you want **Eastern Arabic numerals** (like “٢٦”, “٢٠٢٥”), you *may* need to use `DateTimeFormatter.withDecimalStyle(...)` with a `DecimalStyle` that has the zero digit set to an Arabic zero (`'٠'`). ([Stack Overflow][3])

Example snippet showing that idea:

```java
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.DecimalStyle;
import java.util.Locale;

Locale ar = new Locale("ar", "SA");
DecimalStyle arabicDigits = DecimalStyle.of(ar).withZeroDigit('٠');

DateTimeFormatter arabicFormatter = 
    DateTimeFormatter.ofPattern("dd/MM/yyyy", ar)
                     .withDecimalStyle(arabicDigits);

System.out.println(LocalDate.now().format(arabicFormatter));
```

✔ This can yield output with **Arabic-Indic digits**, e.g., `١٥/٠٤/٢٠٢٥`. ([Stack Overflow][3])

---

## 📌 Summary

| Goal                                 | API                                                    |
|--------------------------------------|--------------------------------------------------------|
| Localize month/day names to Arabic   | `DateTimeFormatter.withLocale(new Locale("ar", "SA"))` |
| Use standard localized style formats | `ofLocalizedDateTime(...).withLocale(...)`             |
| Show **Arabic-Indic digits**         | Add `.withDecimalStyle(...)` with Arabic zero digit    |

---

Want an example that **parses text with Arabic month names back into a date/time object**? (That’s a bit trickier but doable.) Just ask!

[1]: https://stackoverflow.com/questions/23612392/arabic-date-on-android-app?utm_source=chatgpt.com "java - Arabic Date on Android App - Stack Overflow"
[2]: https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/time/format/DateTimeFormatter.html?utm_source=chatgpt.com "DateTimeFormatter (Java SE 25 & JDK 25)"
[3]: https://stackoverflow.com/questions/49839150/how-to-convert-date-from-english-to-arabic?utm_source=chatgpt.com "java - How to convert date from English to Arabic - Stack Overflow"
