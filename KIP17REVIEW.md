**klaytn 17 contract review**
=

klaytn is a public blockchain focused on the metaverse, gamefi and the creator economy.
klaytn compatible token (KCT) is a special type of smart contract that implements certain technical specifications. Everyone who wants to issue tokens on top of Klaytn must follow the specification. Token standards are defined in Klaytn such as KIP-7 and KIP-17. KIP-7 and KIP-17 are tailored for klaytn and are suitable on the klaytn ecosystem.

We will be focused on the KIP17 Non-fungible Token standard  specification in this review.
-


Non-fungible token (NFT) is a special type of token that represents a unique asset. As the name non-fungible implies, every single token is unique and non-divisible. This uniqueness of non-fungible token opens up new horizons of asset digitization. For example, it can be used to represent digital art, game items, or any kind of unique assets and allow people to trade them.
KIP-17 differs from ERC-721 in the sense that Every token transfer/mint/burn must be tracked with event logs. 
KIP-17 has more optional extension,such as the minting extension, minting with URI extention, burning extension and pausing extension.
KIP-17 supports ERC-721, IERC721TokenReceiver in order to be compliant with ERC-721.

written in pragma solidity 0.5.0,
The KIP17 Contract imports IKPI17 interface, IERC721Receiverinterface, SafeMaths library for underflow and overflow checks, Address library which detects EOAs or Contract Adresses, Counters library to provide counters that can be incremented or decremented by one, this is used to track NFT ids, and KIP13 implementation which is used to register new Interfaces.

KIP17 inherits KIP13 and IKIP17 contracts, using using SafeMath Library for uint256, using Address library for address and using Counters library for Counters.Counter;

KIP17 uses byte4 function selectors to interact with both the onKIP17Received and onERC721Received function on the onERC721Receiver and onKIP17Received contracts respectively.

![code2](https://user-images.githubusercontent.com/72661662/162277663-42752309-634c-40e6-be2d-3215eae1637a.png)


the KIP17 has a total of four mappings. 
**_tokenOwner** mapping of uint256 tokenID to 
address owner, **_tokenApprovals** mapping of uint256 tokenID to address approved addresses, **_ownedTokensCount** mapping of address owner to number of owned token, and **_operatorApprovals** 2 dimentional mapping of address owner to address operator and returns a boolean.

![code3](https://user-images.githubusercontent.com/72661662/162277720-d549125c-2b1b-4cee-85e1-03f8d6bd53fa.png)


Each function in the IKIP17 interface has a unique byte4 selector that is generated from the keccak256 has of the function name and its arguments. The KIP17 contract is a summation of all the function selectors, 0x80ac58cd.

![code4](https://user-images.githubusercontent.com/72661662/162277887-1c29783a-2c20-4882-9b61-4e6de323fcd7.png)

The KIP17 contract constructor registers the supported interface to conform to KIP17 via KIP13 registerInterface function which register the interface via its function selector.

![code5](https://user-images.githubusercontent.com/72661662/162277983-064f6295-3c24-4de2-9804-ca02608f2c65.png)

**balanceOf** is a public view function takes in an address and returns a uint256 balance of the address. balanceOf runs a require check to ensure that the address passed in is not a zero address, and return the current value of the _ownedTokensCount mapping. 

![code6](https://user-images.githubusercontent.com/72661662/162278033-aad3ecd6-ccbb-4668-872c-292b4dccd6a1.png)

**ownerOf** is a public view function takes a uint256 tokenId and returns an address, the owner of the tokenId. ownerOf runs a check to ensure the owner of certain tokenId is not address zero and returns the owner address 

![code7](https://user-images.githubusercontent.com/72661662/162278076-f03266b9-fb20-4aed-bb3d-a6cd2aa604a1.png)

**approve** is a public function takes in two parameters, an address to and a uint256 tokenId. The approve function runs two require checks, one is to ensure the to address is not the owners', so you cannot approve yourself. the other require checks that the person calling the approve function(msg.sender), owns that particular tokenId or if the owner approved the msg.sender. the approve function mutates state by assigning the tokenId to the to address via the _tokenApprovals mapping. approve function emits the Approval event

![code8](https://user-images.githubusercontent.com/72661662/162278115-3ac88e06-9480-49ef-a7a6-f1a6c6fa4393.png)

**getApproved** is a public, view function that takes in the tokenId as parameter and returns the approved address of that particular tokenId.

![code9](https://user-images.githubusercontent.com/72661662/162278146-569cfc85-5c72-4292-837f-3362cea5f648.png)

**setApprovalForAll** is a public function that takes in a to address and a boolean as parameters, it runs a require check to ensure the to address is not the msg.sender. setApproval mutates state by setting or unsetting the approval of a given address via the _operatorApprovals 2 dimentional mapping.

![code10](https://user-images.githubusercontent.com/72661662/162278230-888fd0a3-e284-4937-af61-09734932e5c1.png)

**isApprovedForAll** is a public, view function that takes in two address parameters owner and operator respectively and returns a boolean via the _operatorApprovals 2 dimentional mapping.

![code11](https://user-images.githubusercontent.com/72661662/162278271-e14a3836-bb4f-4512-b462-07d7e0a89069.png)


**transferFrom** is a public function that takes in three parameters, a from address, a to address and a uint256 tokenId. the transferFrom runs a require check using the _isApprovedOrOwner internal function, takes in two parameters msg.sender and tokenId, the transferFrom call the _transferFrom internal function if the require check passes.

![code12](https://user-images.githubusercontent.com/72661662/162278314-066fb78c-69bf-44dc-ae29-3383b0792f78.png)

**safeTransferFrom** is a public function, takes in three parameters, an address from, an address to, and a uint256 tokenId. Requires the msg.sender to be the owner, approved, or operator. 

![code13](https://user-images.githubusercontent.com/72661662/162278343-42d7dff2-2e4f-496e-972b-bffd3b3e11b3.png)

**_exists** is an internal, view function, that returns a boolean. _exists takes a tokenId, assigns the owner address of the tokenId via the _tokenOwner mapping and returns true if the owner address is not address 0.

![code14](https://user-images.githubusercontent.com/72661662/162278374-d0c8f74c-6da9-4473-836f-92d07be39526.png)

**_isApprovedOrOwner** is an internal view function that takes two parameters an address spender and a uint256 tokenId and returns a boolean. the function runs a require check and runs if 
an address owns that particular tokenId. 
then assigns the owner of that tokenId to the address owner variable.
returns true if spender is owner or address spender is approved for that tokenId

![code15](https://user-images.githubusercontent.com/72661662/162278414-66906a48-f04d-4a53-8b95-5e6503acf211.png)


**_mint** is an internal function that receives two parameters, an address to and a uint256 tokenId. runs two checks, one that checks if the to address is not address 0, and another that checks that that token id doesnt exist.
the function modifies state by assigning the value of the token id key to the to address via the _tokenOwner mapping, and also increments the mount of tokenId owner by the to address.
_mint emits a Transfer event with values of address(0), to address and tokenId

![code16](https://user-images.githubusercontent.com/72661662/162278461-4a4c3ea5-a3f1-4661-9d93-037f1ef30b8a.png)

**_burn** is an internal function that receives two parameters, an address owner and a uint256 tokenId. the function runs a require check to assertain if the owner of that perticular tokenId is the owner address, clearapproval by changing it to address 0, modifies state variables by decrementing the owned token of the from address by 1, and assigning the token owner of that tokenId to address 0.
_burn emits a transfer event which registers the owner, address(0) and the tokenId.

![code17](https://user-images.githubusercontent.com/72661662/162278508-606c2ede-d30c-434b-ad5b-77acdc5c2f06.png)


function _burn is an internal function that takes a single parameter of uint256 tokenId, and calls _burn using the address of the ownerOf the tokenId and the token Id.

![code18](https://user-images.githubusercontent.com/72661662/162278541-26aa2b85-1dd3-493d-bdb4-fefc4346f0fb.png)


**_transferFrom** is an internal function with three parameters, and address from, an address to and the unique uint256 tokenId. the function runs two require checks, the first to assertain that the ownerOf tokenId is the from address, and the second to assertain that the to address is not address 0. the function calls the _clearApproval function with the tokenId as parameter. this function mutates state by decrementing the ownedTokensCount of the from adddress by 1, incrementing the ownedTokensCount of the to adddress by 1, via the _ownedTokensCount mapping and assigning the tokenId to the to address via the _tokenOwner mapping. emits a Transfer event with the current values of from, to, tokenId.

![code19](https://user-images.githubusercontent.com/72661662/162278566-cbdc3b94-4563-4688-96c9-1f14a75fda19.png)



**_checkOnKIP17Received** is an internal function takes in three parameters, an address from, an address to, a tokenId, bytes _data, and returns a boolean. the function contains two local variables, bool success and bytes memory returndata. the function returns true if to address is not contract address. the function performs a low level call to the 'to' address using abi.encodeWithSelector the function selector of the onERC721Received function inside the IERC721Receiver contract, together with the function arguments. this call returns a true if succesful and a return data. the function assertains if the return data is successful by checking if its greater than 0. and  if the decoded value is same as the the function selector.

![code20](https://user-images.githubusercontent.com/72661662/162278592-cfb31563-d67a-47cc-b320-b3bc55778030.png)


**_clearApproval** is a private function that takes a single parameter uint256 tokenId, it checks if the token approval address
of a certain token Id is not zero, and assigns it to address zero, via the _tokenApprovals mapping.

![code21](https://user-images.githubusercontent.com/72661662/162278611-e8b28359-e549-45f2-95bb-edab255132d2.png)


