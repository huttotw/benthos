---
title: csv
type: input
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the contents of:
     lib/input/csv.go
-->


BETA: This component is experimental and therefore subject to change outside of
major version releases.

Reads one or more CSV files as structured records following the format described
in RFC 4180.


import Tabs from '@theme/Tabs';

<Tabs defaultValue="common" values={[
  { label: 'Common', value: 'common', },
  { label: 'Advanced', value: 'advanced', },
]}>

import TabItem from '@theme/TabItem';

<TabItem value="common">

```yaml
# Common config fields, showing default values
input:
  csv:
    paths: []
    parse_header_row: true
    delimiter: ','
```

</TabItem>
<TabItem value="advanced">

```yaml
# All config fields, showing default values
input:
  csv:
    paths: []
    parse_header_row: true
    delimiter: ','
    batch_count: 1
```

</TabItem>
</Tabs>

When parsing with a header row each line of the file will be consumed as a
structured object, where the key names are determined from the header now. For
example, the following CSV file:

```csv
foo,bar,baz
first foo,first bar,first baz
second foo,second bar,second baz
```

Would produce the following messages:

```json
{"foo":"first foo","bar":"first bar","baz":"first baz"}
{"foo":"second foo","bar":"second bar","baz":"second baz"}
```

If, however, the field `parse_header_row` is set to `false` then
arrays are produced instead, like follows:

```json
["first foo","first bar","first baz"]
["second foo","second bar","second baz"]
```

## Fields

### `paths`

A list of file paths to read from. Each file will be read sequentially until the list is exhausted, at which point the input will close.


Type: `array`  
Default: `[]`  

### `parse_header_row`

Whether to reference the first row as a header row. If set to true the output structure for messages will be an object where field keys are determined by the header row.


Type: `bool`  
Default: `true`  

### `delimiter`

The delimiter to use for splitting values in each record, must be a single character.


Type: `string`  
Default: `","`  

### `batch_count`

Optionally process records in batches. This can help to speed up the consumption of exceptionally large CSV files. When the end of the file is reached the remaining records are processed as a (potentially smaller) batch.


Type: `number`  
Default: `1`  

This input is particularly useful when consuming CSV from files too large to
parse entirely within memory. However, in cases where CSV is consumed from other
input types it's also possible to parse them using the
[Bloblang `parse_csv` method](/docs/guides/bloblang/methods#parse_csv).
