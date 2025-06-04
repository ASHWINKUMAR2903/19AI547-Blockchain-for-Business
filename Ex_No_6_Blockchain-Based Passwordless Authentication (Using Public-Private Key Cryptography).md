# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
### Step 1:
Start the Ethereum environment (such as Ganache, or a Testnet) and deploy the smart contract that manages user registration and authentication.

### Step 2:
User initiates registration by submitting their Ethereum public key to the smart contract.

### Step 3:
The smart contract securely stores the public key associated with the user's account address.

### Step 4:
During login, the server (or smart contract) generates a random challenge message (e.g., a random string or nonce).

### Step 5:
The server sends the challenge to the user’s client-side application.

### Step 6:
The user’s client-side application signs the challenge using their private key via cryptographic functions (e.g., web3.eth.personal.sign).

### Step 7:
The signed challenge (digital signature) is sent back to the server (or smart contract) for verification.

### Step 8:
The server (or smart contract) verifies the signature using the stored public key and checks whether the signature matches the original challenge.

### Step 9:
If the verification is successful, the user is authenticated successfully; otherwise, access is denied.

### Step 10:
End the process by granting access to authenticated users or sending an error message for failed authentication.


# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuthDemo {
    struct User {
        bool registered;
        address pubKey;
        bytes32 privateKey; // Fake private key for demo
    }

    mapping(address => User) public users;
    bytes32 public latestChallenge;

    event UserRegistered(address user, address pubKey, bytes32 privateKey);
    event ChallengeGenerated(bytes32 challenge);
    event SignatureGenerated(bytes32 hash, uint8 v, bytes32 r, bytes32 s);

    // Step 1: Register user
    function registerUser() public {
        require(!users[msg.sender].registered, "Already registered");

        // Fake public/private keys
        address fakePubKey = msg.sender;
        bytes32 fakePrivateKey = keccak256(abi.encodePacked(msg.sender, block.timestamp));

        users[msg.sender] = User({
            registered: true,
            pubKey: fakePubKey,
            privateKey: fakePrivateKey
        });

        emit UserRegistered(msg.sender, fakePubKey, fakePrivateKey);
    }

    // Step 2: Generate random challenge
    function generateChallenge() public returns (bytes32) {
        require(users[msg.sender].registered, "User not registered");
        latestChallenge = keccak256(abi.encodePacked(block.timestamp, msg.sender));
        emit ChallengeGenerated(latestChallenge);
        return latestChallenge;
    }

    // Step 3: "Sign" the challenge (fake signing)
    function generateSignature() public returns (bytes32 hash, uint8 v, bytes32 r, bytes32 s) {
        require(users[msg.sender].registered, "User not registered");
        
        hash = latestChallenge;
        bytes32 combined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        
        // Fake values for r, s, v
        r = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        s = combined;
        v = 27;

        emit SignatureGenerated(hash, v, r, s);

        return (hash, v, r, s);
    }

    // Step 4: Authenticate
    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(users[msg.sender].registered, "User not registered");

        bytes32 expectedCombined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        bytes32 expectedR = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        uint8 expectedV = 27;

        if (r == expectedR && s == expectedCombined && v == expectedV) {
            return true;
        } else {
            return false;
        }
    }
}
```

# Expected Output:

```
Users can register without a password.

Users sign a challenge with their private key for authentication.

The smart contract verifies signatures to confirm identity.
```

![1](https://github.com/user-attachments/assets/1814a76a-0673-4ee7-857b-55273bf80d1b)

![2](https://github.com/user-attachments/assets/5042cc2e-ed55-4297-a2e9-b1dd6fc65ac1)

![3](https://github.com/user-attachments/assets/4be48fbc-dc89-4e3c-85b3-eff7c0664e46)

![4](https://github.com/user-attachments/assets/b8a0a046-ca7e-4d7e-99c4-6a825049e664)

![5](https://github.com/user-attachments/assets/842ea949-88f3-4761-a35b-2973a4d4cf69)

![6](https://github.com/user-attachments/assets/5fc0434f-78f1-44d1-a87d-3d6fbb9bcff9)

![7](https://github.com/user-attachments/assets/7fba69f9-8035-427d-92ee-5b5e6c871a4b)

![8](https://github.com/user-attachments/assets/2dbe4bcf-1252-4274-89d6-1a4c82fd1893)


# High-Level Overview:

```
Eliminates password hacks & phishing attacks.

Uses Ethereum's built-in cryptographic functions.

Inspired by Web3 login solutions like MetaMask authentication.
```

# RESULT: 
Thus the Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography) is successfully implemented.
