# Auto-Compounding-Staking
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract AutoCompoundingStaking {
    uint256 public totalStaked;
    mapping(address => uint256) public userStaked;
    uint256 public lastCompoundTime;

    event Compounded(uint256 rewardAdded);

    function stake() public payable {
        userStaked[msg.sender] += msg.value;
        totalStaked += msg.value;
    }

    function compound() public {
        if (block.timestamp < lastCompoundTime + 1 days) revert("Too soon");
        
        // Simulate reward (in real: calculate from reward pool)
        uint256 reward = totalStaked / 100; // 1% daily example
        totalStaked += reward;
        
        lastCompoundTime = block.timestamp;
        emit Compounded(reward);
    }

    function unstake(uint256 amount) public {
        require(userStaked[msg.sender] >= amount, "Insufficient stake");
        userStaked[msg.sender] -= amount;
        totalStaked -= amount;
        payable(msg.sender).transfer(amount);
    }
}
