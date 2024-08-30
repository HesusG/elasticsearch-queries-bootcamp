### 03 Retrieving Documents by ID

In Elasticsearch, retrieving documents by their ID is one of the most direct and efficient methods to access specific records. The following examples illustrate how to perform this operation on three different indices, showing how you can directly pull up documents with known IDs.

#### Example 1: Retrieving an Order from the eCommerce Index
To retrieve a document from the `kibana_sample_data_ecommerce` index, use the following query:

```json
GET /kibana_sample_data_ecommerce/_doc/_HYjoJEBs_OPuIlVtSRb
```

This query will return the document associated with the ID `_HYjoJEBs_OPuIlVtSRb`. The document will include all fields such as customer details, order date, products purchased, and more, giving you a comprehensive view of the specific order.

#### Example 2: Retrieving a Flight Record from the Flights Index
To retrieve a document from the `kibana_sample_data_flights` index, use the following query:

```json
GET /kibana_sample_data_flights/_doc/9NgjoJEBmzpFaBFXwPfm
```

This query returns the document with the ID `9NgjoJEBmzpFaBFXwPfm`, providing detailed information about the flight, such as the carrier, destination, departure time, and any associated delays.

#### Example 3: Retrieving a Log Entry from the Logs Index
To retrieve a document from the `.ds-kibana_sample_data_logs-2024.08.29-000001` index, use the following query:

```json
GET /.ds-kibana_sample_data_logs-2024.08.29-000001/_doc/99kjoJEBmzpFaBFXxgGN
```

This query will return the document associated with the ID `99kjoJEBmzpFaBFXxgGN`. Here’s a detailed breakdown of the results:

```json
{
  "_index": ".ds-kibana_sample_data_logs-2024.08.29-000001",
  "_id": "99kjoJEBmzpFaBFXxgGN",
  "_version": 1,
  "_seq_no": 1269,
  "_primary_term": 1,
  "found": true,
  "_source": {
    "agent": "Mozilla/5.0 (X11; Linux x86_64; rv:6.0a1) Gecko/20110421 Firefox/6.0a1",
    "bytes": 5464,
    "clientip": "176.25.204.175",
    "extension": "zip",
    "geo": {
      "srcdest": "US:SD",
      "src": "US",
      "dest": "SD",
      "coordinates": {
        "lat": 43.98478278,
        "lon": -73.09594889
      }
    },
    "host": "artifacts.elastic.co",
    "index": "kibana_sample_data_logs",
    "ip": "176.25.204.175",
    "machine": {
      "ram": 13958643712,
      "os": "ios"
    },
    "memory": null,
    "message": "176.25.204.175 - - [2018-07-27T14:34:39.702Z] \"GET /elasticsearch/elasticsearch-6.3.2.zip HTTP/1.1\" 200 5464 \"-\" \"Mozilla/5.0 (X11; Linux x86_64; rv:6.0a1) Gecko/20110421 Firefox/6.0a1\"",
    "phpmemory": null,
    "referer": "http://twitter.com/success/richard-hieb",
    "request": "/elasticsearch/elasticsearch-6.3.2.zip",
    "response": 200,
    "tags": [
      "error",
      "security"
    ],
    "@timestamp": "2024-08-23T14:34:39.702Z",
    "url": "https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.2.zip",
    "utc_time": "2024-08-23T14:34:39.702Z",
    "event": {
      "dataset": "sample_web_logs"
    },
    "bytes_gauge": 5464,
    "bytes_counter": 7280991
  }
}
```

#### Explanation of the Results

- **`_index`:** Indicates the index from which the document was retrieved.
- **`_id`:** The unique identifier for the document.
- **`_version`:** The version of the document, useful for tracking updates.
- **`_seq_no` and `_primary_term`:** These are internal fields that track the sequence number and primary term for Elasticsearch's consistency model.
- **`found`:** Confirms that the document was found in the index.
- **`_source`:** Contains the actual data of the log entry, including:
  - **`agent`:** The user agent string, indicating the browser and operating system used.
  - **`clientip`:** The IP address of the client making the request.
  - **`geo`:** Geographical information related to the IP address, including coordinates and location details.
  - **`host`:** The host from which the request was made.
  - **`message`:** The raw log message, detailing the HTTP request made to the server.
  - **`response`:** The HTTP response code returned by the server (200 indicates success).
  - **`tags`:** Tags associated with this log entry, indicating possible issues like "error" or "security" concerns.

This detailed breakdown provides insight into the specific log entry, allowing you to understand the context of the request, the client making the request, and the server's response. This type of query is crucial for debugging, monitoring, and analyzing server activity in real-time.


### How to Use the `ids` Query

The `ids` query allows you to specify the document IDs you're interested in and can be used across different indices or within the same index. Here's the basic structure:

```json
{
  "query": {
    "ids": {
      "values": [
        "<id_1>",
        "<id_2>",
        "<id_3>"
      ]
    }
  }
}
```

- **`<id_1>, <id_2>, <id_3>`**: These are the IDs of the documents you want to retrieve.

### Example: Retrieving Multiple Documents by ID

Suppose you want to retrieve three specific documents from the `kibana_sample_data_ecommerce` index with the IDs `_HYjoJEBs_OPuIlVtSRb`, `AXYjoJEBs_OPuIlVtSVb`, and `BXYjoJEBs_OPuIlVtSVb`. You would use the following query:

```json
{
  "query": {
    "ids": {
      "values": [
        "_HYjoJEBs_OPuIlVtSRb",
        "AXYjoJEBs_OPuIlVtSVb",
        "BXYjoJEBs_OPuIlVtSVb"
      ]
    }
  }
}
```

This query will return all the documents from the `kibana_sample_data_ecommerce` index that have the specified IDs.

### Explanation of the Results

When you execute this query, Elasticsearch will search for the documents with the specified IDs and return them in a single response. The result will include all the documents that match the provided IDs, along with their `_index`, `_id`, `_source`, and other relevant metadata.

This approach is efficient and useful when dealing with cases where you know the exact IDs of the documents you need and want to retrieve them in a single request.