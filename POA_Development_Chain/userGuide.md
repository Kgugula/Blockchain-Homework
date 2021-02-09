# User Guide - Keiran's Custom Blockchain #

## Introduction ## 

Hello and welcome to my custom built blockchain. Stored here is a step-by-step user guide for setting up your own private blockchain network, initializing nodes, and sending test transactions. First we are going to outline the required dependencies that you will need on your local machine to get all of this up and running, then jump right into the steps for how to setup your private blockchain. At the bottom of the page you can see the information pertaining to the blockchain I created throughout this example, some troubleshooting tips, and finally a couple of readings I found helpful along the way. I will include the 'gugulachain' relevant files/folders on my Github, so if would like to skip a couple of steps and try this on my network private network feel free to do so. Hope you enjoy!

## Dependencies ##

You will need to download the MyCrypto App and GO Ethereum Tools. 

MyCrypto is a free, open-source, client-side interface that allows you to interact directly with the blockchain. This interacting "directly" with the blockchain means you are not using a third-party provider (such as an exchange) to interact with the blockchain and manage your private key and funds. The implication that comes with this is that you are responsible for keeping your sensitive information safe (namely) your private key. If this gets lost there is no way to prove ownership of your account (and the funds in it). I would recommend reading more about MyCrypto on there website here: https://mycrypto.com/. Note that MyCrypto is an Ethereum Wallet Manager - you can only manage Ethereum-based tokens on it. For the purposes of this demonstration we will be using TestNet tokens which have no inherent value (but then again, neither does Dogecoin and it's valued at a couple billion dollars now). 


GO Ethereum Tools is open-source software written in the 'Go' programming language. These tools allow you to create your own blockchain, from creating a genesis block to mining tokens and making transactions. To download go to https://geth.ethereum.org/downloads/ and select the package relevant to your operating system. Select the newest version, and download the 'Geth & Tools' flavor of the package. I recommend creating a new directory on your local machine and moving the downloaded files there. Please feel free to read more about both dependecies on your own at the many available sources. A tutorial on both packages is somewhat out of scope for this project and so described here is more of a high-level overview. 

## Steps ##

    I would recommend opening a word document, text editor, or having a blank piece of paper handy. You will need to keep track of a few addresses, keys, passwords, etc. 

### Step 1 ###

    The first step is to initialize the first two nodes in your network. These nodes will be interacting with each other to mine transactions. In reality, your machine would represent just one node in a network containing many. For educational purposes, however, it is really helpful to see how these nodes interact in the blockchain, and by having two on your local machine you get somewhat of a bird's eye view. 

    After the relevant dependencies have been installed, navigate into the directory where the 'GO Ethereum Tools' are located. On my local they are in a folder called 'BlockchainTools.' From there, enter the two commands below into your terminal (Git bash on Windows). This will initialize the nodes on your network. 

        ./geth --datadir node1 account new
        ./geth --datadir node2 account new

    It will prompt you for a password. Create a password and keep track of it. For simplicity I made both passwords the same and used a simple three-digit combination e.g. 123.  It will then give you a public address and path to your secret key file. Keep track of both of these, along with your password (you will need them later).

### Step 2 ###

    Next you are going to create your genesis block. Think of this like patient zero for COVID-19. It is the first block, and it contains information related to how the disease (the chain) operates. 

    While still in your folder containing your GO Ethereum Tools - run puppeth

        ./puppeth

    Specify a network name to administer (have fun with it!)

    Select the option to configure a new genesis block

    Select the option to create a new genesis block from scratch

    Choose a consensus algorithm to use (I used proof-of-authority here, which I would recommend doing if you are following along, some of the finer details related to configuration are different for the proof-of-work algorithm). Essentially, proof-of-authority uses a nodes identity (i.e. your identity) as collateral in exchange for allowing you to validate blocks. I won't delve into too much detail here since folks on Medium have done a far superior job. There are some helpful links at the bottom of the page related to the POA consensus algorithm, and how it differs from the rest. 

    Enter how many seconds blocks should take to mine (Default is 15, just hit enter to select)

    Paste both account addresses from the first step one at a time into the list of accounts to seal. Note -- DO NOT copy the leading '0x' from the addresses of your nodes. That is already provided. After you have entered both, hit enter again when it prompts you for a third address. 

    Again paste both account address in the list of accounts to pre-fund

    You can choose 'no' for pre-funding the pre-compiled accounts with wei. This keeps the genesis cleaner. 

    Specify your chain/network ID - if you want an explicit one (recommended - enter a three digit code of your choosing. Keep track of this code. You will need it when setting up your private network on MyCrypto). 

    When back at the main page - select 'Manage Existing Genesis'

    Select 'Export Genesis Configurations' - this will create files that will be downloaded on your local machine. 

    If you are already in your folder containing your BlockchainTools, then it will save them there by default (recommended). Hit enter to do so.

    Do not freak out. You will see that it failed to create two of the files from the previous step. We do not need these because we are using a proof-of-authority (POA) algorithm. You can delete the networkname-harmony.json file. Your folder should look something like the picture seen on the Screenshots repository (re: Puppeth Configuration). 

    Congrats! You have just created your genesis block. Now as a bonus go and ask Elon Musk to tweet about it!

### Step 3 ###

    With the genesis block creation complete, we will now initialize the nodes with the genesis' json file. Using geth, initialize each node with the new networkname.json, replacing networkname with the name of your network (make sure you are in your 'BlockchainTools' directory)

        ./geth --datadir node1 init networkname.json
        ./geth --datadir node2 init networkname.json
    
    It should outline a couple lines of code. No need to get caught in the weeds here - if you see green then you are a go! Now the nodes can begin mining blocks. This step should have created two new directories in your folder, titled node1 and node2. 

### Step 4 ###

    Now that the nodes are initialized, they can begin mining blocks on your network with a few simple commands. I hope you have an extra monitor for this step. Things might get hectic, just stay calm and read the instructions carefully. I recommend reading through the instructions related to Step 4 once in their entirety before manuevering so you know what the goal is. It is probably a good idea to open a separate work document or text editor as we will be copy and pasting a few things. 

    Open two SEPERATE terminal windows. Navigate to your directory with the GO Ethereum Tools and your new json file (e.g. BlockchainTools)

    In the terminal window on the left (lets call this Node 1) - run the following command, replacing SEALER_ONE_ADDRESS with the public address of your first node. After entering, TYPE YOUR PASSWORD FROM STEP 1 and hit enter - even if you can't see it visually. 

        ./geth --datadir node1 --unlock "SEALER_ONE_ADDRESS" --mine --rpc --allow-insecure-unlock

        Exampe --> ./geth --datadir node1 --unlock "0xB26513BF972b6a9D61FE4Acbd7E106AF88469c18" --mine --rpc --allow-insecure-unlock
    
    Once you do this, many lines of code will start populating your terminal (deep breath). From these lines of code, you will be looking for the ENODE address of your first node. This will be output near the beginning. Please see the accompanied screenshot titled 'Enode Grab.' Copy this address you will need it for the next step. 

    Moving on to the terminal on the right side of your window (Node 2). Paste the following - replacing the SEALER_TWO_ADDRESS with the public address of Node 2, and replacing SEALER_ONE_ENODE_ADDRESS with the ENODE address copied from above. After entering, TYPE YOUR PASSWORD FROM STEP 1 and hit enter - even if you can't see it visually. 

    ./geth --datadir node2 --unlock "SEALER_TWO_ADDRESS" --mine --port 30304 --bootnodes "enode://SEALER_ONE_ENODE_ADDRESS@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock

    Example --> ./geth --datadir node2 --unlock "0x94a2EA95c78e8E4938E79FDEAcF7Bb8CB830DD87" --mine --port 30304 --bootnodes "enode://0859080a9a5c99fe28f5c0306adce6978cd2e1b56945244df6518ede84f29ac56d4db455301e3a348ef6864a04ace8561606e3895eec88c41363a7a41a6323db@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock

    Keep your terminals running. They should be looking very busy. This is good. They are interacting with eachother and mining blocks. In the next steps we will send a transaction for them to mine. 

    Your private POA blockchain should now be running!

### Step 5 ###

    With both nodes up and running, the blockchain can be added to MyCrypto for testing. In this section we will be sending a transaction from Node 1 to Node 2 using the MyCrypto App. 

    First things first, we are going to create a custom network on MyCrypto for your chain to operate on. 

    Navigate into MyCrypto. Click 'Change Network' in the bottom left. Scroll near the bottom and click "Add Custom Node", and then add the custom network information that you set in the genesis. See Screenshot "Custom Network." Make sure you scroll down to choose "Custom" in the "Network" column to reveal more options like "Chain ID" (seen in the screenshot). In the "Chain ID" box, type the Chain/Network ID your generated during genesis configuration (e.g. mine is 777). In the url box type: http://127.0.0.1:8545 --> this points to the default RPC port on your local machine. Click "Save & Use Custom Node." 

    Note --> the screenshot seen isn't the custom network I connected to this account, it's just a sample. Remember your Chain ID must match the one you created during genesis configuration. 

### Step 6 ###

    Now that we have created a custom network on MyCrypto, we are going to send a transaction between the two nodes. First make sure you are connected to your network created in Step 5.

    Once on the MyCrypto homepage, it will ask you how you want to access your wallet. Select the option "Keystore File."  Then navigate to the keystore directory inside your Node 1 directory, select the file located there, provide your password when prompted and then click "Unlock." 

    This will open your account inside MyCrypto. Under "Account Balance" you should see a very large number. These are test tokens that you assigned to your account during genesis configuration (when you inputed the public address to pre-fund each node). 


### Step 7 ###

    Our final step involves sending funds from our first node to our second. The steps would be the same if you chose to do this the other way around. 

    While in MyCrypto, navigate to the "View & Send" tab located on the left sidebar. Under "To Address" input the public address of Node 2. Enter an arbitrary amount of ETH to send (I recommend sending a larger amount so you will be able to see the change in your Node 1 account - e.g. 150,000). You can keep the transaction fee as is, or slide it left or right, it won't make too much of a difference. If this was the real "crypto world" you would increase the transaction fee (i.e. the fee paid to those who mine the block) to get the transaction done more quickly. Once all the required fields are populated - hit send. You should see a pop up near the bottom of your screen - see screenshot 'Hash.' 

    At this point your transaction has just been broadcast to the custom network (your network). The status of the transaction will show as "PENDING" until your nodes have successfully mined the block. You can view this by selecting "Check TX Status." There will be a unique hash number associated with transaction. You can copy this down if you like, but it's not necessary unless you have completed many many transaction and need it to distinguish one from another.
    
    This process of approving your transaction should take approximately the same amount of time you specified when you configuring your genesis block. Now is a really cool time to navigate back to your window with the two terminal tabs open. If you follow the lines of code after the transaction is sent you will see how a new block is mined in real time. 
    
    There are two ways to check the status of your transaction. If you are already in your wallet, you can select "Recent Transactions" from the dropdown menu located in the middle of your screen. You can also click "TX Status" in the left sidebar and navigate to the dropdown there.  

## Information ##

    Network
    Name: gugulachain
    Consensus algorithm: Clique Proof-of-Authority (POA)
    Seconds to mine: 15
    Chain/Network ID: 777

    Node 1
    Password: 123
    Public address: 0xB26513BF972b6a9D61FE4Acbd7E106AF88469c18
    Path of secret key file: node1/keystore/UTC--2021-02-08T14-42-51.005723000Z--b26513bf972b6a9d61fe4acbd7e106af88469c18

    Node 2
    Password: 123
    Public address: 0x94a2EA95c78e8E4938E79FDEAcF7Bb8CB830DD87
    Path of secret key file: node2/keystore/UTC--2021-02-08T14-45-43.429289000Z--94a2ea95c78e8e4938e79fdeacf7bb8cb830dd87

## Troubleshooting ##

    If you run into problem along your journey, I'd recommend first trying out a few general fixes that worked for me. 

    If you aren't seeing any movements in the wallet amounts in MyCrypto after sending/receiving transactions, try the following:

        Terminate both nodes using CTRL+C in the Node1 and Node2 terminal windows
        Change networks in MyCrypto to a Testnet such as Kovan
        Restart Node1 and Node2 in their terminal windows
        Reconnect to your network in MyCrypto
        Log into your wallet and refresh the amount (you can refresh the page once in MyCrypto by navigating View --> Reload)

    Another tip I found helpful for general troubleshooting (i.e. having trouble connecting to your network, etc.) is to clear the site data from your cache. To do this, open MyCrypto. Go to View --> Toggle Developer Tools --> Application --> Clear Site Data (located at the bottom of the page)

    I also ran into this strange problem while trying to activate the nodes in my network (after they had already been created). I would follow the steps above and run the commands from Step 4 to get my nodes running, but there would be no output. I would try and unlock Node1 and my terminal window would output nothing. I narrowed the scope of the problem to the 'geth' file in my BlockchainTools folder (my terminal would not respond even if I wanted to check the version of geth). This could have been an idiosyncratic error since I had updated my OS system sometime between downloading Geth and completing this assignent, but I essentially had to redownload Geth, and moved only the 'geth' dependency (seen in Screenshots --> Puppeth Configuration) to my BlockchainTools folder. I doubt you will run into this issue but thought I would outline it here just in case!

    Did you enter the passwords for your nodes in Step 4? I forgot to do this a handful of times. 

    As always, restarting your computer is always a good failsafe. 

    Helpful Links

        Github: https://github.com/ethereum/go-ethereum/issues
        Geth Documentation: https://geth.ethereum.org/docs/
        Consensus Algorithms: 
             Medium: https://medium.com/lumiwallet/what-is-proof-of-work-proof-of-stake-and-proof-of-authority-96729b110e62
             Medium: https://unitedcrowd.medium.com/what-is-proof-of-authority-and-how-does-it-work-32483ed597e4
             Medium: https://medium.com/coinmonks/proof-of-work-vs-proof-of-stake-for-real-idiots-a23ac4565649 
             Medium: https://medium.com/poa-network/proof-of-authority-consensus-model-with-identity-at-stake-d5bd15463256


    Happy Mining!
