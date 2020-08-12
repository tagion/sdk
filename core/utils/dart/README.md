# DART Util

The CLI tool to inspect DART database, and write to it directly.

```
Usage:
dartutil <command> [<option>...]

Where:
<command>           one of [--read, --rim, --modify, --rpc]

<option>:
           --version display the version
-dart --dartfilename Sets the dartfile: default /tmp/default.drt
        --initialize Create a dart file
   -i    --inputfile Sets the HiBON input file name
   -o   --outputfile Sets the output file name
              --from Sets from angle: default full
                --to Sets to angle: default full
  -fn   --useFakeNet Enables fake hash test-mode: default false
              --read Excutes a DART read sequency: default false
               --rim Performs DART rim read: default false
            --modify Excutes a DART modify sequency: default false
               --rpc Excutes a HiPRC on the DART: default false
          --generate Generate a fake test dart (recomended to use with --useFakeNet)
              --dump Dumps all the arcvives with in the given angle
   -w        --width Sets the rings width and is used in combination with the generate
   -r        --rings Sets the rings height and is used in  combination with the generate
   -P   --passphrase Passphrase of the keypair : default: verysecret
   -h         --help This help information.
```