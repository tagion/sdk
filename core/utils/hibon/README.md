# HiBON Util

The CLI tool to convert between `.hibon` and `.json` formats.

```
Usage:
./hibonutil [<option>...] <in-file> <out-file>
./hibonutil [<option>...] <in-file>

Where:
<in-file>           Is an input file in .json or .hibon format
<out-file>          Is an output file in .json or .hibon format
                    stdout is used of the output is not specifed the

<option>:
      --version display the version
-i  --inputfile Sets the HiBON input file name
-o --outputfile Sets the output file name
-b        --bin Use HiBON or else use JSON
-V      --value Bill value : default: 1000000000
-p     --pretty JSON Pretty print: Default: false
-h       --help This help information.
```