# ERC404

Originally developed by [0xacme](https://github.com/0xacme).

ERC404 is an experimental, mixed ERC20 / ERC721 implementation with native liquidity and fractionalization. While these two standards are not designed to be mixed, this implementation strives to do so in as robust a manner as possible while minimizing tradeoffs.

In it's current implementation, ERC404 effectively isolates ERC20 / ERC721 standard logic or introduces pathing where possible. Pathing could best be described as a lossy encoding scheme in which token amount data and ids occupy shared space under the assumption that negligible token transfers occupying id space do not or do not need to occur.

This standard is entirely experimental and unaudited, while testing has been conducted in an effort to ensure execution is as accurate as possible. The nature of overlapping standards, however, does imply that integrating protocols will not fully understand their mixed function.

## ERC721 Notes

The ERC721 implementation here is a bit non-standard, where tokens are instead burned and minted repeatedly as per underlying / fractional transfers. This is a aspect of the concept's design is deliberate, with the goal of creating an NFT that has native fractionalization, liquidity and encourages some aspects of trading / engagement to farm unique trait sets.

## Usage

To deploy your own ERC404 token, look at the two examples provided in the src folder, Example.sol and Pandora.sol.

### Example.sol

This is an extremely simple minimal version of an ERC404 that mints the entire supply to the initial owner of the contract.

The key thing to note here is that the initial supply is minted by setting the ERC20 balance directly, unlike how standard ERC20 tokens will mint the initial tokens with a transfer function.

This is because if you were to use the transfer functions, it would not only mint the total supply of ERC20s, it would also mint the entire supply of ERC721s, which would be unnecessary and very expensive in terms of gas.

Also note that if the owner of these minted tokens attempts to send these initially minted tokens without first adding themselves to the whitelist, the transfer will fail because the contract will otherwise assume they hold the corresponding NFTs, when in fact the NFT minting step has been skipped for these initially minted tokens.

Generally the initial tokens minted to the deployer will be added to a DEX as liquidity. The DEX pool address should also be added to the whitelist to prevent minting NFTs to it and burning NFTs from it on transfer.

Future versions of the standard will contain ERC20-only minting functionality.

## Uniswap V3

To predict the address of your Uniswap V3 Pool, use the following simulator: [https://dashboard.tenderly.co/shared/simulation/92dadba3-92c3-46a2-9ccc-c793cac6c33d](https://dashboard.tenderly.co/shared/simulation/92dadba3-92c3-46a2-9ccc-c793cac6c33d).

To use:

1. Click Re-Simulate in the top right corner.
2. Update the simulation parameters: `tokenA` (your token address), `tokenB` (typically WETH, or `0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2`), and set the fee tier to either 500, 3000 (for 0.3%), or 10000 (for 1%).
3. Run Simulate, and then expand the Input/Output section. The output on the right column will show the derived pool address.

## Licensing

This source code is unlicensed, and free for anyone to use as they please. Any effort to improve source or explore the concept further is encouraged!
