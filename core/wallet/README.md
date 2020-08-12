# Tagion Util

The CLI tool to create Tagion bills for devnet.

```
Usage:
tagionwallet [<option>...]

<option>:
          --version display the version
-w         --wallet Walletfile : default tagionwallet.hibon
-c        --invoice Invoicefile : default invoice.hibon
   --create-invoice Create invoice by format LABEL:PRICE. Example: Foreign_invoice:1000
-t       --contract Contractfile : default contract.hibon
-s           --send Send contract to the network
-I            --pay Invoice to be payed : default
-U         --update Update your wallet
-m           --item Invoice item select from the invoice file
-x            --pin Pincode
-p           --port Tagion network port : default 10800
-u            --url Tagion url : default localhost
-g         --visual Visual user interface
-h           --help This help information.
```