[Dnsdir Telechat review of -17 by Vladimír Čunát][review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]
=================================================

[[VC-Review-1][review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]] dnsdir assigned me reviewing this draft.  I found no issues in there, except a
nit below.

[[VC-Review-2][review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]] Note that my knowledge around constrained systems (including CoAP) is close to
zero, so I could easily miss issues in those aspects.  I've read through this
whole document and glanced at some related constrained stuff.

[[VC-Review-3][review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]] Aside: I'd think that DNS people in IETF mostly won't be thrilled to have yet
another transport for DNS, but I understand that the current DNS encryption
options aren't great for such very constrained use cases, and I don't expect
DoC to become common outside of those cases.

[[VC-Review-4][review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]] Nit: some examples use `ARCOUNT` and some don't.  I'd suggest to unify all to
`ADDITIONAL`.

[Gorry Fairhurst's Discuss on draft-ietf-core-dns-over-coap-17][draft-ietf-core-dns-over-coap-17-ballot-gf]
===============================================================

DISCUSS
-------

[[GF-Discuss-1][draft-ietf-core-dns-over-coap-17-ballot-gf]] Thanks for this short and useful specification, I have two areas I would like to discuss to understand how this is expected to operate.

[[GF-Discuss-2][draft-ietf-core-dns-over-coap-17-ballot-gf]] As noted in https://datatracker.ietf.org/doc/statement-iesg-handling-ballot-positions-20220121/, a DISCUSS ballot is a request to have a discussion on the points below; I think that the document would be improved with a small addition here, but can be convinced otherwise.

---

1. [[GF-Discuss-3][draft-ietf-core-dns-over-coap-17-ballot-gf]] Are there actions the server needs to complete when a client has requested
to observe the record, such as to retain a count/list of observing
clients? ... I think I couuld be missing a part of the logic here:

   [[GF-Discuss-4][draft-ietf-core-dns-over-coap-17-ballot-gf]] If the CoAP request indicates that the DoC client wants to observe a
   resource record, a DoC server MAY use a DNS Subscribe message instead
   of a classic DNS query to fetch the information on behalf of a DoC
   client.

---

2. [[GF-Discuss-5][draft-ietf-core-dns-over-coap-17-ballot-gf]] This statement confused me:

   [[GF-Discuss-6][draft-ietf-core-dns-over-coap-17-ballot-gf]] If no more DoC clients observe a resource record for which the DoC
   server has an open subscription, the DoC server MUST use a DNS
   Unsubscribe message to close its subscription to the resource record
   as well.

- [[GF-Discuss-7][draft-ietf-core-dns-over-coap-17-ballot-gf]] 2.1. What does 'no more' mean?
- [[GF-Discuss-8][draft-ietf-core-dns-over-coap-17-ballot-gf]] Is this perhaps when there are no remaining clients, if so how is this
known? See above query about how should (or could) this be detected?
- [[GF-Discuss-9][draft-ietf-core-dns-over-coap-17-ballot-gf]] 2.2. What happens if the DNS Unsubscribe message fails to close its subscription?
... please explain a little the implication and what needs to be done in this case.

----

COMMENT
-------

[[GF-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-gf]] Thanks for responding to the TSV-ART review by T Pauley.

---

[[GF-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-gf]] Editorial:
Please more clearly label the following, so it is seen as an RFC-Ed comment:
"Before publication, please replace ff 0a with the hexadecimal...."

---

[[GF-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-gf]] Editorial:
      If it is "co", a
      CoAP request for CoAP over DTLS MUST be constructed.  Any other
      SvcParamKeys specifying a transport are out of the scope of this
      document.
- [[GF-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-gf]] This seems to be missing a normative reference to: I-D.ietf-core-coap-dtls-alpn.

NiTs:
[[GF-Comment-5][draft-ietf-core-dns-over-coap-17-ballot-gf]] /This document provides no specification on how to map between DoC and
   DoH, e.g., at a CoAP-to-HTTP-proxy.  In fact, such...
/This document provides no specification on how to map between DoC and
   DoH, e.g., at a CoAP-to-HTTP-proxy, such.../
(To connect the RFC2119 requirement to the clause to which it refers.)

[[GF-Comment-6][draft-ietf-core-dns-over-coap-17-ballot-gf]] /...this kind of poisoning attacks./...this kind of poisoning attack./
(remove 's').

[review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]: https://datatracker.ietf.org/doc/review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31/
[draft-ietf-core-dns-over-coap-17-ballot-gf]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_gorry-fairhurst
