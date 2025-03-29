# Ocean Garden Contract Documentation

This Move contract module, `nfts::project_nft`, defines a non-fungible token (NFT) representing a project with progress tracking and theme animal metadata. The contract is designed for use on the Sui blockchain and includes functionality for minting, updating, and burning a Project NFT.

## Contract Reference (Devnet)

- **Package ID:** `0x54991a701f11957028814fdd64de44bd25349f7f7ea9ece7771092f164ead5be`
- **Version:** 1
- **Module:** `project_nft`

## Overview

The contract leverages several Sui modules such as `sui::url`, `sui::object`, `sui::event`, `sui::transfer`, and `sui::tx_context` to implement NFT operations. It integrates both project-level data (like progress and transaction counts) and theme animal metadata (like images, icons, and levels).

## Data Structures

### ThemeAnimal

This structure stores metadata related to a theme animal used within the project. It includes:
- **id**: A string identifier for the animal.
- **name**: The animal's name.
- **image**: A URL (using `sui::url::Url`) for the animal's image.
- **icon** and **icon2**: Byte vectors that represent the icons associated with the animal.
- **level**: The current level of the theme animal.
- **max_level**: The maximum level the theme animal can achieve.

### ProjectNFT

This structure represents the Project NFT and contains:
- **id**: A unique identifier (UID) for the NFT.
- **name**: The name of the project.
- **description**: A description of the project.
- **url**: A URL (using `sui::url::Url`) pointing to project details.
- **progress**: A percentage value representing the project’s progress.
- **level**: The current level of the project NFT.
- **max_level**: The maximum level of the project NFT.
- **target_transactions**: The target number of transactions required for full progress.
- **current_transactions**: The current number of transactions recorded.
- **theme_animal**: The associated `ThemeAnimal` metadata.

## Events

The contract emits several events to notify external systems of changes in NFT state:

- **MintProjectNFTEvent**: Emitted upon minting a new Project NFT. It includes the NFT's object ID, the creator’s address, and the project name.
- **UpdateProgressEvent**: Emitted when the NFT's progress is updated. It provides the new progress percentage and the updated transaction count.
- **LevelUpEvent**: Emitted when the NFT levels up after meeting the required transaction threshold.

## Functions

### mint_project_nft

**Description:**  
Mints a new Project NFT with the provided project and theme animal details. It:
- Converts incoming byte vectors to strings.
- Constructs a URL using `url::new_unsafe_from_bytes`.
- Creates the `ThemeAnimal` and `ProjectNFT` structures.
- Emits a mint event.
- Transfers the newly minted NFT to the creator.

**Parameters:**
- `name`: Byte vector representing the project name.
- `description`: Byte vector for the project description.
- `url`: Byte vector for the project's URL.
- `target_transactions`: The target transaction count for the project.
- `theme_animal_id`: Byte vector for the theme animal's ID.
- `theme_animal_name`: Byte vector for the theme animal's name.
- `theme_animal_image`: Byte vector for the theme animal's image URL.
- `theme_animal_icon` and `theme_animal_icon2`: Byte vectors for the theme animal icons.
- `theme_animal_level`: The starting level for the theme animal.
- `theme_animal_max_level`: The maximum level for the theme animal.
- `ctx`: The transaction context.

### update_description

**Description:**  
Updates the description of an existing Project NFT.

**Parameters:**
- `nft`: A mutable reference to the Project NFT.
- `new_description`: A byte vector for the new description.

### update_progress

**Description:**  
Updates the NFT's progress by adding additional transactions. If the updated transaction count meets or exceeds a calculated threshold (using ceiling division), the NFT levels up (as long as it is below the maximum level). It then emits events for progress updates and level ups.

**Parameters:**
- `nft`: A mutable reference to the Project NFT.
- `additional_transactions`: The number of additional transactions to add.
- `ctx`: The transaction context.

### required_transactions (Helper Function)

**Description:**  
Calculates the required number of transactions for a given level using ceiling division.

**Parameters:**
- `target`: The target transaction count.
- `level`: The current level.
- `max_level`: The maximum level.

### burn

**Description:**  
Deletes a Project NFT, effectively "burning" it from the blockchain.

**Parameters:**
- `nft`: The Project NFT to be burned.

### Getter Functions

- **get_name**: Returns the project name.
- **get_description**: Returns the project description.
- **get_url**: Returns the project URL.
- **get_progress**: Returns the current progress percentage.
- **get_level**: Returns the current level of the NFT.

## Test Module

The test module, `nfts::project_nftTests`, demonstrates a complete flow:
1. **Minting:**  
   A Project NFT is minted using sample data.
2. **Transferring:**  
   The NFT is transferred from one address to another.
3. **Updating:**  
   The NFT’s description and progress are updated, simulating additional transactions.
4. **Burning:**  
   Although not explicitly shown in the test, the contract includes a function to burn the NFT.

The test module uses the Sui testing framework (via `sui::test_scenario`) to simulate transactions and validate the functionality of the contract.
