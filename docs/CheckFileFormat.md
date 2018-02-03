# SimpleumCheck: Check File Format

A SimpleumCheck File (aka "Check File") can contain one or more checks.

## File description
A "Check File" is a XML file using Apple's plist format.

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	<plist version="1.0">
	<dict>
		<key>FormatVersion</key>
		<string>1</string>
		<key>Rules</key>
		<array>
		
		{insert one check here}
		
		</array>
		<key>RulesVersion</key>
		<string>1</string>
	</dict>
	</plist>
	
### Attributes used as File Meta Data:

* FormatVersion: (Integer) Version number of the Check File Format (now: 1)
* RulesVersion: (Integer) Version number of the Check file
* Rules: (Array) An array containing one or more checks


## Check description
A check is written as a dictionary `<dict>...</dict>`.

#### Meta-Attributes:

* `CheckID` (String): an unique string to identify the check.
* `Domain` (String) ::= Basic | Professional | Paranoid
* `Severity` (String) ::= High | Medium | Low | Info
* `Kind` (String) ::= Security | Privacy | Info
* `Title`(String): short description of the check (Shown in SimpleumCheck as bold title)
* `CheckDescription`(String): More detailed description about the check (Shown in SimpleumCheck below the title)
* `InfoLink` (URL): URL to a web page explaining the Check and decribing how to "Fix it"
* `Avaiable` (Dictionry): From and to which macOS version is this check available. 
The value is a dictionary containing the following attributes:
	* `FromVersion` (VersionString): A VersionString is a String of format: `Major Version[.Minor Version[.Patch Level]]`
	* `ToVersion` (VersionString or wildcard): VersionString like above or `*` for any version 
* `Group` (String): Internal used for sorting and grouping in SCDE
* `Enabled`(Boolean): If "false" the check will be not executed. In a released version of SimpleumCheck there will be no Check with status not enabled.
* `Order` (String): Used for lexical ordering of checks in SimpleumCheck.
* `IntenalDocLink` (URL): This is a Simpleum-internal attribute. URL of the internal check documentation, quality gate process, release workflow, etc. 
* `workflowState`(String): describe the workflow progress for developing the check (values t.b.d.)
* `Meta`(Dictionary): Information about the check author
	* `Author`(String): Name of the author
	* `Copyright`(String): Copyright holder
	* `Notes` (String): free notes
	* `Version`(VersionString): Version of the check
* `ShowResult` (Bool): Should the output of the check shown to the user? An "info" button will show. Click on "info" shows the output string.

#### Check-Attributes
These are the attributes for the main check. Many checks can be done by querying "defaults" (Command line: defaults read <plist> <variable>) this is done with the `Type`= *defaults*. The other both types are *shell* (shell-script in user context) and *sudo_shell* (shell-script with administrative rights).
The `Action`should return a value to stdout which can be validated. If the script does not return a value, the `ResultDefault` is used.

To learn more about writing checks, have a look at the released checks.

* `CheckAction`(Dictionary) Execution of the main check
	* `Type `(Sring) ::= defaults | shell | sudo_shell | none
	* `Action` (String): Shell script as text, for Type == Defaults: plist-name variable_name 
	* `ResultDefault`(String): default value if the Action has no or an empty result.
* `Validation`(Dictionary)
	* `Type`(String) ::= contains | equal 
	* `IsSuccess` (Boolean) is a true validation a success (check passed)?
	* `Value`(String): value to be "equal" or "contain"

#### Precondition Check-Attributes
A check will only be executed, if the precondition is fullfilled, otherwise the check result will not be show. Example: To check if the built-in firewall is in stealth mode does only makes sense, when the built-in firewall is active.
If the check has no precondition choose Type = "none".

* `PreconditionCheckAction`(Dictionary) Execution of the main check
	* `Type `(Sring) ::= defaults | shell | sudo_shell | none
	* `Action` (String): Shell script as text, for Type == Defaults: plist-name variable_name 
	* `ResultDefault`(String): default value if the Action has no or an empty result.
* `PreconditionValidation`(Dictionary)
	* `Type`(String) ::= contains | equal 
	* `IsSuccess` (Boolean) is a true validation a success (check passed)?
	* `Value`(String): value to be "equal" or "contain"
	


## Example Check File
First is the precondition checked: Is the built-in firewall enabled?

This is the case, when defaults `read /Library/Preferences/com.apple.alf globalstate` == "1"

Then it checks if the built-in firewall is in stealth mode, done by a shell script: `/usr/libexec/ApplicationFirewall/socketfilterfw --getstealthmode` contains "enabled".

	<?xml version="1.0" encoding="UTF-8"?>
	
	<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	<plist version="1.0">
	<dict>
		<key>FormatVersion</key>
		<string>1</string>
		<key>Rules</key>
		<array>
			<dict>
				<key>Available</key>
				<dict>
					<key>FromVersion</key>
					<string>10.11</string>
					<key>ToVersion</key>
					<string>*</string>
				</dict>
				<key>CheckAction</key>
				<dict>
					<key>Action</key>
					<string>/usr/libexec/ApplicationFirewall/socketfilterfw --getstealthmode</string>
					<key>ResultDefault</key>
					<string></string>
					<key>Type</key>
					<string>shell</string>
				</dict>
				<key>CheckDescription</key>
				<string>Normally every computer in a network response to a ping request. This is often used to know if a computer or server is reachable. Many attacks start with a network scan: try to ping every IP address in the network, to know on which IP address a computer is available.</string>
				<key>CheckID</key>
				<string>ping_stealth</string>
				<key>Domain</key>
				<string>Professional</string>
				<key>Enabled</key>
				<true/>
				<key>Group</key>
				<string>firewall</string>
				<key>InfoLink</key>
				<string>https://simpleum.com/en/faq-items/dont-respond-icmp-ping-network-requests-firewall-im-stealth-mode/</string>
				<key>InternalDocLink</key>
				<string></string>
				<key>Kind</key>
				<string>Security</string>
				<key>Meta</key>
				<dict>
					<key>Author</key>
					<string>Simpleum Media GmbH</string>
					<key>Copyright</key>
					<string>2018 all rights reserved by Simpleum Media GmbH</string>
					<key>Notes</key>
					<string></string>
					<key>Version</key>
					<string>1.0.0</string>
				</dict>
				<key>Order</key>
				<integer>0</integer>
				<key>PreconditionCheckAction</key>
				<dict>
					<key>Action</key>
					<string>/Library/Preferences/com.apple.alf globalstate</string>
					<key>ResultDefault</key>
					<string>0</string>
					<key>Type</key>
					<string>defaults</string>
				</dict>
				<key>PreconditionValidation</key>
				<dict>
					<key>IsSuccess</key>
					<true/>
					<key>Type</key>
					<string>equal</string>
					<key>Value</key>
					<string>1</string>
				</dict>
				<key>Severity</key>
				<string>Medium</string>
				<key>ShowResult</key>
				<false/>
				<key>Title</key>
				<string>Dont respond to ICMP ping network requests (Firewall in stealth mode)</string>
				<key>Validation</key>
				<dict>
					<key>IsSuccess</key>
					<true/>
					<key>Type</key>
					<string>contains</string>
					<key>Value</key>
					<string>enabled</string>
				</dict>
				<key>WorkflowState</key>
				<string>os check</string>
			</dict>
		</array>
		<key>RulesVersion</key>
		<string>1</string>
	</dict>
	</plist>
