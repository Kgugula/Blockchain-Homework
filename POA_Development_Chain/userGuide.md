# User Guide - Keiran's Custom Blockchain #

## Introduction ## 

Hello and welcome to my custom built blockchain. Stored here is a user guide to interacting with nodes on my network and all relevant information you need to do so. 

## Start Nodes ##

### Step 3: Activate Nodes ### 

    Once your nodes have been initialized

    Open two terminal windows side-by-side

        Run this command in the left terminal window (let's call this Node1):

            ./geth --datadir node1 --unlock "SEALER_ONE_ADDRESS" --mine --rpc --allow-insecure-unlock - where "SEALER_ONE_ADDRESS" is the public address of node1 (given at time of creation)

        Once you have entered this into the first terminal (left), there will be a lot of lines of code starting to be produced. Keep your eyes peeled. You need to copy a link (enode address), this will be spit out near the beginning of the process. 

        Grab this enode address --> see screenshot captured below

        The below lines relate to the right terminal window (Node2). Keep Node1 (left terminal) up and running throughout all of these steps. Paste the below lines into the Node2 (right terminal), after replacing the relevant details. 

            ./geth --datadir node2 --unlock "SEALER_TWO_ADDRESS" --mine --port 30304 --bootnodes "enode://SEALER_ONE_ENODE_ADDRESS@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock

            Replace SEALER_TWO_ADDRESS with the public address of node2. Replace the SEALER_ONE_ENODE_ADDRESS with the link copied from the first (left) terminal

        After these steps are completed, both terminal windows should be looking busy with activity. Congrats! You have successfully created two nodes that are mining on my custom network (keiranschain)

### Step 4: MyCrypto ###

    With both nodes up and running, the blockchain can be added to MyCrypto for testing. If you do not have the app on your local machine you can download it here: https://download.mycrypto.com/



      
## Information ##

Network Name: keiranschain
Consensus Algorithm: Clique Proof-of-Authority (POA)
Accounts to seal: Node 1 & Node 2
Chain/Network ID: 777
Password: fintech123


    Node 1

    Public address: 0xC56C6C11B4ad59CF536B554Bf31F5953884b17af
    Path of Secret Key File: node1/keystore/UTC--2021-02-07T15-39-38.573070000Z--c56c6c11b4ad59cf536b554bf31f5953884b17af
   

    Node 2

    Public address: 0xb19Ed2F14FFd7c07D18A8025bA7Eb9FBE472ff56
    Path of Secret Key File: node2/keystore/UTC--2021-02-07T15-42-01.676398000Z--b19ed2f14ffd7c07d18a8025ba7eb9fbe472ff56
    
    


