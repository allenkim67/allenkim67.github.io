---
layout: post
title: Explaining How Bitcoin Works To Programmers
categories: bitcoin
---

## Money As A Single Record Of All Transactions
Normally people think of money as something that they carry around and send to
each other when they want to make a payment. But with digital currency it's more
useful to think of it as a single record of all transactions. So if I want to
pay Bob $100, instead of handing over some cash, I would update the record with
something like:

<Allen's account #> -$100
<Bob's account #> +$100

And if I wanted to know how much money I had I would just refer to the record
and add up all my debits and credits.

So rather than thinking of money as something that you possess and give to
someone else, you can think of it as reading and writing on a single record of
all transactions. As a programmer you might think of this record as a single
table in a database where everyone has read/write access, or maybe a google docs
spreadsheet.

### A Decentralized Distributed Network
But because bitcoin is decentralized, we can't rely on any one party being the
definitive host of this record. Consequently, everyone has (or at least can have)
a complete copy of all bitcoin transactions. In a lot of ways it's similar to
git in that you can clone data, as well as push and pull updates, from the
network.

But the big question is, how do we know that the record of transactions is true?
What prevents people from making fake transactions and stealing other people's
money?


## Logically Consistent Transactions
One problem that we can solve immediately is transactions that don't make
logical sense. For example a credit without a debit. Or debiting more money than
you have. This is because when a new transaction is submitted to the network,
each receiver will validate the transaction. If found to be invalid, they will
reject it from being propagated to the rest of the network.


## Digital Signatures
Another problem is, how do we know that the person sending the money is the
person that owns the money? Can't I just submit a transaction that says Bob
gives me all his bitcoin since this would be a logically valid transaction? This
problem can be solved by using public-key encryption.

Public-key encryption is a system where you can generate two keys, one private
and one released to the public. These keys can be used to encrypt data, and then
only the other key can be used to decrypt that data. This has 2 consequences.
1. Anyone can encrypt data using the public key and be assured that only you can
decrpyt it. (assuming only you have access to the private key). 2. You can
encrypt data with the private key and anyone can use the public key to verify
that the data really came from you, because your public key would only work on
data that was encrypted with your private key.

#2 is what's called a digital signature, and all bitcoin transactions are
digitally signed to confirm that the sender of the money is really who they
say they are.

## Double-Spend Problem
The last problem is a more subtle one, which has to do with the fact that we
can't tell exactly when a transaction was submitted just by looking at it. This
opens up the possibility of double spending, which works like this. Let's say
you only have one bitcoin but you try to pay two people one bitcoin each. But
you send the transactions to geologically opposite ends of the network, say one
guy in China and another in the US. Because it takes some time to propagate the
transaction across the network both sides will think the transaction is valid
until they realize that there is a conflict. Of course both sides will want
their transaction to be the valid one, so there needs to be a way to resolve
this conflict.

### Mining And Consensus
This is where the whole mining thing comes into play. But first, you should know
that transactions are added to the record in batches called "blocks". These
blocks are added on one after the other, so what I've been calling the "record
of all transactions" is generally called the "blockchain". Now before anyone can
add a block to the blockchain, that person has to essentially brute-force guess
a large number which should take on average 10 minutes. If they can successfully
add a block to the blockchain they get a reward based on fees extracted from
that block as well as newly generated bitcoins. But what is the point of
this seemingly mindless computation? It's not to verify the transaction which
can be done by anyone near-instantly. Actually it's a way of resolving double
spend conflicts.

Since a block of transactions is put together by one user, they should all be
logically consistent with each other. So really the problem is when the guy in
China submits one block and the guy in the US submits another block with each
block containing one of the double-spend transactions. So here you can see that
it's possible to submit more than one block that competes to become the official
next block. (This would be true even without the double-spend problem, just as a
result of network latency, but without the double-spend problem it wouldn't
matter which block wins.) So that means there's now a fork in the blockchain,
and people can add new blocks on to either branch. But the rule is that only the
longest branch counts as the official branch.

This rule has some very interesting consequences. Because your block will be
invalid if it turns out to be on the shorter branch (thus invalidating your
reward) there is a very strong incentive not build on top of "losing" branches.
You might have wondered, if one guy wins the right to add a block to the
blockchain, what's to stop him from adding an invalid block? What would happen
is that miners would reject the invalid block by starting a new branch. Miners
would be highly incentivised to abandon the invalid block because most likely
rewards for blocks on that branch will become invalidated (assuming a majority
of miners will choose to build on the valid branch).

## 51% Attack
In this way expending cpu power to mine is similar to voting, where your voting
power is proportional to your cpu power. The correctness of the blockchain is
the result of a majority of participating cpu power voting on what is correct
by deciding to add their blocks to a specific branch. And if you're wondering if
one person had 51% of all participating cpu power could they have control over
which branch ends up the longest branch, the answer if yes! This is known as the
51% attack. But as the number of people mining bitcoin grows it becomes harder
and harder to coordinate 51% of all cpu power.