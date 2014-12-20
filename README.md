# Gilt Standards

Standards and guidelines for defining standards at Gilt (Oh My!)


## List of Standards

* [Events](https://github.com/gilt/standards-events)
* [Open Source](https://github.com/gilt/standards-open-source)
* [REST](https://github.com/gilt/standards-rest)


## Creating a Standard

1. Create a GitHub repository named ```standards-foo``` where ```foo``` is the
   object of the standard. Some examples are: events, schemas, apis, ux, etc.

2. Submit a pull request adding your standard to the [list of
   standards](https://github.com/gilt/standards#list-of-standards) above.


## Governance

We recommend thorough review of changes to standards. The recommended
repository structure at Gilt is:

1. Create a MAINTAINERS file in the root of the repository listing all of the
   people whose opinion we really need in reviewing changes to the standards.
   Each line in the maintainers file should follow the structure:

   ```<github username> (full name)```

   The first line of the file should be the [Benevolent Dictator for
   Life](https://en.wikipedia.org/wiki/Benevolent_dictator_for_life). In the
   maintainers file, append "BDFL" to appropriate name.

   See [the MAINTAINERS file for this
   repo](https://github.com/gilt/standards/blob/master/MAINTAINERS) for an
   example.

2. Create a team in github named "governance-<standard>" with `write` access
   to the repository (all others having `read`-only access).

3. Create a "CONTRIBUTIONS.md" file in the root of the repository that
   describes the policy by which contributions will be accepted. The standard
   Gilt policy is modeled after the docker hub project. Please feel free to
   link directly to the [standard CONTRIBUTIONS.md
   policy](https://github.com/gilt/standards/blob/master/CONTRIBUTIONS.md) for
   your project.


## Contributing

See https://github.com/gilt/standards/blob/master/CONTRIBUTIONS.md
