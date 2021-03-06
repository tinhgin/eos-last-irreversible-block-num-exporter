# eos-last-irreversible-block-num-exporter
EOS last block number exporter

![EOS](https://seeklogo.com/images/E/eos-logo-ECF31E0936-seeklogo.com.png)

## Run a test
```bash
docker run -d -p 8889:8889 -e INTERVAL=5 -e BLOCKCHAIN_NODE_URL=http://your-blockchain-node:8888 tinhgin/eos-last-irreversible-block-num-exporter
```

## Prometheus Alert Rule
```yaml
  - name: eos
    rules:
    - alert: BlockchainNoNewBlock
      expr: changes(eos_last_block_num[1m]) == 0
      for: 5s
      labels:
        severity: critical
      annotations:
        summary: "{{$labels.blockchain_node}} No new block"
        description: "No new block in last 1 minute"
    - alert: BlockchainCanNotGetLastBlockNumber
      expr: eos_last_block_num == 0
      for: 1m
      labels:
        severity: critical
      annotations:
        summary: "Can not get last block number from {{$labels.blockchain_node}}"
        description: "Exporter can not access {{$labels.blockchain_node}}"
```