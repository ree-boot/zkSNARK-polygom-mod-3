# Polygon Advanced Final Project - zkCircuits

This directory is a project that performs a custom circuit logic.
![image](https://github.com/BuilderPrid/zardkat/assets/106882895/0a5716bf-b86b-41fc-a278-f1f035034c7f)

## Description
Imagine that you wake up, check your email, and you see an interesting task: Polygon is asking you to design a new zkSNARK circuit that implements some logical operations. You need to implement the circuit and deploy a verifier on-chain to verify proofs generated from this circuit

For this project, you will create a circuit using the circom programming language that implements the above logical gate

## Getting Started
Compile the Multiplier2() circuit and verify it against a smart contract verifier.
```javascript
pragma circom 2.0.0;


template Multiplier2 () {  

   // Declaration of signals.  
   signal input a;  
   signal input b;
   signal x,y;  
   signal output c;  

   //component declarations
   component andGate = AND();
   component orGate = OR();
   component notGate = NOT();

   // Circuit description.
   andGate.a <== a;
   andGate.b <== b;
   x <== andGate.out;
   
   notGate.in <== b;
   y <== notGate.out;

   orGate.a <== x;
   orGate.b <== y;

   //output
   c <== orGate.out;


}
template AND() {
    signal input a;
    signal input b;
    signal output out;

    out <== a*b;
}

template OR() {
    signal input a;
    signal input b;
    signal output out;

    out <== a + b - a*b;
}

template NOT() {
    signal input in;
    signal output out;

    out <== 1 + in - 2*in;
}


component main = Multiplier2();

```

### Install
`npm i`

### Compile
`npx hardhat circom` 
This will generate the **out** file with circuit intermediaries and geneate the **MultiplierVerifier.sol** contract

### Prove and Deploy
`npx hardhat run scripts/deploy.ts`
This script does 4 things  
1. Deploys the MultiplierVerifier.sol contract
2. Generates a proof from circuit intermediaries with `generateProof()`
3. Generates calldata with `generateCallData()`
4. Calls `verifyProof()` on the verifier contract with calldata

With two commands you can compile a ZKP, generate a proof, deploy a verifier, and verify the proof 🎉

## Authors

Rishabh Rawat

## License

This project is licensed under the MIT License - see the LICENSE.md file for details
