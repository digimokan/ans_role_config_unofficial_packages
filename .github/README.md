# ans_role_config_unofficial_packages

Configure and add unofficial packages.

[![Release](https://img.shields.io/github/release/digimokan/ans_role_config_unofficial_packages.svg?label=release)](https://github.com/digimokan/ans_role_config_unofficial_packages/releases/latest "Latest Release Notes")
[![License](https://img.shields.io/badge/license-MIT-blue.svg?label=license)](LICENSE.md "Project License")

## Table Of Contents

* [Purpose](#purpose)
* [Supported Operating Systems](#supported-operating-systems)
* [Quick Start](#quick-start)
    * [Use From Playbook](#use-from-playbook)
    * [Use From Parent Role As Dependency](#use-from-parent-role-as-dependency)
* [Role Options](#role-options)
* [Role Dependencies](#role-dependencies)
* [Module Dependencies](#module-dependencies)
* [Contributing](#contributing)

## Purpose

* Configure OS infrastructure required to install unofficial packages.
* Install an unofficial package to the OS.
* For Arch Linux, use the
  [Arch User Repository](https://wiki.archlinux.org/index.php/Arch_User_Repository).
* Add system aur [utility script](../templates/do_aur_sysupgrade.j2).

## Supported Operating Systems

* Arch Linux.

## Quick Start

### Use From Playbook

1. Create `requirements.yml` in ansible project root, and add this content:

   ```yaml
   # requirements.yml
   - src: https://github.com/digimokan/ans_role_config_unofficial_packages
   ```

2. From the project root directory, install/download the role:

   ```shell
   $ ansible-galaxy install --role-file requirements.yml --roles-path ./roles --force-with-deps
   ```

   * _NOTE:_ `--force-with-deps` _ensures subsequent calls download updates_

3. Include the main role, to set up the package-installation framework:

   ```yaml
   - name: "Set up configuration for installing unofficial packages"
     ansible.builtin.include_role:
       name: ans_role_config_unofficial_packages
   ```

4. Use a role "utility task" (from the `inc` directory) to add a package:

   ```yaml
   - name: "Install chromium-widevine package"
     ansible.builtin.include_role:
       name: ans_role_config_unofficial_packages
       tasks_from: inc/add_package.yml
     vars:
       package_name: "chromium-widevine"
   ```

### Use From Parent Role As Dependency

1. List in parent role's `meta/main.yml`, with `never` tag to avoid duplication:

   ```yaml
   dependencies:
     - src: https://github.com/digimokan/ans_role_config_unofficial_packages
       tags:
         - never
   ```

2. Call role with step 3 or 4 from [Use From Playbook](#use-from-playbook)
   section.

## Role Options

See the role `defaults` file, for overridable vars:

  * [defaults/main.yml](../defaults/main.yml)

Define these _required_ vars for the role:

  * `package_name`: name of unofficial package to install (string, or list)
  * `repo_name`: the repo that contains the packge (not needed for Arch Linux)

## Role Dependencies

* [ans_role_config_sudo](https://github.com/digimokan/ans_role_config_sudo)

## Module Dependencies

* [kewlfft/ansible-aur](https://github.com/kewlfft/ansible-aur)

## Contributing

* Feel free to report a bug or propose a feature by opening a new
  [Issue](https://github.com/digimokan/ans_role_config_unofficial_packages/issues).
* Follow the project's [Contributing](CONTRIBUTING.md) guidelines.
* Respect the project's [Code Of Conduct](CODE_OF_CONDUCT.md).

