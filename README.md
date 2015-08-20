# Samba Server ('samba-server')

**Part of the BAS Ansible Role Collection (BARC)**

Installs Samba server for resource sharing over SMB

## Overview

* Installs Samba server and related packages.
* Configures smb.conf to create specified file shares
* If enabled, sets Samba passwords for controller and app users to allow login

## Availability

This role is designed for internal use but if useful can be shared publicly.

## Usage

### Requirements

#### BAS Ansible Role Collection (BARC)

* `core`

### Variables

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
	* Default: "password"
* `samba_server_app_samba_user_enabled`
    * If "true" the Samba password for the app OS user will be set to allow the user to login.
	* Default: true
* `samba_server_app_samba_user_password`
	* Samba password for the controller OS user.
	* Default: "password"
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

#### Share definitions

The following examples give examples of defining different types of shares.  
The documentation [here](https://www.samba.org/samba/docs/man/manpages-3/smb.conf.5.html) details the purpose and syntax for each option used.

##### Read/Write access by all Samba users

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

## Contributing

This project welcomes contributions, see `CONTRIBUTING` for our general policy.

## Developing

### Committing changes

The [Git flow](https://github.com/fzaninotto/Faker#formatters) workflow is used to manage development of this package.

Discrete changes should be made within *feature* branches, created from and merged back into *develop* (where small one-line changes may be made directly).

When ready to release a set of features/changes create a *release* branch from *develop*, update documentation as required and merge into *master* with a tagged, [semantic version](http://semver.org/) (e.g. `v1.2.3`).

After releases the *master* branch should be merged with *develop* to restart the process. High impact bugs can be addressed in *hotfix* branches, created from and merged into *master* directly (and then into *develop*).

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the BAS Web & Applications Team Jira project ([BASWEB](https://jira.ceh.ac.uk/browse/BASWEB)).

## License

Copyright 2015 NERC BAS. Licensed under the MIT license, see `LICENSE` for details.