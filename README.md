// SPDX-License-Identifier: MIT
pragma solidity 0.8.20; // This should be at the top, before any contract or function definitions

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract HumansToken is ERC20, Ownable  {
    uint256 private constant TOTAL_SUPPLY = 132_000_000_000 * 10**18;  // 132 milliards de tokens
    uint256 public constant AIRDROP_AMOUNT = 1000 * 10**18;  // 1000 tokens par airdrop
    uint256 public airdropSupply;
    mapping(address => bool) public hasClaimedAirdrop;

    constructor() ERC20("Humans", "HMS") Ownable(msg.sender)  {
        _mint(msg.sender, TOTAL_SUPPLY);
        airdropSupply = TOTAL_SUPPLY / 10;  // 10% of total supply for airdrop
    }
    
    function claimAirdrop() public {
        require(!hasClaimedAirdrop[msg.sender], "Airdrop already claimed");
        require(airdropSupply >= AIRDROP_AMOUNT, "Airdrop supply exhausted");
        
        hasClaimedAirdrop[msg.sender] = true;
        airdropSupply -= AIRDROP_AMOUNT;
        _transfer(owner(), msg.sender, AIRDROP_AMOUNT);
    }

    function remainingAirdropSupply() public view returns (uint256) {
        return airdropSupply;
    }
}
