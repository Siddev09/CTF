### OBJECTIVE 

- Find the keccak256 hash of aliceHash and bobHash.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

contract Confidential {
    string public firstUser = "ALICE";
    uint public alice_age = 24;
		bytes32 private ALICE_PRIVATE_KEY; //Super Secret Key //! access storage slot read 
    bytes32 public ALICE_DATA = "QWxpY2UK";
    bytes32 private aliceHash = hash(ALICE_PRIVATE_KEY, ALICE_DATA);

    string public secondUser = "BOB";
    uint public bob_age = 21;
    bytes32 private BOB_PRIVATE_KEY; // Super Secret Key //! access storage slot read 
    bytes32 public BOB_DATA = "Qm9iCg";
    bytes32 private bobHash = hash(BOB_PRIVATE_KEY, BOB_DATA);
		
		constructor() {}

    function hash(bytes32 key1, bytes32 key2) public pure returns (bytes32) {
        return keccak256(abi.encodePacked(key1, key2));
    }

 //! hash both the key and send to this function it'll return true 
    function checkthehash(bytes32 _hash) public view returns(bool){
        require (_hash == hash(aliceHash, bobHash));
        return true;
    }
}

```

### Explaination 


- Storage Layout 

- 0x0 has firstUser string, which is a string that can fit in less than 32 bytes.
- 0x1 has 256-bit Alice age.
- 0x2 has the 32-byte Alice private key.
- 0x3 has the 32-byte Alice data.
- 0x4 has the 32-byte Alice hash.
- 0x5 has secondUser string, which is a string that can fit in less than 32 bytes.
- 0x6 has 256-bit Bob age.
- 0x7 has the 32-byte Bob private key.
- 0x8 has the 32-byte Bob data.
- 0x9 has the 32-byte Bob hash

- Use Ether.js to fetch the storage slots
- Ex. `[ethers.provider.getStorageAt(contract.address, ethers.utils.hexValue(4)]`
- After fetching the hashes at `0x4` and `0x9` 
- `const hash = ethers.utils.solidityKeccak256(['bytes32', 'bytes32'], [aliceHash, bobHash]);`
- now pass it into checkHash function











