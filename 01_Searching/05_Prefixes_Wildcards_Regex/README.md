### 05 Prefixes, Wildcards, & Regular Expressions: Flexible Search Techniques in Elasticsearch

#### Introduction

Elasticsearch offers advanced search capabilities that allow you to perform flexible and powerful searches using prefixes, wildcards, and regular expressions. These techniques enable you to find partial matches, variations in text, or specific patterns within your data. This section will guide you through using these search techniques, with a focus on their syntax and practical applications.

#### Prefix Queries

A **prefix query** allows you to search for documents where a field starts with a specific prefix. This type of query is useful for autocomplete features or when you need to match the beginning of a string.

##### Syntax:

```json
{
  "query": {
    "prefix": {
      "<field_name>": "<prefix_value>"
    }
  }
}
```

##### Example: Searching for Products by Prefix in `kibana_sample_data_ecommerce`

To find all products in the `kibana_sample_data_ecommerce` index where the product name starts with "Shirt", you can use the following query:

```json
GET /kibana_sample_data_ecommerce/_search
{
  "query": {
    "prefix": {
      "products.product_name.keyword": "Shirt"
    }
  }
}
```

This query will return all documents where the `products.product_name.keyword` field begins with "Shirt", such as "Shirt - Blue" or "Shirt Dress - Red".

#### Wildcard Queries

**Wildcard queries** allow for flexible pattern matching within text fields. The `*` character represents zero or more characters, while the `?` character represents a single character. Wildcard queries are useful when you need to search for variations of a term or when the exact term is not fully known.

##### Syntax:

```json
{
  "query": {
    "wildcard": {
      "<field_name>": "<pattern>"
    }
  }
}
```

##### Example: Using Wildcards in `kibana_sample_data_flights`

To find all flights where the carrier name contains "Air" anywhere within the string, you can use the following query:

```json
GET /kibana_sample_data_flights/_search
{
  "query": {
    "wildcard": {
      "Carrier": "*Air*"
    }
  }
}
```

This query matches any carrier name that includes "Air" in any position, such as "Airline A", "British Airways", or "Air Canada".

#### Regular Expressions

**Regular expressions** (regex) provide the most powerful method for pattern matching within text fields. They allow you to define complex search patterns using a variety of symbols and operators. Regular expressions can handle very intricate patterns but are also useful for simpler tasks like matching specific keywords or phrases.

##### Syntax:

```json
{
  "query": {
    "regexp": {
      "<field_name>": "<regex_pattern>"
    }
  }
}
```

##### Example 1: Matching All Email Addresses from a Specific Domain

If you want to find all documents in an index where the `email` field contains addresses from the domain "example.com", you can use the following regex:

```json
{
  "query": {
    "regexp": {
      "email": ".*@example\\.com"
    }
  }
}
```

- **Explanation**:
  - **`.*`**: Matches any character (except for line terminators) zero or more times.
  - **`@example\\.com`**: Matches the literal string "@example.com". The backslash (`\\`) is used to escape the dot (`.`) since it is a special character in regex.

This query will return all documents where the `email` field ends with "@example.com".

##### Example 2: Matching Email Addresses with a Hyphen in the Domain

To find all email addresses where the domain contains a hyphen, such as "daniels-family.zzz" in the email "clarice@daniels-family.zzz", you can use the following regex:

```json
{
  "query": {
    "regexp": {
      "email": ".*@[a-zA-Z0-9-]+\\.[a-zA-Z]{2,}"
    }
  }
}
```

Let's break down the regular expression pattern used in the example to better understand how each part works:

### Regular Expression: `.*@[a-zA-Z0-9-]+\\.[a-zA-Z]{2,}`

This regular expression is designed to match email addresses, particularly focusing on those that may contain a hyphen in the domain name (e.g., "clarice@daniels-family.zzz"). Here's a detailed explanation of each component:

#### 1. `.*@`
- **`.` (dot)**: This matches **any single character** except for line terminators (like newline characters). In the context of this regex, it represents any character in the local part of an email address (i.e., before the `@` symbol).
- **`*` (asterisk)**: This means **"zero or more occurrences"** of the preceding element, which in this case is the dot (`.`). Therefore, `.*` matches **any sequence of characters** (including an empty string) in the email's local part.
- **`@`**: This matches the **literal "@" symbol**. In an email address, the `@` separates the local part (before `@`) from the domain part (after `@`).

**Summary**: `.*@` together matches any sequence of characters that come before the `@` symbol in an email address.

#### 2. `[a-zA-Z0-9-]+`
- **`[a-zA-Z0-9-]`**: This is a **character class** that matches any **single character** that is either:
  - A lowercase letter (`a-z`)
  - An uppercase letter (`A-Z`)
  - A digit (`0-9`)
  - A hyphen (`-`)

- **`+` (plus sign)**: This means **"one or more occurrences"** of the preceding element. In this case, `[a-zA-Z0-9-]+` matches one or more characters that are letters, numbers, or hyphens.

**Summary**: `[a-zA-Z0-9-]+` matches the domain name part of an email address, allowing it to include letters, numbers, and hyphens.

#### 3. `\\.[a-zA-Z]{2,}`
- **`\\.`**: This matches a **literal dot (`.`)** in the domain name. The backslash (`\\`) is necessary to escape the dot because, in regex, a dot normally matches any single character. By escaping it, we ensure that the regex matches an actual period.

- **`[a-zA-Z]{2,}`**: This is another character class, but it is followed by a **quantifier**:
  - `[a-zA-Z]` matches any **letter**, whether lowercase or uppercase.
  - `{2,}` specifies that the preceding element (in this case, a letter) must appear at least **two times**, with no upper limit specified. This is typically used to match the top-level domain (TLD) of an email, such as "com", "org", "net", or any other domain that is at least two characters long.

**Summary**: `\\.[a-zA-Z]{2,}` matches the period followed by a top-level domain, ensuring that the TLD is at least two characters long.

### Putting It All Together

- **`.*@`**: Matches any sequence of characters before the `@` symbol, corresponding to the local part of the email address.
- **`[a-zA-Z0-9-]+`**: Matches the domain name part of the email, allowing it to include letters, numbers, and hyphens.
- **`\\.[a-zA-Z]{2,}`**: Matches the dot followed by the TLD, ensuring the TLD is at least two characters long.

### Example Matches
- **`clarice@daniels-family.zzz`**: Matches, as the domain includes a hyphen and a valid TLD.
- **`john.doe@example-site.com`**: Matches, as it fits the pattern with a hyphen in the domain and "com" as the TLD.

This regex is versatile and effective for matching email addresses with various domain structures, particularly those that include hyphens, which are common in many modern domains.

#### Considerations and Performance

While prefix, wildcard, and regular expression queries are powerful tools, they can be resource-intensive, especially on large datasets. These queries might take longer to execute and consume more computational resources. Regular expressions, in particular, should be used cautiously and optimized for performance.

- **Prefix Queries**: Generally fast and efficient, especially when applied to keyword fields.
- **Wildcard Queries**: More flexible but can be slower, particularly when the wildcard is at the beginning of the pattern.
- **Regular Expressions**: Extremely versatile but can be the most resource-intensive. Use them for specific needs where other queries are insufficient.

#### Conclusion

Prefixes, wildcards, and regular expressions offer advanced text-searching capabilities in Elasticsearch, enabling you to craft queries that are both powerful and precise. By mastering these techniques, you can tailor your searches to find exactly what you need, even when dealing with complex or variable data.

In the next section, we will explore how to query by field existence, further enhancing your ability to retrieve the most relevant data from your Elasticsearch indices.