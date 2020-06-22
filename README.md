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

Keep in mind: If you want to execute a file in your current working directory, you'd want to prefix the filename with "./". For the above example, that would mean "./mnchecker" instead of "mnchecker". The example makes the generic assumption that "mnchecker" has been added to or linked into a directory that is globally being searched whenever you type in a command (because it is being referenced in the $PATH variable), such as /usr/local/bin, for example.

The most straightforward way to automate the recurring execution of the script would be crontab (which you can edit using the following command: *crontab -e*). To execute the above example mnchecker usage command, you'd put the following line into your crontab:

\*/30 \* \* \* \* /usr/local/bin/mnchecker --currency-handle="vivo" --currency-bin-cli="/home/user/wallets/vivo/vivo-cli" --currency-bin-daemon="/home/user/wallets/vivo/vivod" --currency-datadir="/home/user/masternodes/vivo/kardakhim/data" >/dev/null 2>&1

NOTE: The script features a command that lets you generate a crontab line: Just add *--gen-crontab* to it, and instead of running, it'll generate a line for you. You'll then have to copy that line into your crontab (all on one line). WARNING: That feature is new. Be observant upon using it.

WARNING: If you're reading this in pure text form and not on github (or some markdown editor or viewer), disregard all the backslash characters preceding the asterisks in the above example, they are related to markdown formatting in this case and are not a part of the actual line that's supposed to go into your crontab.

Don't forget that */usr/local/bin/mnchecker* is just an example, and that you'll have to replace that with the actual path to where you've ended up putting it. Say, if you used "*git clone*" in your home directory, the full path would look like this: "*/home/user/mnchecker/mnchecker*". If you downloaded the script by hand, or pasted it into an editor, you'd have to find out the absolute path of the file in question and reference that. Don't forget that absolute paths on Linux always start with a "/", so don't miss that one. :)

## Installing the Script

1. You need *git* installed. Assuming that you're running Ubuntu or another debian based system, you'd run the following command: *apt-get install git*
2. Enter the directory you intend to download this git repository to (it will put everything into a distinct folder called *mnchecker*).
    - Example command: *cd*
        - Running *cd* without any parameters will change your directory to your home directory, which, assuming your system user is called *c0inr0ckz0r478*, would be located in */home*, like this: */home/c0inr0ckz0r478*, unless you're running your masternode setup as *root* (not recommended), in which case the home directory path would be the following: */root*.

3. Download this git repository using git: *git clone https://github.com/aziroshin/mnchecker*
    - You can be done at this stage. If you choose so, replace *mnchecker* in the example command given in the crontab section with the full path of the mnchecker script. If, say, you've ran the *git clone* command from */home/c0inr0ckz0r478*, you'd specify */home/c0inr0ckz0r478 mnchecker/mnchecker* instead of *mncheckers*. Don't forget the parameters as laid out in that example, of course.

4. If you decide you'd like to make mnchecker accessible from anywhere on the VPS (without having to call it with the entire path specified or "./" prefixed when you're in the directory), you can do the following (assuming the following path for mnchecker: "/home/c0inr0ckz0r478/mnchecker/mnchecker"): *sudo cp /home/c0inr0ckz0r478/mnchecker/mnchecker /usr/local/bin/*
    - Once you update mnchecker, you'll have to execute this command again. You could also just symlink mnchecker (replace *cp* with *ln -s*), but that'd potentially open a security hole if you've properly set up a user account and are not running any of this as root.
    - As you'll probably mostly run mnchecker through cron anyway, and you'll want to specify an absolute path for cron, this step isn't that important.

## Updating the Script
1. Enter the git directory that was created when you ran *git clone* (e.g. if you ran that command in */home/c0inr0ckz0r478*, you'd *cd /home/c0inr0ckz0r478/mnchecker*).
2. Run the following command: *git pull*
3. [optional] If you've copied the mnchecker script somewhere when you installed it (e.g. to */usr/local/bin*), you'll have to do that again.

## Further help and Diagnosis

To get the full list of commands for the script, run it with *--help*. Fore some insights into how mnchecker interprets your options and sees its environment, try running it with --info (with all your other options specified as well).
