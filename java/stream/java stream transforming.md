Below is an **advanced Java Stream example** showing how to convert a list of **Transaction** objects into a list of **Summary** objects (or other summary structures) using the Stream API. The key idea is to use `map()` to transform each transaction into another type and, optionally, use collectors to aggregate or summarize results. ([GeeksforGeeks][1])

---

## ✅ Example: Convert `Transaction` to `TransactionSummary`

### 📌 Domain Classes

```java
public class Transaction {
    private final String id;
    private final String category;
    private final double amount;
    private final LocalDate date;
    private final String status;

    public Transaction(String id, String category, double amount, LocalDate date, String status) {
        this.id = id;
        this.category = category;
        this.amount = amount;
        this.date = date;
        this.status = status;
    }

    public String getId() { return id; }
    public String getCategory() { return category; }
    public double getAmount() { return amount; }
    public LocalDate getDate() { return date; }
    public String getStatus() { return status; }
}
```

```java
public class TransactionSummary {
    private final String id;
    private final String category;
    private final double amount;

    public TransactionSummary(String id, String category, double amount) {
        this.id       = id;
        this.category = category;
        this.amount   = amount;
    }

    @Override
    public String toString() {
        return "Summary[id=" + id + ", category=" + category + ", amount=" + amount + "]";
    }
}
```

---

## 🧠 Basic Stream Mapping

Transform a list of `Transaction` to a list of `TransactionSummary`:

```java
List<TransactionSummary> summaries = transactions.stream()
    .map(tx -> new TransactionSummary(
            tx.getId(),
            tx.getCategory(),
            tx.getAmount()
    ))
    .collect(Collectors.toList());
```

🔹 Here `.map()` applies a function to each element, converting each `Transaction` into a `TransactionSummary`.
🔹 `.collect(Collectors.toList())` gathers the stream into a `List<TransactionSummary>`. ([GeeksforGeeks][1])

---

## 📌 Filtering Before Mapping

Only summarize valid transactions:

```java
List<TransactionSummary> validSummaries = transactions.stream()
    .filter(tx -> "VALID".equals(tx.getStatus()))  // filter step
    .map(tx -> new TransactionSummary(tx.getId(), tx.getCategory(), tx.getAmount()))
    .collect(Collectors.toList());
```

---

## 📊 Advanced: Grouping & Summarizing

Suppose you want **total amounts by category** (a summary per category):

```java
Map<String, Double> totalsByCategory = transactions.stream()
    .collect(Collectors.groupingBy(
        Transaction::getCategory,
        Collectors.summingDouble(Transaction::getAmount)
    ));
```

This gives you a map like `{Food=1234.50, Travel=987.65}` — total amounts by category. ([GeeksforGeeks][1])

---

## 📈 More Complex Summary Object

If your `Summary` object holds multiple fields (e.g., count + total), you can do:

```java
public class CategorySummary {
    private final String category;
    private final long count;
    private final double totalAmount;

    public CategorySummary(String category, long count, double totalAmount) {
        this.category    = category;
        this.count       = count;
        this.totalAmount = totalAmount;
    }
}

List<CategorySummary> categorySummaries =
    transactions.stream()
        .collect(Collectors.groupingBy(Transaction::getCategory))
        .entrySet()
        .stream()
        .map(entry -> new CategorySummary(
                entry.getKey(),
                entry.getValue().size(),
                entry.getValue().stream().mapToDouble(Transaction::getAmount).sum()
        ))
        .collect(Collectors.toList());
```

---

## 🧩 Parallel Streams (Optional)

For large lists you can use parallel streams:

```java
List<TransactionSummary> parallelSummaries = transactions
    .parallelStream()
    .map(tx -> new TransactionSummary(tx.getId(), tx.getCategory(), tx.getAmount()))
    .collect(Collectors.toList());
```

Be cautious if your mapping logic has side effects — streams are best with **pure functions**.

---

## 🔍 Summary of Techniques

| Use Case            | Stream API Pattern                |
|---------------------|-----------------------------------|
| Convert objects     | `map()` + constructor             |
| Filter then convert | `filter()` + `map()`              |
| Group + aggregate   | `collect(groupingBy(...))`        |
| Complex summary     | Combine grouping + map to summary |
| Parallel processing | `parallelStream()`                |

---

If you want, I can tailor this to your exact domain model (fields you have and the summary you need). Just share your actual class definitions!

[1]: https://www.geeksforgeeks.org/java-8-stream-tutorial/?utm_source=chatgpt.com "Java 8 Stream Tutorial - GeeksforGeeks"

```java
package com.javatmp.demo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.Arrays;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

@SpringBootTest
@Slf4j
public class JavaStreamSummaryTests {
    @Test
    void mapStreamSummaryTest() {
        List<Amount> transactions = Arrays.asList(
                new Amount("Food", 50.0, 1),
                new Amount("Transport", 20.0, 10),
                new Amount("IT", 20.0, 10),
                new Amount("Food", 30.0, 2),
                new Amount("Transport", 10.0, 1),
                new Amount("Food", 15.0, 1),
                new Amount("IT", 15.0, 1)
        );
        Map<String, Summary> summaryMap = transactions.stream()
                .collect(Collectors.groupingBy(
                        Amount::getCategory, // Key Mapper: Group by category name
                        Collectors.reducing( // Downstream Collector: Reduce elements in the group
                                new Summary(0.0, 0), // Identity: Starting point (initial empty Summary object)
                                amount -> new Summary(amount.getValue(), amount.getCount()), // Mapper: Transform Amount to a Summary
                                (sum1, sum2) ->
                                        new Summary(
                                                sum1.getTotalValue() + sum2.getTotalValue(),
                                                sum1.getTotalCount() + sum2.getTotalCount()) // Accumulator: Merge two Summary objects
                        )
                ));

        System.out.println(summaryMap);

    }
}
@Data
@AllArgsConstructor
class Amount {
    private String category;
    private double value;
    private int count;
}
@Data
@AllArgsConstructor
class Summary {
    private double totalValue;
    private int totalCount;
}
```