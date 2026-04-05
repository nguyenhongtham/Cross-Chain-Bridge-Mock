# Cross-Chain-Bridge-Mock
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import "@openzeppelin/contracts/access/Ownable.sol";

contract BaseBridge is Ownable {
    mapping(uint256 => bool) public processedNonces;
    uint256 public nonce;

    event Bridged(address from, address to, uint256 amount, uint256 dstChainId);

    function bridge(address to, uint256 amount, uint256 dstChainId) external payable {
        nonce++;
        emit Bridged(msg.sender, to, amount, dstChainId);
        // In production: lock tokens + send message via LayerZero
    }

    function receiveBridge(address from, uint256 amount, uint256 srcNonce) external onlyOwner {
        require(!processedNonces[srcNonce], "Already processed");
        processedNonces[srcNonce] = true;
        // Unlock / mint tokens
    }
}
