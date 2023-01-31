# encodeFunctionResult

Encodes structured return data into ABI encoded data. It is the opposite of [`decodeFunctionResult`](/docs/contract/decodeFunctionResult).

## Install

```ts
import { encodeFunctionResult } from 'viem';
```

## Usage

Given an ABI (`abi`) and a function (`functionName`), pass through the values (`values`) to encode:

::: code-group

```ts [example.ts]
import { encodeFunctionResult } from 'viem';

const data = encodeFunctionResult({
  abi: wagmiAbi,
  functionName: 'ownerOf',
  value: ['0xa5cc3c03994db5b0d9a5eedd10cabab0813678ac'],
});
// '0x000000000000000000000000a5cc3c03994db5b0d9a5eedd10cabab0813678ac'
```

```ts [abi.ts]
export const wagmiAbi = [
  ...
  {
    inputs: [{ name: 'tokenId', type: 'uint256' }],
    name: 'ownerOf',
    outputs: [{ name: '', type: 'address' }],
    stateMutability: 'view',
    type: 'function',
  },
  ...
] as const;
```

```ts [client.ts]
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

export const publicClient = createPublicClient({
  chain: mainnet,
  transport: http(),
});
```

:::

### A more complex example

::: code-group

```ts [example.ts]
import { decodeFunctionResult } from 'viem'

const data = decodeFunctionResult({
  abi: wagmiAbi,
  functionName: 'getInfo',
  value: [
    {
      foo: {
        sender: '0xa5cc3c03994DB5b0d9A5eEdD10CabaB0813678AC',
        x: 69420n,
        y: true
      },
      sender: '0xa5cc3c03994DB5b0d9A5eEdD10CabaB0813678AC',
      z: 69
    }
  ]
})
// 0x000000000000000000000000a5cc3c03994db5b0d9a5eedd10cabab0813678ac0000000000000000000000000000000000000000000000000000000000010f2c0000000000000000000000000000000000000000000000000000000000000001000000000000000000000000a5cc3c03994db5b0d9a5eedd10cabab0813678ac0000000000000000000000000000000000000000000000000000000000000045
```

```ts [abi.ts]
export const wagmiAbi = [
  ...
  {
    inputs: [],
    name: 'getInfo',
    outputs: [
      {
        components: [
          {
            components: [
              {
                internalType: 'address',
                name: 'sender',
                type: 'address',
              },
              {
                internalType: 'uint256',
                name: 'x',
                type: 'uint256',
              },
              {
                internalType: 'bool',
                name: 'y',
                type: 'bool',
              },
            ],
            internalType: 'struct Example.Foo',
            name: 'foo',
            type: 'tuple',
          },
          {
            internalType: 'address',
            name: 'sender',
            type: 'address',
          },
          {
            internalType: 'uint32',
            name: 'z',
            type: 'uint32',
          },
        ],
        internalType: 'struct Example.Bar',
        name: 'res',
        type: 'tuple',
      },
    ],
    stateMutability: 'pure',
    type: 'function',
  },
  ...
] as const;
```

```ts [client.ts]
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

export const publicClient = createPublicClient({
  chain: mainnet,
  transport: http(),
});
```

:::

## Return Value

The decoded data. Type is inferred from the ABI.

## Parameters

### abi

- **Type:** [`Abi`](/docs/glossary/types#TODO)

The contract's ABI.

```ts
const data = encodeFunctionResult({
  abi: wagmiAbi, // [!code focus]
  functionName: 'ownerOf',
  value: ['0xa5cc3c03994db5b0d9a5eedd10cabab0813678ac'],
});
```

### functionName

- **Type:** `string`

The function to encode from the ABI.

```ts
const data = encodeFunctionResult({
  abi: wagmiAbi,
  functionName: 'ownerOf', // [!code focus]
  value: ['0xa5cc3c03994db5b0d9a5eedd10cabab0813678ac'],
});
```

### values

- **Type**: `Hex`

Return values to encode.

```ts
const data = encodeFunctionResult({
  abi: wagmiAbi,
  functionName: 'ownerOf',
  value: ['0xa5cc3c03994db5b0d9a5eedd10cabab0813678ac'], // [!code focus]
});
```