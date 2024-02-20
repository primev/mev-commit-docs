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

# Consume bids from mev-commit

Providers who run their own block-building infrastructure and who are staked on the settlement chain will start receiving bids on the mev-commit p2p network from bidders. In order to see the incoming bids, the providers need to communicate with the provider API using the RPC clients or the HTTP APIs. Once they see the bid and they decide whether to accept/reject it, they will send the status to the mev-commit node. If the bid is ACCEPTED, a signed preconfirmation is sent back to the bidder and the mev-commit node will issue a transaction to the settlement layer to commit the preconfirmation. It is the responsibility of the provider node to guarantee inclusion of the transactions in the block they build once they send their acceptance. If they win the block auction and they include the transactions correctly, the rewards will be settled by the oracle in due course. If the bid is rejected, no preconfirmation is sent to the bidder. Also, no transaction is created on chain. There is no penalty for rejecting bids. The decision logic is completely dependent on the provider’s block-building infrastructure.

There are 4 options to communicate with the API

*   Use the official go RPC client. In order to use this in go code, you can go get the mev-commit package and then import the generated client.

    ```go
    import providerapiv1 "[github.com/primevprotocol/mev-commit/gen/go/rpc/providerapi/v1](<http://github.com/primevprotocol/mev-commit/gen/go/rpc/providerapi/v1>)"

    conn, err = grpc.DialContext(
    			context.Background(),
    			"localhost:13524",
    			grpc.WithBlock(),
    			grpc.WithTransportCredentials(insecure.NewCredentials()),
    )

    if err != nil {
    	// handle error
    }

    client := providerapiv1.NewProviderClient(conn)

    bidStream, err := client.ReceiveBids(context.Background(), &providerapiv1.EmptyMessage{})
    if err != nil {
    	// handle error
    }

    bidC := make(chan *providerapiv1.Bid)
    go func() {
    	defer close(bidC)
    	for {
    		bid, err := bidStream.Recv()
    		if err != nil {
    			// handle error
    		}
    		select {
    		case <-bidStream.Context().Done():
    		case bidC <- bid:
    		}
    	}
    }()

    stream, err := client.SendProcessedBids(context.Background())
    if err != nil {
    	// handle error
    }

    for bid := range bidS {
    	// check the bid details and communicate with the block-building
    	// infrastructure to make a decision
    	status := getDecision(bid)

    	err := stream.Send(&providerapiv1.BidResponse{
    			BidDigest: bid.BidDigest,
    			Status:    status,
    	})
    	if err != nil {
    		// handle error
    	}	
    }
    ```

    There is an [example implementation](https://github.com/primevprotocol/mev-commit/tree/main/examples/provideremulator) of dummy provider client that can be found in the repository which blindly accepts all the bids that it gets. This could be a good starting point for providers to use if they are trying to integrate a Golang project in.
* Use [https://github.com/fullstorydev/grpcurl](https://github.com/fullstorydev/grpcurl) or [https://github.com/bloomrpc/bloomrpc](https://github.com/bloomrpc/bloomrpc) or other GUI clients like [Postman](https://www.postman.com). The protobuf files are available in the repository.
* Use the REST API Check the API docs to see how to use the APIs. The RPC APIs are also available on the HTTP server.
* Generate a client in language of your choice Users can use the protobuf files to generate client in language of their choice.
