==============================================
foundata.sshd Ansible collection Release Notes
==============================================

.. contents:: Topics

v3.0.0
======

Release Summary
---------------

Release Date: 2026-07-25

Feature release, but it includes potentially breaking changes, so this is a
major version bump according to SemVer. Both of them harden the defaults of
``foundata.sshd.run`` and can affect working setups:

1. The SSH daemon now offers a hybrid post-quantum key exchange only.
   **Clients older than OpenSSH 8.5** (such as Debian 11, Ubuntu 20.04, RHEL 8
   or macOS up to and including 12) **can no longer connect, so verify your
   clients before upgrading** or re-add a classical key exchange through
   ``run_sshd_service_settings``. In exchange, the default configuration
   passes ``ssh-audit`` without warnings again.
2. A new access-preservation preflight (anti-lockout) aborts the run *before*
   the SSH daemon is restarted if the effective configuration would lock the
   Ansible connection account out of SSH. Playbooks that deliberately exclude
   that account from SSH access now fail until they set
   ``run_sshd_config_access_check: false``.

See the changelog and/or commit messages for implementation details.

Minor Changes
-------------

- ``run`` role - Added an access-preservation preflight (anti-lockout) that runs before the SSH daemon is restarted. It evaluates the effective configuration for the Ansible connection account using ``sshd -T`` (extended test mode, including resolved ``Match`` blocks) and aborts the run *before* the restart handler fires if that account would be locked out, for example by a restrictive ``AllowUsers``, ``AllowGroups``, ``DenyUsers``, ``DenyGroups``, ``PermitRootLogin no`` (when connecting as ``root``), or by disabling every authentication method. Syntax validation (``sshd -t``) alone could not catch these lockouts. The new ``run_sshd_config_access_check`` variable (default ``true``) can be set to ``false`` to bypass the safeguard.
- ``run`` role - The default configuration passes `ssh-audit <https://github.com/jtesta/ssh-audit>`__ without warnings again. ssh-audit 3.9.0 flags every classical-only key exchange with "does not provide protection against post-quantum attacks", which the previous default triggered. ``sntrup761x25519-sha512@openssh.com`` was chosen over ``mlkem768x25519-sha256`` (NIST FIPS 203) because it is available on every platform supported by this role (OpenSSH 8.5 and later), whereas ``mlkem768x25519-sha256`` requires OpenSSH 9.9 and is therefore missing on Debian 12, Ubuntu 22.04 and Ubuntu 24.04.

Breaking Changes / Porting Guide
--------------------------------

- ``run`` role - Because the SSH daemon now only offers a post-quantum key exchange, **clients older than OpenSSH 8.5 can no longer connect** (for example Debian 11, Ubuntu 20.04, RHEL 8 or macOS up to and including 12). Verify your clients before upgrading. To keep a classical key exchange available as a fallback, override the default, keeping in mind that this re-introduces the "harvest now, decrypt later" exposure:
  .. code-block:: yaml

      run_sshd_service_settings:
        KexAlgorithms: "sntrup761x25519-sha512@openssh.com,curve25519-sha256"
- ``run`` role - The default ``KexAlgorithms`` changed from ``curve25519-sha256@libssh.org`` to the hybrid post-quantum ``sntrup761x25519-sha512@openssh.com``. Classical X25519/ECDH key exchange offers no protection against "harvest now, decrypt later" attacks: an adversary can record a handshake today and recover the session key once a quantum computer can run Shor's algorithm against the X25519 ECDH. ``sntrup761x25519-sha512@openssh.com`` combines the NTRU Prime KEM *with* X25519, so the session key stays secure unless both are broken. Note that this is not about the ``@libssh.org`` vendor alias: the standardized name ``curve25519-sha256`` denotes the same algorithm and is equally affected.

Bugfixes
--------

- The comment written into neutralized distribution config files contained a stray double quote in the Debian hint (``dpkg -S '<file>'"``), so the suggested command could not be copied and pasted as-is. The quote is removed.
- ``run`` role - The managed sshd drop-in config file is now validated with ``sshd -t`` before it is written (``validate:`` on the template task), so an invalid ``run_sshd_service_settings`` value can no longer reach the restart handler and leave sshd unable to start.
- ``run`` role - The service restart handler was gated only on ``run_sshd_service_state != 'unmanaged'``. With ``run_sshd_service_state: "disabled"`` a configuration change still notified them and, because handlers run after the service management tasks, the restart started the just-stopped unit again, leaving a running service although the declared state is stopped. The handlers are now gated on ``run_sshd_service_state in ['enabled', 'running']``.

v2.0.0
======

Release Summary
---------------

Release Date: 2026-05-10

Mostly a maintenance release, but it includes a breaking change, so this is a
major version bump according to SemVer.

Minor Changes
-------------

- Added Fedora 44 as supported platform for all collection roles and Molecule test scenarios.
- Added Ubuntu 26.04 LTS (Resolute Raccoon) as supported platform for all collection roles and Molecule test scenarios.
- ``foundata.sshd.run``: Drop ``PrintLastLog`` option on SUSE (openssh built with --disable-lastlog).
- ``foundata.sshd.run``: Implement case-insensitive key handling for sshd settings to properly handle keys like 'Port' vs 'port'.

Breaking Changes / Porting Guide
--------------------------------

- ``foundata.sshd.run``: Renamed user-facing variables for consistency with naming
  conventions. Update your playbooks and inventory accordingly:

  - ``run_sshd_sshd_settings`` → ``run_sshd_service_settings``
  - ``run_sshd_config_dropin_file_name`` → ``run_sshd_config_service_dropin_file_name``

Removed Features (previously deprecated)
----------------------------------------

- Removed Fedora 42 support (End of Life, EOL) from collection roles and Molecule scenarios. The collection may still work on Fedora 42, but no testing or bugfixes will be provided. A warning will be displayed when used on unsupported platforms.

v1.4.0
======

Release Summary
---------------

Release Date: 2026-01-11

Feature release.

Minor Changes
-------------

- ``foundata.sshd.run``: Add ``run_sshd_config_dropin_file_name`` variable to allow customizing the sshd drop-in configuration filename. Default changed from ``00-ansible.conf`` to ``00-managed.conf`` for consistency with other roles.

v1.3.0
======

Release Summary
---------------

Release Date: 

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
