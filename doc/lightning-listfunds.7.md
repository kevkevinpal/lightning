lightning-listfunds -- Command showing all funds currently managed by the Core Lightning node
==========================================================================================

SYNOPSIS
--------

**listfunds** [*spent*]

DESCRIPTION
-----------

The **listfunds** RPC command displays all funds available, either in
unspent outputs (UTXOs) in the internal wallet or funds locked in
currently open channels.

*spent* is a boolean: if true, then the *outputs* will include spent outputs
in addition to the unspent ones. Default is false.

RETURN VALUE
------------

[comment]: # (GENERATE-FROM-SCHEMA-START)
On success, an object is returned, containing:

- **outputs** (array of objects):
  - **txid** (txid): the ID of the spendable transaction
  - **output** (u32): the index within *txid*
  - **amount\_msat** (msat): the amount of the output
  - **scriptpubkey** (hex): the scriptPubkey of the output
  - **status** (string) (one of "unconfirmed", "confirmed", "spent", "immature")
  - **reserved** (boolean): whether this UTXO is currently reserved for an in-flight tx
  - **address** (string, optional): the bitcoin address of the output
  - **redeemscript** (hex, optional): the redeemscript, only if it's p2sh-wrapped

  If **status** is "confirmed":

    - **blockheight** (u32): Block height where it was confirmed

  If **reserved** is "true":

    - **reserved\_to\_block** (u32): Block height where reservation will expire
- **channels** (array of objects):
  - **peer\_id** (pubkey): the peer with which the channel is opened
  - **our\_amount\_msat** (msat): available satoshis on our node's end of the channel
  - **amount\_msat** (msat): total channel value
  - **funding\_txid** (txid): funding transaction id
  - **funding\_output** (u32): the 0-based index of the output in the funding transaction
  - **connected** (boolean): whether the channel peer is connected
  - **state** (string): the channel state, in particular "CHANNELD_NORMAL" means the channel can be used normally (one of "OPENINGD", "CHANNELD_AWAITING_LOCKIN", "CHANNELD_NORMAL", "CHANNELD_SHUTTING_DOWN", "CLOSINGD_SIGEXCHANGE", "CLOSINGD_COMPLETE", "AWAITING_UNILATERAL", "FUNDING_SPEND_SEEN", "ONCHAIN", "DUALOPEND_OPEN_INIT", "DUALOPEND_AWAITING_LOCKIN")

  If **state** is "CHANNELD_NORMAL":

    - **short\_channel\_id** (short\_channel\_id): short channel id of channel

  If **state** is "CHANNELD_SHUTTING_DOWN", "CLOSINGD_SIGEXCHANGE", "CLOSINGD_COMPLETE", "AWAITING_UNILATERAL", "FUNDING_SPEND_SEEN" or "ONCHAIN":

    - **short\_channel\_id** (short\_channel\_id, optional): short channel id of channel (only if funding reached lockin depth before closing)

[comment]: # (GENERATE-FROM-SCHEMA-END)

AUTHOR
------

Felix <<fixone@gmail.com>> is mainly responsible.

SEE ALSO
--------

lightning-newaddr(7), lightning-fundchannel(7), lightning-withdraw(7), lightning-listtransactions(7)

RESOURCES
---------

Main web site: <https://github.com/ElementsProject/lightning>

[comment]: # ( SHA256STAMP:62a8754ad2a24dfb5bb4e412a2e710748bd54ef0cffaaeb7ce352f6273742431)
