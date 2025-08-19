Revision Plan for draft-ietf-core-dns-over-coap-18
==================================================

- [Major Issues](#major-issues)
  - [x] [Clarify (or Discuss with Gory) What Happens if DNS Unsubscribe Fails](#clarify-or-discuss-with-gory-what-happens-if-dns-unsubscribe-fails) (@chrysn)
  - [x] [Add Operational Considerations Section](#add-operational-considerations-section)
      - [x] [Preference of Different Transport Flavors](#preference-of-different-transport-flavors)
      - [x] [Add Operational Considerations on Redirection](#add-operational-considerations-on-redirection)
      - [x] [Provide Operational Considerations on Non-Confirmable vs Confirmable](#provide-operational-considerations-on-non-confirmable-vs-confirmable)
      - [x] [Add Considerations on Hop-Limit Option](#add-considerations-on-hop-limit-option)
      - [x] [How Should DNS Errors Be Signaled to DoC Client?](#how-should-dns-errors-be-signaled-to-doc-client)
- [Minor Issues](#minor-issues)
  - [x] [Statement about ALPN Extension Contradicts DNR?](#statement-about-alpn-extension-contradicts-dnr)
  - [x] [Mention That Only OPCODE=0 Is Supported in Abstract and Introduction](#mention-that-only-opcode0-is-supported-in-abstract-and-introduction)
  - [x] [Provide Extension Point For Other OPCODEs](#provide-extension-point-for-other-opcodes)
  - [x] [Mention DNSSEC Earlier](#mention-dnssec-earlier)
  - [x] [Clarify (or Discuss with Gory) Actions Server Needs to Complete When Client Observes Resource](#clarify-or-discuss-with-gory-actions-server-needs-to-complete-when-client-observes-resource) (@chrysn)
  - [x] [Clarify (or Discuss with Gory) What “No More Clients Observe A Resource Record” Means](#clarify-or-discuss-with-gory-what-no-more-clients-observe-a-resource-record-means) (@chrysn)
  - [x] [Refer to Drafts as If They Were Already Published](#refer-to-drafts-as-if-they-were-already-published)
  - [x] [RFC 9460 Should be Normative](#rfc-9460-should-be-normative)
  - [x] [State More Clearly Why DNS ID = 0 Is OK to Use With DTLS or OSCORE](#state-more-clearly-why-dns-id--0-is-ok-to-use-with-dtls-or-oscore)
  - [x] [Remove Confusing MAY](#remove-confusing-may)
  - [x] [Clarify Why SVCB Record Algorithm SHOULD Be Repeated (and not MUST)](#clarify-why-svcb-record-algorithm-should-be-repeated-and-not-must)
- [Nits](#nits)
  - [x] [Unify `ARCOUNT` to `ADDITIONAL` in Examples](#unify-arcount-to-additional-in-examples) ([db57f8e3](https://github.com/core-wg/draft-dns-over-coap/commit/db57f8e35f6ac90bc914f4eba343d0f3fbb1bd62))
  - [x] [Remove AD Bit from Query Examples](#remove-ad-bit-from-query-examples)
  - [x] [Label Note on `ff 0a` More Clearly as an RFC-Ed Note](#label-note-on-ff-0a-more-clearly-as-an-rfc-ed-note)
  - [x] [Add Missing Citation to -coap-dtls-alpn](#add-missing-citation-to--coap-dtls-alpn)
  - [x] [DoC Server May be the Authoritative Nameserver](#doc-server-may-be-the-authoritative-nameserver)
  - [x] [MUST Configure from Trusted Source](#must-configure-from-trusted-source)
  - [x] [Redundant requirement on "application/dns-message"](#redundant-requirement-on-applicationdns-message)
  - [x] [Use BCP/STD instead of RFCs When Applicable](#use-bcpstd-instead-of-rfcs-when-applicable)
  - [x] [Use Singular in Title When Only One Example in Section](#use-singular-in-title-when-only-one-example-in-section)
  - [x] [Remove Unused Reference](#remove-unused-reference) ([d772a47](https://github.com/core-wg/draft-dns-over-coap/commit/d772a47b6810cadbf91e3ae93626f25359963a05))
  - [x] [Mention Protocols First, Then Sequence Up the References / Explicitly Mention (D)TLS 1.2, 1.3](#mention-protocols-first-then-sequence-up-the-references-explicitly-mention-dtls-12-13)
  - [x] [Are There Other Integrity Mechanisms Than DNSSEC](#are-there-other-integrity-mechanisms-than-dnssec)
  - [x] [Rephrasings Regarding Accept Option](#rephrasings-regarding-accept-option)
  - [x] [Language Nits](#language-nits)
- [No-Ops](#no-ops)
  - [x] [Longer Segments Than Allowed "docpath" MUST NOT be Allowed?](#longer-segments-than-allowed-docpath-must-not-be-allowed)
  - [x] [IoT Devices Can Not Do DNSSEC Validation](#iot-devices-can-not-do-dnssec-validation)
  - [x] [Where Does the Destination Address in DDR/DNR Come From](#where-does-the-destination-address-in-ddrdnr-come-from)
  - [x] [Remove RFC Editor's Note on I-D.ietf-iotops-7228bis](#remove-rfc-editors-note-on-i-dietf-iotops-7228bis)
  - [x] [Mention TLS as Security Mechanism](#mention-tls-as-security-mechanism)
  - [x] [Link-layer vs. Link Layer](#link-layer-vs-link-layer)
  - [x] [Add Space to `255OCTET`](#add-space-to-255octet)
- [Unclear Issues](#unclear-issues)
  - [ ] [Indicate B2B Entity in Figure 1](#indicate-b2b-entity-in-figure-1)

Major Issues
============

## Clarify (or Discuss with Gory) What Happens if DNS Unsubscribe Fails

> 2. [[GoFa-Discuss-2][draft-ietf-core-dns-over-coap-17-ballot-gofa]] This statement confused me:
>
> > If no more DoC clients observe a resource record for which the DoC
> > server has an open subscription, the DoC server MUST use a DNS
> > Unsubscribe message to close its subscription to the resource record
> > as well.
>
> [...]
>
> - [[GoFa-Discuss-5][draft-ietf-core-dns-over-coap-17-ballot-gofa]] 2.2. What happens if the DNS Unsubscribe message fails to close its subscription?
> ... please explain a little the implication and what needs to be done in this case.

We indeed should describe this case more in-depth. My [Martine] proposal would be: Do not delete the
client from the list of observers until we got confirmation that the DNS Unsubscribe succeeded.
This would cause the server to keep sending notifications to the client and the client to send
Resets (as per the cancellation procedure in
[RFC 7641, section 3.6](https://datatracker.ietf.org/doc/html/rfc7641#section-3.6)).


Minor Issues
============

## Statement about ALPN Extension Contradicts DNR?

### [[MoBo-Comment-11][draft-ietf-core-dns-over-coap-17-ballot-mobo]] On OSCORE

> CURRENT:
>    Because the ALPN extension is only
>    defined for (D)TLS, these mechanisms cannot be used for DoC servers
>    which use only OSCORE [RFC8613] and Ephemeral Diffie-Hellman Over
>    COSE (EDHOC) [RFC9528] (with COSE abbreviating "Concise Binary Object
>    Notation (CBOR) Object Signing and Encryption" [RFC9052]) for
>    security.
>
> Please note that DNR says the following:
>
>       The "alpn" SvcParam may not be required in contexts such as a
>       variant of DNS over the Constrained Application Protocol (CoAP)
>       where messages are encrypted using Object Security for Constrained
>       RESTful Environments (OSCORE) [RFC8613].

Remove

> Because the ALPN extension is only defined for (D)TLS, [...]

## Mention That Only OPCODE=0 Is Supported in Abstract and Introduction

> #### [[ErVy-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Abstract
>
> Please mention that only OPCODE=0 is supported, i.e., not 'generic DNS messages' but only "DNS queries".

> #### Section 1
>
> [[ErVy-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Please mention that only OPCODE=0 is supported, i.e., not 'generic DNS messages' but only "DNS queries".

Will do, but I [Martine] think it is somewhat confusing that both the client initiated message and
the communication type defined by OPCODE=0 is called “DNS query” (see also old title of the draft
until [draft-lenders-dns-over-coap-03](https://datatracker.ietf.org/doc/draft-lenders-dns-over-coap/03/)).
So we should be really careful about the wording.

"Support for DNS queries (OPCODE = 0) ..."

## Provide Extension Point For Other OPCODEs
> #### [[ErVy-Comment-8][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Section 4.1
>
> Is there any way for provide an extension to other RR codes ? `For the purposes of this document, only OPCODE 0 (Query) is supported for DNS messages. Future work might provide specifications and considerations for other values of OPCODE.` seems rather vague about extension points. The UPDATE op-code could be very interesting.

Proposal:

"This document only specifies OPCODE 0 (Query) for DNS over CoAP messages. This document only specifies OPCODE 0 (Query) for DNS over CoAP messages. Future documents, providing considerations for additional OPCODEs or extending its specification (e.g. by describing whether other CoAP codes need to be used for some operations)."

## Mention DNSSEC Earlier

> #### Section 8
>
> [[ErVy-Comment-11][draft-ietf-core-dns-over-coap-17-ballot-ervy]] More generally, I was expecting more text about DNSSEC earlier in the text, e.g, by stating that a DoC server MAY (or SHOULD or MUST) be the DNSSEC validator.

Also considering [Paul's comment](#iot-devices-can-not-do-dnssec-validation), we should do that.

## Clarify (or Discuss with Gory) Actions Server Needs to Complete When Client Observes Resource

> 1. [[GoFa-Discuss-1][draft-ietf-core-dns-over-coap-17-ballot-gofa]] Are there actions the server needs to complete when a client has requested
> to observe the record, such as to retain a count/list of observing
> clients? ... I think I couuld be missing a part of the logic here:
>
> > If the CoAP request indicates that the DoC client wants to observe a
> > resource record, a DoC server MAY use a DNS Subscribe message instead
> > of a classic DNS query to fetch the information on behalf of a DoC
> > client.

Should be covered by [RFC 7641, Section 4.1][https://datatracker.ietf.org/doc/html/rfc7641#section-4.1].
As the query is part of the FETCH request, it should be part of the observable resource as per [RFC
7641, Section 1.4](https://datatracker.ietf.org/doc/html/rfc7641#section-1.4). We should
clarify that.

## Clarify (or Discuss with Gory) What “No More Clients Observe A Resource Record” Means

> 2. [[GoFa-Discuss-2][draft-ietf-core-dns-over-coap-17-ballot-gofa]] This statement confused me:
>
> > If no more DoC clients observe a resource record for which the DoC
> > server has an open subscription, the DoC server MUST use a DNS
> > Unsubscribe message to close its subscription to the resource record
> > as well.
>
> - [[GoFa-Discuss-3][draft-ietf-core-dns-over-coap-17-ballot-gofa]] 2.1. What does 'no more' mean?
> - [[GoFa-Discuss-4][draft-ietf-core-dns-over-coap-17-ballot-gofa]] Is this perhaps when there are no remaining clients, if so how is this
> known? See above query about how should (or could) this be detected?
>
> [...]

We mean “If the list of observers on that resource record is empty” as per the cancellation procedure
in [RFC 7641, section 3.6](https://datatracker.ietf.org/doc/html/rfc7641#section-3.6) here. We will
clarify.

## Refer to Drafts as If They Were Already Published

> > [[PaWo-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-pawo]] A full SVCB mapping is being prepared in
> > [I-D.ietf-core-transport-indication],
>
> Phrase this more neutrally, as if the document is also already published.

Proposed phrasing:

> [!TIP]
> A full SVCB mapping is specified in [I-D.ietf-core-transport-indication].
> It generalizes mechanisms for all CoAP services.
> This document introduces only the discovery of DoC services.

## RFC 9460 Should be Normative

> ### [[MoBo-Comment-7][draft-ietf-core-dns-over-coap-17-ballot-mobo]] SVCB is normative  (*)
>
> Text such as (and similar)
>
>    This document specifies "docpath" as a single-valued SvcParamKey that
>    is mandatory for DoC SVCB records.
>
> or
>
>    Paths with longer segments cannot be advertised
>    with the "docpath" SvcParam and are thus NOT RECOMMENDED for the path
>    to the DoC resource.
>
> Suggests that understanding the concept of SvcParam is required. I suggest to cite SVCB as normative.

Will do.

## State More Clearly Why DNS ID = 0 Is OK to Use With DTLS or OSCORE

> [[DeCo-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-deco]] Section 8, para 4:  I don't understand the first sentence:  'impact....limited,..both harden against injecting...' seem to be opposite?  If the impact is limited, how does it harden against injecting?

That sentence is indeed a bit of a word salad. Will fix.

## Remove Confusing MAY

> #### [[OrSte-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-orste]] confusing MAY
>
> ```
> 302	   list.  The same considerations for the "," and "" characters in
> 303	   docpath-segments for zone-file implementations as for the alpn-ids in
> 304	   an "alpn" SvcParam MAY apply (Section 7.1.1 of [RFC9460]).
> ```
>
> Do you need the "MAY" here, is it not enough to say the same considerations apply?
> I only skimmed 7.1.1 but it also contains a MAY, which I read as being sufficiently clear.

Will do.

## Clarify Why SVCB Record Algorithm SHOULD Be Repeated (and not MUST)

> #### [[OrSte-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-orste]] Why not MUST?
>
> ```
> 375	      If not, or if the endpoint becomes unreachable, the algorithm
> 376	      SHOULD be repeated with the SVCB record with the next highest
> 377	      priority.
> ```
>
> Are there other strategies implied by this should, or is this a should only because retry is not required?

Discovery of the server is somewhat optional (there are other ways to configure a DoC server), so it
would be weird to require using the next highest priority SVCB record (also what if there is none).
I [Martine] think we should clarify at least that.

Nits
====

## Unify `ARCOUNT` to `ADDITIONAL` in Examples

> [[VlaCu-Review-1][review-ietf-core-dns-over-coap-17-dnsdir-telechat-cunat-2025-07-31]] Nit: some examples use `ARCOUNT` and some don't.  I'd suggest to unify all to
`ADDITIONAL`.

Will do.

## Remove AD Bit from Query Examples
> [[PaWo-Discuss-1][draft-ietf-core-dns-over-coap-17-ballot-pawo]] Section 4.2.3.
>
> I don't see RFC3655 or RFC2535 allowing setting the AD bit in a query,
> only in the answer. But here it shows the example query with an AD bit
> set. Shouldn't this be the DO bit?

Was copy-pasta from the response examples (we have no tool that outputs queries this way). Will do.

## Label Note on `ff 0a` More Clearly as an RFC-Ed Note

> [[GoFa-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-gofa]] Editorial:
> Please more clearly label the following, so it is seen as an RFC-Ed comment:
> "Before publication, please replace ff 0a with the hexadecimal...."

Not sure how we can do that more clearer as it [already is](https://www.ietf.org/archive/id/draft-ietf-core-dns-over-coap-17.html#section-3.2.1-1)
However, it is a bit away from the "RFC Ed.:" in the more text-based / HTMLized view. So maybe move
this sentence to the top and give the reasoning after? With another note to also remove that
whole paragraph once done at the end.

## Add Missing Citation to -coap-dtls-alpn

> [[GoFa-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-gofa]] Editorial:
>       If it is "co", a
>       CoAP request for CoAP over DTLS MUST be constructed.  Any other
>       SvcParamKeys specifying a transport are out of the scope of this
>       document.
> - This seems to be missing a normative reference to: I-D.ietf-core-coap-dtls-alpn.

Will do.

## DoC Server May be the Authoritative Nameserver

> ### [[MoBo-Comment-8][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Recursion termination in the CoAP realm
>
> Figure 1 may be interpreted as if “conventional” DNS message will always be triggered by a query from a DoC client. Should we mention that DoC server can terminate the query if it is authoritative for the queried resource?

The dashed line might be interpreted as such, but might be worth to explicitly mention this in the
Introduction text.

## MUST Configure from Trusted Source

> ### [[MoBo-Comment-10][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Trusted source
>
> CURRENT:
>   Automatic configuration
>    SHOULD only be done from a trusted source.
>
> Why isn’t this a MUST?

> [[DeCo-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-deco]] Section 3, last sentence:  What are the consequences of not complying with the SHOULD?

> #### [[OrSte-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-orste]] Why not MUST?
>
> ```
> 252	   discovery of designated resolvers [RFC9462].  Automatic configuration
> 253	   SHOULD only be done from a trusted source.
> ```
>
> Suggest to strengthen the language to "MUST" or provide a rationale for why it is "SHOULD".

I [Martine] am a bit hesitant about this since we do not really define what “a trusted source” is.
However, three reviews pointed this out (and Deb asked for the inverse of the SHOULD) so we probably
should change the SHOULD to a MUST.

## Redundant requirement on "application/dns-message"
> ### [[MoBo-Comment-13][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Redundant requirement?
>
> CURRENT:
>    A DoC server MUST be able to parse requests of
>    Content-Format "application/dns-message".
>
> How is this different from:
>
>   “Both DoC client and DoC server MUST be able to parse contents in the "application/dns-message"
>   Content-Format"?

We will remove the redundant requirement in Section 4.2.1.

## Use BCP/STD instead of RFCs When Applicable
> [[ErVy-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Rather than using RFC 1035, you may want to use STD 13.

> [[ErVy-Comment-12][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Rather then referring to RFC 9364, you may want to refer to BCP 237.

Will do.


## Use Singular in Title When Only One Example in Section

> #### [[ErVy-Comment-9][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Section 4.2.3
>
> As there is only one example, please use singular in the section title.

Will do.

## Remove Unused Reference

>   `==` [[RoDa-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-roda]] Unused Reference: 'RFC8742' is defined on line 951, but no explicit reference was found in the text

Will do. This is a left-over from when `docpath` was a CBOR sequence.

## Mention Protocols First, Then Sequence Up the References / Explicitly Mention (D)TLS 1.2, 1.3

> [[DeCo-Comment-1][draft-ietf-core-dns-over-coap-17-ballot-deco]] Section 1, para 1:  Instead of just referencing DTLS with two RFCs, perhaps say that 'be secured by DTLS 1.2 or 1.3' and then add the RFC references (and you do need RFC 6347 even though 9147 obsoletes it).  If you can do the same for TLS, then do it (obviously RFC8446 is TLS 1.3).  [this construct appears throughout the draft, not just in Section 1]

Will do.

## Are There Other Integrity Mechanisms Than DNSSEC

> [[DeCo-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-deco]] Section 8, para 5, last sentence:  The 'e.g.' makes this confusing.  Are there other options besides DNSSEC?

Never say never, regarding other options, but we can remove the 'e.g.' if it confuses people.

## Rephrasings Regarding Accept Option
> #### [[OrSte-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-orste]] MUST be able to understand when not accepted
>
> ```
> 525	   Section 4.1) when requested.  A DoC client MUST be able to understand
> 526	   responses in the "application/dns-message" Content-Format when it
> 527	   does not send an Accept option.  Any response Content-Format other
> ```
>
> I think this is better phrased as "use of the accept header is optional, however, all DoC clients MUST understand the "application/dns-message" Content-Format".
> Possibly an alternative wording would be clearer.

It's an option in CoAP, not a header, but will do.

## Language Nits
> [[GoFa-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-gofa]] /This document provides no specification on how to map between DoC and
>    DoH, e.g., at a CoAP-to-HTTP-proxy.  In fact, such...
> /This document provides no specification on how to map between DoC and
>    DoH, e.g., at a CoAP-to-HTTP-proxy, such.../
> (To connect the RFC2119 requirement to the clause to which it refers.)

> [[GoFa-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-gofa]] /...this kind of poisoning attacks./...this kind of poisoning attack./
(remove 's').

> #### Section 8
>
> [[ErVy-Comment-10][draft-ietf-core-dns-over-coap-17-ballot-ervy]] s/That information can also imply trust in the DNSSEC validation by that server./That information can also imply trust in the DNSSEC validation by that *DoC* server./

No-Ops
======

## Longer Segments Than Allowed "docpath" MUST NOT be Allowed?
> > [[PaWo-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-pawo]] Paths with longer segments cannot be advertised with the "docpath"
> > SvcParam and are thus NOT RECOMMENDED for the path to the DoC
> > resource.
>
> Should this not be a MUST NOT?

No, longer path segments are possible with DoC (or CoAP for that matter) in general, they are just
not advertisable with the "docpath" SvcParam.

## IoT Devices Can Not Do DNSSEC Validation

> [[PaWo-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-pawo]] Section 8
>
> > A DoC client may not be able to perform DNSSEC validation,
> > e.g., due to code size constraints, or due to the size of
> > the responses. It may trust its DoC server to perform DNSSEC
> > validation;
>
> I can't see an IoT device doing DNSSEC validation. It would have to
> validate a large number of queries in a row to get the validation from
> the DNS root down to the domain. Validation of a single query would
> not contain all the information required, so you either need to implement
> a fully recursive validating DNS server, or support RFC 7901 CHAIN Query
> Requests in both the DoC Server and DoC client. While I think RFC7901 support
> is doable, implementing a fully recursive validating DNS server is not.
> I would rewrite the Section based on these assumptions (eg it has to trust
> the AD bit)

DoC server are not IoT devices. Implementing CHAIN Query Requests is up to the implementation,
nothing in DoC prevents that.

## Add Operational Considerations Section

> #### [[MoBo-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Lack of operation considerations (*)
>
> Other than discovery, the document does not discuss operational considerations that are worth to take into account to ease the deployability of DoC. At least, co-existence considerations (when several transport flavors are supported) should be covered (e.g., preference order). The following additional points may be considered:

### Preference of Different Transport Flavors

> #### [[MoBo-Comment-2][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Lack of operation considerations (*)
>
> Other than discovery, the document does not discuss operational considerations that are worth to take into account to ease the deployability of DoC. At least, co-existence considerations (when several transport flavors are supported) should be covered (e.g., preference order). The following additional points may be considered:

### Add Operational Considerations on Redirection

> ##### [[MoBo-Comment-3][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Redirection (*)
>
> Likewise, do we allow (and thus need to support) server redirection in case of 5.03? We should be explicit about the expected behavior. An example of deployment case where redirection support may be useful is: bootstrapping with a centralized DoC server and then redirect the DoC client to a local DoC server. I’m not pushing for any direction here, but simply providing an example of aspects to cover.

### Provide Operational Considerations on Non-Confirmable vs Confirmable

> ##### [[MoBo-Comment-4][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Confirmable and Non-Confirmable (*)
>
> Should the spec provide more guidance about which mode should be used by default? I guess, the default mode is Confirmable. Right?

### Add Considerations on Hop-Limit Option

> ##### [[MoBo-Comment-5][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Control of CoAP Proxy Hops (*)
>
> The spec mentions CoAP proxies, but I wonder whether it needs to mention the use of Hop-Limit Option to control how queries are propagated or help detect infinite loops due to incorrect proxy configuration.

### How Should DNS Errors Be Signaled to DoC Client?

> ##### [[MoBo-Comment-6][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Troubleshooting (*)
>
> Do we need extra codes to demux DNS-related errors vs. conventional CoAP? For example, CoAP leg may be OK but the upstream DNS query may not be sent out because of a DNS failure. How to report that back to the DoC client?

## Where Does the Destination Address in DDR/DNR Come From

### [[MoBo-Comment-12][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Mysterious destination IP address (*)

> CURRENT:
>    *  The destination address for the request SHOULD be taken from
>       additional information about the target, e.g., from an AAAA resource record
>       associated with the target name or from an "ipv6hint" SvcParam
>
> How we got that resolution as at this stage we don’t have a DNS server to communicate with? Maybe I’m missing something.

The DoC server is either configured by DNS (via SVCB records from another DNS server), DHCP, or RA,
as described in RFC 9462 and RFC 9463. The title of the section “Discovery using SVCB Resource
Records or DNR” says as much. Is there something that needs to be clarified here?

## Remove RFC Editor's Note on I-D.ietf-iotops-7228bis
> ### [[MoBo-Comment-14][draft-ietf-core-dns-over-coap-17-ballot-mobo]] I-D.ietf-iotops-7228bis note
>
> It is unlikely that I-D.ietf-iotops-7228bis will make it before DoC. I suggest you simply delete that RFC Editor note.

This was a recommendation by a reviewer and it does not hurt to have the note there.

## Mention TLS as Security Mechanism

> [[MiBi-Comment-1][draft-ietf-core-dns-over-coap-16-ballot-mibi]] One more item I noticed while preparing the ballot: the abstract and introduction state the use of DTLS and OSCORE for message protection, but TLS is also supported according to the remaining sections. Please either explicitly mention TLS or adjust to "(D)TLS" where appropriate.

Already done in [`d1f0f2f`](https://github.com/core-wg/draft-dns-over-coap/commit/d1f0f2ffb8ce30bc6a91d114e51284d0c4d7d438) (review was on a previous version of the draft).

## Link-layer vs. Link Layer

> [[ErVy-Comment-5][draft-ietf-core-dns-over-coap-17-ballot-ervy]] s/link layer frame sizes/link-layer frame sizes/ ?

As far as I [Martine] know, the common spelling for “[data] link layer” is without hyphen (see also,
e.g., titles of RFCs 935 and 7133), but I let myself convince otherwise (e.g. if the IETF has their
own style guide on that; e.g., RFC 4957 uses hyphens).

## Add Space to `255OCTET`
> #### [[ErVy-Comment-7][draft-ietf-core-dns-over-coap-17-ballot-ervy]] Section 3.2
>
> Should there be a space in `255OCTET`?

This notation was directly taken from [RFC 9460, section 7.1.1](https://datatracker.ietf.org/doc/html/rfc9460#section-7.1.1-2)
to be in line with that definition.

Unclear Issues
==============

## Indicate B2B Entity in Figure 1
> ### [[MoBo-Comment-9][draft-ietf-core-dns-over-coap-17-ballot-mobo]] Back-to-Back DoC Server and Do53/DoT/DoH
>
> There is no mapping frozen in the spec between various DNS transport. It would be cleaner from an architectural standpoint to indicate the B2B entity in Figure 1.

Not sure what is meant with Back-to-Back here.

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
