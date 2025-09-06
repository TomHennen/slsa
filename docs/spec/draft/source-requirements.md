---
title: "Source: Requirements for producing source"
description: This page covers the detailed technical requirements for producing producing Source Revisions at each SLSA level. The intended audience is Source Control System implementers and security engineers.
---

## Objective

The primary purpose of the SLSA Source track is to provide producers and consumers with increasing levels of trust in the source code they produce and consume.
It describes increasing levels of trustworthiness and completeness of how a Source Revision was created.

The expected process for creating a new Source Revision is determined solely by that repository's owner (the Organization) who also determines the intent of the software in the repository and administers technical controls to enforce the process.

Consumers can review attestations to verify whether a particular Source Revision meets their standards.

## Definitions

A **Version Control System (VCS)** is a system of software and protocols for
managing the version history of a set of files. Git, Mercurial, and Subversion
are all examples of Version Control Systems.

The following terms apply to Version Control Systems:

| Term | Description
| --- | ---
| Source Repository (Repo) | A self-contained unit that holds the content and revision history for a set of files, along with related metadata like Branches and Tags.
| Source Revision | A specific, logically immutable snapshot of the repository's tracked files. It is uniquely identified by a revision identifier, such as a cryptographic hash like a Git commit SHA or a path-qualified sequential number like `25@trunk/` in SVN. A Source Revision includes both the content (the files) and its associated version control metadata, such as the author, timestamp, and parent revision(s). Note: Path qualification is needed for Version Control Systems that use represent Branches and Tags using paths, such as Subversion and Perforce.
| Named Reference | A user-friendly name for a specific Source Revision, such as `main` or `v1.2.3`.
| Change | A modification to the state of the Source Repository, such as creation of a new Source Revision based on a previous Source Revision, or creation, deletion, or modification of a Named Reference.
| Change History | A record of the history of Source Revisions that preceded a specific revision.
| Branch | A Named Reference that moves to track the Change History of a cohesive line of development within a Source Repository. E.g. `main`, `develop`, `feature-x`
| Tag | A Named Reference that is intended to be immutable. Once created, it is not moved to point to a different revision. E.g. `v1.2.3`, `release-20250722`

> **NOTE:** The 'branch' and 'tag' features within Version Control Systems may
not always align with the ‘Branch’ and ‘Tag’ definitions provided in this
specification. For example, in git and other Version Control Systems, the UX may
allow 'tags' to be moved. For the purposes of this specification these would be
classified as 'Named References' and not as 'Tags'.

A **Source Control System (SCS)** is a platform or combination of services
(self-hosted or SaaS) that hosts a Source Repository and provides a trusted
foundation for managing Source Revisions by enforcing policies for
authentication, authorization, and change management, such as mandatory code
reviews or passing status checks.

The following terms apply to Source Control Systems:

| Term | Description
| --- | ---
| Organization | A set of people who collectively create Source Revisions within a Source Repository. Examples of Organizations include open-source projects, a company, or a team within a company. The Organization defines the goals of a Source Repository and the methods used to produce new Source Revisions.
| Proposed Change | A proposal to make a Change in a Source Repository.
| Propose | When an actor uploads a Proposed Change, making it available to Review, Approve, or Submit.
| Review | When an actor considers and comments upon a Proposed Change.
| Approve | When an actor endorses a Proposed Change.
| Submit | When an actor applies a Proposed Change to the repository, making it a Change.
| Source Provenance | Information about how a Source Revision came to exist, where it was hosted, when it was generated, what process was used, who the contributors were, and which parent revisions preceded it.

### Source Roles

| Role | Description
| --- | ---
| Administrator | A human who can perform privileged operations on one or more projects. Privileged actions include, but are not limited to, modifying the Change History and modifying project- or Organization-wide security policies.
| Trusted person | A human who is authorized by the Organization to propose and approve changes to the source.
| Trusted robot | Automation authorized by the Organization to act in explicitly defined contexts. The Robot’s identity and codebase cannot be unilaterally influenced.
| Untrusted person | A human who has limited access to the project. They MAY be able to read the source. They MAY be able to propose or review changes to the source. They MAY NOT approve changes to the source or perform any privileged actions on the project.

## Onboarding

When onboarding a Branch to the SLSA Source Track or increasing the level of
that Branch, Organizations are making claims about how the Branch is managed
from that Source Revision forward. This establishes [continuity](#continuity).

No claims are made for prior Source Revisions.

## Basics

NOTE: This table presents a simplified view of the requirements. See the
[Requirements](#requirements) section for the full list of requirements for each
level.

| Track/Level | Requirements | Focus
| ----------- | ------------ | -----
| [Source L1](#source-l1)  | Use a Version Control System | First steps towards operational maturity
| [Source L2](#source-l2)  | History and controls for protected Branches & Tags | Preserve history and ensure the process has been followed
| [Source L3](#source-l3)  | Signed provenance | Tampering by the Source Control System
| [Source L4](#source-l4)  | Code review | Tampering by project contributors

<section id="source-l1">

### Level 1: Version controlled

<dl class="as-table">
<dt>Summary<dd>

The source is stored and managed through a modern Version Control System.

<dt>Intended for<dd>

Organizations currently storing source in non-standard ways who want to quickly gain some benefits of SLSA and better integrate with the SLSA ecosystem with minimal impact to their current workflows.

<dt>Benefits<dd>

Migrating to the appropriate tools is an important first step on the road to operational maturity.

</dl>
</section>
<section id="source-l2">

### Level 2: Controls

<dl class="as-table">
<dt>Summary<dd>

Clarifies which Branches and Tags in a repo are consumable and guarantees that
all Changes to protected Branches and Tags are recorded and subject to the
Organization's technical controls.

<dt>Intended for<dd>

All Organizations of any size producing software of any kind.

<dt>Benefits<dd>

Allows Organizations and source consumers the ability to ensure the change
management process has been followed to track changes to the software over time
and attribute those changes to the actors that made them.

</dl>
</section>
<section id="source-l3">

### Level 3: Signed and Auditable Provenance

<dl class="as-table">
<dt>Summary<dd>

The SCS generates credible, tamper-resistant, and contemporaneous evidence of how a specific Source Revision was created.
It is provided to authorized users of the Source Repository in a documented format.

<dt>Intended for<dd>

Organizations that want strong guarantees and auditability of their change management processes.

<dt>Benefits<dd>

Provides information to policy enforcement tools to reduce the risk of tampering
within the SCS's storage systems.

</dl>
</section>
<section id="source-l4">

### Level 4: Two-party review

<dl class="as-table">
<dt>Summary<dd>

The SCS requires two Trusted persons to review all Changes to protected
Branches.

<dt>Intended for<dd>

Organizations that want strong guarantees that the software they produce is not
subject to unilateral Changes that would subvert their intent.

<dt>Benefits<dd>

Makes it harder for an actor to introduce malicious Changes into the software
and makes it more likely that the source reflects the intent of the
Organization.

</dl>
</section>

## Requirements

Many examples in this document use the [git Version Control System](https://git-scm.com/), but use of git is not a requirement to meet any level on the SLSA source track.

### Organization

[Organization]: #organization

<table>
<tr><th>Requirement<th>Description<th>L1<th>L2<th>L3<th>L4

<tr id="choose-scs"><td>Choose an appropriate Source Control System <a href="#choose-scs">🔗</a><td>

An Organization producing Source Revisions MUST select a SCS capable of reaching
their desired SLSA Source Level.

> For example, if an Organization wishes to produce Source Revisions at Source Level 3,
they MUST choose a Source Control System capable of producing Source Level 3
attestations.

<td>✓<td>✓<td>✓<td>✓

<tr id="protect-consumable-branches-and-tags"><td>Protect consumable Branches and Tags <a href="#protect-consumable-branches-and-tags">🔗</a><td>

An Organization producing Source Revisions MUST implement a change management
process to ensure Changes to source matches the Organization's intent.

The Organization MUST specify which Branches and Tags are covered by the process
and are intended for use in its own applications or services or those of
downstream consumers of the software.

> For example, if an Organization has Branches 'main' and 'experimental' and it
intends for 'main' to be protected then it MUST indicate to the SCS that 'main'
should be protected. From that point forward Source Revisions on 'main' will be
eligible for Source Level 2+ while Source Revisions made solely on 'experimental' will
not.

The Organization MUST use the SCS provided
[Identity Management capability](#identity-management) to configure the actors
and roles that are allowed to perform sensitive actions on protected Branches
and Tags.

> For example, an Organization may configure the SCS to assign users to a `maintainers` role and only allow users in `maintainers` to make updates to `main`.

The Organization MUST specify what technical controls consumers can expect to be
enforced for Source Revisions in each protected Branch and Tag using the
[Enforced change management process](#enforced-change-management-process)
and it MUST document the meaning of those controls.

> For example, an Organization may claim that Source Revisions on `main` passed unit
tests before being accepted.  The Organization could then configure the SCS to
enforce this requirement and store corresponding [test result attestations] for
all affected Source Revisions.  They may then embed the `ORG_SOURCE_UNIT_TESTED`
property in the [Source VSA](#source-verification-summary-attestation). Consumers
would then expect that future Source Revisions on `main` have been united tested and
determine if that expectation has been met by looking for the
`ORG_SOURCE_UNIT_TESTED` property in the VSAs and, if desired, consult the
[test result attestations] as well.

[test result attestations]: https://github.com/in-toto/attestation/blob/main/spec/predicates/test-result.md

<td><td>✓<td>✓<td>✓

<tr id="safe-expunging-process"><td>Safe Expunging Process <a href="#safe-expunging-process">🔗</a><td>

SCSs MAY allow the Organization to expunge (remove) content from a repository and its Change History without leaving a public record of the removed content,
but the Organization MUST only allow these Changes in order to meet legal or privacy compliance requirements.
Content changed under this process includes changing files, history, references, or any other metadata stored by the SCS.

#### Warning

Removing a Source Revision from a repository is similar to deleting a package version from a registry: it's almost impossible to estimate the amount of downstream supply chain impact.
> For example, in Git, each revision ID is based on the ones before it. When you remove a Source Revision, you must generate new Source Revisions (and new revision IDs) for any Source Revisions that were built on top of it. Consumers who took a dependency on the old Source Revisions may now be unable to refer to the Source Revision they've already integrated into their products.

It may be the case that the specific set of Changes targeted by a legal takedown can be expunged in ways that do not impact consumed Source Revisions, which can mitigate these problems.

It is also the case that removing content from a repository won't necessarily remove it everywhere.
The content may still exist in other copies of the repository, either in backups or on developer machines.

#### Process

An Organization MUST document the Safe Expunging Process and describe how requests and actions are tracked and SHOULD log the fact that content was removed.
Different Organizations and tech stacks may have different approaches to the problem.

SCSs SHOULD have technical mechanisms in place which require an Administrator plus, at least, one additional 'Trusted person' to trigger any expunging (removals) made under this process.

The application of the safe expunging process and the resulting logs MAY be private to both prevent calling attention to potentially sensitive data (e.g. PII) or to comply with local laws
and regulations which may require the Change to be kept private to the extent possible.
Organizations SHOULD prefer to make logs public if possible.

<td><td>✓<td>✓<td>✓

</table>

### Source Control System

<table>
<tr><th>Requirement<th>Description<th>L1<th>L2<th>L3<th>L4

<tr id="repository-ids"><td>Repositories are uniquely identifiable <a href="#repository-ids">🔗</a><td>

The repository ID is defined by the SCS and MUST be uniquely identifiable within
the context of the SCS with a stable locator, such as a URI.

<td>✓<td>✓<td>✓<td>✓
<tr id="revision-ids"><td>Revisions are immutable and uniquely identifiable <a href="#revision-ids">🔗</a><td>
The Source Revision ID is defined by the SCS and MUST be uniquely identifiable within the context of the repository.
When the Source Revision ID is a digest of the content of the Source Revision (as in git) nothing more is needed.
When the Source Revision ID is a number or otherwise not a digest, then the SCS MUST document how the immutability of the Source Revision is established.
The same Source Revision ID MAY be present in multiple repositories.

See also [Use cases for non-cryptographic, immutable, digests](https://github.com/in-toto/attestation/blob/main/spec/v1/digest_set.md#use-cases-for-non-cryptographic-immutable-digests).

<td>✓<td>✓<td>✓<td>✓
<tr id="source-summary"><td>Source Verification Summary Attestations <a href="#source-summary">🔗</a><td>

The SCS MUST generate a
[Source Verification Summary Attestation](#source-verification-summary-attestation) (Source VSA)
to indicate the SLSA Source Level of any Source Revision at Level 1 or above.

If a consumer is authorized to access a Source Revision, they MUST be able to fetch the
corresponding Source VSA.

If the SCS DOES NOT generate a VSA for a Source Revision, the Source Revision has Source Level
0.

At Source Levels 1 and 2 the SCS MAY issue these attestations based on its
understanding of the underlying system (e.g. based on design docs, security
reviews, etc...), but at Level 3+ the SCS MUST use
the SCS issued [Source Provenance](#source-provenance) when making the issuing
the VSAs.

<td>✓<td>✓<td>✓<td>✓
<tr id="branches"><td>Protected Branches <a href="#branches">🔗</a><td>

The SCS MUST provide a mechanism for Organizations to indicate which Branches
should be protected by SLSA Source Level 2+ requirements.

E.g. The Organization may configure the SCS to protect `main` and
`refs/heads/releases/*`, but not `refs/heads/playground/*`.

<td><td>✓<td>✓<td>✓
<tr id="history"><td>History <a href="#history">🔗</a><td>

Source Revisions are created by applying specific code Changes (a "diff" in git) on
top of earlier Source Revisions of a Branch. This sequence of Changes, the Source Revisions
they produced, and how they were introduced into a Branch constitute the history
of that Branch.

The SCS MUST record the sequence of Changes, the Source Revisions they created,
the actors that introduced them and the context they were introduced into.

The SCS MUST prevent tampering with these records on protected Branches.

> For example, in systems like GitHub or GitLab, this can be accomplished by
enabling branch protection rules that prevent force pushes and branch deletions.

<td><td>✓<td>✓<td>✓
<tr id="enforced-change-management-process"><td>Enforced change management process <a href="#enforced-change-management-process">🔗</a><td>

The SCS MUST

-   Ensure Organization-defined technical controls are enforced for Changes made
   to protected Branches.
-   Allow Organizations to specify
   [additional properties](#additional-properties) to be included in the
   [Source VSA](#source-verification-summary-attestation) when the corresponding controls are
   enforced.
-   Allow Organizations to distribute additional attestations related to their
   technical controls to consumers authorized to access the corresponding source
   Source Revision.
-   Prevent Organization-specified properties from beginning with any value
   other than `ORG_SOURCE_` unless the SCS endorses the veracity of the
   corresponding claims.

> For example: enforcement of the Organization-defined technical controls could
be accomplished by the configuration of branch protection rules (e.g.
[GitHub](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets),
[GitLab](https://docs.gitlab.com/ee/user/project/repository/branches/protected.html))
which require additional checks to 'pass' (e.g. unit tests, linters) or the
application and verification of [gittuf](https://github.com/gittuf/gittuf)
policies.

<td><td>✓<td>✓<td>✓
<tr id="continuity"><td>Continuity <a href="#continuity">🔗</a><td>

In a Source Control System, each new Source Revision is built on top of prior
Source Revisions. Controls (e.g. [History](#history) or
[enforced change management process](#enforced-change-management-process)) are
only effective if they are used continuously from one Source Revision to another. If
a control is disabled for the introduction of a new Source Revision and then re-enabled
it is difficult to reason about the effectiveness of the control. 'Continuity' is
the concept of ensuring controls are enforced continuously from the time they
were introduced, leading to a higher degree of trust in the Source Revisions produced
after their introduction.

On [protected Branches](#branches) continuity for [History](#history) and
[enforced change management process](#enforced-change-management-process)
controls MUST be established and tracked from a specific Source Revision forward
through each new Source Revision created. If there is a lapse in continuity for a
specific control, continuity of that control MUST be re-established from a new
Source Revision.

Continuity exceptions are allowed via the [safe expunging process](#safe-expunging-process).

<td><td>✓<td>✓<td>✓
<tr id="protected-tags"><td>Protected Tags <a href="#protected-tags">🔗</a><td>

If the SCS supports Tags (or other non-Branch revision trackers), additional
care must be taken to prevent unintentional Changes.
Unlike Branches, Tags have no built-in continuity enforcement mechanisms or
change management processes.

The SCS MUST provide a mechanism for Organizations to indicate which Tags should
be protected by SLSA Source Level 2+ requirements.

The SCS MUST prevent protected Tags from being moved or deleted.

<td><td>✓<td>✓<td>✓
<tr id="identity-management"><td>Identity Management <a href="#identity-management">🔗</a><td>

The SCS MUST provide an identity management system or some other means of
identifying and authenticating actors.

The SCS MUST allow Organizations to specify which actors and roles are allowed
to perform sensitive actions within a repository (e.g. creation or updates of
Branches, approval of Changes).

Depending on the SCS, identity management may be provided by Source Control
services (e.g., GitHub, GitLab), implemented using cryptographic signatures
(e.g., using gittuf to manage public keys for actors), or extend existing
authentication systems used by the Organization (e.g., Active Directory, Okta,
etc.).

The SCS MUST document how actors are identified for the purposes of attribution.

Activities conducted on the SCS SHOULD be attributed to authenticated
identities.

<td><td>✓<td>✓<td>✓
<tr id="source-provenance"><td>Source Provenance <a href="#source-provenance">🔗</a><td>

[Source Provenance](#source-provenance-attestations) are attestations that
contain information about how a specific Source Revision was created and how it came to
exist on a protected Branch or how a Tag came to point at it. They are
associated with the Source Revision identifier delivered to consumers and are a
statement of fact from the perspective of the SCS. The SCS MUST document the
format and intent of all Source Provenance attestations it produces.

At Source Level 3, Source Provenance MUST be created contemporaneously with the
Branch being updated to use that Source Revision such that they provide a credible,
auditable, record of Changes.

If a consumer is authorized to access a Source Revision, they MUST be able to fetch the
corresponding Source Provenance documents for that Source Revision.

It is possible that an SCS can make no claims about a particular Source Revision.

> For example, this would happen if the Source Revision was created on another SCS,
or if the Source Revision was not the result of an accepted change management process.

<td><td><td>✓<td>✓
<tr id="two-party-review"><td>Two party review <a href="#two-party-review">🔗</a><td>

Changes in protected Branches MUST be agreed to by two or more Trusted persons prior to submission.
The following combinations are acceptable:

-   Uploader and reviewer are two different Trusted persons.
-   Two different reviewers are Trusted persons.

Reviews SHOULD cover, at least, security relevant properties of the code.

**[Final revision approved]** This requirement applies to the final Source Revision
submitted. I.e. if additional Changes are made during the review process, those Changes MUST
be reviewed as well.

**[Context-specific approvals]** Approvals are for a specific context, such as a
repo + Branch in git. Moving fully reviewed content from one context to another
still requires Review. The exact definition of “context” depends on the project,
and this does not preclude well-understood automatic merges, such as cutting a release Branch.

**[Informed Review]** The SCS MUST provide a human readable diff of all
plain-text Changes being reviewed and SHOULD provide mechanisms to provide human
understandable interpretations of non-plain-text Changes (e.g. render images,
verify and display provenance for binaries, etc...).

**[Trusted Robot Contributions]** An Organization MAY choose to grant a Trusted
Robot a perpetual exception to a policy (e.g. a bot may be able to merge a Change
that has not been reviewed by two parties).

Examples:

-   Import and migration bots that move code from one repo to another.
-   Dependabot

<td><td><td><td>✓
</table>

## Communicating source levels

SLSA source level details are communicated using attestations.
These attestations either refer to a Source Revision itself or provide context needed to evaluate an attestation that _does_ refer to a Source Revision.

There are two broad categories of source attestations within the source track:

1.  Source Verification Summary Attestations (Source VSAs): Used to communicate to downstream users what high level security properties a given Source Revision meets.
2.  Source Provenance attestations: Provide trustworthy, tamper-proof, metadata with the necessary information to determine what high level security properties a given Source Revision has.

To provide interoperability and ensure ease of use, it's essential that the Source VSAs are applicable across all Source Control Systems.
However, due to the significant differences in how SCSs operate and how they may chose to meet the Source Track requirements, it is preferable to
allow for flexibility with the full Source Provenance attestations. To that end, SLSA leaves Source Provenance attestations undefined and up to the SCSs to determine
what works best in their environment.

### Source verification summary attestation

Source Verification Summary Attestations (Source VSAs) are issued by some authority that has sufficient evidence to make the determination of a given
Source Revision's source level.  Source VSAs convey properties about the Source Revision as a whole and summarize properties computed over all
the Changes that contributed to that Source Revision over its history.

The source track issues Source VSAs using the [Verification Summary Attestations](./verification_summary.md) format as follows:

1.  `subject.uri` SHOULD be set to a URI where a human can find details about
    the Source Revision. This field is not intended for policy decisions. Instead, it
    is only intended to direct a human investigating verification failures.
    -   For example: `https://github.com/slsa-framework/slsa/commit/6ff3cd75c8c9e0fcedc62bd6a79cf006f185cedb`
2.  `subject.digest` MUST include the Source Revision identifier (e.g. `gitCommit`) and MAY include other digests over the contents of the Source Revision (e.g. `gitTree`, `dirHash`, etc...).
SCSs that do not use cryptographic digests MUST define a canonical type that is used to identify immutable Source Revisions and MUST include the repository within the type[^1].
    -   For example: `svn_revision_id: svn+https://svn.myproject.org/svn/MyProject/trunk@2019`
3.  `subject.annotations.sourceRefs` SHOULD be set to a list of references that pointed to this Source Revision when the attestation was created. The list MAY be non-exhaustive.
    -   git references MUST be fully qualified (e.g. `refs/head/main` or `refs/tags/v1.0`) to reduce the likelihood of confusing downstream tooling.
4.  `resourceUri` MUST be set to the URI of the repository, preferably using [SPDX Download Location](https://spdx.github.io/spdx-spec/v2.3/package-information/#77-package-download-location-field).
E.g. `git+https://github.com/foo/hello-world`.
5.  `verifiedLevels` MUST include the SLSA source track level the SCS asserts the Source Revision meets. One of `SLSA_SOURCE_LEVEL_0`, `SLSA_SOURCE_LEVEL_1`, `SLSA_SOURCE_LEVEL_2`, `SLSA_SOURCE_LEVEL_3`.
MAY include additional properties as asserted by the SCS.  The SCS MUST include _only_ the highest SLSA source level met by the Source Revision.
6.  `dependencyLevels` MAY be empty as Source Revisions are typically terminal nodes in a supply chain. For example, this could be used to indicate the source level of any git submodules present in the Source Revision.

#### Additional properties

The SLSA source track MAY create additional properties to include in
`verifiedLevels` which attest to other claims concerning a Source Revision (e.g. if it
was code reviewed).

The SCS MAY embed Organization-provided properties within `verifiedLevels`
corresponding to technical controls enforced by the SCS. If such properties are
provided they MUST be prefixed with `ORG_SOURCE_` to distinguish them from other
properties the SCS may wish to use.

-   `ORG_SOURCE_` to indicate a property that is meant for consumption by
   external consumers.
-   `ORG_SOURCE_INTERNAL_` to indicate a property that is not meant for
   consumption by external consumers.

The meaning of the properties is left entirely to the Organization. Inclusion of
Organization-provided properties within `verifiedLevels` SHOULD NOT be
considered an endorsement of the veracity of the Organization defined property
by the SCS.

#### Populating sourceRefs

The Source VSA issuer may choose to populate `sourceRefs` in any way they wish.
Downstream users are expected to be familiar with the method used by the issuer.

Example implementations:

-   Issue a new VSA for each merged Pull Request and add the destination Branch to `sourceRefs`.
-   Issue a new VSA each time a 'consumable Branch' is updated to point to a new Source Revision.
-   Issue a new VSA each time a 'consumable Tag' is created to point to a new Source Revision.

#### Example

```json
"_type": "https://in-toto.io/Statement/v1",
"subject": [{
  "uri": "https://github.com/foo/hello-world/commit/9a04d1ee393b5be2773b1ce204f61fe0fd02366a",
  "digest": {"gitCommit": "9a04d1ee393b5be2773b1ce204f61fe0fd02366a"},
  "annotations": {"sourceRefs": ["refs/heads/main", "refs/heads/release_1.0"]}
}],

"predicateType": "https://slsa.dev/verification_summary/v1",
"predicate": {
  "verifier": {
    "id": "https://example.com/source_verifier",
  },
  "timeVerified": "1985-04-12T23:20:50.52Z",
  "resourceUri": "git+https://github.com/foo/hello-world",
  "policy": {
    "uri": "https://example.com/slsa_source.policy",
  },
  "verificationResult": "PASSED",
  "verifiedLevels": ["SLSA_SOURCE_LEVEL_3"],
}
```

#### How to verify

-   VSAs for Source Revisions MUST follow [the standard method of VSA verification](./verification_summary.md#how-to-verify).
-   Users SHOULD check that an allowed Branch is listed in `subject.annotations.sourceRefs` to ensure the Source Revision is from an appropriate context within the repository.
-   Users SHOULD check that the expected `SLSA_SOURCE_LEVEL_` is listed within `verifiedLevels`.
-   Users MUST ignore any unrecognized values in `verifiedLevels`.

### Source provenance attestations

Source Provenance attestations provide tamper-proof evidence ([attestation model](attestation-model)))
that can be used to determine what SLSA Source Level or other high level properties a given Source Revision meets.
This evidence can be used by:

-   an authority as the basis for issuing a [Source VSA](#source-verification-summary-attestation)
-   a consumer to cross-check a [Source VSA](#source-verification-summary-attestation) they received for a Source Revision
-   a consumer to enforce a more detailed policy than the Organization's change management process

SCSs may have different methods of operating that necessitate different forms of evidence.
E.g. GitHub-based workflows may need different evidence than Gerrit-based workflows, which would both likely be different from workflows that
operate over Subversion repositories.

These differences also mean that, depending on the configuration, the issuers of provenance attestations may vary from implementation to implementation, often because entities with the knowledge to issue them may vary.
The authority that issues [Source VSAs](#source-verification-summary-attestation) MUST understand which entity should issue each provenance attestation type, and ensure all Source Provenance attestations come from their expected issuers.

'Source Provenance attestations' is a generic term used to refer to any type of attestation that provides evidence the process used to create a Source Revision.

Example Source Provenance attestations:

-   A TBD attestation which describes the Source Revision's parents and the actors involved in creating this Source Revision.
-   A "code review" attestation which describes the basics of any code review that took place.
-   An "authentication" attestation which describes how the actors involved in any Source Revision were authenticated.
-   A [Vuln Scan attestation](https://github.com/in-toto/attestation/blob/main/spec/predicates/vuln.md)
    which describes the results of a vulnerability scan over the contents of the Source Revision.
-   A [Test Results attestation](https://github.com/in-toto/attestation/blob/main/spec/predicates/test-result.md)
 which describes the results of any tests run on the Source Revision.
-   An [SPDX attestation](https://github.com/in-toto/attestation/blob/main/spec/predicates/spdx.md)
 which provides a software bill of materials for the Source Revision.
-   A [SCAI attestation](https://github.com/in-toto/attestation/blob/main/spec/predicates/scai.md) used to
 describe which source quality tools were run on the Source Revision.

Irrespective of the types of provenance attestations generated by an SCS and
their implementations, the SCS MUST document provenance
formats, and how each provenance attestation can be used to reason about the
Source Revision's properties recorded in the summary attestation.

[^1]: in-toto attestations allow non-cryptographic digest types: https://github.com/in-toto/attestation/blob/main/spec/v1/digest_set.md#supported-algorithms.

## Potential Change Management Controls

In addition to the requirements for SLSA Source L4, most Organizations will
require multiple of these controls as part of their required protections.

If an Organization has indicated that use of these these controls are part of
their repository's expectations, consumers SHOULD be able to verify that the
process was followed for the Source Revision they are consuming by examining the
[summary](#source-verification-summary-attestation) or [Source
Provenance](#source-provenance-attestations) attestations.

> For example: consumers can look for the related `ORG_SOURCE` properties in the
`verifiedLevels` field of the [summary attestation](#source-verification-summary-attestation).

### Expert Code Review

Summary: All Changes to the source are pre-approved by experts in those areas.

Intended for: Enterprise repositories and mature open source projects.

Benefits: Prevents mistakes made by developers who are unfamiliar with the area.

Requirements:

#### Code ownership

Each part of the source MUST have a clearly identified set of experts.

#### Approvals from all relevant experts

For each portion of the source modified by a Change proposal, pre-approval MUST be granted by a member of the defined expert set.
An approval from an actor that is a member of multiple expert groups may satisfy the requirement for all groups in which they are a member.

### Review Every Single Revision

Summary: The final Source Revision was reviewed by all relevant experts prior to submission.

Intended for: The highest-of-high-security-posture repos.

Benefits: Provides the maximum chance for experts to spot and reject problems before they ship.

Requirements:

#### Reset votes on all changes

If the Proposed Change is modified after receiving expert approval, all previously granted approvals MUST be revoked.
A new approval MUST be granted from ALL required reviewers.

The new approval MAY be granted by an actor who approved a previous iteration.

### Automated testing

Summary:
The final Source Revision was validated against a suite of automated tests.

Intended for:
All Organizations and repositories.

Benefits:
Automatic testing has many benefits, including improved accuracy, error prevention and reduced workload on human developers.

Requirements:
The Organization MUST configure a Branch protection rule to require that only Source Revisions with passing test results can be pointed-to by the Branch.

Automatic tests SHOULD be executed in a trustworthy environment (see SLSA build track).

Results of each test (or an aggregate) MUST be collected by the change review tool and made available for verification.

Tests SHOULD be run against a Source Revision created for testing by merging the topic Branch (containing the proposed Changes) into the target Branch.

Use of the proposed merge commit should be preferred to using the tip of the topic Branch.

### Every revision reachable from a branch was approved

Summary:
New Source Revisions are created based ONLY on the Changes that were approved.

Benefits:
Prevents a large class of internal threat attacks based on hiding a malicious commit in a series of good commits such that the malicious commit does not appear in the reviewed diff.

Requirements:

#### Context

In many Organizations it is normal to review only the "net difference" between the tip of  the topic Branch and the "best merge base", the closest shared commit between the topic and target Branches computed at the time of review.

The topic Branch may contain many commits of which not all were intended to represent a shippable state of the repository.

If a repository merges Branches with a standard merge commit, all those unreviewed commits on the topic Branch will become "reachable" from the protected Branch by virtue of the multi-parent merge commit.

When a repo is cloned, all commits _reachable_ from the main Branch are fetched and become accessible from the local checkout.

This combination of factors allows attacks where the victim performs a `git clone` operation followed by a `git reset --hard <unreviewed revision id>`.

#### Mitigations

##### Informed Review

The reviewer is able and encouraged to make an informed decision about what they're approving.
The reviewer MUST be presented with a full, meaningful content diff between the proposed Source Revision and the previously reviewed Source Revision.

It is not sufficient to indicate that a file changed without showing the contents.

##### Use only rebase operations on the protected branch

Require a squash merge strategy for the protected Branch.

To guarantee that only commits representing reviewed diffs are cloned, the SCS MUST rebase (or "squash") the reviewed diff into a single new commit (the "squashed" commit) that has only a single parent (the Source Revision previously pointed-to by the protected Branch).
This is different than a standard merge commit strategy which would cause all the user-contributed commits to become reachable from the protected Branch via the second parent.

It is not acceptable to replay the sequence of commits from the topic Branch onto the protected Branch.
The intent is to reduce the accepted Changes to the exact diffs that were reviewed.
Constituent commits of the topic Branch may or may not have been reviewed on an individual basis, and should not become reachable from the protected Branch.

### Immutable Change Discussion

Summary:
The discussion around a Change may often be as important as the Change itself.

Intended for:
Large Organizations, or any where discussion is an important part of the change management process.

Benefits:
From any Source Revision, it's possible for future developers to read through the discussion that ultimately produced it.
This has many educational, forensics, and security auditing benefits.

Requirements:

The SCS SHOULD record a description of the Proposed Change and all discussions / commentary related to it.

The SCS MUST link this discussion to the Source Revision itself. This is regularly done via commit metadata.

All collected content SHOULD be made immutable if the Change is accepted.
It SHOULD NOT be possible to edit the discussion around a Source Revision after it has been accepted.

### Fast moving repos and "merge trains"

Large Organizations must keep the number of updates to key protected Branches under certain limits to allow time for code review to happen.
For example, if a team tries to merge 60 change requests per hour into the `main` Branch, the tip of the `main` Branch would only be stable for about 1 minute.
This would leave only 1 minute for a new diff to be both generated and reviewed before it becomes stale again.

The normal way to work in this environment is to create a buffer Branch (sometimes called a "train") to collect a certain number of approved Changes.
In this model, when a Change is approved for submission to the protected Branch, it is added to the train Branch instead.
After a certain amount of time, the train Branch will be merged into the protected Branch.
If there are problems detected with the contents on the train Branch, it's normal for the whole train to be abandoned and a new train to be formed.
Approved Changes will be re-applied to a new train in this scenario.

The key benefit to this approach is that the protected Branch remains stable for longer, allowing more time for human and automatic code review.
A key downside to this approach is that Organizations will not know the final Source Revision id that represents a Change until the entire train process completes.

A change review process will now be associated with multiple distinct Source Revisions.

-   ID 1: The Source Revision which was reviewed before concluding the change review process. It represents the ideal state of the protected Branch applying only this proposed Change.
-   ID 2: The Source Revision created when the Change is applied to the train Branch. It represents the state of the protected Branch _after other Changes have been applied_.

It is important to note that no human or automatic Review will have the chance to pre-approve ID2. This will appear to violate any Organization policies that require pre-approval of Changes before submission.
The SCS and the Organization MUST protect this process in the same way they protect other artifact build pipelines.

At a minimum the SCS MUST issue an attestation that the Source Revision id generated by a merged train is identical ("MERGESAME" in git terminology) to the state of the protected Branch after applying each approved changeset in sequence.
No other content may be added or removed during this process.

## Future Considerations

### Authentication

-   Better protection against phishing by forbidding second factors that are not
  phishing resistant.
-   Protect against authentication token theft by forbidding bearer tokens
  (e.g. PATs).
-   Including length of continuity in the VSAs
