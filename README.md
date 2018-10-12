# mongo-marbles

1. git clone https://github.com/CsterKuroi/mongo-marbles.git

2. download justledger peer,orderer images (1.2.1) from svn

3. load images
```
docker load -i peer.tar
docker load -i orderer.tar
```

4. start network
```
cd basic-network
./start.sh
```

6. docker exec -it cli bash

```
// install cc
peer chaincode install -n marbles03 -v 1.0 -p github.com/marbles03/go

// instantiate cc
peer chaincode instantiate -o orderer.example.com:7050 -C mychannel -n marbles03 -v 1.0 -c '{"Args":["init"]}'

// three marbles
peer chaincode invoke -o orderer.example.com:7050   -C mychannel  -n marbles03 -c '{"Args":["initMarble","marble1","blue","35","tom"]}'
peer chaincode invoke -o orderer.example.com:7050   -C mychannel  -n marbles03 -c '{"Args":["initMarble","marble2","red","50","tom"]}'
peer chaincode invoke -o orderer.example.com:7050   -C mychannel  -n marbles03 -c '{"Args":["initMarble","marble3","blue","70","tom"]}'

// GetState()
peer chaincode query -o orderer.example.com:7050    -C mychannel  -n marbles03 -c '{"Args":["readMarble","marble1"]}'

// CompositeKey
peer chaincode invoke -o orderer.example.com:7050   -C mychannel  -n marbles03 -c '{"Args":["transferMarblesBasedOnColor","blue","jerry"]}'

// rich query
peer chaincode query -o orderer.example.com:7050    -C mychannel  -n marbles03 -c '{"Args":["queryMarbles","{\"query\":{\"owner\":\"jerry\"}}"]}'

// rich query (json string in cc)
peer chaincode query -o orderer.example.com:7050    -C mychannel  -n marbles03 -c '{"Args":["queryMarblesByOwner","tom"]}'

// range query
peer chaincode query -o orderer.example.com:7050    -C mychannel  -n marbles03 -c '{"Args":["getMarblesByRange","marble1","marble4"]}'

// GetHistoryForKey
peer chaincode query -o orderer.example.com:7050    -C mychannel  -n marbles03 -c '{"Args":["getHistoryForMarble","marble1"]}'
```
