# 4-Ether-Smart_Contract-_-Security-and-Privacy-Engineering
Security and Privacy Engineering course Hacking and Creaking Assignment 4-Ether-Smart_Contract



---

# **Solving "Not A Wallet" Challenge**  

## **Vulnerability Analysis**  
The vulnerability in the `removeOwner` function is due to an incorrect use of the assignment operator (`=`) instead of the equality comparison operator (`==`).  

```solidity
function removeOwner(address oldowner) public rightStudent {
    require(owners[msg.sender] = true); 
    owners[oldowner] = false;
}
```

### **What I Found Vulnerable**  
1. The statement `require(owners[msg.sender] = true);` does not check if the caller is an actual owner. Instead, it assigns `true` to `owners[msg.sender]`, making **any caller an owner**.  
2. An attacker can call this function to grant themselves ownership.  
3. Once the attacker becomes an owner, they can withdraw all funds from the wallet using the `withdraw` function.  

### **Exploitation Steps**  
To exploit this, I performed the following steps:  
1. Connected to the Ethereum network and verified the connection.  
2. Used a private key to authenticate my account.  
3. Called `removeOwner()` to add myself as an owner.  
4. Verified my new owner status.  
5. Withdrew the entire contract balance to my account.  

---


-
---

# **Solving "Bad Parity" Challenge**

## **Vulnerability Analysis**

### **What I Found Vulnerable**
The main vulnerability in this challenge lies in the misuse of `delegatecall` in Solidity. The function `initWallet` allows arbitrary users to set themselves as the owner. The function is defined as follows:

```solidity
function initWallet(address payable _owner) public payable {
    owner = _owner;
}
```

#### **Why is this vulnerable?**
1. **Delegatecall Behavior:** Since the contract uses `delegatecall`, the function executes in the context of the calling contract, potentially allowing attackers to manipulate the `owner` state variable.
2. **Lack of Access Control:** The function `initWallet` does not verify if the caller is an authorized user. Anyone can call this function and set `_owner` to their own address.
3. **Takeover Risk:** Once an attacker becomes the owner, they gain full control over the contract, including the ability to withdraw all ETH stored in it.

By exploiting this flaw, an attacker can take over ownership of the wallet and drain all funds.

---

## **Exploit Code to Get the Flag**

### **Steps to Exploit**
1. **Connect to the Ethereum network** and verify the connection.
2. **Use my private key** to authenticate my account.
3. **Call `initWallet()`** with my own address to set myself as the owner.
4. **Verify ownership takeover** by checking the `getOwner()` function.
5. **Withdraw all ETH from the wallet** to my account.



---
