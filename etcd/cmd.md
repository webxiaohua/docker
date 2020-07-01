curl -L http://localhost:2379/v3/kv/put -X POST -d '{"key": "Zm9v", "value": "YmFy"}'

curl -L http://localhost:2379/v3/kv/range -X POST -d '{"key": "Zm9v"}'

curl -L http://localhost:2379/v3/kv/range -X POST -d '{"key": "Zm9v", "range_end": "Zm9w"}'