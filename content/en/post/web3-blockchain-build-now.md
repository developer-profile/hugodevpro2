---
title: "Web3 Blockchain Build Now"
description: "There are a blockchain example under the hood."
date: 2022-01-16T20:58:07+03:00
draft: false
featured_image: ""
tags: ["blockchain","web3","go"]
---
# Building blockchain now

Who needs blockchain? Decentralized apps? Cryptocurrencies? Definetly, yes!

How can one build a blockchain? Easy!


### Let's start with Go

Importing all the necessary packages

```
package main

import (
        "crypto/sha256"
        "encoding/json"
        "fmt"
        "strconv"
        "strings"
        "time"
)
```

Creating a custom blockchain block type to hold necessary data

```
type Block struct {
        data         map[string]interface{}
        hash         string
        previousHash string
        timestamp    time.Time
        pow          int
}
```

Here we are defining custom blockchain container for our blocks

```
type Blockchain struct {
        genesisBlock Block
        chain        []Block
        difficulty   int
}
```

# We need hash of a block

Let's include method for block type

```
func (b Block) calculateHash() string {
        data, _ := json.Marshal(b.data)
        blockData := b.previousHash + string(data) + b.timestamp.String() + strconv.Itoa(b.pow)
        blockHash := sha256.Sum256([]byte(blockData))
        return fmt.Sprintf("%x", blockHash)
}
```

# Mining new blocks

Mining a new block involves generating a block hash that starts with a desired number of zeros.

```
func (b *Block) mine(difficulty int) {
        for !strings.HasPrefix(b.hash, strings.Repeat("0", difficulty)) {
                b.pow++
                b.hash = b.calculateHash()
        }
}
```

# Creating the genesis block

Here we implementing a function that creates a genesis block for our blockchain

```
func CreateBlockchain(difficulty int) Blockchain {
        genesisBlock := Block{
                hash:      "0",
                timestamp: time.Now(),
        }
        return Blockchain{
                genesisBlock,
                []Block{genesisBlock},
                difficulty,
        }
}
```

# Adding new blocks to the blockchain

You can see in next example how to include new blocks into a blockchain.

```
func (b *Blockchain) addBlock(from, to string, amount float64) {
        blockData := map[string]interface{}{
                "from":   from,
                "to":     to,
                "amount": amount,
        }
        lastBlock := b.chain[len(b.chain)-1]
        newBlock := Block{
                data:         blockData,
                previousHash: lastBlock.hash,
                timestamp:    time.Now(),
        }
        newBlock.mine(b.difficulty)
        b.chain = append(b.chain, newBlock)
}
```

# Checking the validity of the blockchain

We need a functionality that checks if the blockchain is valid

```
func (b Blockchain) isValid() bool {
        for i := range b.chain[1:] {
                previousBlock := b.chain[i]
                currentBlock := b.chain[i+1]
                if currentBlock.hash != currentBlock.calculateHash() || currentBlock.previousHash != previousBlock.hash {
                        return false
                }
        }
        return true
}
```

# Using the blockchain to make transactions

Let’s create a main() function to show its usage.

```
func main() {
        // create a new blockchain instance with a mining difficulty of 2
        blockchain := CreateBlockchain(2)

        // record transactions on the blockchain for Alice, Bob, and John
        blockchain.addBlock("Alice", "Bob", 5)
        blockchain.addBlock("John", "Bob", 2)

        // check if the blockchain is valid; expecting true
        fmt.Println(blockchain.isValid())
}
```

*Here you learned how blockchains work under the hood*
