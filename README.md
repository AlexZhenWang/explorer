Explorer
========

`cennznet/explorer` is a monorepo of various components required to run the UNcover - CENNZnet Block Explorer application including extraction, API and web application itself

Prerequisites
-------------

-   Docker
-   CENNZnet node Rimu-0.9.23 or higher
    - for running a node locally on your machine follow instructions in [CENNZnet Node](../../../cennznet). 
    Please ensure WebSocket is on `--ws-external` and node is accessible from Docker. 
    You may want to run it with `--release` flag to overcome WASM optimization issue e.g. 
       ```
      cargo run --release -- --dev --ws-external --rpc-external
      ```       

QuickStart
----------

1.  Configuration settings

    ```
    cp config.json.template etl-config.json
    ```

     -   `node.ws`: where your CENNZnet node is hosted (e.g. `127.0.0.1:9944` or any publicly available CENNZnet node). 
    **Note**: Docker may have problem to resolve IP address when connecting to a service on local host, please use `host.docker.internal:9944` instead, e.g.

         ```
         "node": {
           "ws": "host.docker.internal:9944"
         },
         ```

2.  Build and start containers locally

     ```
     docker-compose up --build
     ```

3.  Open block explorer in your browser: <http://localhost:3000>
    - open API in your browser, e.g. <http://localhost:8080/blocks>
    - access extraction tasks using your MongoDB client: `mongodb://localhost:27018/cennznettasks`
    - access database using your PostgreSQL client: `postgresql://username:password@localhost:5433/cennznetdata`

4.  Shut down the containers

    ```
    docker-compose down
    ```

Schedule Task
---------------

Useful when you need re-extract or backfill missing blocks after starting the containers

```
docker exec -it $(docker ps -q -f name=explorer-etl") ./taskgen -config etl-config.json -start <start_block> -end <end_block>
```

