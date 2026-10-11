# Ansible collection: `foundata.sshd`

<!--
SPDX-FileCopyrightText: foundata GmbH <https://foundata.com>
SPDX-License-Identifier: GPL-3.0-or-later
-->

This repository contains the `foundata.sshd` Ansible Collection.

It provides resources to manage and use the [OpenSSH](https://www.openssh.com/)
secure shell daemon ([sshd](https://man.openbsd.org/sshd.8)), propably the
world's most used
[SSH (Secure SHell)](https://en.wikipedia.org/wiki/Secure_Shell) server.


<!-- Preserve the existing centered project banner. -->
<!-- rumdl-disable MD033 -->

<div align="center" id="project-readme-header">
<br>
<br>

**⭐ Found this useful? Support open-source and star this project:**

[![GitHub repository](https://img.shields.io/github/stars/foundata/ansible-collection-sshd.svg)](https://github.com/foundata/ansible-collection-sshd)

<br>
</div>

<!-- rumdl-enable MD033 -->



## Table of contents<a id="toc"></a>

- [Included content](#content)
- [Dependencies](#dependencies)
- [Licensing, copyright](#licensing-copyright)
- [Author information](#author-information)



## Included content<a id="content"></a>

### Role: `foundata.sshd.run`

The primary role in this collection to configure and maintain `sshd` with
security best practices by default. [Its `README.md`](./roles/run/README.md)
covers configuration, usage examples, and more:

<!-- ANSIBLE DOCSMITH TOC-FULL run START -->
- [Ansible role: `foundata.sshd.run`](roles/run/README.md#ansible-role-foundatasshdrun)
  - [Table of contents](roles/run/README.md#toc)
  - [Features](roles/run/README.md#features)
  - [Example playbooks, using this role](roles/run/README.md#examples)
  - [Supported tags](roles/run/README.md#tags)
  - [Role variables](roles/run/README.md#variables)
    - [`run_sshd_state`](roles/run/README.md#variable-run_sshd_state)
    - [`run_sshd_autoupgrade`](roles/run/README.md#variable-run_sshd_autoupgrade)
    - [`run_sshd_service_state`](roles/run/README.md#variable-run_sshd_service_state)
    - [`run_sshd_service_settings`](roles/run/README.md#variable-run_sshd_service_settings)
    - [`run_sshd_config_service_dropin_file_name`](roles/run/README.md#variable-run_sshd_config_service_dropin_file_name)
    - [`run_sshd_config_access_check`](roles/run/README.md#variable-run_sshd_config_access_check)
  - [Dependencies](roles/run/README.md#dependencies)
  - [Compatibility](roles/run/README.md#compatibility)
  - [External requirements](roles/run/README.md#requirements)
<!-- ANSIBLE DOCSMITH TOC-FULL run END -->



## Dependencies<a id="dependencies"></a>

See `dependencies` in [`galaxy.yml`](./galaxy.yml).



## Licensing, copyright<a id="licensing-copyright"></a>

<!--REUSE-IgnoreStart-->
<!-- should match SPDX-PackageSupplier -->
Copyright (c) 2025, 2026, foundata GmbH <https://foundata.com>

This project is licensed under the GNU General Public License v3.0 or later
(SPDX-License-Identifier: `GPL-3.0-or-later`), see
[`LICENSES/GPL-3.0-or-later.txt`](./LICENSES/GPL-3.0-or-later.txt) for the full
text.

The [`REUSE.toml`](./REUSE.toml) file provides detailed licensing and copyright
information in a human- and machine-readable format. This includes parts that
may be subject to different licensing or usage terms, such as third-party
components. The repository conforms to the
[REUSE specification](https://reuse.software/spec/). You can use
[`reuse spdx`](https://reuse.readthedocs.io/en/latest/readme.html#cli) to create
an
[SPDX software bill of materials (SBOM)](https://en.wikipedia.org/wiki/Software_Package_Data_Exchange).
<!--REUSE-IgnoreEnd-->

[![REUSE status](https://api.reuse.software/badge/github.com/foundata/ansible-collection-sshd)](https://api.reuse.software/info/github.com/foundata/ansible-collection-sshd)



### Trademarks<a id="trademarks"></a>

Third-party trademarks used in this repository:

- Ansible®, CentOS®, Fedora®, Red Hat® and RHEL® are trademarks of Red Hat, LLC,
  registered in the United States and other countries.
- Debian® is a trademark of Software in the Public Interest, Inc., registered in
  Germany and the United States.
- openSUSE® is a trademark of SUSE LLC or its affiliates, registered in the
  European Union and the United States.
- SUSE® is a trademark of SUSE LLC or its affiliates, registered in Germany, the
  European Union and the United States.
- Ubuntu® is a trademark of Canonical Ltd., registered in Germany, the European
  Union and the United States.

Their use here is purely descriptive and does not imply any affiliation with or
endorsement by the trademark holders.

Own and licensed trademarks used in this repository:

- foundata® is a trademark of [IPAM GmbH](https://ipam-services.com/),
  registered in Germany and the European Union, licensed to
  [foundata GmbH](https://foundata.com/).


## Author information<a id="author-information"></a>

This [project](https://foundata.com/en/projects/) was created and is maintained
by [foundata](https://foundata.com/).
