# Ansible Lint Documentation

https://docs.ansible.com/ansible-lint/index.html

## About Ansible Lint

Ansible Lint is a commandline tool for linting playbooks. Use it to detect behaviors and practices that could potentially be improved.

The tool is used by the [Ansible Galaxy project](https://github.com/ansible/galaxy/) to lint and calculate quality scores for content contributed to the [Galaxy Hub](https://galaxy.ansible.com/).

The project was originally started by [@willthames](https://github.com/willthames/), and has since been transferred to the Ansible project team.

Installing

- Installing
  - [Using Pip](https://docs.ansible.com/ansible-lint/installing/installing.html#using-pip)
  - [From Source](https://docs.ansible.com/ansible-lint/installing/installing.html#from-source)

Usage

- Usage
  - [Command Line Options](https://docs.ansible.com/ansible-lint/usage/usage.html#command-line-options)
  - [Linting Playbooks and Roles](https://docs.ansible.com/ansible-lint/usage/usage.html#linting-playbooks-and-roles)
  - [Examples](https://docs.ansible.com/ansible-lint/usage/usage.html#examples)

Configuring

- Configuring
  - [Configuration File](https://docs.ansible.com/ansible-lint/configuring/configuring.html#configuration-file)
  - [Pre-commit Setup](https://docs.ansible.com/ansible-lint/configuring/configuring.html#pre-commit-setup)

Rules

- Rules
  - Specifying Rules at Runtime
    - [Using Tags to Include Rules](https://docs.ansible.com/ansible-lint/rules/rules.html#using-tags-to-include-rules)
    - [Excluding Rules](https://docs.ansible.com/ansible-lint/rules/rules.html#excluding-rules)
  - [False Positives: Skipping Rules](https://docs.ansible.com/ansible-lint/rules/rules.html#false-positives-skipping-rules)
  - [Creating Custom Rules](https://docs.ansible.com/ansible-lint/rules/rules.html#creating-custom-rules)