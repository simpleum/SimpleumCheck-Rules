# Contribution
We appreciate any contribution to improve, maintain, correct and add new checks.

Each contribution must follow the main objective of SimpleumCheck:

**"Help less experienced users to understand the possibilities to improve the security and privacy settings for their Mac."**

If you have written a check or modified a check you can create a pull request.

A check must contain:

* The check definition in [SimpleumCheck XML](docs/CheckFileFormat.md) format.
* A description "Why is the check important and how it effects the user"
* A description "How to fix it"

A check is not allowed:

* Modifications of files
* Modifications of settings
* Transmission of any collected data
* Discovery any data from other computers or devices

You agree that the check will be released under GPL 3 license and that Simpleum has the right to include the check in SimpleumCheck. If Simpleum decides to release a commercial / paid version of SimpleumCheck it can contain any check published on this repository for free, but including you as an author of your check in the rules file.

Simpleum does not guarantee that a contributed check will be included in a released version of SimpleumCheck.

### Checks you want to share with experienced users (not for SimpleumCheck)

If you have developed checks which

* Are not suitable for the "normal" SimpleumCheck audience
* Interessting for administrators
* Violating the rules above
* Special forensic checks
* Automatic hardening checks, ...

you should store them in `checks/special`.

These checks will only run with SCDE.
