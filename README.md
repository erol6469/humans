// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract AgriCoopToken {
    string public name = "AgriCoopToken";
    string public symbol = "ACT";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    mapping(address => uint256) public balanceOf;
    address public coopAdmin;

    event Investment(address indexed investor, uint256 amount, string animalId);
    event Dividend(address indexed investor, uint256 amount);

    constructor(uint256 _initialSupply) {
        totalSupply = _initialSupply * 10 ** decimals; // 10B ACT
        balanceOf[msg.sender] = totalSupply;
        coopAdmin = msg.sender;
    }

    function invest(address _investor, uint256 _amount, string memory _animalId) public returns (bool) {
        require(msg.sender == coopAdmin, "Seul l'admin peut enregistrer");
        require(balanceOf[coopAdmin] >= _amount, "Fonds insuffisants");
        balanceOf[coopAdmin] -= _amount;
        balanceOf[_investor] += _amount;
        emit Investment(_investor, _amount, _animalId);
        return true;
    }

    function distributeDividends(address[] memory _investors, uint256[] memory _amounts) public {
        require(msg.sender == coopAdmin, "Seul l'admin peut distribuer");
        for (uint256 i = 0; i < _investors.length; i++) {
            balanceOf[_investors[i]] += _amounts[i];
            emit Dividend(_investors[i], _amounts[i]);
        }
    }
}
