This is the open source documentation repository for [mev-commit](https://github.com/primev/mev-commit).

# What is mev-commit?

mev-commit is P2P software that creates a network of execution providers and bidders. Bidders can broadcast bids receive commitments from providers. A high throughput POA EVM chain settles the bids at the end of a block slot.

# Contribution Guidelines

This repository contains the source code for our documentation website. If you'd like to contribute, please follow these guidelines:

1. Fork the repository and create a new branch from the `main` branch.
2. Make your changes to the markdown files.
3. If you're creating a new file, edit the `mint.json` file to make it appear on the sidebar.
4. Submit a pull request to the `main` branch.

## Standards and conventions

### Links
- Ensure all links that reference the documentation is linking using an absolute path. For example, to reference fund node page, you do as follows: `[fund node](/get-started/bidders/fund-node)`.

### New Files
- Ensure all files are in lowercase.
- Ensure all files are using kebab-case. For example, the kebab-case version of "My First Preconf" is "my-first-preconf.mdx".

We'd appericiate if you would run mintlify dev to verify changes locally and mintlify broken-links to ensure there aren't any broken links introduced from your changes. Thank you!
