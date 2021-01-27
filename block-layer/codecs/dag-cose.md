# Specification: DAG-COSE

**Status: Prescriptive - Draft**

COSE is a data format for signed and encrypted CBOR objects standardized as [RFC 8152](https://tools.ietf.org/html/rfc8152)

## Format

COSE describes a top common formats for signed/encrypted and mac'ed data
DAG-COSE therefore follows the same enconding and deconding steps as outlined in the COSE specification, with a few noteable differences:

1. Objects **must** be tagged
2. The canonical CBOR encoding as described in the [DAG-CBOR spec](./dag-cbor.md) **must** be used. Espeacially the sorting of keys in maps has to be implemented. (See below for details)
3. The content field **must** be a CID, either poiting to a detached block or on inline CID holding the content

TODO: IPLD schema for encoded/decoded COSE objects

### The Header Maps
The protected & unprotected headers a fields in a COSE object that can hold metadata about the payload/ciphertext and signatures/recipients such as the algorithm used, a key ID etc.
These headers are encoded as a CBOR map, where values are keyed by `integer labels`. 
There are a few ranges of keys layed out in the [IANA registry](https://iana.org/assignments/cose/cose.xhtml):

- **-65536 and above** are reserved for the registry or require specification/expert review
- **below -65536** private use (you are free to use them however you want)

To encode a header map you have to perform the following steps:
1. Translate all known parameter names into their integer label equivalent (and reject unkown labels)
    If keys are already integers keep them as-is. 
3. Sort the map by key value as outlined in the DAG-CBOR specification

To decode a header map you have to translate the integer labels back to their string representation, if you encounter an integer label that is not present in the registry tabel (such as a label in the private range) you **must** 'stringify' the integer.

## Security considerations

TODO

## Implementations