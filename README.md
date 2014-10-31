# Samba Server ('samba-server')

**Part of the BAS Ansible Role Collection (BARC)**

Installs Samba server for resource sharing over SMB

## Overview

* Installs Samba server and related packages.
* Configures smb.conf to create specified file shares
* If enabled, sets Samba passwords for controller and app users to allow login

## Author

[British Antarctic Survey](http://www.antarctica.ac.uk) - Web & Applications Team

Contact: [basweb@bas.ac.uk](mailto:basweb@bas.ac.uk).

## Availability

This role is designed for internal use but if useful can be shared publicly.

## Branches

This project uses three permanent branches with the *Git Flow* branching model managing the interaction between branches.

* **Develop:** unstable, potentially non-working but most current version of roles. Bug fixes and features interact with this branch only.
* **Master:** stable, tested, working version of role with full documentation. Releases and hot fixes mainly interact with this branch. This branch should when consuming roles internally.
* **Public:** equivalent to the *master* branch, but available externally. Some configuration details may be altered or features removed to make available for public release.

## Testing

Manual testing is performed for all roles to ensure roles achieve their aims and this forms a prerequisite task for merging changes into the *master* and *public* branches.
Wherever possible testing is as complete as possible meaning tasks such as downloading dependencies are performed as part of each test.

## Issues

Please log issues to the [BAS Web and Applications Team](https://jira.ceh.ac.uk/browse/BASWEB) project in Jira, within the *Project - Ansible Roles* component.

If outside of NERC please get in touch to report any issues.

## Contributions

We have no formal contribution policy, if you spot any bugs or potential improvements please submit a pull request or get in touch.

These roles are used for internal projects which may dictate whether any contributions can be included.

## License

[Open Government Licence V2](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/2/)

## Requirements

### BAS Ansible Role Collection (BARC)

* `core`

## Variables

* `samba_server_controller_user_username`
    * The username of the controller OS user, used for system tasks, if enabled.
    * This variable must be a valid OS user.
	* Default: "controller"
* `samba_server_app_user_username`
    * The username of the app OS user, used for day to day tasks, if enabled.
    * This variable must be a valid OS user.
	* Default: "app"
* `samba_server_controller_samba_user_enabled`
    * If "true" the Samba password for the controller OS user will be set to allow the user to login.
	* Default: true
* `samba_server_controller_samba_user_password`
	* Samba password for the controller OS user.
	* Default: "controller"
* `samba_server_app_samba_user_enabled`
    * If "true" the Samba password for the app OS user will be set to allow the user to login.
	* Default: true
* `samba_server_app_samba_user_password`
	* Samba password for the controller OS user.
	* Default: "app"
* `samba_server_workgroup`
	* Workgroup of the Samba server.
	* See [here](https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html#WORKGROUP) for more information.
	* Default: "NERC"
* `samba_server_role`
	* Operating mode of the Samba server.
	* See [here](https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html#SERVERROLE) for more information and list of possible values this **must** be.
	* Default: "standalone server"
* `samba_server_sync_password_changes`
	* If "yes" OS and Samba passwords will be synchronised.
	* See [here](https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html#UNIXPASSWORDSYNC) for more information.
	* Default: "no"
* `samba_server_pam_password_changes`
	* Use PAM when changing Samba passwords.
	* See [here](https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html#UNIXPASSWORDSYNC) for more information.
	* Default: "no"
* `samba_server_file_creation_mask`
	* Controls initial permissions applied to newly created files.
	* See [here](https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html#CREATEMASK) for more information.
	* Default: "0600"
* `samba_server_directory_creation_mask`
	* Controls initial permissions applied to newly created directories.
	* See [here](https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html#DIRECTORYMASK) for more information.
	* Default: "0700"
* `samba_server_default_shares`
	* Array of default Samba shares.
	* **Do not** override this variable, use `samba_server_user_shares` instead.
	* Structured as an array of items where each item consists of a share descriptor and array of options and values:
		* `descriptor`
			* Identifier used with this variable only.
		* `options` [array] 
			* `option`
				* Name of config option (e.g. "path") 
			* `value`
				* Value for option (e.g. "/app")
	* See below for typical values for shares
	* Default: (see variable)
* `samba_server_user_shares`
	* Array of Samba shares.
	* Structured as an array of items where each item consists of a share descriptor and array of options and values:
		* `descriptor`
			* Identifier used with this variable only.
		* `options` [array] 
			* `option`
				* Name of config option (e.g. "path") 
			* `value`
				* Value for option (e.g. "/app")
	* See below for typical values for shares
	* Default: "[]  (empty array)"

###Share definitions

The following examples give examples of defining different types of shares.  
The documentation [here](https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html) details the purpose and syntax for each option used.

#### Read/Write access by all Samba users

      - name: app
        options:
          - option: name
            value: app
          - option: path
            value: /app
          - option: comment
            value: Application Root
          - option: guest_ok
            value: no
          - option: browseable
            value: no
          - option: writable
            value: yes
          - option: file_creation_mask
            value: "0775"
          - option: directory_creation_mask
            value: "0775"

## Changelog

### 0.1.0 - October 2014

* Initial version
