[Dnsdir Telechat review of -17 by Vladimír Čunát][review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]
=================================================

dnsdir assigned me reviewing this draft.  I found no issues in there, except a
nit below.

Note that my knowledge around constrained systems (including CoAP) is close to
zero, so I could easily miss issues in those aspects.  I've read through this
whole document and glanced at some related constrained stuff.

Aside: I'd think that DNS people in IETF mostly won't be thrilled to have yet
another transport for DNS, but I understand that the current DNS encryption
options aren't great for such very constrained use cases, and I don't expect
DoC to become common outside of those cases.

[[VlaCu-Review-1][review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]] Nit: some examples use `ARCOUNT` and some don't.  I'd suggest to unify all to
`ADDITIONAL`.

[Gorry Fairhurst's Discuss on draft-ietf-core-dns-over-coap-17][draft-ietf-core-dns-over-coap-17-ballot-gofa]
===============================================================

DISCUSS
-------

Thanks for this short and useful specification, I have two areas I would like to discuss to understand how this is expected to operate.

As noted in https://datatracker.ietf.org/doc/statement-iesg-handling-ballot-positions-20220121/, a DISCUSS ballot is a request to have a discussion on the points below; I think that the document would be improved with a small addition here, but can be convinced otherwise.

---

1. [[GoFa-Discuss-1][draft-ietf-core-dns-over-coap-17-ballot-gofa]] Are there actions the server needs to complete when a client has requested
to observe the record, such as to retain a count/list of observing
clients? ... I think I couuld be missing a part of the logic here:

> If the CoAP request indicates that the DoC client wants to observe a
> resource record, a DoC server MAY use a DNS Subscribe message instead
> of a classic DNS query to fetch the information on behalf of a DoC
> client.

---

2. [[GoFa-Discuss-2][draft-ietf-core-dns-over-coap-17-ballot-gofa]] This statement confused me:

> If no more DoC clients observe a resource record for which the DoC
> server has an open subscription, the DoC server MUST use a DNS
> Unsubscribe message to close its subscription to the resource record
> as well.

- [[GoFa-Discuss-3][draft-ietf-core-dns-over-coap-17-ballot-gofa]] 2.1. What does 'no more' mean?
- [[GoFa-Discuss-4][draft-ietf-core-dns-over-coap-17-ballot-gofa]] Is this perhaps when there are no remaining clients, if so how is this
known? See above query about how should (or could) this be detected?
- [[GoFa-Discuss-5][draft-ietf-core-dns-over-coap-17-ballot-gofa]] 2.2. What happens if the DNS Unsubscribe message fails to close its subscription?
... please explain a little the implication and what needs to be done in this case.

----

COMMENT
-------

Thanks for responding to the TSV-ART review by T Pauley.

---

[[GoFa-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-gofa]] Editorial:
Please more clearly label the following, so it is seen as an RFC-Ed comment:
"Before publication, please replace ff 0a with the hexadecimal...."

---

[[GoFa-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-gofa]] Editorial:
      If it is "co", a
      CoAP request for CoAP over DTLS MUST be constructed.  Any other
      SvcParamKeys specifying a transport are out of the scope of this
      document.
- This seems to be missing a normative reference to: I-D.ietf-core-coap-dtls-alpn.

NiTs:
[[GoFa-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-gofa]] /This document provides no specification on how to map between DoC and
   DoH, e.g., at a CoAP-to-HTTP-proxy.  In fact, such...
/This document provides no specification on how to map between DoC and
   DoH, e.g., at a CoAP-to-HTTP-proxy, such.../
(To connect the RFC2119 requirement to the clause to which it refers.)

[[GoFa-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-gofa]] /...this kind of poisoning attacks./...this kind of poisoning attack./
(remove 's').

[Paul Wouters' Discuss on draft-ietf-core-dns-over-coap-17][draft-ietf-core-dns-over-coap-17-ballot-pawo]
===========================================================

DISCUSS
-------
I have a DISCUSS that should be easy to fix, and a few comments.

[[PaWo-Discuss-1][draft-ietf-core-dns-over-coap-17-ballot-pawo]] Section 4.2.3.

I don't see RFC3655 or RFC2535 allowing setting the AD bit in a query,
only in the answer. But here it shows the example query with an AD bit
set. Shouldn't this be the DO bit?

COMMENT
-------
> [[PaWo-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-pawo]] A full SVCB mapping is being prepared in
> [I-D.ietf-core-transport-indication],

Phrase this more neutrally, as if the document is also already published.

> [[PaWo-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-pawo]] Paths with longer segments cannot be advertised with the "docpath"
> SvcParam and are thus NOT RECOMMENDED for the path to the DoC
> resource.

Should this not be a MUST NOT?

[[PaWo-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-pawo]] Section 8

> A DoC client may not be able to perform DNSSEC validation,
> e.g., due to code size constraints, or due to the size of
> the responses. It may trust its DoC server to perform DNSSEC
> validation;

I can't see an IoT device doing DNSSEC validation. It would have to
validate a large number of queries in a row to get the validation from
the DNS root down to the domain. Validation of a single query would
not contain all the information required, so you either need to implement
a fully recursive validating DNS server, or support RFC 7901 CHAIN Query
Requests in both the DoC Server and DoC client. While I think RFC7901 support
is doable, implementing a fully recursive validating DNS server is not.
I would rewrite the Section based on these assumptions (eg it has to trust
the AD bit)

[Mike Bishop's Yes on draft-ietf-core-dns-over-coap-16][draft-ietf-core-dns-over-coap-16-ballot-mibi]
=======================================================

COMMENT
-------
[[MiBi-Comment-1][draft-ietf-core-dns-over-coap-16-ballot-mibi]] One more item I noticed while preparing the ballot: the abstract and introduction state the use of DTLS and OSCORE for message protection, but TLS is also supported according to the remaining sections. Please either explicitly mention TLS or adjust to "(D)TLS" where appropriate.

[Mohamed Boucadair's Yes on draft-ietf-core-dns-over-coap-17][draft-ietf-core-dns-over-coap-17-ballot-mobo]
=============================================================

COMMENT
-------

Hi Martine, Christian, Cenk, Thomas, and Matthias,

Thank you for the effort put into this specification.

Also, thanks to Tim Wicinski for the DNSDIR reviews and for the authors for taking care of his comments.

[[MoBo-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Although there are a bunch of DNS transport out there (Do53, DoT, DoQ, DoH, etc.), the motivations for DoC are solid. Better, the proposed design is well-thought. I support this effort, hence my "Yes" ballot.

Please find below some comments; major comments are tagged with (*):

### [[MoBo-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Lack of operation considerations (*)

Other than discovery, the document does not discuss operational considerations that are worth to take into account to ease the deployability of DoC. At least, co-existence considerations (when several transport flavors are supported) should be covered (e.g., preference order). The following additional points may be considered:

#### [[MoBo-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Redirection (*)

Likewise, do we allow (and thus need to support) server redirection in case of 5.03? We should be explicit about the expected behavior. An example of deployment case where redirection support may be useful is: bootstrapping with a centralized DoC server and then redirect the DoC client to a local DoC server. I’m not pushing for any direction here, but simply providing an example of aspects to cover.

#### [[MoBo-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Confirmable and Non-Confirmable (*)

Should the spec provide more guidance about which mode should be used by default? I guess, the default mode is Confirmable. Right?

#### [[MoBo-Comment-5][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Control of CoAP Proxy Hops (*)

The spec mentions CoAP proxies, but I wonder whether it needs to mention the use of Hop-Limit Option to control how queries are propagated or help detect infinite loops due to incorrect proxy configuration.

#### [[MoBo-Comment-6][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Troubleshooting (*)

Do we need extra codes to demux DNS-related errors vs. conventional CoAP? For example, CoAP leg may be OK but the upstream DNS query may not be sent out because of a DNS failure. How to report that back to the DoC client?

### [[MoBo-Comment-7][draft-ietf-core-dns-over-coap-17-ballot-mobo]] SVCB is normative  (*)

Text such as (and similar)

   This document specifies "docpath" as a single-valued SvcParamKey that
   is mandatory for DoC SVCB records.

or

   Paths with longer segments cannot be advertised
   with the "docpath" SvcParam and are thus NOT RECOMMENDED for the path
   to the DoC resource.

Suggests that understanding the concept of SvcParam is required. I suggest to cite SVCB as normative.

### [[MoBo-Comment-8][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Recursion termination in the CoAP realm

Figure 1 may be interpreted as if “conventional” DNS message will always be triggered by a query from a DoC client. Should we mention that DoC server can terminate the query if it is authoritative for the queried resource?

### [[MoBo-Comment-9][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Back-to-Back DoC Server and Do53/DoT/DoH

There is no mapping frozen in the spec between various DNS transport. It would be cleaner from an architectural standpoint to indicate the B2B entity in Figure 1.

### [[MoBo-Comment-10][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Trusted source

CURRENT:
  Automatic configuration
   SHOULD only be done from a trusted source.

Why isn’t this a MUST?

### [[MoBo-Comment-11][draft-ietf-core-dns-over-coap-17-ballot-mobo]] On OSCORE

CURRENT:
   Because the ALPN extension is only
   defined for (D)TLS, these mechanisms cannot be used for DoC servers
   which use only OSCORE [RFC8613] and Ephemeral Diffie-Hellman Over
   COSE (EDHOC) [RFC9528] (with COSE abbreviating "Concise Binary Object
   Notation (CBOR) Object Signing and Encryption" [RFC9052]) for
   security.

Please note that DNR says the following:

      The "alpn" SvcParam may not be required in contexts such as a
      variant of DNS over the Constrained Application Protocol (CoAP)
      where messages are encrypted using Object Security for Constrained
      RESTful Environments (OSCORE) [RFC8613].

### [[MoBo-Comment-12][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Mysterious destination IP address (*)

CURRENT:
   *  The destination address for the request SHOULD be taken from
      additional information about the target, e.g., from an AAAA resource record
      associated with the target name or from an "ipv6hint" SvcParam

How we got that resolution as at this stage we don’t have a DNS server to communicate with? Maybe I’m missing something.

### [[MoBo-Comment-13][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Redundant requirement?

CURRENT:
   A DoC server MUST be able to parse requests of
   Content-Format "application/dns-message".

How is this different from:

  “Both DoC client and DoC server MUST be able to parse contents in the "application/dns-message"
  Content-Format"?

### [[MoBo-Comment-14][draft-ietf-core-dns-over-coap-17-ballot-mobo]] I-D.ietf-iotops-7228bis note

It is unlikely that I-D.ietf-iotops-7228bis will make it before DoC. I suggest you simply delete that RFC Editor note.

Cheers,
Med

[Éric Vyncke's Yes on draft-ietf-core-dns-over-coap-17][draft-ietf-core-dns-over-coap-17-ballot-ervy]
=======================================================

## COMMENT

### Éric Vyncke, INT AD, comments for draft-ietf-avtcore-rtp-scip-05
CC @evyncke

[[ErVy-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Thank you for the work put into this document. Like others, I was puzzled at first sight about a n+1 mechanism to transport DNS traffic, but after reading the motivation in section 1, it does make sense even if actual data (packet size, transaction, ...) would be helpful as DNS payload is already compressed.

Please find below some non-blocking COMMENT points/nits (replies would be appreciated even if only for my own education).

Special thanks to Marco Tiloca for the shepherd's write-up including the WG consensus and the justification of the intended status.

Other thanks to Vladimír Čunát , the DNS directorate reviewer (at my request), please consider this dns-dir review:
https://datatracker.ietf.org/doc/review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31/

I hope that this review helps to improve the document,

Regards,

-éric

### COMMENTS (non-blocking)

#### [[ErVy-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Abstract

Please mention that only OPCODE=0 is supported, i.e., not 'generic DNS messages' but only "DNS queries".

#### Section 1

[[ErVy-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Rather than using RFC 1035, you may want to use STD 13.

[[ErVy-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Please mention that only OPCODE=0 is supported, i.e., not 'generic DNS messages' but only "DNS queries".

[[ErVy-Comment-5][draft-ietf-core-dns-over-coap-17-ballot-ervy]] s/link layer frame sizes/link-layer frame sizes/ ?

[[ErVy-Comment-6][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Thanks for providing SVG graphics, the HTML rendering is so much nicer :)

#### [[ErVy-Comment-7][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Section 3.2

Should there be a space in `255OCTET`?

#### [[ErVy-Comment-8][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Section 4.1

Is there any way for provide an extension to other RR codes ? `For the purposes of this document, only OPCODE 0 (Query) is supported for DNS messages. Future work might provide specifications and considerations for other values of OPCODE.` seems rather vague about extension points. The UPDATE op-code could be very interesting.

#### [[ErVy-Comment-9][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Section 4.2.3

As there is only one example, please use singular in the section title.

#### Section 8

[[ErVy-Comment-10][draft-ietf-core-dns-over-coap-17-ballot-ervy]] s/That information can also imply trust in the DNSSEC validation by that server./That information can also imply trust in the DNSSEC validation by that *DoC* server./

[[ErVy-Comment-11][draft-ietf-core-dns-over-coap-17-ballot-ervy]] More generally, I was expecting more text about DNSSEC earlier in the text, e.g, by stating that a DoC server MAY (or SHOULD or MUST) be the DNSSEC validator.

[[ErVy-Comment-12][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Rather then referring to RFC 9364, you may want to refer to BCP 237.

[Andy Newton's No Objection on draft-ietf-core-dns-over-coap-17][draft-ietf-core-dns-over-coap-17-ballot-anne]
================================================================

## Andy Newton, ART AD, comments for draft-ietf-core-dns-over-coap-17
CC @anewton1998

* line numbers:
  - https://author-tools.ietf.org/api/idnits?url=https://www.ietf.org/archive/id/draft-ietf-core-dns-over-coap-17.txt&submitcheck=True

* comment syntax:
  - https://github.com/mnot/ietf-comments/blob/main/format.md

* "Handling Ballot Positions":
  - https://ietf.org/about/groups/iesg/statements/handling-ballot-positions/

## No Objection

I have no objection to the publication of this document as an RFC.

Many thanks to Todd Herr for the ARTART review.

[Deb Cooley's No Objection on draft-ietf-core-dns-over-coap-17][draft-ietf-core-dns-over-coap-17-ballot-deco]
===============================================================

COMMENT
-------

Thanks to Ben Schwartz for his earlier review (if not his secdir review) of this draft.

[[DeCo-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-deco]] Section 1, para 1:  Instead of just referencing DTLS with two RFCs, perhaps say that 'be secured by DTLS 1.2 or 1.3' and then add the RFC references (and you do need RFC 6347 even though 9147 obsoletes it).  If you can do the same for TLS, then do it (obviously RFC8446 is TLS 1.3).  [this construct appears throughout the draft, not just in Section 1]

[[DeCo-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-deco]] Section 3, last sentence:  What are the consequences of not complying with the SHOULD?

[[DeCo-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-deco]] Section 8, para 4:  I don't understand the first sentence:  'impact....limited,..both harden against injecting...' seem to be opposite?  If the impact is limited, how does it harden against injecting?

[[DeCo-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-deco]] Section 8, para 5, last sentence:  The 'e.g.' makes this confusing.  Are there other options besides DNSSEC?

[Orie Steele's No Objection on draft-ietf-core-dns-over-coap-17][draft-ietf-core-dns-over-coap-17-ballot-orste]
================================================================

COMMENT
-------

### Orie Steele, ART AD, comments for draft-ietf-core-dns-over-coap-17
CC @OR13

* line numbers:
  - https://author-tools.ietf.org/api/idnits?url=https://www.ietf.org/archive/id/draft-ietf-core-dns-over-coap-17.txt&submitcheck=True

* comment syntax:
  - https://github.com/mnot/ietf-comments/blob/main/format.md

* "Handling Ballot Positions":
  - https://ietf.org/about/groups/iesg/statements/handling-ballot-positions/

Thanks to Todd Herr for the ARTART review.

### Comments

#### [[OrSte-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-orste]] Why not MUST?

```
252	   discovery of designated resolvers [RFC9462].  Automatic configuration
253	   SHOULD only be done from a trusted source.
```

Suggest to strengthen the language to "MUST" or provide a rationale for why it is "SHOULD".


#### [[OrSte-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-orste]] confusing MAY

```
302	   list.  The same considerations for the "," and "" characters in
303	   docpath-segments for zone-file implementations as for the alpn-ids in
304	   an "alpn" SvcParam MAY apply (Section 7.1.1 of [RFC9460]).
```

Do you need the "MAY" here, is it not enough to say the same considerations apply?
I only skimmed 7.1.1 but it also contains a MAY, which I read as being sufficiently clear.

#### [[OrSte-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-orste]] Why not MUST?

```
375	      If not, or if the endpoint becomes unreachable, the algorithm
376	      SHOULD be repeated with the SVCB record with the next highest
377	      priority.
```

Are there other strategies implied by this should, or is this a should only because retry is not required?

#### [[OrSte-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-orste]] MUST be able to understand when not accepted

```
525	   Section 4.1) when requested.  A DoC client MUST be able to understand
526	   responses in the "application/dns-message" Content-Format when it
527	   does not send an Accept option.  Any response Content-Format other
```

I think this is better phrased as "use of the accept header is optional, however, all DoC clients MUST understand the "application/dns-message" Content-Format".
Possibly an alternative wording would be clearer.

[Roman Danyliw's No Objection on draft-ietf-core-dns-over-coap-17][draft-ietf-core-dns-over-coap-17-ballot-roda]
===============

COMMENT
-------

Thank you to Elwyn Davies for the GENART review.

From idnits:

  == [[RoDa-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-roda]] Unused Reference: 'RFC8742' is defined on line 951, but no explicit
     reference was found in the text

[review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]: https://datatracker.ietf.org/doc/review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31/
[draft-ietf-core-dns-over-coap-17-ballot-gofa]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_gorry-fairhurst
[draft-ietf-core-dns-over-coap-17-ballot-pawo]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_paul-wouters
[draft-ietf-core-dns-over-coap-16-ballot-mibi]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_mike-bishop
[draft-ietf-core-dns-over-coap-17-ballot-mobo]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_mohamed-boucadair
[draft-ietf-core-dns-over-coap-17-ballot-ervy]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_eric-vyncke
[draft-ietf-core-dns-over-coap-17-ballot-anne]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_andy-newton
[draft-ietf-core-dns-over-coap-17-ballot-deco]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_deb-cooley
[draft-ietf-core-dns-over-coap-17-ballot-orste]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_orie-steele
[draft-ietf-core-dns-over-coap-17-ballot-roda]: https://datatracker.ietf.org/doc/draft-ietf-core-dns-over-coap/ballot/#draft-ietf-core-dns-over-coap_roman-danyliw
