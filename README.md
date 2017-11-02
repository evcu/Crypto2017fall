# Crypto2017fall
Utku Evci - ue225

Andy Le - al4099

Run proj3.ipynb


1. Invalid blocks
	1. Block #11181 - Sum of outputs is greater than sum of inputs + 50btc block reward. The coinbase reward for this block is 5000000010. 
	2. Block #12042 - Output #7998 has already been spent.
	3. Block #15567 - Tx # 5698 tries to spend transaction #15697, which does not exist yet since it is in the same block.
	4. Block #30223 - Two inputs are pointing to the same previous output.
	5. Block #52534 - Two coinbase transactions in this block. 
	6. Block #56565 - Same output (#65403) is being used within the same block in two different transactions.
	7. Block #72902 - Sum of outputs is greater than sum of inputs + 50btc block reward. The coinbase reward for this block is 5000000010.
	8. Block #75047 - Transaction #105281 has a negative value. 
	9. Block #88755 - Transaction #137237 has an output that is also an input.
	10. Block #97423 - Output #249860 has already been spent.


2. After removing the above invalid blocks, there are 71,889 UTXOs that exist at of the last block in the data set. UTXO #170430 has the highest associated value, of 9000000000000. Note: we had 10 errors with finding inputs for some transactions, but if we tried invalidating those transactions, then many other transactions also became invalidated, so we left those 10 transactions in the UTXO set. 

3. Clustering addresses

	a. The lowest address of the entity controlling the greatest number of bitcoins is pk_id #172. The total value of all bitcoins it controls is 98346739827397
	
	b. Transaction #93039 sends the entity above 4675300000000, which is the largest value that another entity sends to this one.
	
	c. False positives can exist in our clustering because we assumed that every N-to-1 transaction is owned by the same entity. In reality, maybe the N input transactions are owned by different individuals, who decided to purchase/send their btc to someone else. Moreover, for 1-to-1 transactions, we assumed that since there is no change address, that the same party owns both the input and output addresses. However, it is also possible that someone decided to send all of their btc (in the input address) to someone else, so there is no change address, but they are two different parties. 
	
	False negatives may also exist in our clustering because it is possible one individual owns multiple addresses, but has simply never had any cross interactions between those separate addresses. In this case, it would be nearly impossible to ever associate these addresses together. Similarly, an individual could have sent themselves btc, while also including a change address that they control. For instance, if an individual wanted to have exactly 10btc in a new address, but only had 15btc in their current address, they may send 10btc to the new output address, and have 5btc return to a new change address. In this case, it is very hard to differentiate between this type of transaction and when the individual is simply sending 10btc to another unaffiliated party. 
	
	You could make the clustering more accurate by trying to analyze different patterns of popular Bitcoin applications. For instance, large mining pools may have frequent payouts to many different addresses of small amounts. Moreover, many of these inputs should be rooted from coinbase transactions. As such, we could potentially verify this by joining the mining pool ourself and trying to label that cluster as a mining pool. Similarly, popular applications such as Satoshi Dice would also have large quantities of incoming and outgoing transactions, also to many different addresses of its users. For each individual user, they may send the entire contents of one of their addresses to Satoshi Dice, and may later on receive a payment back from Satoshi Dice to the same address. However, this does not mean that the owner of the address also owns Satoshi Dice, and this would be a false positive. Depending on our goal of clustering addresses, we may either want to minimize the number of false positives, or minimize the number of false negatives. As such, we could also change our clustering guidelines with one of these two goals in mind.
