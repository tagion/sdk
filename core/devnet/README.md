# Tagionwave

The program that runs the devnet.

``` bash
                       --version display the version
-O                   --overwrite Overwrite the config file
-T           --transcript-enable Transcript test enable: default: false
-D             --transaction-max Transaction max = 0 means all nodes: default 0
                          --port Host port
-I                        --path Sets the search path
-g                --trace-gossip Sets the search path
-N                       --nodes Sets the number of nodes: default 4
                          --seed Sets the random seed: default 42
-t                     --timeout Sets timeout: default 800 (ms)
-d                       --delay Sets delay: default: 200 (ms)
                         --loops Sets the loop count (loops=0 runs forever): default 30
                           --url Sets the url: default 127.0.0.1
-M                     --sockets Sets maximum number of monitors opened: default 0
                           --tmp Sets temporaty work directory: default '/tmp/'
-P                     --monitor Sets first monitor port of the port sequency (port>=6000): default 10900
-s                         --seq The event is produced sequential this is only used in test mode: default false
                        --stdout Set the stdout: default /dev/tty
                --transaction-ip Sets the listener ip address: default 0.0.0.0
-p            --transaction-port Sets the listener port: default 10800
             --transaction-queue Sets the listener max queue lenght: default 100
            --transaction-maxcon Sets the maximum number of connections: default: 1000
          --transaction-maxqueue Sets the maximum queue length: default: 100
          --transaction-maxreuse Sets the maximum number of fibre reuse: default: 1000
               --transcript-from Transcript test from delay: default: 333
                 --transcript-to Transcript test to delay: default: 888
                --transcript-log Transcript log filename: default: transcript
                 --dart-filename Dart file name. Default: dart.drt
              --dart-synchronize Need synchronization
          --dart-angle-from-port Set dart from/to angle based on port
   --dart-master-angle-from-port Master angle based on port
                     --dart-init Initialize block file
                 --dart-generate Generate dart with random data
                     --dart-from Dart from angle
                       --dart-to Dart to angle
                  --dart-request Request dart data
               --logger-filename Logger file name: default: /tmp/tagion.log
-h                        --help This help information.
```