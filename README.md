# Structures and Examples for the IdentityHub Service Directory

> *This repository contains Service Directory information for the
> IdentityHub component.*

After trying really hard to use the existing definitions such as
`member`, `roleOccupant` and such, the frivolous inconsistencies
between those and their relative incompleteness drove me to
abandon them.  Which ended in something really, *really* simple!

We now define four types of node for IdentityHub:

  * `identityHubUser`
  * `identityHubGroup`
  * `identityHubRole`
  * `resourceObject`

The `identityHubXXX` all require a `uid` attribute (which also is
used as the RDN by which these entries are found under their
`associatedDomain`).  In addition, they can have a mixture of

  * `uidPseudonym`
  * `uidAlias`
  * `uidRole` and `uidOccupant` (bidirectional acceptance)
  * `uidGroup` and `uidMember` (bidirectional acceptance)
  * `uidDecline` (to silence invitation notifications)
  * `cn`, `description`, `labeledURI`, `seeAlso` (annotations)

The `resourceObject` defines a resource, and can be setup with
a class and instance represented as a UUID:

  * `resourceClassUUID` (required)
  * `resourceInstanceUUID` (optional)

When `identityHubXXX` objects are access-controlled, they
are said to implement *communication access control*, which
aims to fend off outside visitors from communicating with
our domain's users through the various realm-crossing
services such as email, chat, telephony.

When `resouceXXX` objects are access-controlled, they are
said to implement *resource access control*, which aims to
fend off internal users from too sensitive resources for
them to handle.  External users will usually be rejected
by default, though access can still be granted explicitly
to select external users, assuming that they authenticate.

To control access, add the auxiliary class
`accessControlledObject` and fill out a list of
`accessControlList` attributes.  Each of these consists
of a line number, a selection and any number of
[DoNAI Selectors](http://donai.arpa2.net/selector.html),
all separated by a space.

An example LDIF file with entries is provided as a tree
under `ou=ServiceDIT`, the official location of the
Service Directory.  Also, an example PulleyScript is
provided with the instructions to pull from LDAP and
feed the Erlang code with the various pairs found in
LDAP.  (TODO: Remote users are probably dropped.)
The Erlang backend is generic, and drivers are
parameterised with the node name (defaults to
`noname@nonode` when an empty string is provided)
and an atom that is used to pass the information from
the various variables to the named Erlang node.

