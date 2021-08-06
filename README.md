# Polyweave RC

## Technology

- Unity
- Chainsafe SDK: https://chainsafe.github.io/game-docs/
- Polygon
- Arweave: https://3ucrxgrvoyhqtxauektbubjepnwgk77aefszidlw3or7bajhlooa.arweave.net/3QUbmjV2DwncFCKmGgUke2xlf-AhZZQNdtuj8IEnW5w/

## Demo

[![](https://user-images.githubusercontent.com/19412160/126887373-7b08ed82-cd51-4379-bdaa-8cd504f02db6.png)](https://www.youtube.com/watch?v=hnD4N8BelzY)

https://mumbai.polygonscan.com/address/0x881af60689904af9c5d01c40c43f4e307d895e51

```solidity
/**
 *Submitted for verification at polygonscan.com on 2021-07-24
*/

// SPDX-License-Identifier: MIT
pragma solidity 0.8.4;

contract Leaderboard {

  // person who deploys contract is the owner
  address owner;

  // lists top 10 users
  uint leaderboardLength = 10;

  // create an array of Users
  mapping (uint => User) public leaderboard;
    
  // each user has a username and score
  struct User {
    address user;
    uint score;
  }
    
  constructor() {
    owner = msg.sender;
  }

  // owner calls to update leaderboard
  function addScore(address user, uint score) public {
    require(owner == msg.sender, "Sender not authorized");
    require(score >= leaderboard[leaderboardLength-1].score, "Score too low");

    // loop through the leaderboard
    for (uint i=0; i<leaderboardLength; i++) {
      // find where to insert the new score
      if (leaderboard[i].score < score) {

        // shift leaderboard
        User memory currentUser = leaderboard[i];
        for (uint j=i+1; j<leaderboardLength+1; j++) {
          User memory nextUser = leaderboard[j];
          leaderboard[j] = currentUser;
          currentUser = nextUser;
        }

        // insert
        leaderboard[i] = User({
          user: user,
          score: score
        });

        // delete last from list
        delete leaderboard[leaderboardLength];
        
        break;
      }
    }
  }
}
```
