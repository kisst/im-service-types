---
title: "IM Service Type Registry for vCard"
abbrev: "vCard IM Service Types"
category: std
ipr: trust200902
submissiontype: IETF
docname: draft-kisst-calext-im-service-type-00
date: 2026-04-07
area: ART
workgroup: calext
keyword:
  - vCard
  - instant messaging
  - IMPP
  - SERVICE-TYPE

author:
  - fullname: KissT
    country: CH

normative:
  RFC6350:
  RFC8126:
  RFC9554:

informative:

--- abstract

This document defines an IANA registry for Instant Messaging (IM)
service type tokens for use with the vCard IMPP property and its
SERVICE-TYPE parameter.  The registry provides a canonical,
deduplicated set of identifiers for instant messaging services and
protocols, enabling interoperable exchange of IM contact details
in vCard objects.  An initial set of 106 service type values is
registered.

--- middle

# Introduction

The vCard format {{RFC6350}} defines the IMPP property for
representing instant messaging and presence protocol contact
information.  {{RFC9554}} introduced the SERVICE-TYPE parameter,
which may be applied to both IMPP and SOCIALPROFILE properties to
name the online service associated with a contact address.

However, no registry exists to govern the values used with
SERVICE-TYPE when applied to instant messaging services.  In
practice, implementations have invented their own ad-hoc
identifiers, leading to inconsistency, duplication, and
interoperability failures.  For example, one application may use
"WhatsApp" while another uses "whatsapp" or "wa" for the same
service.

This document establishes an IANA registry of IM service type
tokens.  Each token is a short, lowercase, ASCII string that
uniquely identifies an instant messaging service or protocol.
The registry provides:

- A canonical identifier for each IM service
- A reference URL for the service or its defining specification
- A suggested URI format for addressing contacts on that service
- Registration policies that allow the registry to grow as new
  services emerge

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY",
and "OPTIONAL" in this document are to be interpreted as described
in BCP 14 {{!RFC2119}} {{!RFC8174}} when, and only when, they
appear in all capitals, as shown here.

# Conventions

## Token Format

IM service type tokens registered in the registry defined by this
document MUST conform to the following ABNF {{!RFC5234}}:

~~~
im-service-type = 1*( ALPHA / DIGIT )
~~~

Tokens MUST consist solely of lowercase ASCII letters (a-z) and
digits (0-9).  Tokens MUST NOT contain hyphens, underscores,
spaces, or other punctuation.  This constraint ensures tokens are
unambiguous, easy to compare, and safe for use in URIs and
structured data formats.

Tokens MUST be compared case-insensitively for lookup purposes,
but MUST be stored and transmitted in their registered lowercase
form.

## Usage with IMPP

When the SERVICE-TYPE parameter is used on an IMPP property, its
value SHOULD be a token from the IM Service Type Registry defined
in this document.

The IMPP property value SHOULD be a URI that can be used to
initiate communication via the identified service.  Where no
standard URI scheme exists for a service, an HTTPS URL to the
user's profile or a service-specific deep link MAY be used.

Examples:

~~~
IMPP;SERVICE-TYPE=xmpp:xmpp:alice@example.com
IMPP;SERVICE-TYPE=signal:tel:+15551234567
IMPP;SERVICE-TYPE=matrix:matrix:u/alice:example.com
IMPP;SERVICE-TYPE=telegram:https://t.me/alice
IMPP;SERVICE-TYPE=discord:https://discord.com/users/123456789
IMPP;SERVICE-TYPE=irc:irc://irc.libera.chat/alice
IMPP;SERVICE-TYPE=simplex:https://simplex.chat/contact#...
IMPP;SERVICE-TYPE=threema:https://threema.id/ABCD1234
IMPP;SERVICE-TYPE=whatsapp:https://wa.me/+15551234567
IMPP;SERVICE-TYPE=sip:sip:alice@example.com
~~~

# Service Categories

While the registry itself is a flat list, implementations may
find it useful to understand the general categories of services
represented.  These categories are informational and are not
part of the registry data:

- **Open protocols**: Services based on open, standardized
  protocols (e.g., xmpp, matrix, sip, irc, activitypub)

- **Commercial platforms**: Proprietary messaging services
  (e.g., whatsapp, signal, telegram, discord, slack)

- **Mesh/offline-capable**: Services designed for
  communication without internet infrastructure
  (e.g., meshtastic, briar, bridgefy)

- **Enterprise**: Workplace collaboration platforms
  (e.g., microsoftteams, slack, mattermost, rocketchat)

# Security Considerations

## Privacy

The presence of IMPP properties with SERVICE-TYPE parameters in
a vCard reveals which messaging services a contact uses.  This
metadata may be sensitive, as it can indicate security practices,
political affiliations, or geographic location.  Implementations
SHOULD treat IM service type information with the same privacy
considerations as other contact details.

## Enumeration

A vCard containing multiple IMPP entries with different
SERVICE-TYPE values effectively provides a messaging service
fingerprint of the contact.  Parties handling such vCards SHOULD
be aware that this information could be used for profiling or
correlation across services.

## Service Verification

The registration of a service type token in this registry does
not constitute an endorsement of the service's security
properties.  Implementations MUST NOT infer security guarantees
(such as end-to-end encryption) from the presence of a
SERVICE-TYPE token alone.

## URI Scheme Trust

When no standard URI scheme exists for a service, HTTPS URLs or
service-specific deep links are used as IMPP values.
Implementations MUST validate and sanitize such URIs before use
to prevent injection attacks or unintended navigation.

# IANA Considerations

## IM Service Type Registry

IANA is requested to create a new registry titled "vCard IM
Service Type Tokens" within the "vCard Elements" registry group.

### Registration Policy

New registrations in this registry are subject to Expert Review
{{RFC8126}}.  The designated expert(s) SHOULD verify that:

1. The token conforms to the format defined in Section 2.1.
2. The service or protocol is a real, operational instant
   messaging system or protocol with text-based messaging as
   a primary feature.
3. The entry represents a service or protocol, not a client
   application that connects to another protocol.
4. The token does not duplicate an existing entry (including
   equivalent services under different names).
5. The reference URL points to a legitimate, publicly accessible
   resource describing the service or protocol.
6. Defunct or abandoned services SHOULD be marked as deprecated
   rather than removed, to prevent token reuse.

### Registry Template

Each entry in the registry contains the following fields:

- **Token**: The service type identifier (lowercase ASCII)
- **Name**: Human-readable name of the service
- **Reference URL**: URL to the service or its specification
- **Suggested URI**: Example URI format for addressing users
- **Status**: "active" or "deprecated"
- **Change Controller**: Entity authorized to update the entry

### Initial Registry Contents

The following table contains the initial set of IM service type
tokens.  All entries have Status "active".

| Token | Reference URL | Suggested URI |
|-------|---------------|---------------|
| 2go | https://www.2go.im | (phone-number-based) |
| activitypub | https://www.w3.org/TR/activitypub/ | https://instance/@user |
| adamantmessenger | https://adamant.im | https://msg.adamant.im/?address=U... |
| ayoba | https://www.ayoba.me | (phone-number-based) |
| bale | https://bale.ai | https://bale.ai/username |
| berty | https://berty.tech | berty://id/... |
| bip | https://bip.com | (phone-number-based) |
| bitchat | https://mesh.im | (peer-identity-based) |
| blockscan | https://chat.blockscan.com | (ethereum-address-based) |
| botim | https://botim.me | (phone-number-based) |
| briar | https://briarproject.org | briar://... |
| bridgefy | https://bridgefy.me | (peer-identity-based) |
| brosix | https://www.brosix.com | (instance-specific) |
| cabal | https://cabal.chat | cabal://key |
| campfire | https://once.com/campfire | (instance-specific) |
| chanty | https://www.chanty.com | (instance-specific) |
| chatwork | https://go.chatwork.com | https://www.chatwork.com/#!rid... |
| chitchatter | https://chitchatter.im | https://chitchatter.im/room-id |
| ciscojabber | https://www.cisco.com/c/en/us/products/unified-communications/jabber/index.html | xmpp:user@domain |
| cliq | https://www.zoho.com/cliq/ | (instance-specific) |
| comm | https://comm.app | https://comm.app/invite/... |
| confide | https://getconfide.com | (app-based) |
| cwtch | https://docs.cwtch.im | (tor-onion-based) |
| databag | https://github.com/balzack/databag | (instance-specific) |
| deltachat | https://delta.chat | (email-address-based) |
| didcomm | https://didcomm.org | did:method:identifier |
| dingtalk | https://www.dingtalk.com | dingtalk://dingtalkclient/... |
| discord | https://discord.com | https://discord.com/users/id |
| eitaa | https://eitaa.com | https://eitaa.com/username |
| facebookmessenger | https://www.messenger.com | https://m.me/username |
| feishu | https://www.feishu.cn | feishu://... |
| fleep | https://fleep.io | (instance-specific) |
| flock | https://www.flock.com | (instance-specific) |
| gadugadu | https://gg.pl | gg:user_id |
| gap | https://gap.im | https://gap.im/username |
| garminmessenger | https://www.garmin.com/en-US/p/893837/ | (device-paired) |
| googlechat | https://workspace.google.com/products/chat/ | https://chat.google.com/dm/id |
| googlemessages | https://messages.google.com | tel:+number |
| guilded | https://www.guilded.gg | https://www.guilded.gg/u/username |
| igap | https://igap.net/ | https://igap.net/username |
| imessage | https://support.apple.com/messages | imessage:phone-or-email |
| imo | https://imo.im/ | (phone-number-based) |
| irc | https://datatracker.ietf.org/doc/html/rfc1459 | irc://server/channel or ircs:// |
| jami | https://jami.net/ | jami:username-or-hash |
| jiochat | https://www.jiochat.com/ | (phone-number-based) |
| kakaotalk | https://www.kakaocorp.com/page/service/service/kakaotalk | https://open.kakao.com/o/id |
| katzenpost | https://katzenpost.network/ | (protocol-specific) |
| keet | https://keet.io/ | (invite-key-based) |
| keybase | https://keybase.io/ | https://keybase.io/username |
| kik | https://www.kik.com/ | https://kik.me/username |
| lark | https://www.larksuite.com/ | (instance-specific) |
| line | https://line.me/en/ | https://line.me/ti/p/username |
| lxmf | https://github.com/markqvist/LXMF | lxmf:address |
| matrix | https://matrix.org/ | matrix:u/user:server |
| mattermost | https://mattermost.com/ | https://instance/channels/ch |
| meshtastic | https://meshtastic.org/ | (node-id-based) |
| microsoftteams | https://www.microsoft.com/microsoft-teams/ | msteams:/l/chat/0/0?users=email |
| mixin | https://mixin.one/ | mixin://users/id |
| moyaapp | https://moya.app/ | (phone-number-based) |
| nextcloudtalk | https://nextcloud.com/talk/ | https://instance/call/token |
| nostr | https://nostr.com/ | nostr:npub... |
| olvid | https://olvid.io/ | https://invitation.olvid.io/... |
| potatochat | https://www.potato.im/ | (app-based) |
| pronto | https://pronto.io/ | (instance-specific) |
| pumble | https://pumble.com/ | (instance-specific) |
| qaul | https://qaul.net/ | (peer-identity-based) |
| qq | https://im.qq.com/ | tencent://message/?uin=number |
| quiet | https://tryquiet.org/ | (invite-code-based) |
| rcs | https://www.gsma.com/solutions-and-impact/technologies/networks/rcs/ | tel:+number |
| retroshare | https://retroshare.cc/ | (peer-key-based) |
| ricochetrefresh | https://www.ricochetrefresh.net/ | ricochet:onion-address |
| ringcentral | https://www.ringcentral.com/ | (instance-specific) |
| rocketchat | https://www.rocket.chat/ | https://instance/user/username |
| rubika | https://rubika.ir/ | (app-based) |
| session | https://getsession.org/ | session:session-id |
| signal | https://signal.org/ | https://signal.me/#p/+number |
| silentcircle | https://www.silentcircle.com | (app-based) |
| simple | https://datatracker.ietf.org/wg/simple/about/ | im:user@domain |
| simplex | https://simplex.chat | https://simplex.chat/contact#... |
| sip | https://datatracker.ietf.org/doc/html/rfc3261 | sip:user@domain |
| slack | https://slack.com | https://team.slack.com/ |
| snapchat | https://www.snapchat.com | https://www.snapchat.com/add/user |
| snikket | https://snikket.org | xmpp:user@instance |
| soroush | https://soroushplus.com | (app-based) |
| status | https://status.app | (ethereum-address-based) |
| stealthtalk | https://stealthtalk.com | (app-based) |
| steamfriends | https://store.steampowered.com/chat | https://steamcommunity.com/id/user |
| stoat | https://stoat.chat/ | (instance-specific) |
| symphony | https://symphony.com | (instance-specific) |
| telegram | https://telegram.org | https://t.me/username |
| threema | https://threema.ch | https://threema.id/XXXXXXXX |
| tinode | https://tinode.co | (instance-specific) |
| tox | https://tox.chat | tox:tox-id |
| troopmessenger | https://www.troopmessenger.com | (instance-specific) |
| twinme | https://www.twin.me | (peer-based) |
| twist | https://twist.com | (instance-specific) |
| utopia | https://u.is/en/ | (app-based) |
| veilid | https://veilid.com | (public-key-based) |
| viber | https://www.viber.com | viber://chat?number=+number |
| vkmessenger | https://vk.com/messenger | https://vk.me/username |
| vkteams | https://biz.mail.ru/teams | (instance-specific) |
| waku | https://waku.org | (protocol-specific) |
| webexmessaging | https://www.webex.com/team-collaboration.html | (instance-specific) |
| wechat | https://www.wechat.com | weixin://dl/chat?username |
| wecom | https://work.weixin.qq.com | (enterprise-specific) |
| whatsapp | https://www.whatsapp.com | https://wa.me/+number |
| wickr | https://aws.amazon.com/wickr/ | (enterprise-specific) |
| wire | https://wire.com | (app-based) |
| xmpp | https://xmpp.org | xmpp:user@domain |
| xmtp | https://xmtp.org | https://xmtp.chat/dm/0x... |
| yalla | https://www.yalla.com | (app-based) |
| yandexmessenger | https://360.yandex.com/business/messenger/ | (enterprise-specific) |
| zalo | https://zalo.me | https://zalo.me/username |
| zangi | https://zangi.com | (app-based) |
| zoommessaging | https://www.zoom.com/en/products/team-chat/ | (instance-specific) |
| zulip | https://zulip.com | https://instance/#narrow/dm/user |

--- back

# Acknowledgements

The initial service list was compiled as a community effort to
create a comprehensive, deduplicated inventory of instant
messaging services and protocols.

# Change Log

## draft-kisst-calext-im-service-type-00

- Initial version
- Validated all entries: removed clients, defunct services,
  non-IM platforms, and hardware devices
- Added suggested URI formats for each service
