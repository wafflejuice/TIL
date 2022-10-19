# 2022-10-19
## UUID (universally unique identifier)
128-bit, `xxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx` format.
M(4-bit) indicates the version, N(1~3 most significant bits) indicates the variant.

Version 4: randomly generated

Q) Is UUID unique enough?

A) The number of random version-4 UUIDs which need to be generated in order to have a 50% probability of at least one collision is 2.71 quintillion(2.71 * 10^18).

source:
- [RFC4122](https://datatracker.ietf.org/doc/html/rfc4122.html)
- [wikipedia](https://en.wikipedia.org/wiki/Universally_unique_identifier)
