# Python Blockchain & Cryptocurrency

A fully functional blockchain implementation with cryptocurrency features built with Python and Flask. This project demonstrates core blockchain concepts including proof-of-work consensus, transaction management, and decentralized network consensus.

## Features

- **Blockchain Core**
  - Block creation with proof-of-work mining
  - SHA-256 cryptographic hashing
  - Chain validation and integrity checking
  - Genesis block initialization

- **Cryptocurrency**
  - Transaction creation and management
  - Mining rewards system
  - Transaction pooling before block confirmation

- **Decentralization**
  - Multi-node network support
  - Consensus algorithm (longest chain wins)
  - Node discovery and connection
  - Chain synchronization across network

- **REST API**
  - Mine new blocks
  - Add transactions
  - View blockchain
  - Validate chain integrity
  - Connect nodes
  - Replace chain with longest valid chain

## Technology Stack

- **Python 3.x** - Core programming language
- **Flask** - Web framework for REST API
- **hashlib** - Cryptographic hashing (SHA-256)
- **requests** - HTTP requests for node communication
- **flask-ngrok** - Expose local server (optional)

## Prerequisites

- Python 3.7 or higher
- pip (Python package manager)

## Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/yourusername/python-blockchain.git
   cd python-blockchain
   ```

2. **Create a virtual environment**

   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

## Dependencies

Create a `requirements.txt` file with:

```
Flask==2.3.0
requests==2.31.0
flask-ngrok==0.0.25
```

## Usage

### Starting the Blockchain Node

```bash
python blockchain.py
```

The Flask server will start on `http://localhost:5000` (or ngrok URL if configured).

### API Endpoints

#### 1. **Mine a Block**

```http
GET /mine_block
```

Mines a new block and adds it to the chain. The miner receives a reward transaction.

**Response:**

```json
{
  "message": "You have mined a new block",
  "index": 2,
  "timestamp": "2024-03-08 10:30:45.123456",
  "proof": 533,
  "previous_hash": "0000a1b2c3...",
  "transactions": [...]
}
```

#### 2. **Get Full Chain**

```http
GET /get_chain
```

Returns the entire blockchain.

**Response:**

```json
{
  "chain": [...],
  "length": 5
}
```

#### 3. **Validate Chain**

```http
GET /is_valid
```

Checks if the blockchain is valid.

**Response:**

```json
{
  "message": "Blockchain is valid"
}
```

#### 4. **Add Transaction**

```http
POST /add_transaction
```

**Request Body:**

```json
{
  "sender": "Alice",
  "receiver": "Bob",
  "amount": 50
}
```

**Response:**

```json
{
  "message": "Transaction will be added to block 3"
}
```

#### 5. **Connect Nodes**

```http
POST /connect_node
```

**Request Body:**

```json
{
  "nodes": ["http://127.0.0.1:5001", "http://127.0.0.1:5002"]
}
```

#### 6. **Replace Chain (Consensus)**

```http
GET /replace_chain
```

Implements consensus algorithm - replaces the chain with the longest valid chain in the network.

## How It Works

### Proof of Work

The blockchain uses a proof-of-work consensus mechanism. Miners must find a number (nonce) such that:

```python
SHA256(new_proof² - previous_proof²) starts with "0000"
```

This computational puzzle ensures security and prevents spam.

### Block Structure

Each block contains:

- **Index**: Position in the chain
- **Timestamp**: When the block was created
- **Proof**: The nonce that satisfies proof-of-work
- **Previous Hash**: Hash of the previous block
- **Transactions**: List of transactions in this block

### Chain Validation

The chain is validated by checking:

1. Each block's `previous_hash` matches the actual hash of the previous block
2. Each block's proof-of-work is valid

### Consensus Algorithm

When multiple nodes exist, the network follows the **longest valid chain** rule. This prevents forks and ensures all nodes agree on the same history.

## Testing with Postman

1. **Mine the first block**

   ```
   GET http://localhost:5000/mine_block
   ```

2. **Add a transaction**

   ```
   POST http://localhost:5000/add_transaction
   Body: {"sender": "Alice", "receiver": "Bob", "amount": 100}
   ```

3. **Mine another block** (includes the transaction)

   ```
   GET http://localhost:5000/mine_block
   ```

4. **View the blockchain**
   ```
   GET http://localhost:5000/get_chain
   ```

## Running Multiple Nodes

To simulate a decentralized network:

1. **Start first node** (port 5000)

   ```bash
   python blockchain.py
   ```

2. **Start second node** (port 5001)
   Modify the Flask app to run on a different port:

   ```python
   app.run(port=5001)
   ```

3. **Connect the nodes**

   ```
   POST http://localhost:5000/connect_node
   Body: {"nodes": ["http://127.0.0.1:5001"]}
   ```

4. **Test consensus**
   Mine blocks on different nodes, then call:
   ```
   GET http://localhost:5000/replace_chain
   ```

## Security Considerations

**This is an educational project**, not production-ready:

- No real cryptographic signatures for transactions
- Simple proof-of-work (in production, use more sophisticated algorithms)
- No peer discovery mechanism
- No persistent storage (chain resets on restart)

## License

This project is open source and available under the MIT License.
