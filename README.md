# Configuration Enabling HTTPS-SSL for Neo4j 4.4.x

### $NEO4J_HOME/conf/neo4j.conf

    ## Enable Bolt connector over TLS
    dbms.connector.bolt.enabled=true
    dbms.connector.bolt.tls_level=OPTIONAL
    
    #Disable HTTP Connector. There can be zero or one HTTP connectors.**
    dbms.connector.http.enabled=false
    
    # Enable HTTPS Connector. There can be zero or one HTTPS connectors
    dbms.connector.https.enabled=true
    dbms.connector.https.listen_address=:7473
    dbms.connector.https.advertised_address=:7473
    
    ## Bolt SSL configuration
    dbms.ssl.policy.bolt.enabled=true
    dbms.ssl.policy.bolt.base_directory=certificates/bolt
    dbms.ssl.policy.bolt.private_key=private.key
    dbms.ssl.policy.bolt.public_certificate=public.crt
    dbms.ssl.policy.bolt.client_auth=NONE
    
    ## Https SSL configuration
    dbms.ssl.policy.https.enabled=true
    dbms.ssl.policy.https.base_directory=certificates/https
    dbms.ssl.policy.https.private_key=private.key
    dbms.ssl.policy.https.public_certificate=public.crt
    dbms.ssl.policy.https.client_auth=NONE

![image](https://github.com/arliputraa/neo4j-https-ssl/assets/110078907/26aa859b-4046-4367-a41a-3869bfe2012e)

### Setup Your Network Connector Configuration neo4j.conf

    # With default configuration Neo4j only accepts local connections.
    # To accept non-local connections, uncomment this line:
    dbms.default_listen_address=0.0.0.0
    
    # Bolt connector
    dbms.connector.bolt.enabled=true
    dbms.connector.bolt.tls_level=REQUIRED
    
    # HTTP Connector. There can be zero or one HTTP connectors.
    dbms.connector.http.enabled=false
    
    # HTTPS Connector. There can be zero or one HTTPS connectors.
    dbms.connector.https.enabled=true
    dbms.connector.https.listen_address=:7473
    dbms.connector.https.advertised_address=:7473

![image](https://github.com/arliputraa/neo4j-https-ssl/assets/110078907/f5876e78-3ab7-4e5d-8d67-8b2676a2eb9c)

### Create directory in $NEO4J_HOME

    mkdir certificates 
    mkdir certificates/https
    mkdir certificates/https/trusted
    mkdir certificates/https/revoked
    mkdir certificates/bolt
    mkdir certificates/bolt/trusted
    mkdir certificates/bolt/revoked

### Create New Private Key & Certificate

    openssl req -x509 -newkey rsa:2048 -keyout private.key -out public.crt -nodes -days 1000

#### Output private.key & public.crt

![image](https://github.com/arliputraa/neo4j-https-ssl/assets/110078907/e9f6a6c1-0e00-4b5c-b739-d4a34238562a)

![image](https://github.com/arliputraa/neo4j-https-ssl/assets/110078907/c17a3deb-b31e-471c-9eb1-80cf45218953)

### Copy a private key to directory certificates

    # in directory $NEO4J_HOME
    cp private.key certificates/bolt
    cp public.crt certificates/bolt
    cp private.key certificates/https
    cp public.crt certificates/https

### Restart Neo4j in directory $NEO4J_HOME/bin

    ./neo4j restart

![image](https://github.com/arliputraa/neo4j-https-ssl/assets/110078907/04d185ff-b92c-4c42-8f58-8aa377450291)

### Login into cypher use command

    ./cypher-shell -a neo4j+ssc://0.0.0.0:7687

![image](https://github.com/arliputraa/neo4j-https-ssl/assets/110078907/faad377a-369c-497a-82cf-45bfa06eec26)

### Open Your Browser https://YourIp:7473

![image](https://github.com/arliputraa/neo4j-https-ssl/assets/110078907/5d544788-7c2e-44f7-bfc0-13519f116ecc)

