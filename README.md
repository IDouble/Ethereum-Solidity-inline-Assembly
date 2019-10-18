# 🌐 Ethereum Solidity inline Assembly 🔧
🌐 A Collection of Notes about Solidity inline Assembly 🔧

## 🔧 sumPureAsm 🔧
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
