# Native Apache Parquet Footer & Thrift Compact Protocol Parser (C++)

A high-performance, zero-dependency binary metadata decoder written from scratch in C++. This utility is engineered to extract schema structures, column-level metadata, and file metrics directly from raw Apache Parquet file footers by implementing a native parser for the Apache Thrift Compact Protocol.

## Key Technical Features
* **Zero-Copy Byte Ingress:** Bypasses heavy parsing frameworks to process, slice, and deserialize raw hardware byte arrays (`unsigned char*`) directly out of data streams.
* **LEB128 / VLQ Variable-Length Integer Decoder:** Features an optimized runtime loop (`decodeIntegerType`) that evaluates continuation bits dynamically, allowing it to parse variable-length integers with minimal CPU overhead.
* **ZigZag Bitwise Decompression:** Implements fast, inline bitwise operations (`unzag`) to decode signed integers instantly without relying on heavy mathematical branching paths or evaluation loops.
* **Polymorphic AST Generation:** Structures a customized abstract syntax tree (AST) via an efficient `ParseResult` wrapper that utilizes memory unions to minimize memory allocation overhead while tracking distinct metadata types.
* **Dynamic Function Selection Matrix:** Maps token configurations directly to internal function pointers (`thrift_compact_processor_t`) to execute specialized parsing logic adaptively as the binary stream progresses.

## Project Structure
* `main.cpp` - File stream ingestion harness and binary parsing loops.
* `ParquetFooterParser` - Core state machine tracking LEB128 loops and field identifier offsets.
* `ParseResult` - Polymorphic structural tree nodes capturing integer variants, strings, lists, and structs.

## Compilation & Execution
Compile using any standard modern C++ compiler (`C++17` or newer recommended):

```bash
# Compile the utility
g++ -std=c++17 -o parquet_parser read_parquet.cpp

# Execute against a targeted Parquet binary footer payload
./parquet_parser
```
