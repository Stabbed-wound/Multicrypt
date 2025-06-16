# Multicrypt

## What

Multicrypt is what I (due to a lack of proper terminology) call a multiseat
encryption format, in reference to a multiseat operating system. It allows for
one block of ciphertext to encode multiple seperate plaintexts under seperate
multi-key access with each "seat" being completely hidden from each other.

## Why

It's like [Veracrypt](https://veracrypt.io) but with up to 32 volumes and as
such fulfills a very similar purpose. The primary advantage over veracrypt is
that each seat (volume) can be opened with multiple keys, similarly to LUKS.

## How

Current version: [1.0](Versions/v1.md)

The header section contains 32 keyslots which can be decrypted with the entered
password. Each slot contains the master key for the metadata contained in the
header, the master key for the seat the key has access to, and the position and
size of the seat's local header in the data section. The decrypted seat header
contains all necessary information to decrypt the associated plaintext.

### Example

One seat's perspective of the decrypted block

```text
        ┌────────────────────────────────────────┐
     ┌──┼────────────────────────────────────────┼─┬─────────────────────────────────────────────────────────┐
     │  │                                        │ │                                                         │
     │  │           ┌────────────────────────────┼─┼───┐ ┌────────────┐                                      │
     ↓  │           ↓                            ↓ │   │ │            ↓                                      ↓
┌────┬──────┬───┬───┬────────────────────┬───────┬─────────────┬──────┬──────────────┬─────────────────┬─────┬──────┬──┐
│    │ slot │   │   │    data cluster    │       │ seat header │      │ data cluster │                 │     │ slot │  │
└────┴──────┴───┴───┴────────────────────┴───────┴─────────────┴──────┴──────────────┴─────────────────┴─────┴──────┴──┘
 └─────────────┘ └────────────────────────────────────────────────────────────────────────────────────┘ └─────────────┘
     header                                               data                                           backup header
```
