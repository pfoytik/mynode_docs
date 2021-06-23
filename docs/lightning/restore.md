# Restore Lightning Wallet

If you have created a Lighting Wallet in the past and need to recover it, you can do so via the myNode interface.

**You will need your seed phrase and Static Channel Backup file!**

First, click on Manage under the Lightning app on the myNode home page.

Then, click on "Restore Wallet from Seed" on the Lightning page. 

**Note:** You will only see the Restore button if no wallet exists. If a wallet had been created, you will first need to delete the Lightning wallet via the settings page.

<center>
  <figure>
    <img src="/images/lightning/restore-1.png" style="width: 300px">
  </figure>
</center>

On the next page, enter your seed phrase and upload your Static Channel Backup (SCB) file. Click Create.

![](/images/lightning/restore-2.png)

This will restore you on-chain balance, close all channels that had been opened, and restore those funds in you on chain wallet. Seeing the channel funds appear may take a while.

Your Lightning wallet is now setup and ready to use!

## Following Closing Channels
SSH into the mynode device or access the terminal from the status page listed next to Manage. Use lncli to check for pending channels by typing the command:

`lncli pendingchannels`

This should provide 4 array data structures "pending_open_channels", "pending_closing_channels", "pending_force_closing_channels", "waiting_close_channels". If the SCB file was successfull the channels should appear in these structures. 

**Note:** You will see 0 balance for these listed channels and this is normal as your lightning node is relying on the other peer to force close the channels. Below is an example of what a SCB recovery pending channel might look like:

`"waiting_close_channels": [
{
"channel": {
"remote_node_pub": "0232xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
"channel_point": "a20488xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx9:1",
"capacity": "1000000",
"local_balance": "0",
"remote_balance": "0",
"local_chan_reserve_sat": "0",
"remote_chan_reserve_sat": "0",
"initiator": "INITIATOR_REMOTE",
"commitment_type": "STATIC_REMOTE_KEY"
},
"limbo_balance": "0",
"commitments": {
"local_txid": "",
"remote_txid": "",
"remote_pending_txid": "",
"local_commit_fee_sat": "0",
"remote_commit_fee_sat": "0",
"remote_pending_commit_fee_sat": "0"
}
}`

It is also possible that during this period of waiting for channels to close you will see in the lnd.log file messages that look similar to:

`2021-06-11 20:30:27.139 [ERR] HSWC: ChannelLink(6xxxxx0:487:1): failing link: unable to synchronize channel states: ChannelPoint(0232xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx:1) with CommitPoint(a20488xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx9) had possible local commitment state data loss with error: unable to resume channel, recovery required`

This is normal and it is best to grab a beer and give it time to process, some have described channels taking weeks to close as a reference.


