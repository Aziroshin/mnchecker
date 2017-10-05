# mnchecker
A tool to check the health of masternode wallets, which currently only supports comparing the block count of a block explorer to that of a masternode, triggering a re-initialization of the masternode's block chain data if the discrepancy is evaluated as too great.

To operate, the script relies on the following pieces of information, which can be specified on the command line as well as configuation variables in the script itself:

- Name, handle or symbol of the currency you're checking a masternode of. This is determined by the explorer in question (explorerz.top at the time of this writing, which means you'll use the subdomain used by that service for that particular currency). The command line parameter for this is "--currency-handle". The default value for this is "vivo".

- The location of the "cli" and the "d" (daemon) binary of the masternode wallet. This is specified by the "--currency-bin-cli" (default value: "vivo-cli") and "--currency-bin-daemon"  (default value: "vivod") command line parameter specifically.

- The datadir of the masternode, where all the blockchain and wallet data is stored. This defaults to the directory ".vivocore" in the home directory of the user you're running the script as. If you're running Linux **and** you're using "sudo", be mindful that the HOME variable might not be set to what you'd intiutively expect, and you might want to set it specifically (e.g. HOME=/home/c0inr0ckz0r478 sudo -u c0inrockz0r478 mnchecker ). Note: That's just something I've encountered in the past in general, I have not tested that scenario with this script (yet)).

- Not strictly required, but it might be useful for those with rather heavily customized setups. If not specified, the wallet will be used without specifying a configuration file, which will cause the wallet to default to its own default file location. The script's parameter for this is: "--currency-conf".

- Depending on your coin, you might want to specify a particular maximum difference the explorer's block count and the block count of your masternode should have before having the script take action. That block difference can be specified using the following parameter: "--threshold". The default value is 5.

The script is primarily intended to be integrated into a larger setup, where it is being run regularly through something like crontab, for example. If you have multiple masternodes running on your system, you'd want to integrate the script with whatever way you're managing them with. If you manage them by hand, you might want to create a bash script with multiple calls to this script for every masternode, or simply add a line per node that calls this script to your crontab configuration.

If you use a masternode management script of sorts, you might want to add this script to its loop or callback method, passing the appropriate values available there to this script as you're calling it from there.

Example usage:
*mnchecker --currency-handle="vivo" --currency-bin-cli="/home/user/wallets/vivo/vivo-cli" --currency-bin-daemon="/home/user/wallets/vivo/vivod" --currency-datadir="/home/user/masternodes/vivo/kardakhim/data"*