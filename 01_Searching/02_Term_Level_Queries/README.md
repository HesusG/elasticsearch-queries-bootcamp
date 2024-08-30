### Term Level Queries in Elasticsearch: A Practical Guide

#### Introduction to Term Level Queries

In Elasticsearch, understanding how to construct and execute queries is essential to leveraging the full power of the search engine. One of the fundamental types of queries you'll encounter is the **Term Level Query**. This type of query is used to search for exact values in specific fields. Unlike full-text queries, which analyze the input and match based on the content's meaning or relevance, term-level queries match the exact terms that are stored in the index.

#### What is a Term?

In Elasticsearch, a **term** is the smallest unit of data that can be indexed and searched. When you index a document in Elasticsearch, the data is tokenized (split into individual terms) and these terms are stored in an inverted index, which allows for fast lookups. For example, in a field that stores a keyword (like `order_id`), the entire value (e.g., "12345") is treated as a single term.

Term-level queries operate on these exact terms. They do not analyze the text before searching, which makes them perfect for fields that require exact matches, such as keywords, IDs, and other non-analyzed fields.

#### Basic Structure of a Term Query

The most basic form of a term query is used to find documents where a field contains an exact term. The structure of a term query is simple:

```json
{
  "query": {
    "term": {
      "<field_name>": {
        "value": "<exact_value>"
      }
    }
  }
}
```

Here’s a breakdown:
- **`<field_name>`**: This is the field in your document where you want to search for the term.
- **`<exact_value>`**: This is the exact term you're searching for in the specified field.

#### Example of a Term Query

Suppose you want to find all documents in an index where the `status` field is exactly "shipped". Your term query would look like this:

```json
{
  "query": {
    "term": {
      "status": {
        "value": "shipped"
      }
    }
  }
}
```

This query tells Elasticsearch to search the `status` field in every document and return those that have the exact term "shipped".

#### When to Use Term Level Queries

Term level queries are best used when you need precise, non-analyzed matching. They are ideal for:
- Matching specific IDs, such as `order_id`, `user_id`, or `flight_number`.
- Filtering by keywords or exact values in fields like `status`, `category`, or `type`.
- Querying against fields that are not analyzed, meaning the text is stored exactly as it is entered.

#### Refreshing Your Knowledge of Kibana

Before diving into executing term-level queries, it’s helpful to recall the basics of using Kibana's Dev Tools. Kibana is the visualization and management interface for Elasticsearch, and Dev Tools is where you can directly interact with Elasticsearch by running queries.

To access Dev Tools:
1. Navigate to your Kibana instance.
2. Click on **Dev Tools** in the left-hand menu.
3. The Console interface will appear, where you can input and execute queries.

The Console is split into two parts: the left side is where you write your queries in JSON format, and the right side displays the response from Elasticsearch. You can execute your queries by clicking the green play button or pressing `Ctrl + Enter`.

Understanding how to effectively use term queries in Elasticsearch is a powerful skill, especially when working with structured data that requires precise filtering and matching. Now that you have a foundational understanding, we will explore how to apply these concepts to the specific indices you'll be working with in the next part of this guide.

### Term Level Queries in Action: Querying Our Indices

In the previous section, we discussed the basics of term-level queries in Elasticsearch, focusing on what a term is and how to structure a basic term query. Now, let's put this knowledge into action by querying the three indices you're working with: `kibana_sample_data_ecommerce`, `kibana_sample_data_flights`, and `.ds-kibana_sample_data_logs-2024.08.29-000001`.

#### Querying the `kibana_sample_data_ecommerce` Index

The `kibana_sample_data_ecommerce` index contains data related to eCommerce transactions, such as customer information, order details, and product data. A term query is particularly useful when you want to retrieve documents that match specific criteria, such as an exact order ID or customer ID.

**Example 1: Find Orders by Customer ID**

Suppose you want to find all orders made by a customer with the ID "12". You can use a term query to achieve this:

```json
{
  "query": {
    "term": {
      "customer_id": {
        "value": "12"
      }
    }
  }
}
```

This query will return all documents in the `kibana_sample_data_ecommerce` index where the `customer_id` is exactly "12".

<details>
  <summary>Sample Response</summary>

{
  "took": 3,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 135,
      "relation": "eq"
    },
    "max_score": 3.5412264,
    "hits": [
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "_HYjoJEBs_OPuIlVtSRb",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Clothing",
            "Women's Accessories"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte Morris",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "Morris",
          "customer_phone": "",
          "day_of_week": "Tuesday",
          "day_of_week_i": 1,
          "email": "brigitte@morris-family.zzz",
          "manufacturer": [
            "Gnomehouse",
            "Tigress Enterprises"
          ],
          "order_date": "2024-09-03T12:15:50+00:00",
          "order_id": 576696,
          "products": [
            {
              "base_price": 59.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Gnomehouse",
              "tax_amount": 0,
              "product_id": 22364,
              "category": "Women's Clothing",
              "sku": "ZO0353103531",
              "taxless_price": 59.99,
              "unit_discount_amount": 0,
              "min_price": 31.79,
              "_id": "sold_product_576696_22364",
              "discount_amount": 0,
              "created_on": "2016-12-20T12:15:50+00:00",
              "product_name": "Cardigan - black",
              "price": 59.99,
              "taxful_price": 59.99,
              "base_unit_price": 59.99
            },
            {
              "base_price": 14.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Tigress Enterprises",
              "tax_amount": 0,
              "product_id": 16364,
              "category": "Women's Accessories",
              "sku": "ZO0079500795",
              "taxless_price": 14.99,
              "unit_discount_amount": 0,
              "min_price": 7.79,
              "_id": "sold_product_576696_16364",
              "discount_amount": 0,
              "created_on": "2016-12-20T12:15:50+00:00",
              "product_name": "Watch - black",
              "price": 14.99,
              "taxful_price": 14.99,
              "base_unit_price": 14.99
            }
          ],
          "sku": [
            "ZO0353103531",
            "ZO0079500795"
          ],
          "taxful_total_price": 74.98,
          "taxless_total_price": 74.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      },
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "AXYjoJEBs_OPuIlVtSVb",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Clothing"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte Butler",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "Butler",
          "customer_phone": "",
          "day_of_week": "Friday",
          "day_of_week_i": 4,
          "email": "brigitte@butler-family.zzz",
          "manufacturer": [
            "Spherecords",
            "Champion Arts"
          ],
          "order_date": "2024-09-06T01:33:36+00:00",
          "order_id": 580088,
          "products": [
            {
              "base_price": 24.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Spherecords",
              "tax_amount": 0,
              "product_id": 24526,
              "category": "Women's Clothing",
              "sku": "ZO0655206552",
              "taxless_price": 24.99,
              "unit_discount_amount": 0,
              "min_price": 11.75,
              "_id": "sold_product_580088_24526",
              "discount_amount": 0,
              "created_on": "2016-12-23T01:33:36+00:00",
              "product_name": "Cardigan - black",
              "price": 24.99,
              "taxful_price": 24.99,
              "base_unit_price": 24.99
            },
            {
              "base_price": 24.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Champion Arts",
              "tax_amount": 0,
              "product_id": 11238,
              "category": "Women's Clothing",
              "sku": "ZO0489604896",
              "taxless_price": 24.99,
              "unit_discount_amount": 0,
              "min_price": 11.75,
              "_id": "sold_product_580088_11238",
              "discount_amount": 0,
              "created_on": "2016-12-23T01:33:36+00:00",
              "product_name": "Denim dress - black denim",
              "price": 24.99,
              "taxful_price": 24.99,
              "base_unit_price": 24.99
            }
          ],
          "sku": [
            "ZO0655206552",
            "ZO0489604896"
          ],
          "taxful_total_price": 49.98,
          "taxless_total_price": 49.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      },
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "BXYjoJEBs_OPuIlVtSVb",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Shoes"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte King",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "King",
          "customer_phone": "",
          "day_of_week": "Thursday",
          "day_of_week_i": 3,
          "email": "brigitte@king-family.zzz",
          "manufacturer": [
            "Pyramidustries active",
            "Angeldale"
          ],
          "order_date": "2024-08-29T21:04:19+00:00",
          "order_id": 570552,
          "products": [
            {
              "base_price": 32.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Pyramidustries active",
              "tax_amount": 0,
              "product_id": 10729,
              "category": "Women's Shoes",
              "sku": "ZO0216402164",
              "taxless_price": 32.99,
              "unit_discount_amount": 0,
              "min_price": 16.82,
              "_id": "sold_product_570552_10729",
              "discount_amount": 0,
              "created_on": "2016-12-15T21:04:19+00:00",
              "product_name": "Sports shoes - dark blue",
              "price": 32.99,
              "taxful_price": 32.99,
              "base_unit_price": 32.99
            },
            {
              "base_price": 64.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Angeldale",
              "tax_amount": 0,
              "product_id": 23042,
              "category": "Women's Shoes",
              "sku": "ZO0666306663",
              "taxless_price": 64.99,
              "unit_discount_amount": 0,
              "min_price": 35.74,
              "_id": "sold_product_570552_23042",
              "discount_amount": 0,
              "created_on": "2016-12-15T21:04:19+00:00",
              "product_name": "High heels - gold",
              "price": 64.99,
              "taxful_price": 64.99,
              "base_unit_price": 64.99
            }
          ],
          "sku": [
            "ZO0216402164",
            "ZO0666306663"
          ],
          "taxful_total_price": 97.98,
          "taxless_total_price": 97.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      },
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "I3YjoJEBs_OPuIlVtSVb",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Shoes",
            "Women's Accessories"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte Graham",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "Graham",
          "customer_phone": "",
          "day_of_week": "Thursday",
          "day_of_week_i": 3,
          "email": "brigitte@graham-family.zzz",
          "manufacturer": [
            "Oceanavigations",
            "Tigress Enterprises"
          ],
          "order_date": "2024-08-15T00:23:02+00:00",
          "order_id": 550405,
          "products": [
            {
              "base_price": 59.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Oceanavigations",
              "tax_amount": 0,
              "product_id": 14517,
              "category": "Women's Shoes",
              "sku": "ZO0240802408",
              "taxless_price": 59.99,
              "unit_discount_amount": 0,
              "min_price": 30.59,
              "_id": "sold_product_550405_14517",
              "discount_amount": 0,
              "created_on": "2016-12-01T00:23:02+00:00",
              "product_name": "Trainers - rosa",
              "price": 59.99,
              "taxful_price": 59.99,
              "base_unit_price": 59.99
            },
            {
              "base_price": 11.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Tigress Enterprises",
              "tax_amount": 0,
              "product_id": 14132,
              "category": "Women's Accessories",
              "sku": "ZO0083400834",
              "taxless_price": 11.99,
              "unit_discount_amount": 0,
              "min_price": 6.35,
              "_id": "sold_product_550405_14132",
              "discount_amount": 0,
              "created_on": "2016-12-01T00:23:02+00:00",
              "product_name": "Scarf - offwhite",
              "price": 11.99,
              "taxful_price": 11.99,
              "base_unit_price": 11.99
            }
          ],
          "sku": [
            "ZO0240802408",
            "ZO0083400834"
          ],
          "taxful_total_price": 71.98,
          "taxless_total_price": 71.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      },
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "JXYjoJEBs_OPuIlVtSVb",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Shoes",
            "Women's Clothing"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte Foster",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "Foster",
          "customer_phone": "",
          "day_of_week": "Friday",
          "day_of_week_i": 4,
          "email": "brigitte@foster-family.zzz",
          "manufacturer": [
            "Tigress Enterprises",
            "Gnomehouse"
          ],
          "order_date": "2024-08-16T11:55:41+00:00",
          "order_id": 552413,
          "products": [
            {
              "base_price": 29.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Tigress Enterprises",
              "tax_amount": 0,
              "product_id": 7459,
              "category": "Women's Shoes",
              "sku": "ZO0019300193",
              "taxless_price": 29.99,
              "unit_discount_amount": 0,
              "min_price": 15.89,
              "_id": "sold_product_552413_7459",
              "discount_amount": 0,
              "created_on": "2016-12-02T11:55:41+00:00",
              "product_name": "High heeled ankle boots - black",
              "price": 29.99,
              "taxful_price": 29.99,
              "base_unit_price": 29.99
            },
            {
              "base_price": 32.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Gnomehouse",
              "tax_amount": 0,
              "product_id": 17662,
              "category": "Women's Clothing",
              "sku": "ZO0352903529",
              "taxless_price": 32.99,
              "unit_discount_amount": 0,
              "min_price": 15.18,
              "_id": "sold_product_552413_17662",
              "discount_amount": 0,
              "created_on": "2016-12-02T11:55:41+00:00",
              "product_name": "Jumper - Navy",
              "price": 32.99,
              "taxful_price": 32.99,
              "base_unit_price": 32.99
            }
          ],
          "sku": [
            "ZO0019300193",
            "ZO0352903529"
          ],
          "taxful_total_price": 62.98,
          "taxless_total_price": 62.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      },
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "NnYjoJEBs_OPuIlVtSVc",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Shoes",
            "Women's Clothing"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte Morris",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "Morris",
          "customer_phone": "",
          "day_of_week": "Saturday",
          "day_of_week_i": 5,
          "email": "brigitte@morris-family.zzz",
          "manufacturer": [
            "Tigress Enterprises",
            "Gnomehouse"
          ],
          "order_date": "2024-08-31T08:18:14+00:00",
          "order_id": 572515,
          "products": [
            {
              "base_price": 41.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Tigress Enterprises",
              "tax_amount": 0,
              "product_id": 4683,
              "category": "Women's Shoes",
              "sku": "ZO0015100151",
              "taxless_price": 41.99,
              "unit_discount_amount": 0,
              "min_price": 21.01,
              "_id": "sold_product_572515_4683",
              "discount_amount": 0,
              "created_on": "2016-12-17T08:18:14+00:00",
              "product_name": "Over-the-knee boots - black",
              "price": 41.99,
              "taxful_price": 41.99,
              "base_unit_price": 41.99
            },
            {
              "base_price": 59.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Gnomehouse",
              "tax_amount": 0,
              "product_id": 8986,
              "category": "Women's Clothing",
              "sku": "ZO0337203372",
              "taxless_price": 59.99,
              "unit_discount_amount": 0,
              "min_price": 32.39,
              "_id": "sold_product_572515_8986",
              "discount_amount": 0,
              "created_on": "2016-12-17T08:18:14+00:00",
              "product_name": "Summer dress - winsdor wine",
              "price": 59.99,
              "taxful_price": 59.99,
              "base_unit_price": 59.99
            }
          ],
          "sku": [
            "ZO0015100151",
            "ZO0337203372"
          ],
          "taxful_total_price": 101.98,
          "taxless_total_price": 101.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      },
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "aHYjoJEBs_OPuIlVtSVc",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Clothing",
            "Women's Shoes"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte Morris",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "Morris",
          "customer_phone": "",
          "day_of_week": "Saturday",
          "day_of_week_i": 5,
          "email": "brigitte@morris-family.zzz",
          "manufacturer": [
            "Oceanavigations",
            "Primemaster"
          ],
          "order_date": "2024-09-07T20:48:29+00:00",
          "order_id": 582575,
          "products": [
            {
              "base_price": 16.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Oceanavigations",
              "tax_amount": 0,
              "product_id": 22749,
              "category": "Women's Clothing",
              "sku": "ZO0264402644",
              "taxless_price": 16.99,
              "unit_discount_amount": 0,
              "min_price": 8.49,
              "_id": "sold_product_582575_22749",
              "discount_amount": 0,
              "created_on": "2016-12-24T20:48:29+00:00",
              "product_name": "Basic T-shirt - grey",
              "price": 16.99,
              "taxful_price": 16.99,
              "base_unit_price": 16.99
            },
            {
              "base_price": 124.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Primemaster",
              "tax_amount": 0,
              "product_id": 19986,
              "category": "Women's Shoes",
              "sku": "ZO0363903639",
              "taxless_price": 124.99,
              "unit_discount_amount": 0,
              "min_price": 66.24,
              "_id": "sold_product_582575_19986",
              "discount_amount": 0,
              "created_on": "2016-12-24T20:48:29+00:00",
              "product_name": "Lace-up boots - Midnight Blue",
              "price": 124.99,
              "taxful_price": 124.99,
              "base_unit_price": 124.99
            }
          ],
          "sku": [
            "ZO0264402644",
            "ZO0363903639"
          ],
          "taxful_total_price": 141.98,
          "taxless_total_price": 141.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      },
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "g3YjoJEBs_OPuIlVtSVc",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Clothing",
            "Women's Shoes"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte Shaw",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "Shaw",
          "customer_phone": "",
          "day_of_week": "Sunday",
          "day_of_week_i": 6,
          "email": "brigitte@shaw-family.zzz",
          "manufacturer": [
            "Tigress Enterprises",
            "Low Tide Media"
          ],
          "order_date": "2024-08-18T23:49:55+00:00",
          "order_id": 555773,
          "products": [
            {
              "base_price": 28.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Tigress Enterprises",
              "tax_amount": 0,
              "product_id": 14626,
              "category": "Women's Clothing",
              "sku": "ZO0032200322",
              "taxless_price": 28.99,
              "unit_discount_amount": 0,
              "min_price": 13.92,
              "_id": "sold_product_555773_14626",
              "discount_amount": 0,
              "created_on": "2016-12-04T23:49:55+00:00",
              "product_name": "Trousers - stone",
              "price": 28.99,
              "taxful_price": 28.99,
              "base_unit_price": 28.99
            },
            {
              "base_price": 99.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Low Tide Media",
              "tax_amount": 0,
              "product_id": 5128,
              "category": "Women's Shoes",
              "sku": "ZO0374303743",
              "taxless_price": 99.99,
              "unit_discount_amount": 0,
              "min_price": 52.99,
              "_id": "sold_product_555773_5128",
              "discount_amount": 0,
              "created_on": "2016-12-04T23:49:55+00:00",
              "product_name": "Boots - Midnight Blue",
              "price": 99.99,
              "taxful_price": 99.99,
              "base_unit_price": 99.99
            }
          ],
          "sku": [
            "ZO0032200322",
            "ZO0374303743"
          ],
          "taxful_total_price": 128.98,
          "taxless_total_price": 128.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      },
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "nnYjoJEBs_OPuIlVtSVc",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Clothing"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte Tran",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "Tran",
          "customer_phone": "",
          "day_of_week": "Monday",
          "day_of_week_i": 0,
          "email": "brigitte@tran-family.zzz",
          "manufacturer": [
            "Spherecords",
            "Gnomehouse"
          ],
          "order_date": "2024-09-09T22:19:12+00:00",
          "order_id": 585381,
          "products": [
            {
              "base_price": 10.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Spherecords",
              "tax_amount": 0,
              "product_id": 14663,
              "category": "Women's Clothing",
              "sku": "ZO0659006590",
              "taxless_price": 10.99,
              "unit_discount_amount": 0,
              "min_price": 5.6,
              "_id": "sold_product_585381_14663",
              "discount_amount": 0,
              "created_on": "2016-12-26T22:19:12+00:00",
              "product_name": "3 PACK - Shorts - black/white/printed",
              "price": 10.99,
              "taxful_price": 10.99,
              "base_unit_price": 10.99
            },
            {
              "base_price": 41.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Gnomehouse",
              "tax_amount": 0,
              "product_id": 24101,
              "category": "Women's Clothing",
              "sku": "ZO0334503345",
              "taxless_price": 41.99,
              "unit_discount_amount": 0,
              "min_price": 18.9,
              "_id": "sold_product_585381_24101",
              "discount_amount": 0,
              "created_on": "2016-12-26T22:19:12+00:00",
              "product_name": "Jersey dress - jester red",
              "price": 41.99,
              "taxful_price": 41.99,
              "base_unit_price": 41.99
            }
          ],
          "sku": [
            "ZO0659006590",
            "ZO0334503345"
          ],
          "taxful_total_price": 52.98,
          "taxless_total_price": 52.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      },
      {
        "_index": "kibana_sample_data_ecommerce",
        "_id": "oHYjoJEBs_OPuIlVtSVc",
        "_score": 3.5412264,
        "_source": {
          "category": [
            "Women's Shoes",
            "Women's Clothing"
          ],
          "currency": "EUR",
          "customer_first_name": "Brigitte",
          "customer_full_name": "Brigitte Wise",
          "customer_gender": "FEMALE",
          "customer_id": 12,
          "customer_last_name": "Wise",
          "customer_phone": "",
          "day_of_week": "Monday",
          "day_of_week_i": 0,
          "email": "brigitte@wise-family.zzz",
          "manufacturer": [
            "Tigress Enterprises",
            "Spherecords"
          ],
          "order_date": "2024-09-09T19:58:05+00:00",
          "order_id": 585263,
          "products": [
            {
              "base_price": 32.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Tigress Enterprises",
              "tax_amount": 0,
              "product_id": 12991,
              "category": "Women's Shoes",
              "sku": "ZO0021900219",
              "taxless_price": 32.99,
              "unit_discount_amount": 0,
              "min_price": 16.5,
              "_id": "sold_product_585263_12991",
              "discount_amount": 0,
              "created_on": "2016-12-26T19:58:05+00:00",
              "product_name": "High heeled ankle boots - cognac",
              "price": 32.99,
              "taxful_price": 32.99,
              "base_unit_price": 32.99
            },
            {
              "base_price": 13.99,
              "discount_percentage": 0,
              "quantity": 1,
              "manufacturer": "Spherecords",
              "tax_amount": 0,
              "product_id": 23101,
              "category": "Women's Clothing",
              "sku": "ZO0647806478",
              "taxless_price": 13.99,
              "unit_discount_amount": 0,
              "min_price": 6.72,
              "_id": "sold_product_585263_23101",
              "discount_amount": 0,
              "created_on": "2016-12-26T19:58:05+00:00",
              "product_name": "Long sleeved top - dark blue",
              "price": 13.99,
              "taxful_price": 13.99,
              "base_unit_price": 13.99
            }
          ],
          "sku": [
            "ZO0021900219",
            "ZO0647806478"
          ],
          "taxful_total_price": 46.98,
          "taxless_total_price": 46.98,
          "total_quantity": 2,
          "total_unique_products": 2,
          "type": "order",
          "user": "brigitte",
          "geoip": {
            "country_iso_code": "US",
            "location": {
              "lon": -74,
              "lat": 40.8
            },
            "region_name": "New York",
            "continent_name": "North America",
            "city_name": "New York"
          },
          "event": {
            "dataset": "sample_ecommerce"
          }
        }
      }
    ]
  }
}
</details>


### Interpreting the Results of the Term Query

Let's break down the results returned by the term query executed on the `kibana_sample_data_ecommerce` index, where the query searched for documents with `customer_id` equal to "12".

#### 1. **Query Execution Details**
   - **`took: 3`**: This indicates that the query took 3 milliseconds to execute. This is a very fast response time, which is typical for Elasticsearch, especially when querying on an indexed field like `customer_id`.
   - **`timed_out: false`**: This confirms that the query did not time out, meaning it completed successfully within the allowed time.
   - **`_shards`**:
     - **`total: 1`**: Indicates that the query was executed on a single shard. Since this is likely a small dataset or a test environment, having a single shard is common.
     - **`successful: 1`**: The query was successful on all shards involved.
     - **`skipped: 0`**: No shards were skipped during the query execution.
     - **`failed: 0`**: There were no errors during execution.

#### 2. **Hits Summary**
   - **`hits.total.value: 135`**: This indicates that there are 135 documents in the index that match the query condition (`customer_id` = "12").
   - **`hits.total.relation: "eq"`**: This means that the total count provided is exact, not an approximation.
   - **`max_score: 3.5412264`**: The highest score among the returned documents. This score indicates the relevance of the documents to the query, although in this case (term query), the score might not be as informative since we're searching for exact matches.

#### 3. **Sample Documents**
   The query returned a list of documents where `customer_id` equals "12". Let's examine a couple of these documents to understand the data better.

   **Document 1**:
   - **`_index: "kibana_sample_data_ecommerce"`**: Indicates the index from which the document is retrieved.
   - **`_id: "_HYjoJEBs_OPuIlVtSRb"`**: The unique identifier of the document within the index.
   - **`_score: 3.5412264`**: The relevance score, which is the same across the returned documents, since it's an exact match.
   - **Customer Information**:
     - **`customer_first_name: "Brigitte"`** and **`customer_last_name: "Morris"`**: The customer's full name.
     - **`customer_gender: "FEMALE"`**: The gender of the customer.
     - **`email: "brigitte@morris-family.zzz"`**: The customer’s email address.
   - **Order Details**:
     - **`order_id: 576696`**: The unique order ID.
     - **`order_date: "2024-09-03T12:15:50+00:00"`**: The date and time when the order was placed.
     - **Products Purchased**: The document lists products such as a "Cardigan - black" and a "Watch - black", along with pricing and quantity information.

   **Document 2**:
   - **Customer Information**:
     - **`customer_first_name: "Brigitte"`** and **`customer_last_name: "Butler"`**: Another customer with the same `customer_id` but different last name and email.
   - **Order Details**:
     - **`order_id: 580088`**: The order ID for this particular purchase.
     - **Products Purchased**: Includes items like a "Cardigan - black" and a "Denim dress - black denim".

#### 4. **Understanding the Results**

The results indicate that the customer with `customer_id` "12" has placed multiple orders under different last names or possibly aliases. Each returned document represents an order placed by this customer, with details about the products purchased, pricing, and the geographic location from which the order was placed.

Elasticsearch's ability to return multiple hits for the same customer ID showcases its powerful indexing and search capabilities. The consistent `max_score` across the documents suggests that all these orders are equally relevant to the search query, given the exact match nature of a term query.

In summary, this query efficiently retrieved all documents associated with `customer_id` "12", enabling a detailed analysis of this customer's purchasing behavior across multiple orders.

**Example 2: Retrieve Orders with a Specific Product Category**

If you're interested in finding all orders that contain products from a specific category, such as "Electronics", you can use the following term query:

```json
{
  "query": {
    "term": {
      "products.category.keyword": {
        "value": "Electronics"
      }
    }
  }
}
```

In this query, we're targeting the `products.category.keyword` field, which stores product categories as exact keywords. This will return orders where the category "Electronics" is present.

#### Querying the `kibana_sample_data_flights` Index

The `kibana_sample_data_flights` index is used to track and analyze flight data, including information on carriers, destinations, and delays.

**Example 1: Find Flights by Carrier**

To find all flights operated by a specific carrier, such as "Kibana Airlines", you can use a term query like this:

```json
{
  "query": {
    "term": {
      "Carrier": {
        "value": "Kibana Airlines"
      }
    }
  }
}
```

This query will return all flight records in the `kibana_sample_data_flights` index where the carrier is "Kibana Airlines".

**Example 2: Retrieve Flights Delayed by Weather**

If you want to identify flights that were delayed due to specific weather conditions, you can use the following query:

```json
{
  "query": {
    "term": {
      "FlightDelayType": {
        "value": "Weather"
      }
    }
  }
}
```

This query will fetch all flights where the delay was attributed to weather conditions, helping you analyze the impact of weather on flight operations.

#### Querying the `.ds-kibana_sample_data_logs-2024.08.29-000001` Index

This index contains log data, which is crucial for monitoring and troubleshooting system performance. Term queries can be used to pinpoint exact log entries based on specific criteria.

**Example 1: Find Logs from a Specific IP Address**

If you want to find all log entries from a particular IP address, say "192.168.1.1", you can use this term query:

```json
{
  "query": {
    "term": {
      "clientip": {
        "value": "192.168.1.1"
      }
    }
  }
}
```

This query will return all logs where the `clientip` matches "192.168.1.1".

**Example 2: Retrieve Logs for a Specific Event Type**

To find logs related to a specific event, such as those associated with the "access" dataset, you can use the following query:

```json
{
  "query": {
    "term": {
      "event.dataset": {
        "value": "access"
      }
    }
  }
}
```

This query will filter the logs to only those that belong to the "access" dataset, allowing you to focus on specific types of events within your log data.

#### Conclusion

By applying term-level queries to our specific indices, you can efficiently retrieve precise information from large datasets. Whether you’re analyzing eCommerce transactions, tracking flights, or monitoring logs, Elasticsearch’s term queries provide the accuracy and performance needed to explore your data in depth. As you continue to explore Elasticsearch, you'll find that these basic building blocks can be combined and extended to handle even more complex querying needs.