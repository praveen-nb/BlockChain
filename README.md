# BlockChain
import hashlib
import datetime

class Block:
    def __init__(self, data, previous_hash):
        self.timestamp = datetime.datetime.now()
        self.data = data
        self.previous_hash = previous_hash
        self.hash = self.calculate_hash()

    def calculate_hash(self):
        data_string = str(self.timestamp) + str(self.data) + str(self.previous_hash)
        return hashlib.sha256(data_string.encode()).hexdigest()

class Blockchain:
    def __init__(self):
        self.chain = [self.create_genesis_block()]

    def create_genesis_block(self):
        return Block("Genesis Block", "0")

    def get_latest_block(self):
        return self.chain[-1]

    def add_block(self, new_block):
        new_block.previous_hash = self.get_latest_block().hash
        new_block.hash = new_block.calculate_hash()
        self.chain.append(new_block)

    def is_chain_valid(self):
        for i in range(1, len(self.chain)):
            current_block = self.chain[i]
            previous_block = self.chain[i - 1]

            if current_block.hash != current_block.calculate_hash():
                return False

            if current_block.previous_hash != previous_block.hash:
                return False

        return True

# Testing the blockchain
blockchain = Blockchain()

# Adding blocks to the blockchain
blockchain.add_block(Block("Block 1 Data", ""))
blockchain.add_block(Block("Block 2 Data", ""))
blockchain.add_block(Block("Block 3 Data", ""))

# Displaying the blockchain
for block in blockchain.chain:
    print("Block Hash:", block.hash)
    print("Block Data:", block.data)
    print("Previous Hash:", block.previous_hash)
    print("Timestamp:", block.timestamp)
    print()

# Verifying the integrity of the blockchain
print("Is Blockchain Valid?", blockchain.is_chain_valid())
