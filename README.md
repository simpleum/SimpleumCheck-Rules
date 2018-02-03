# Checks for SimpleumCheck
SimpleumCheck is a Security and Privacy Advisor for Apple Mac Computers.

Homepage of SimpleumCheck [https://simpleum.com/en/security-advisor-simpleumcheck/](https://simpleum.com/en/security-advisor-simpleumcheck/)

We publish the used checks (aka "rules") in this repository.


## Structure
SimpleumCheck contains a all-in-one check file. While developing checks we us one file for every check.

* Released checks are at `checks/released`
* Check in development are at `checks/development`
* Special checks (see below) are at `checks/special`


## Security policy
SimpleumCheck runs shell scripts with user and administration rights to determine security and privacy settings.
So, it is very important, that checks cannot be modified after a new version of SimpleumCheck is released. For this reason, it is not possible to load own checks or to modify the existing checks within SimpleumCheck. 

At every launch of SimpleumCheck the App and checks integrity is controlled. The App aborts running, when any modification was detected.

SimpleumCheck has a helper daemon for running scripts with administration rights. To ensure that no modified scripts could be run within the helper, all scripts with administration rights must be white listed within the helper at compile time. The helper does a self integrity check at every start and abort running, when any modification was detected.


### Summary
SimpleumCheck does not allow to alter the checks nor to load other checks. All checks included in SimpleumCheck must pass a quality gate at Simpleum before they are included in a new version of SimpleumCheck.

To develop checks we will publish a Developer Edition of SimpleumCheck. 


## SimpleumCheck Developer Edition (SCDE)
Because of the security policy - SimpleumCheck does not allow to run own checks - we developed SimpleumCheck Developer Edition (short SCDE) to do so.

SCDE allows to load, save, add, delete checks. It has also an editor for writing and testing checks. The SCDE has an own helper which is not restricted to white listed scripts. This is the reason SCDE should be used with care and by experienced users.

SCDE is not released yet!


## Contribution
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

## Trademark Information
Here we use "Simpleum" for Simpleum Media GmbH.

Any trademarks contained in the source code, binaries, and/or in the documentation, are the sole property of their respective owners.

## License

Copyright (c) 2018 Simpleum Media GmbH. All rights reserved.

Checks licensed under the [GPL V3](LICENSE) License.
