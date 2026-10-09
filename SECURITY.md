# Security Policy

## Security maintainer

Belgelik has a single security maintainer, Alpaslan Fatih Sözer
(GitHub [@afsozer](https://github.com/afsozer)), who receives every report,
decides on fixes and publishes security advisories.

## Reporting a vulnerability

Please do not open a public issue for a security problem. Report it privately
through GitHub's private vulnerability reporting: open the repository's
**Security** tab and choose **Report a vulnerability**, or go directly to
<https://github.com/afsozer/belgelik/security/advisories/new>.

If you cannot use GitHub, email bilgi@avfatihsozer.com with a subject line
that starts with `[SECURITY] belgelik`.

A useful report names the affected version or commit, says which part is
affected (the Flutter client on which platform, or the FastAPI server), lists
the steps to reproduce the problem and explains what an attacker could achieve
with it. A
working exploit is not required; a clear description is enough.

## What to expect

- Your report is acknowledged within 3 business days.
- An initial assessment, saying whether the issue is accepted and how severe it
  is, follows within 10 business days.
- Accepted issues are fixed or mitigated as soon as practical, with a target of
  30 days for critical and high-severity issues.
- Disclosure is coordinated: once a fix is released, a GitHub Security Advisory
  is published and, where the issue qualifies, a CVE is requested through
  GitHub. Reporters are credited unless they ask not to be. If a fix takes
  longer, the advisory is published no later than 90 days after the report
  unless we agree on a different date.

## Supported versions

Security fixes are made on the latest release line.

| Version | Supported |
| ------- | --------- |
| 1.14.x  | Yes       |
| < 1.14  | No        |

## Scope

In scope is the code in this repository: the Flutter client, the FastAPI server
and its API, synchronisation between devices, the backup and restore tools, and
the legislation import pipeline. Belgelik is meant to run on a private network;
a way to reach the server's data or files from outside that network is treated
as a security issue.

Out of scope:

- the accuracy or completeness of legislation texts, which is a data-quality
  question rather than a security one (please open a normal issue);
- vulnerabilities in Dayanak (formerly Emsal MCP), which Belgelik uses to fetch legislation; please
  report those under that project's own security policy;
- publicly known vulnerabilities in third-party dependencies, unless Belgelik
  uses the dependency in a way that makes them exploitable;
- installations run by other people.

## Safe harbour

Good-faith research that follows this policy is welcome. Test only against your
own installation, do not access data that is not yours, and do not degrade
services that others rely on, including the official legislation sources.
Research carried out this way will not be the subject of legal action by the
maintainer.

## Türkçe özet

Güvenlik açıklarını herkese açık issue olarak değil, deponun **Security**
sekmesindeki **Report a vulnerability** bağlantısıyla ya da konu satırı
`[SECURITY] belgelik` ile başlayan bir e-postayla bilgi@avfatihsozer.com
adresine bildirin. Bildirimler 3 iş günü içinde yanıtlanır; düzeltme
yayımlandıktan sonra GitHub güvenlik duyurusu çıkar ve uygunsa CVE istenir.
Testleri yalnızca kendi kurulumunuzda yapın.
