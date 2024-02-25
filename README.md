# 🌐 Ethereum Solidity inline Assembly 🔧
🌐 A Collection of Notes & Knowledge about Solidity inline Assembly 🔧

##  What is Solidity inline Assembly ❓
Solidity defines an assembly language that you can use without Solidity and also as “**inline assembly**” **inside Solidity source code**. 

You can interleave Solidity statements with inline assembly in a language close to the one of the **Ethereum Virtual Machine (EVM)**.

As the **Ethereum Virtual Machine (EVM) is a stack machine**, it is often hard to address the correct stack slot and provide arguments to opcodes at the correct point on the stack. Solidity’s inline assembly helps you do this, and with other issues that arise when writing manual assembly.
## GetCode 🔍
```
pragma solidity ^0.4.0;

library GetCode {
    function at(address _addr) public view returns (bytes o_code) {
        assembly {
            // retrieve the size of the code, this needs assembly
            let size := extcodesize(_addr)
            // allocate output byte array - this could also be done without assembly
            // by using o_code = new bytes(size)
            o_code := mload(0x40)
            // new "memory end" including padding
            mstore(0x40, add(o_code, and(add(add(size, 0x20), 0x1f), not(0x1f))))
            // store length in memory
            mstore(o_code, size)
            // actually retrieve the code, this needs assembly
            extcodecopy(_addr, add(o_code, 0x20), 0, size)
        }
    }
}
```
## sumPureAsm 🔧
```
pragma solidity ^0.4.16;

library VectorSum {
    function sumPureAsm(uint[] _data) public view returns (uint o_sum) {
        assembly {
           // Load the length (first 32 bytes)
           let len := mload(_data)

           // Skip over the length field.
           // Keep temporary variable so it can be incremented in place.
           // NOTE: incrementing _data would result in an unusable
           //       _data variable after this assembly block
           let data := add(_data, 0x20)

           // Iterate until the bound is not met.
           for
               { let end := add(data, len) }
               lt(data, end)
               { data := add(data, 0x20) }
           {
               o_sum := add(o_sum, mload(data))
           }
        }
    }
}
```

![Binance Ready to give crypto a try ? buy bitcoin and other cryptocurrencies on binance](Images/binance.jpg)
