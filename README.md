# Railway Neo4j Template Overview

This template allows you to easily deploy and manage a Neo4j instance on Railway.

## Features

- **Bulk Insert Support**  
  You can bulk insert data using environment variables:  
  - `RELATION_CSV_URLS` or `NODE_CSV_URLS`  
  - Provide CSV file URLs separated by commas.

- **Automatic TCP Address**  
  - A TCP address is automatically generated.  
  - Use it in your driver as:  
    ```
    neo4j://your-tcp-address-from-railway
    ```
  - You can find this address in the **Service Settings** → **Networking** tab.  
  - Click the **Copy** button to copy the value.

- **Auto-Generated Password**  
  - A password is automatically generated for your instance.  
  - You can change it later (must be at least **8 characters long**).

- **Logging Enabled by Default**  
  - Both **user logs** and **server logs** are enabled out-of-the-box.

## Example Code Snippets

### JavaScript (Node.js)
```javascript
import neo4j from 'neo4j-driver';

const host = configService.get<string>('NEO4J_HOST'); // neo4j://your-tcp-address-from-railway
const username = configService.get<string>('NEO4J_USERNAME'); // This must be neo4j
const password = configService.get<string>('NEO4J_PASSWORD');

const driver = neo4j.driver(host, neo4j.auth.basic(username, password));
const result = await driver.executeQuery('MATCH (n) RETURN n');
console.log(result);
```

### Python
```python
from neo4j import GraphDatabase

uri = "neo4j://your-tcp-address-from-railway"
username = "neo4j"
password = "your-password"

driver = GraphDatabase.driver(uri, auth=(username, password))

with driver.session() as session:
    result = session.run("MATCH (n) RETURN n")
    for record in result:
        print(record)
```

### Java
```java
import org.neo4j.driver.*;

public class Neo4jExample {
    public static void main(String[] args) {
        String uri = "neo4j://your-tcp-address-from-railway";
        String user = "neo4j";
        String password = "your-password";

        try (Driver driver = GraphDatabase.driver(uri, AuthTokens.basic(user, password));
             Session session = driver.session()) {
            Result result = session.run("MATCH (n) RETURN n");
            while (result.hasNext()) {
                System.out.println(result.next().asMap());
            }
        }
    }
}
```

## Future Improvements

- Expanding bulk insert support beyond CSV files.  
- Providing access to the Neo4j Web UI.  
