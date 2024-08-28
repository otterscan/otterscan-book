# Verification

## Verifying through the Sourcify UI

You can verify contracts using the Sourcify UI by going to http://localhost:3000/#/verifier in your browser.

## Verifying with Forge

1. If you deploy using a [Forge script](https://book.getfoundry.sh/reference/forge/forge-script), you should add the following contract verification arguments to the command:

```shell
forge script Deploy.s.sol .... --verify --verifier sourcify --verifier-url http://localhost:5555
```

2. If you want to deploy a contract without a script, you can use `forge create` to create the deployment transaction. Create a folder named `contracts` and put your contract files in there.

Here is how you would deploy a smart contract called `MyContract` with constructor arguments `0x67b1d87101671b127f5f8714789C7192f7ad340e` and `123456`:

```shell
./forge create --verify --verifier sourcify --verifier-url http://localhost:5555/ --interactive --optimize --rpc-url http://localhost:8545/ MyContract --constructor-args 0x67b1d87101671b127f5f8714789C7192f7ad340e 123456
```

3. To verify a contract that already has been deployed, you can use `forge verify-contract`. Here is an example of a verification of the above `MyContract` contract example to address `0xADC11306fcD68F47698D66047d923a52816Ee44F`. Forge can usually infer constructor arguments automatically:

```shell
./forge verify-contract --verifier sourcify --verifier-url http://localhost:5555/ --optimizer-runs 200 --rpc-url http://localhost:8545/ 0xADC11306fcD68F47698D66047d923a52816Ee44F MyContract
```
