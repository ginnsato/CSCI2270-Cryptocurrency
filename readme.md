# CSCI 2270 – Data Structures - Final Project 

Start by carefully reading the write-up contained in `CSCI2270_Spring22_Project.pdf`.

Please include a thorough description of your program's functionality. Imagine that you are publishing this for users who know nothing about this project. Also, include the names of the team-members/authors.


# Money Moves by Ginn Sato and Terence Williams

For this project we implemented a simple blockchain that allows users to transfer a currency back and forth.
The blockchain is essentially a linked list that contains different blocks. Each block has its own hash value, the hash value of the previous block in the block chain, a vector of transactions, a timestamp, and a value nonce used in the calculation of its hashcode. 

Each transaction contains three fields, the sender, recipient, and amount transfered. The main purpose of each block is to hold the information of these transactions. 

The blockchain also contains its own vector of pending transactions located outside of individual block. This list of pending transactions grows until the process of mining occurs. The purpose of mining is to package the list of pending transactions into a block, and insert this block into the blockchain. This process is computationally complex in order to more intesely secure our blockchain. We implemented a mineBlock function that alters the value of nonce until we recieve a hashcode that contains the number of difficulty leading 0s. For example if difficulty = 4, then our mineBlock function would find the nonce value that allows the hashcode to have 4 leading zeros. This works because we concatenate our nonce value with the other values of our block and pass it into the SHA256 function to receive a hash code. 

In order to mine these blocks, a user must call the minePendingTransaction(string minerAddress) function. This function instantiates a new block with the list of transactions equal to the list of pending transactions. It then mines this block, clears the list of pending transactions, and adds it into the blockchain. The reward of doing such a task is to add a new pending transaction sent by "BFC" to the minerAddress passed into function in the amount of miningReward. 

In order to successfully add a transaction to the list of pending transactions, we need to ensure that our sender address has enough funds to even make the transaction in the first place. It would be unrealisitc to allow users to send money they do not have. We have an addTransaction(string src, string dst, int coin) function that will first check if the src has enough coins by calling getBalanceofAddress(string address) with the address of src. One reason why the blockchain is so secure is that we do not store the value of any given address in a single place in the block chain, but rather the information leading to the total value is scattered throughout the blockchain's various transactions. Thus in order to determine if the given address has enough coins, we iterate through the blockchain, then iterate through the individual block's transactions and search for transactions that contain the given address as either the sender or recipient. If the address is the recipient then we increase their total and likewise if the address is the sender we decrease their total. It is important to note that we must also iterate through the list of pending transactions to ensure that our sender does not try to send more money then they have. If at any point the value becomes negative, which it should not, we shall return an error and return -1. Otherwise, the total of the address's net value is returned. Our addTransaction function will ensure that the value of the src is greater than or equal to the desired amount being sent. If it is, we can add this transaction to the vector of pending transactions, otherwise we do not add it and report than the sender has insufficient funds, much like a debit card machine at the grocery store. 

We also have a function isChainValid() that ensures our chain is valid by checking the previousHash values of each block, and ensuring that the difficulty level of each block (expect the first) is maintained throughout the blockchain. 

Another useful function we implemented is the prettyPrint(). This function prints the different blocks located in the chain and all their content in a neat manner that represents a linked list of blocks. Where each previous block points to its next block. This function is useful for gathering information about the functionality of our blockchain. 

Users can create their own main function to interact with the blockchain. When you call the blockchain constructor, you create one sample block that contains useless transaction information. Then, when an address mines a block by calling minePendingTransactions(), it creates a new block, mines it, and then rewards this address with 10 units of currency which will not be recieved until the next block is mined since this reward transaction is added to the list of pending transactions. 

Once address's have been rewarded currency by mining blocks, they can transfer this currency amongst eachother by addTransaction() and users of the program can keep track of the blockchain with prettyPrint(). 

We implemented a simple main function that allows users to interact with the blockchain. You can addTransactions, mine blocks, and check if the chain is valid. Each iteration will print out the block chain for the user to view. 