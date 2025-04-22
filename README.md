# Stacks Streaming Protocol

The Stacks Streaming Protocol is a smart contract implemented in Clarity for managing token streams on the Stacks blockchain. It allows users to create, manage, and interact with token streams, enabling automated and programmable token transfers over time.

## Features

** ST3S9E18YKY18RQBR6WVZQ816C19R3FB3K3M0K3XX.stacks-streaming-protocol **

- **Stream Creation**: Create a new token stream with a specified recipient, initial balance, payment rate, and timeframe.
- **Stream Refueling**: Add more tokens to an existing stream.
- **Stream Balance Calculation**: Calculate the balance available for withdrawal for both the sender and recipient.
- **Token Withdrawal**: Withdraw tokens that have been streamed to the recipient.
- **Refund Excess Tokens**: Refund unused tokens to the sender after the stream ends.
- **Stream Configuration Update**: Update the payment rate and timeframe of an existing stream with signature verification.
- **Signature Validation**: Verify the authenticity of updates using cryptographic signatures.

## Error Codes

The contract defines the following error codes for handling various failure scenarios:

- `ERR_UNAUTHORIZED (u0)`: The caller is not authorized to perform the action.
- `ERR_INVALID_SIGNATURE (u1)`: The provided signature is invalid.
- `ERR_STREAM_STILL_ACTIVE (u2)`: The stream is still active and cannot be refunded.
- `ERR_INVALID_STREAM_ID (u3)`: The specified stream ID does not exist.

## Data Structures

### Data Variables

- `latest-stream-id (uint)`: Tracks the latest stream ID.

### Mappings

- `streams (map)`: Stores information about each stream. Each stream is identified by a `stream-id` and contains the following fields:
    - `sender (principal)`: The address of the stream creator.
    - `recipient (principal)`: The address of the stream recipient.
    - `balance (uint)`: The total locked balance in the stream.
    - `withdrawn-balance (uint)`: The amount already withdrawn by the recipient.
    - `payment-per-block (uint)`: The amount of tokens transferred per block.
    - `timeframe (tuple)`: The start and stop blocks for the stream.

## Public Functions

### `stream-to`

Creates a new token stream.

- **Parameters**:
    - `recipient (principal)`: The recipient of the stream.
    - `initial-balance (uint)`: The initial balance to lock in the stream.
    - `timeframe (tuple)`: The start and stop blocks for the stream.
    - `payment-per-block (uint)`: The amount of tokens transferred per block.
- **Returns**: The ID of the newly created stream.

### `refuel`

Adds more tokens to an existing stream.

- **Parameters**:
    - `stream-id (uint)`: The ID of the stream to refuel.
    - `amount (uint)`: The amount of tokens to add.
- **Returns**: The amount added.

### `withdraw`

Withdraws tokens that have been streamed to the recipient.

- **Parameters**:
    - `stream-id (uint)`: The ID of the stream to withdraw from.
- **Returns**: The amount withdrawn.

### `refund`

Refunds unused tokens to the sender after the stream ends.

- **Parameters**:
    - `stream-id (uint)`: The ID of the stream to refund.
- **Returns**: The amount refunded.

### `update-details`

Updates the payment rate and timeframe of an existing stream.

- **Parameters**:
    - `stream-id (uint)`: The ID of the stream to update.
    - `payment-per-block (uint)`: The new payment rate.
    - `timeframe (tuple)`: The new start and stop blocks.
    - `signer (principal)`: The address of the signer.
    - `signature (buff 65)`: The cryptographic signature for verification.
- **Returns**: `true` if the update is successful.

## Read-Only Functions

### `calculate-block-delta`

Calculates the number of blocks a stream has been active.

- **Parameters**:
    - `timeframe (tuple)`: The start and stop blocks for the stream.
- **Returns**: The number of active blocks.

### `balance-of`

Checks the balance available for a party involved in a stream.

- **Parameters**:
    - `stream-id (uint)`: The ID of the stream.
    - `who (principal)`: The address of the party (sender or recipient).
- **Returns**: The available balance.

### `hash-stream`

Generates a hash of the stream configuration.

- **Parameters**:
    - `stream-id (uint)`: The ID of the stream.
    - `new-payment-per-block (uint)`: The new payment rate.
    - `new-timeframe (tuple)`: The new start and stop blocks.
- **Returns**: The hash of the stream configuration.

### `validate-signature`

Validates a cryptographic signature.

- **Parameters**:
    - `hash (buff 32)`: The hash of the data being signed.
    - `signature (buff 65)`: The cryptographic signature.
    - `signer (principal)`: The address of the signer.
- **Returns**: `true` if the signature is valid.

## Security Considerations

- **Authorization**: Only the sender or recipient of a stream can perform certain actions.
- **Signature Verification**: Updates to stream configurations require valid cryptographic signatures.
- **Stream Lifecycle**: Refunds are only allowed after the stream has ended.

## Deployment

The contract can be deployed on the Stacks blockchain. Replace `ST3S9E18YKY18RQBR6WVZQ816C19R3FB3K3M0K3XX` with the appropriate contract address.

## License

This project is licensed under the MIT License.