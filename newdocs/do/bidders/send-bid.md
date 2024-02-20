---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Send bid

On the bidder side, once the allowance has been added to the bidder node wallet, they can start sending bids to the network. We should make sure that we are connected to at least 1 provider node on the bidder side. This can be checked using the same `[localhost:13523/topology](<http://localhost:13523/topology>)` endpoint.

In order to send a bid to the bidder node there are 4 options:

*   Use the official go RPC client. In order to use this in go code, you can go get the mev-commit package and then import the generated client.

    ```go
    import bidderapiv1 "[github.com/primevprotocol/mev-commit/gen/go/rpc/bidderapi/v1](<http://github.com/primevprotocol/mev-commit/gen/go/rpc/providerapi/v1>)"

    conn, err = grpc.DialContext(
    			context.Background(),
    			"localhost:13524",
    			grpc.WithBlock(),
    			grpc.WithTransportCredentials(insecure.NewCredentials()),
    )

    if err != nil {
    	// handle error
    }

    bid := &pb.Bid{
    	TxHashes:    []string{
    		"fe4cb47db3630551beedfbd02a71ecc69fd59758e2ba699606e2d5c74284ffa7",
    		"71c1348f2d7ff7e814f9c3617983703435ea7446de420aeac488bf1de35737e8",
    	},
    	Amount:      "1000000000", // amount in wei
    	BlockNumber: 123456,
    }

    rcv, err := bidderClient.SendBid(context.Background(), bid)
    if err != nil {
    	// handle error
    }

    for {
    		pc, err := rcv.Recv()
    		if err == io.EOF {
    			break
    		}
    		if err != nil {
    			// handle error
    		}
    		fmt.Println("received preconfirmation", pc.String())
    }
    ```
* Use [https://github.com/fullstorydev/grpcurl](https://github.com/fullstorydev/grpcurl) or [https://github.com/bloomrpc/bloomrpc](https://github.com/bloomrpc/bloomrpc) or other GUI clients like [Postman](https://www.postman.com). The protobuf files are available in the repository.
*   Use the REST API Check the API docs to see how to use the APIs. The RPC APIs are also available on the HTTP server.

    * A request sample with API can be as follows:

    ```
    > curl -X POST localhost:13523/v1/bidder/bid -d '{
      "amount": "1000000000",
      "blockNumber": 123456,
      "txHashes": [
        "fe4cb47db3630551beedfbd02a71ecc69fd59758e2ba699606e2d5c74284ffa7",
        "71c1348f2d7ff7e814f9c3617983703435ea7446de420aeac488bf1de35737e8"
      ]
    }'
    ```
* Generate a client in language of your choice Users can use the protobuf files to generate client in language of their choice.
