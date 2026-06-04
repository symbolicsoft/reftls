# ProVerif Models for TLS 1.2 and TLS 1.3

[Download ProVerif from the ProVerif website](http://proverif.inria.fr).

The `.pv` files corresponding to various combinations of the protocol should be run by the command:

    proverif -lib <library> <filename>

where `<library>` is the matching library (without the `.pvl` extension) listed
alongside each model below.

### Generic Libraries with Threat Model and Protocol Processes

* [tls-lib.pvl](tls-lib.pvl) — TLS 1.2 and TLS 1.3 (draft 18)
* [tls-lib-draft20.pvl](tls-lib-draft20.pvl) — TLS 1.3 (draft 20)
* [tls-lib-rfc8446.pvl](tls-lib-rfc8446.pvl) — TLS 1.3 (RFC 8446, final)
* [tls-lib-pq.pvl](tls-lib-pq.pvl) — TLS 1.3 with hybrid and standalone post-quantum (ML-KEM) key exchange


### ProVerif Models for TLS 1.2 and TLS 1.3

Each model is run with the library named on its line:

*   TLS 1.2 only: [tls12.pv](tls12.pv) — `proverif -lib tls-lib tls12.pv`
*   TLS 1.3 (draft 18) DHE+PSK 0-RTT+1-RTT: [tls13-draft18-only.pv](tls13-draft18-only.pv) — `proverif -lib tls-lib tls13-draft18-only.pv`
*   TLS 1.2 + TLS 1.3 (draft 18): [tls12-tls13-draft18.pv](tls12-tls13-draft18.pv) — `proverif -lib tls-lib tls12-tls13-draft18.pv`
*   TLS 1.3 (draft 20): [tls13-draft20-only.pv](tls13-draft20-only.pv) — `proverif -lib tls-lib-draft20 tls13-draft20-only.pv`
*   TLS 1.3 (RFC 8446): [tls13-rfc8446-only.pv](tls13-rfc8446-only.pv) — `proverif -lib tls-lib-rfc8446 tls13-rfc8446-only.pv`


### ProVerif Models for Post-Quantum (Hybrid) TLS 1.3

These models extend the RFC 8446 handshake with standalone and hybrid ML-KEM key
exchange and are run with the `tls-lib-pq` library. The accompanying analysis is
in [../paper/tls-hybrid.pdf](../paper/tls-hybrid.pdf).

*   Robustness — combined classical, standalone ML-KEM, and hybrid key exchange (secrecy and authentication hold unless *both* the DH and the KEM component are weak): [tls13-pq.pv](tls13-pq.pv) — `proverif -lib tls-lib-pq tls13-pq.pv`
*   KEM key reuse and forward secrecy: [tls13-pq-reuse.pv](tls13-pq-reuse.pv) — `proverif -lib tls-lib-pq tls13-pq-reuse.pv`
*   Non-static roles (role confusion / unknown-key-share): [tls13-pq-roles.pv](tls13-pq-roles.pv) — `proverif -lib tls-lib-pq tls13-pq-roles.pv`


### Understanding the results

ProVerif generates a large amount of output, so you may want to run:

    proverif -lib <library> <filename> | tee results.txt | grep ^RESULT

which will put all the results in a file `results.txt` and summarize the success
or failure of various security queries.
(Warning: verifying these models takes a long time and a significant amount of
RAM even on powerful workstations.)

To understand the results, look at the comments above the queries in the PV
file. Roughly, the "true" queries correspond to security goals like (Forward)
Secrecy, Authenticity, Replay Prevention, and Unique Channel Identifiers for the
session keys and for the various data fragments. The "false" queries correspond
to queries that we expect to fail; they show that our verification is tight,
disabling some of the conditions in our "true" queries would cause them to be
false.
