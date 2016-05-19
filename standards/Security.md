Security-related Standards & Guidelines
==========================================

# User accounts

[Okta](https://gilt.okta.com/login/do-login) login should be integrated and used with all services that support it.

[2FA](https://en.wikipedia.org/wiki/Two-factor_authentication) is mandatory everywhere where it is supported. That includes not only Gilt internal systems but 3rd party systems, like [GitHub](https://github.com/gilt).


# Services

Sensitive information must be managed separately from source code and config.

[KMS](https://aws.amazon.com/kms/) should be used to keep sensitive data secure and control access to it. Design and different use cases are [described here](https://d0.awsstatic.com/whitepapers/KMS-Cryptographic-Details.pdf). In particular, "Envelope Encryption" should cover many of Gilt server-side needs.

[IAM](https://aws.amazon.com/iam/) should be used to control access to AWS resources on the backend, including KMS-encrypted secret data. [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) should be the preferred method of access control.
