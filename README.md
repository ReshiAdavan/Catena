# Catena

A Simple Actor-Based Blockchain built in Scala.

### Inspiration

After [CryptGO](https://github.com/ReshiAdavan/CryptGO), I wanted to look into web3.0, blockchain, decentralized systems, etc. So I decided to implement a round robin algorithm to decide between the many projects I could choose and it gave me blockchain (haha just kidding).

I thought the first thing I could tackle was a blockchain as it was similar in architecture to what I have created before. As with any project I gave it my best shot!

Voila! Catena!

### Topics

- Languages: Scala
- Architectures: Actor-Based Blockchain
- Concepts: PoW Algorithm, Transactions, Chains, Nodes

### Use It Yourself

Run the Project following these steps:

1.  Build the Docker image through the following breakdown:

#### Docker image

The Dockerfile lets you build the Docker Image of a catena node. In details, the Dockerfile:

- you need a ssh private key to pull from the repo
- download the [sbt](https://www.scala-sbt.org/) version specified through the image `build-arg` parameter
- install git and openssh
- checkout the project
- run the project and `EXPOSE` port 8080 correctly

The image can be built using the command (given the right configuration on your end):

`docker build --build-arg SBT_VERSION="1.2.7" -t <your_user>/<project_name> .`

Then you can upload the docker image on Dockerhub and go from there

##### Run the docker container

The docker container compiles the source code and run the catena node. For this reason, when running the container it is required to pass an argument as per when you run the source code.

To run a branch of the remote repo simply pass the name of the branch as the argument. e.g.:

`docker run --name <whatever_name_you_choose> -p 8000:8000 <your_user>/<project_name> <branch_name>`

This will run the code of the branch `<branch_name>`.

To run the local source code mounting the local folder to the container `<branch_name>` one, use the `-v /your/source/folder:/<branch_name>` and pass `local` as an argument. You can optionally specify a folder inside the container where the source in the volume should be mounted. The source will be automatically copied inside the `/<branch_name>` folder. This is useful when you are running multiple containers in the same machine.

When you stop the container and start it again, the code may need to be manually updated from the repo in case you are running a remote branch, or will compile the local code in case you are running in local mode. Of course a script on your end can automate this (I may or may not have done this).

2.  Run the cluster of 3 nodes using the `docker-compose up` command in the root folder of the project.
3.  You will need to host the API on any API platform (Postman, Insomnia, etc) and run the respective endpoints to interact with Scalachain nodes (it should just take time to play around with).

### Architectures (In Detail)

The blockchain is a distributed ledger: it registers some transaction of values b/w a sender & receiver. What differs blockchain from a database is the decentralized nature of the blockchain; it is distributed among several communicating nodes that guarantee the validity of the transactions registered.

The blockchain stores transactions in blocks, that are created (mined) by nodes investing computational power. Every block is created by solving a puzzle that is hard to solve, but easy to verify. Following this light, every block represents the work needed to solve the grand puzzle. This is the reason why the puzzle is called the "Proof of Work"; the solution of the puzzle is the proof that a node spent a certain amount of work to solve it and mine the block.

Block Architecture:

<img src="https://github.com/ReshiAdavan/Catena/blob/master/imgs/blockchain-architecture.PNG" />

The main components of our blockchain model are the Transaction, the Chain, the Proof of Work (PoW) algorithm, and the Node. The transactions are stored inside the blocks of the chain, that are mined using the PoW. The node is the server that runs the blockchain.

Solution Architecture:

<img src="https://github.com/ReshiAdavan/Catena/blob/master/imgs/solution-architecture.PNG" />

#### Transaction

Transactions register the movement of our currency between two entities. Every transaction is composed by a sender, a recipient, and an amount of said currency. Transactions will be registered inside the blocks of our blockchain.

#### Chain

The chain is a linked list (LL) of blocks containing a list of transactions. Every block of the chain has an index, the proof that validates it, the list of transactions, the hash of the previous block, the list of previous blocks, and a timestamp. Every block is chained to the previous one by its hash, that is computed converting the block to a JSON string and then hashing it through a SHA-256 hashing function.

#### PoW

The PoW algorithm is required to mine the blocks composing the blockchain. The idea is to solve a puzzle that is hard to solve, but easy to verify having the proof. The PoW algorithm that is implemented in Catena is similar to the Bitcoin one (based on Hashcash). It consists in finding a hash with N leading zeros, that is computed starting from the hash of the last block and a number, that is the proof of our algorithm (I have NOT implemented the hashing function myself).

#### Node

The Node is the server running our blockchain. It provides some REST API to interact with it and perform basic operations such as send a new transaction, get the list of pending transactions, mine a block, and get the current status of the blockchain.

If you made it this far, congrats! That concludes Catena's README.
