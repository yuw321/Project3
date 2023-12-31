# 95-702 Distributed Systems For Information Systems Management
## Project 3 Blockchain                             Spring 2023


Assigned: Friday, February 24, 2022
Due Friday, March 17, 2023 at 11:59pm

### Important Note

Place your email address and full name at top of each source code file.

### Principles
One of our primary objectives in this course is to make clear the fundamental
distinction between functional and nonfunctional characteristics of distributed
systems. The functional characteristics describe the business or organizational
purpose of the system. The non-functional characteristics affect the quality of
the system. Is it fast? Does it easily interoperate with others? Is it fault
tolerant? Is it reliable and secure?

In this project, we will illustrate an important nonfunctional characteristic
of blockchain technology - its tamper evident design. We will build a stand-alone
blockchain (Task 0) and a distributed system where a remote client interacts with a blockchain API (Task 1).

Another important non-functional characteristic is interoperability. This will be illustrated in Task 2 where HTTP and JSON are used to interact with the Algorand blockchain.

Unlike Bitcoin, Algorand is based on an environmentally friendly consensus protocol. In Algorand, there is no proof-of-work. It is based on pure proof-of-stake.

### Prerequisites

In this project we will be using the Gson class to parse JSON messages. In this prerequisite section, there is guidance on setting up Gson in Intellij. JSON is a popular data format. It competes with XML. Either JSON or XML is appropriate to transfer textual data from one machine to another. Be sure to review the JSON grammar at www.json.org.

0. Create a new project named TestGsonProject and select "Maven" as the build system.
1. Edit the pom.xml file and include this XML element at the end of the file (before the closing project tag).
```
<dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.9.0</version>
</dependency>
```
2. Select File/Project Structure/Libraries/+/From Maven/.
   Using the search bar, search for com.google.code.gson.
   And then select com.google.code.gson:2.9.0.

3. Include the following import in your source code:

```
import com.google.gson.Gson;
```    

4. Test by compiling and running the following program (you may need to add a package):

```
import com.google.gson.Gson;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello world!");
        Gson gson = new Gson();
        System.out.println("Gson is available!");
    }
}
```

5. Suppose we have a message object that we want to serialize to JSON. Why do this?
We may want to transmit the data over a network in an interoperable and textual way.

```
package org.example;
import com.google.gson.Gson;

class Message {
    String name;
    int id;
    public Message(String name, int id) {
        this.name = name;
        this.id = id;
    }
}

public class Main {
    public static void main(String[] args) {
        // Create a message
        Message msg = new Message("Alice", 30);
        // Create a Gson object
        Gson gson = new Gson();
        // Serialize to JSON
        String messageToSend = gson.toJson(msg);
        // Display the JSON string
        System.out.println(messageToSend);
    }
}
```

6. Suppose we receive a message as a JSON string. We may want to deserialize the JSON string to a Java object. Why do this? This is a huge convenience. We do not have to parse the message ourselves.

```
package org.example;
import com.google.gson.Gson;

class Message {
    String name;
    int id;
    public Message(String name, int id) {
        this.name = name;
        this.id = id;
    }
}

public class Main {
    public static void main(String[] args) {
        // Create a message
        Message msg = new Message("Alice", 30);
        // Create a Gson object
        Gson gson = new Gson();
        // Serialize to JSON
        String messageToSend = gson.toJson(msg);
        // Display the JSON string
        System.out.println(messageToSend);

        // Suppose we receive the following JSON string from a network or file.
        // Double quotes would be used in a real message. Single quotes are used
        // here because we are doing this within a Java program.
        String someJSON = "{'id':45,'name':'Bob'}";
        Message incommingMsg = gson.fromJson(someJSON,Message.class);
        System.out.println(incommingMsg.name);
        System.out.println(incommingMsg.id);

    }
}
```

### Overview

In Task 0, you will write a blockchain by carefully following the directions found in our JavaDoc:

[See Block JavaDoc](https://www.andrew.cmu.edu/course/95-702/examples/javadoc/blockchaintask0/Block.html).

[See BlockChain Javadoc](https://www.andrew.cmu.edu/course/95-702/examples/javadoc/blockchaintask0/BlockChain.html).

Note that the Javadoc describes writing "data" or a "transaction" to the blockchain. In this project, our "data" or "transaction" will be simple statements transferring "dscoin" from one player to another.

The Javadoc describes two classes that you need to write - Block.java and BlockChain.java.

In Task 1, you will distribute the application that you created in Task 0. You will write a client server application. The interaction between the client and the server will be with JSON messages (using Gson) over TCP sockets. Thus, some work from Project2 will be very useful and may be reused here.

You will be submitting complete Java programs and console screen interactions on a single PDF file. These should be clearly labelled as described below.

You will also be submitting two IntelliJ projects as two zip files.
See below for a precise description of what needs to be submitted.

Documenting code is important. Be sure to provide comments in your code explaining what the code is doing and why.

Be sure to separate concerns when appropriate. It is fine to add methods that you feel are appropriate.

Be sure to include comments within your methods.

Data validation (of user input) is very important but we are not doing that here.

Any code from external sources, e.g., stack overflow, **must be clearly cited with a URL**.

In Task 2, there is no programming. You will work with an Algorand wallet and learn how to transfer funds (Algos) from one account to another. You will learn how to examine account values and transactions on Algorand's testnet blockchain. Algorand's REST API will be accessed with HTTP requests. To demonstrate that you have completed your work, you will submit screenshots on your single pdf.  

## Task 0  

Write a solution to Task 0 by studying the Javadoc provided (Block.java and BlockChain.java). The logic found in Task 0 will be reused in Task 1.

The execution of Task 0, a non-distributed stand-alone program, will look like the following interaction. As part of the submission of Task 0, you must turn in a copy and paste from the console - similar to the one below.

Label this first section ***Task 0 Execution*** in your PDF. Of course, your code - not mine - will produce the console interaction.

**In addition, wherever the name "Mike" is used, replace it with "Alice". Also, replace the names "Marty" and "Joe" and "Andy" with "Bill" and "Clara" and "Daisy".**

### Task 0 Execution

```
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
0
Current size of chain: 1
Difficulty of most recent block: 2
Total difficulty for all blocks: 2
Approximate hashes per second on this machine: 3231017
Expected total hashes required for the whole chain: 256.000000
Nonce for most recent block: 286
Chain hash: 0026883909AA470264145129F134489316E6A38439240D0468D69AA9665D4993
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
1
Enter difficulty > 0
2
Enter transaction
Mike pays Marty 100 DSCoin
Total execution time to add this block was 19 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
1
Enter difficulty > 0
2
Enter transaction
Marty pays Joe 50 DSCoin
Total execution time to add this block was 42 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
1
Enter difficulty > 0
2
Enter transaction
Joe pays Andy 10 DS Coin
Total execution time to add this block was 6 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
2
Chain verification: TRUE
Total execution time to verify the chain was  1 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
3
View the Blockchain
{"ds_chain" : [ {"index" : 0,"time stamp " : "2022-02-25 17:41:11.927","Tx ": "Genesis","PrevHash" : "","nonce" : 286,"difficulty": 2},
{"index" : 1,"time stamp " : "2022-02-25 17:42:46.053","Tx ": "Mike pays Marty 100 DSCoin","PrevHash" : "0026883909AA470264145129F134489316E6A38439240D0468D69AA9665D4993","nonce" : 165,"difficulty": 2},
{"index" : 2,"time stamp " : "2022-02-25 17:44:27.43","Tx ": "Marty pays Joe 50 DSCoin","PrevHash" : "000D14B83028DD36BD6330B8DAB185012F8625E9C9D1A8704E9C1189FD98D9DF","nonce" : 819,"difficulty": 2},
{"index" : 3,"time stamp " : "2022-02-25 17:45:22.044","Tx ": "Joe pays Andy 10 DSCoin","PrevHash" : "00B4CC539C5CC36AE2F09CC7B857A1330D2D02C00CA90D4A34ACD7E01D7225FC","nonce" : 167,"difficulty": 2}
 ], "chainHash":"002EEC64A0ABB7FF1FBBF72BE17BD3DC3C1D5FE5FB01360680930B1CFCF5A84A"}
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
4
corrupt the Blockchain
Enter block ID of block to corrupt
1
Enter new data for block 1
Mike pays Marty 76 DSCoin
Block 1 now holds Mike pays Marty 76 DSCoin
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
3
View the Blockchain
{"ds_chain" : [ {"index" : 0,"time stamp " : "2022-02-25 17:41:11.927","Tx ": "Genesis","PrevHash" : "","nonce" : 286,"difficulty": 2},
{"index" : 1,"time stamp " : "2022-02-25 17:42:46.053","Tx ": "Mike pays Marty 76 DSCoin","PrevHash" : "0026883909AA470264145129F134489316E6A38439240D0468D69AA9665D4993","nonce" : 165,"difficulty": 2},
{"index" : 2,"time stamp " : "2022-02-25 17:44:27.43","Tx ": "Marty pays Joe 50 DSCoin","PrevHash" : "000D14B83028DD36BD6330B8DAB185012F8625E9C9D1A8704E9C1189FD98D9DF","nonce" : 819,"difficulty": 2},
{"index" : 3,"time stamp " : "2022-02-25 17:45:22.044","Tx ": "Joe pays Andy 10 DSCoin","PrevHash" : "00B4CC539C5CC36AE2F09CC7B857A1330D2D02C00CA90D4A34ACD7E01D7225FC","nonce" : 167,"difficulty": 2}
 ], "chainHash":"002EEC64A0ABB7FF1FBBF72BE17BD3DC3C1D5FE5FB01360680930B1CFCF5A84A"}
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
2
Chain verification: FALSE
Improper hash on node 1 Does not begin with 00
Total execution time to verify the chain was  0 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
5
Total execution time required to repair the chain was 8 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
2
Chain verification: TRUE
Total execution time to verify the chain was  1 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
1
Enter difficulty > 0
4
Enter transaction
Andy pays Sean 25 DSCoin
Total execution time to add this block was 224 milliseconds
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
0
Current size of chain: 5
Difficulty of most recent block: 4
Total difficulty for all blocks: 12
Approximate hashes per second on this machine: 3231017
Expected total hashes required for the whole chain: 66560.000000
Nonce for most recent block: 9610
Chain hash: 0000DF114971BAF2F0DCC51777451973DF1AFE93189B64D7AC8BA06E39681067
0. View basic blockchain status.
1. Add a transaction to the blockchain.
2. Verify the blockchain.
3. View the blockchain.
4. Corrupt the chain.
5. Hide the corruption by repairing the chain.
6. Exit
6

Process finished with exit code 0
```

Label this second section **Task 0 Block.java** and include a complete listing of Block.java


Label this third section **Task 0 BlockChain.java** and include a complete listing of BlockChain.java

See the Javadoc's main routine. You are asked to experiment and provide some timing data and analysis. That commentary should be present in the comments of your main routine.

----

### Task 0 Grading Rubric 40 Points

Rubric:
1. The execution is correct and includes the same tests as above - in the same order (the names have been replaced with Alice, Bill, Clara, and Daisy): 20 points.
2. The code is well documented: 5 points.
3. The analysis in the main routine is detailed and clear: 5 Points.
Within your comments in the main routine, you must describe how this system behaves as the difficulty level increases. Run some experiments by adding new blocks with increasing difficulties. Describe what you find. Be specific and quote some times.
You need not employ a system clock. You should be able to make clear statements describing the approximate run times associated with addBlock(), isChainValid(), and chainRepair().
4. The code illustrates separation of concerns and good style: 5 points.
5. The single PDF file includes sections correctly labelled: 5 Points.  

-----

## Task 1

The client side execution of Task 1 will appear exactly the same as in Task 0. The primary difference will be that, behind the scenes, there will be a client server interaction using JSON over TCP sockets. The blockchain will exist on the server. It will be constructed there and the client will make requests for operations over a TCP socket. The client will be focused on driving the menu driven interaction and communicating with the server on the backend. If the client exits, the server will still handle new requests with the existing blockchain intact.

You are required to design and use two JSON messages types - a message to encapsulate
requests from the client and a message to encapsulate responses from the server. See the server side interaction below. Your JSON messages must have the same name value pairs. The order of the names may differ.

You should have a class named RequestMessage and a class named ResponseMessage to encapsulate the JSON data. You need to include these classes in your submission.

Use the following four labels in your single PDF:

**Task 1 Client Side Execution**

Copy and paste your client side console.

**Task 1 Server Side Execution (The names "Mike", "Marty", "Joe" and "Andy" will be replaced, as described above.)**

```
Blockchain server running
We have a visitor
Response : {"selection":0,"size":1,"chainHash":"0026883909AA470264145129F134489316E6A38439240D0468D69AA9665D4993","totalHashes":256,"totalDiff":2,"recentNonce":286,"diff":2,"hps":3231017}
Adding a block
Setting response to Total execution time to add this block was 19 milliseconds
...{"selection":1,"response":"Total execution time to add this block was 19 milliseconds"}
Adding a block
Setting response to Total execution time to add this block was 42 milliseconds
...{"selection":1,"response":"Total execution time to add this block was 42 milliseconds"}
Adding a block
Setting response to Total execution time to add this block was 6 milliseconds
...{"selection":1,"response":"Total execution time to add this block was 6 milliseconds"}
Verifying entire chain
Chain verification: TRUE
Total execution time required to verify the chain was 1 milliseconds
Setting response to Total execution time to verify the chain was  1 milliseconds
View the Blockchain
Setting response to {"ds_chain" : [ {"index" : 0,"time stamp " : "2022-02-25 17:41:11.927","Tx ": "Genesis","PrevHash" : "","nonce" : 286,"difficulty": 2},
{"index" : 1,"time stamp " : "2022-02-25 17:42:46.053","Tx ": "Mike pays Marty 100 DSCoin","PrevHash" : "0026883909AA470264145129F134489316E6A38439240D0468D69AA9665D4993","nonce" : 165,"difficulty": 2},
{"index" : 2,"time stamp " : "2022-02-25 17:44:27.43","Tx ": "Marty pays Joe 50 DSCoin","PrevHash" : "000D14B83028DD36BD6330B8DAB185012F8625E9C9D1A8704E9C1189FD98D9DF","nonce" : 819,"difficulty": 2},
{"index" : 3,"time stamp " : "2022-02-25 17:45:22.044","Tx ": "Joe pays Andy 10 DSCoin","PrevHash" : "00B4CC539C5CC36AE2F09CC7B857A1330D2D02C00CA90D4A34ACD7E01D7225FC","nonce" : 167,"difficulty": 2}
 ], "chainHash":"002EEC64A0ABB7FF1FBBF72BE17BD3DC3C1D5FE5FB01360680930B1CFCF5A84A"}
Corrupt the Blockchain
Block 1 now holds Mike pays Marty 76 DSCoin
Setting response to Block 1 now holds Mike pays Marty 76 DSCoin
View the Blockchain
Setting response to {"ds_chain" : [ {"index" : 0,"time stamp " : "2022-02-25 17:41:11.927","Tx ": "Genesis","PrevHash" : "","nonce" : 286,"difficulty": 2},
{"index" : 1,"time stamp " : "2022-02-25 17:42:46.053","Tx ": "Mike pays Marty 76 DSCoin","PrevHash" : "0026883909AA470264145129F134489316E6A38439240D0468D69AA9665D4993","nonce" : 165,"difficulty": 2},
{"index" : 2,"time stamp " : "2022-02-25 17:44:27.43","Tx ": "Marty pays Joe 50 DSCoin","PrevHash" : "000D14B83028DD36BD6330B8DAB185012F8625E9C9D1A8704E9C1189FD98D9DF","nonce" : 819,"difficulty": 2},
{"index" : 3,"time stamp " : "2022-02-25 17:45:22.044","Tx ": "Joe pays Andy 10 DSCoin","PrevHash" : "00B4CC539C5CC36AE2F09CC7B857A1330D2D02C00CA90D4A34ACD7E01D7225FC","nonce" : 167,"difficulty": 2}
 ], "chainHash":"002EEC64A0ABB7FF1FBBF72BE17BD3DC3C1D5FE5FB01360680930B1CFCF5A84A"}
Verifying entire chain
Chain verification: FALSE
Improper hash on node 1 Does not begin with 00
Total execution time required to verify the chain was 0 milliseconds
Setting response to Total execution time to verify the chain was  0 milliseconds
Repairing the entire chain
Setting response to Total execution time required to repair the chain was 8 milliseconds
Verifying entire chain
Chain verification: TRUE
Total execution time required to verify the chain was 1 milliseconds
Setting response to Total execution time to verify the chain was  1 milliseconds
Adding a block
Setting response to Total execution time to add this block was 224 milliseconds
...{"selection":1,"response":"Total execution time to add this block was 224 milliseconds"}
Response : {"selection":0,"size":5,"chainHash":"0000DF114971BAF2F0DCC51777451973DF1AFE93189B64D7AC8BA06E39681067","totalHashes":66560,"totalDiff":12,"recentNonce":9610,"diff":4,"hps":3231017}

```

**Task 1 Client Source Code**

Include all client side source code clearly labelled. This includes the RequestMessage and ResponseMessage classes. 

**Task 1 Server Source Code**

Include all server side source code clearly labelled. This includes the RequestMessage and ResponseMessage classes. These will be the same classes as found on the client side.

**Task 1 Grading Rubric 40 Points**

Rubric:
1. The execution is correct and includes the same tests as above - in the same order (the names "Mike", "Marty", "Joe" and "Andy" have been replaced) and a client server architecture based on TCP sockets is used. 20 points.
2. The JSON message being sent to the server is well designed (RequestMessage.java): 5 Points
3. The JSON message being sent from the server to the client is well designed (ResponseMessage.java): 5 Points.
4. Separation of concerns is well done: 5 points.
5. The single PDF file includes sections correctly labelled: 5 Points.

-----

## Task 2 Exploring Web3 using Algorand

We might define Web 1.0 as a read only distributed system. Fetch a document and read it in your browser. Web 2.0 might be described as a "read-write" distributed system. Using a browser, the user is able to enter data and communicate that data to servers. Web 3.0 might be categorized as a "read-write-own" distributed system. Cryptocurrency and tokens may be owned and transferred. 

The overall architecture on most Web3 systems is "decentralized peer-to-peer" rather than the more centralized "client server".

Algorand is a Web3 blockchain that competes with Ethereum.

To get started using Algorand, we need a wallet and an account.  

0. Visit https://perawallet.app/ and launch Pera Web to visit https://web.perawallet.app/.
1. Choose "Create an Account" and choose a password. This is stored on your local machine.
2. Choose an account name. You might use your Andrew id. This too is stored on your local machine.
3. Locate your recovery phrase. See "view pass phrase" after clicking the three horizontal dots on the Accounts page. Take a screen shot and save it locally on disk. A recovery phrase should never be shared. From the recovery phrase, your private key can be generated.
4. Keep the recovery phrase in some place private. It is suggested that this phrase be stored on paper and off any computer. If you lose your recovery phrase, you lose your money. Do not include your recovery phrase on the PDF that you will submit.
5. Change to the Algorand testnet by selecting "Settings" and then "Node Settings" and then "testnet". We are NOT using the mainnet. The mainnet Algos have real value. The testnet Algos have no value. But note that your private key (or recovery phrase) will work on both the mainnet and the testnet.
6. Visit https://testnet.algoexplorer.io/dispenser to get funds for the testnet.
7. Use your account address to get funds from the dispenser and be sure to save the transaction ID. You will need that ID later.
8. You may be asked to prove that you are human.
9. Algos will be dispensed to your account (that is, the transfer will be recorded on the testnet blockchain). There are no Algos physically "in" your wallet. Note that you need to look at the testnet and not the mainnet to see the Algos controlled by your wallet.
10. After some time, the wallet will show the new balance of 10 Algos.
11. Transfer 5 Algos from your account to my account address. My account address is
```
K2EP3LIPR3KEI7QOVW3UHLN6JGASMF442YRI5IPO6N6UWPUVNZJ6BVFT4U
```
NOTE: These are testnet Algos and not real Algos. We cannot exchange them for anything of value. 

12. Also, keep a copy of the transaction ID.
In a recent transaction, my transaction ID was 4CTARWVWGPBL6G4GOKBIQQLKH5NOHOGV3EVF5XMFQGIGUA72HN6Q.

Next, we want to visit the Algorand blockchain using API's. There are two API's available. One is the Algorand Node V2 and the other is the Algorand Indexer V2.

13. In an IntelliJ project, right click the Project node and select New HTTP Request.
We can enter our HTTP request directly into IntelliJ. For example, to view the details of the genesis block on the Algorand testnet, enter the following HTTP request and click the green triangle just to the left of the request:

```
###
GET https://node.testnet.algoexplorerapi.io/genesis

```
14. Note that you may directly post curl requests into the IntelliJ HTTP client and it will convert the request into IntelliJ's format.

15. Visit the URL https://testnet.algoexplorer.io/api-dev/indexer-v2 and note the base URL. 
The base URL is https://algoindexer.testnet.algoexplorerapi.io/.

16. Using IntelliJ, create two HTTP requests to visit the testnet blockchain and get the details associated with both of the transfer transactions. The first will show the transation details when you took funds from the dispenser. The second will show the transaction details when you sent 5 Algos to me. To do this, you will need to use the Algorand Indexer V2 API.

Using the IntelliJ HTTP client, copy (or take a screenshot) of the HTTP requests and the HTTP responses (including JSON). Include the two request/response pairs on your PDF.

:checkered_flag:**On your single pdf, make a copy of the screenshots labelled clearly as Project3Task2.**

**Task 2 Grading Rubric 20 Points**

Rubric:
1. The IntelliJ HTTP client is being used correctly. 5 points.
2. The HTTP client resquest is correct. 5 Points.
3. The HTTP client response is correct: 5 Points.
4. The correct transactions are shown on the PDF: 5 points.

**Project 3 Submission Requirements**

Name your IntelliJ project folders Project3Task0 and Project3Task1.

For each IntelliJ project, File->Export Project->To Zip... each. You must export in this way and NOT just zip the IntelliJ project folders.

You should now have two zip files:
* Project3Task0.zip
* Project3Task1.zip

You will also have a single pdf named Project3.pdf. The PDF will include the screenshot from Project3Task2.

Create a new empty folder named with your Andrew id (**very important**). Put the three files above into this new folder. Zip that folder, and submit it to Canvas.

**The file named YourAndrewID.zip contains two zips and a pdf:**
* Project3Task0.zip
* Project3Task1.zip
* Project3.pdf (includes screenshot from Project3Task2)