# stresschain

Load-testing framework made for blockchain nodes like `bitcoind` and `parity` using the [K6 Performance Testing Tool](https://k6.io). Built for service providers and exchanges that run blockchain nodes at scale which require maximum uptime.

Currently, tests are only built to target the `JSON-RPC` spec for Bitcoin and Ethereum. For standard UTXO blockchains (bitcoin forks), this is sufficient. For Ethereum, more could be tested like the `web3` API or Websockets.


# Usage

There is a `Makefile` for the project which accepts two arguments, `client` and `endpoint`

Example:

```shell
$ make test client=bitcoin endpoint=http://localhost:8332
...

$ make test client=ethereum endpoint=http://localhost:8545
...
```

#### Test Results

Loadtest metics are currently output to `results.json` but they can also be sent to an `InfluxDB` server or `Datadog`. Read more: https://docs.k6.io/docs/results-output

# Project Structure

```
stresschain
|
|---- README.md                # This document
|
|---- test-all-endpoints.js    # Script used for PoC of automated new version testing
|
|---- endpoints.json           # List of endpoints used for PoC of automated new version testing
|
|---- results.json             # Output file for test results
|
|---- clients                  # Parent directory containing client specific files
|     |---- bitcoin            # Code implementation of stress tests for bitcoin
|     |---- ethereum           # Code implementation of stress tests for ethereum
|
|---- libs                     # Libraries and methods shared across clients
```

# Testing Methodology

For every client type (Bitcoin | Ethereum), a virtual user script is created which mimics the actions of an ordinary end-user.

The scripts live under `clients/$CLIENT/stresstest.js` and should ideally be created based off-of live metric data. The current user behavior is a fist-pass guess at average load-intensive user behavior.

# Automated Testing

This framework can potentially be used to automatically run load-tests against new versions of clients. An example script (`test-all-endpoints.js`) is provided to outline the logic required for the automation.

# Future improvements

- [ ] ETH Websocket Testing
- [ ] ETH Web3 API Testing
- [ ] Pull images from Dockerhub for automated tests of new version