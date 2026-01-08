==============================================
foundata.sshd Ansible collection Release Notes
==============================================

.. contents:: Topics

v1.3.0
======

Release Summary
---------------

Release Date: 2026-01-08

Maintenance release.

Minor Changes
-------------

- Molecule: Added openSUSE Leap 16.0 as a test target platform.
- ``foundata.sshd.run``: Added openSUSE Leap 16.0 as a supported platform.
- ``foundata.sshd.run``: Configuration file permissions now match each operating system's default behavior. Red Hat-like systems retain restrictive permissions (0600/0700), while Debian-like and SUSE systems use their standard defaults (typically 0644/0755 for config files and directories).

v1.2.1
======

Release Summary
---------------

Release Date: 2025-12-26

Bugfix release.

Bugfixes
--------

- run role - Fixed a bug where the regex for commenting out duplicate sshd_config
  options would re-match already-commented lines on every run, prepending
  additional ``#`` characters. This occurred on Fedora 43 where
  ``/etc/ssh/sshd_config.d/50-redhat.conf`` contains a TAB-indented
  ``AcceptEnv`` line, causing an empty string to enter the options list.
  When joined with ``|``, patterns like ``Port||HostKey`` matched empty
  strings at any position, affecting all lines.

v1.2.0
======

Release Summary
---------------

Release Date: 2025-12-26

Maintenance release.

Minor Changes
-------------

- Added Fedora 43 as supported platform for all collection roles and Molecule test scenarios

Removed Features (previously deprecated)
----------------------------------------

- Removed Fedora 41 support (End of Life, EOL) from collection roles and Molecule scenarios. The collection may still work on Fedora 41, but no testing or bugfixes will be provided. A warning will be displayed when used on unsupported platforms.

v1.1.0
======

Release Summary
---------------

Release Date: 2025-11-02

Maintenance release.

Minor Changes
-------------

- Molecule: Added Debian 13 (Trixie) as a test target platform.
- ``foundata.sshd.run``: Added Debian 13 (Trixie) as a supported platform.

Removed Features (previously deprecated)
----------------------------------------

- Molecule: Removed Debian 11 (Bullseye) as a test target platform.
- ``foundata.sshd.run``: Removed Debian 11 (Bullseye) from the list of supported platforms. The role will continue to work on Debian 11 but will display a warning. To avoid this, either remain on or pin the previous version of the collection. Bugs and issues related to Debian 11 will no longer be fixed.

v1.0.0
======

Release Summary
---------------

Release Date: 2025-04-23

First public release, providing all functionality and files.
