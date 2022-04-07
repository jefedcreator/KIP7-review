klaytn 17 contract

klaytn is a public blockchain focused on the metaverse, gamefi and the creator economy.

Klaytn compatible token (KCT) is a special type of smart contract that implements certain technical specifications. Everyone 
who wants to issue tokens on top of Klaytn must follow the specification. Token standards are defined in Klaytn such as KIP-7 and KIP-17.
KIP-7 and KIP-17 are tailored for klaytn and are suitable on the klaytn ecosystem.
We will be focused on the KIP17 Non-fungible Token standard  specification in this review.
Non-fungible token (NFT) is a special type of token that represents a unique asset. As the name non-fungible implies, every single token is unique and non-divisible. This uniqueness of non-fungible token opens up new horizons of asset digitization. For example, it can be used to represent digital art, game items, or any kind of unique assets and allow people to trade them.
KIP-17 differs from ERC-721 in the sense that Every token transfer/mint/burn must be tracked with event logs. 
KIP-17 has more optional extension. such as the minting extension, minting with URI extention, burning extension and pausing extension.
KIP-17 supports ERC-721, IERC721TokenReceiver in order to be compliant with ERC-721.

written in pragma solidity 0.5.0,
The KIP17 Contract imports IKPI17 interface, IERC721Receiverinterface, SafeMaths library for underflow and overflow checks, Address library which checks, Counters library to provide counters that can be incremented or decremented by one, this is used to track NFT ids, and KIP13 implementation which is used to register new Interfaces.

KIP17 inherits KIP13 and IKIP17 contracts, using using SafeMath Library for uint256, using Address library for address and using Counters library for Counters.Counter;

KIP17 uses byte4 function selectors to interact with both the onKIP17Received and onERC721Received function on the onERC721Receiver and onKIP17Received contracts respectively.
![](C:\Users\HP\Documents\Development\KIP17\code2.png)

the KIP17 has a total of four mappings. 
_tokenOwner mapping of uint256 tokenID to 
address owner, _tokenApprovals mapping of uint256 tokenID to address approved addresses, _ownedTokensCount mapping of address owner to number of owned token, and _operatorApprovals 2 dimentional mapping of address owner to address operator and returns a boolean.

Each function in the IKIP17 interface has a unique byte4 selector that is generated from the keccak256 has of the function name and its arguments. The KIP17 contract is a summation of all the function selectors, 0x80ac58cd.

The KIP17 contract constructor registers the supported interface to conform to KIP17 via KIP13 registerInterface function which register the interface via its function selector.

balanceOf is a public view function takes in an address and returns a uint256 balance of the address. balanceOf runs a require check to ensure that the address passed in is not a zero address, and return the current value of the _ownedTokensCount mapping. 

ownerOf is a public view function takes a uint256 tokenId and returns an address, the owner of the tokenId. ownerOf runs a check to ensure the owner of certain tokenId is not address zero and returns the owner address 

approve is a public function takes in two parameters, an address to and a uint256 tokenId. The approve function runs two require checks, one is to ensure the to address is not the owners', so you cannot approve yourself. the other require checks that the person calling the approve function(msg.sender), owns that particular tokenId or if the owner approved the msg.sender. the approve function mutates state by assigning the tokenId to the to address via the _tokenApprovals mapping. approve function emits the Approval event

getApproved is a public, view function that takes in the tokenId as parameter and returns the approved address of that particular tokenId.

setApprovalForAll is a public function that takes in a to address and a boolean as parameters, it runs a require check to ensure the to address is not the msg.sender. setApproval mutates state by setting or unsetting the approval of a given address via the _operatorApprovals 2 dimentional mapping.

isApprovedForAll is a public, view function that takes in two address parameters owner and operator respectively and returns a boolean via the _operatorApprovals 2 dimentional mapping.

transferFrom is a public function that takes in three parameters, a from address, a to address and a uint256 tokenId. the transferFrom runs a require check using the _isApprovedOrOwner internal function, takes in two parameters msg.sender and tokenId, the transferFrom call the _transferFrom internal function if the require check passes.

safeTransferFrom is a public function, takes in three parameters, an address from, an address to, and a uint256 tokenId. Requires the msg.sender to be the owner, approved, or operator. 

_exists is an internal, view function, that returns a boolean. _exists takes a tokenId, assigns the owner address of the tokenId via the _tokenOwner mapping and returns true if the owner address is not address 0.

_isApprovedOrOwner is an internal view function that takes two parameters an address spender and a uint256 tokenId and returns a boolean. the function runs a require check and runs if 
an address owns that particular tokenId. 
then assigns the owner of that tokenId to the address owner variable.
returns true if spender is owner or address spender is approved for that tokenId

_mint is an internal function that receives two parameters, an address to and a uint256 tokenId. runs two checks, one that checks if the to address is not address 0, and another that checks that that token id doesnt exist.
the function modifies state by assigning the value of the token id key to the to address via the _tokenOwner mapping, and also increments the mount of tokenId owner by the to address.
_mint emits a Transfer event with values of address(0), to address and tokenId

_burn is an internal function that receives two parameters, an address owner and a uint256 tokenId. the function runs a require check to assertain if the owner of that perticular tokenId is the owner address, clearapproval by changing it to address 0, modifies state variables by decrementing the owned token of the from address by 1, and assigning the token owner of that tokenId to address 0.
_burn emits a transfer event which registers the owner, address(0) and the tokenId.

function _burn is an internal function that takes a single parameter of uint256 tokenId, and calls _burn using the address of the ownerOf the tokenId and the token Id.

_transferFrom is an internal function with three parameters, and address from, an address to and the unique uint256 tokenId. the function runs two require checks, the first to assertain that the ownerOf tokenId is the from address, and the second to assertain that the to address is not address 0. the function calls the _clearApproval function with the tokenId as parameter. this function mutates state by decrementing the ownedTokensCount of the from adddress by 1, incrementing the ownedTokensCount of the to adddress by 1, via the _ownedTokensCount mapping and assigning the tokenId to the to address via the _tokenOwner mapping. emits a Transfer event with the current values of from, to, tokenId.

_checkOnKIP17Received function takes in three parameters and returns a boolean

_clearApproval is a private function that takes a single parameter uint256 tokenId, it checks if the token approval address
of a certain token Id is not zero, and assigns it to address zero, via the _tokenApprovals mapping.

