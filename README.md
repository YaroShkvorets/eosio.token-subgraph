# EOS token transfer subgraph

## Prerequisites
1. Docker
2. `SUBSTREAMS_API_TOKEN` env variable set

## Quick start

1. Start `graph-node`, `ipfs`, `postgres` and `pgweb` containers
```bash
> cd graph-node
> SUBSTREAMS_ENDPOINT=https://eos.substreams.pinax.network:443 ./up.sh -c
```
Make sure they start without errors. You should see messages like this in the log as a sign that graph node started successfully and is tracking network head block:
```bash
graph-node-token  | Jan 30 22:07:43.128 INFO Substreams backend graph_out last block is 354915877, 0 stages, 0 jobs, trace_id: f874d9b35da403c1ca7797db77b1160c, provider: substreams-mainnet,
```

2. In another terminal window, build, create and deploy the subgraph:
```bash
> yarn install
> yarn build
> yarn create-local
> yarn deploy-local
```
Once you see messages like this in the docker terminal, it means graph node is consuming the data from substreams backend:
```bash
graph-node-token  | Jan 30 22:12:20.870 INFO Applying 2 entity operation(s), block_hash: 0x30303030303163613266303662306265636462323138323339616135373461653237653865326462383663643562633637623830333039643966306639616438, block_number: 458, sgd: 1, subgraph_id: QmempVDyiT6sr1R4A8uFjtYp833fqg33f9RTAyutT3zwgK, component: SubgraphInstanceManager
```

3. Open http://127.0.0.1:8000/subgraphs/name/eosiotoken in the browser and try running a query, i.e.
```graphql
query{
  transfers{
    from
    to
    quantity
    memo
    trx_id
  }
}
```
You may need to wait a couple of minutes until the first batch of transfers is written into the database