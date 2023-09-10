## [WIP.]

## 1.CLI for zkMove Virtual Machine

Currently, we only support one command 'Run', which run the full sequence of circuit building, setup, proving, and verifying.
It also reports the proof size, prove time and verify time when the execution is successful.

For example, the following command will first compile test.move into bytecode,
then build the circuit and setup the proving/verifying key, and then generate a zkp for the execution with the proving key and
finally verify the proof with the verifying key.

```bash
bin/zkmove run -s examples/scripts/test.move
```

### Import modules
The Move program consists of scripts and modules. For testing, directive 'mods' can be added to script source file to import a module. For example,

```rust
//! mods: eth_data.move,vector.move
//! word_capacity: 65
script {
    use 0x1::EthData;
    use 0x1::vector;
    fun main() {
        let block_number = 17000000;
        let block_hash = EthData::get_block_hash(block_number);
        let expected = x"96cfa0fb5e50b0a3f6cc76f3299cfbf48f17e8b41798d1394474e67ec8a97e9f";
        assert!(block_hash == expected, 101);
    }
}
```
And we need tell vm where to load the module with option '-m':

```bash
bin/zkmove run -s examples/scripts/eth_data_test.move -m examples/modules/
```

### Word capacity


To efficiently represent a complex value in the circuit, Version 0.2.0 introduces the concept of 'word', a uniform flattened value representation, to flatten the complex value into simple values. and developers can configure the circuit param `word_capacity` to specify the max number of simple values that a complex value can include.

for example, in `examples/scripts/eth_data_test.move`, we set the param to 65, and you can run it with `zkmove run -s examples/scripts/eth_data_test.move`.

## 2.Run zkMove TDS Node

WIP.