# Ansible Documentation

[doc](https://docs.ansible.com/ansible/latest/index.html)

## About Ansible

Ansible is an IT automation tool. It can configure systems, deploy software, and orchestrate more advanced IT tasks such as continuous deployments or zero downtime rolling updates.

Ansible’s main goals are simplicity and ease-of-use. It also has a strong focus on security and reliability, featuring a minimum of moving parts, usage of OpenSSH for transport (with other transports and pull modes as alternatives), and a language that is designed around auditability by humans–even those not familiar with the program.

We believe simplicity is relevant to all sizes of environments, so we design for busy users of all types: developers, sysadmins, release engineers, IT managers, and everyone in between. Ansible is appropriate for managing all environments, from small setups with a handful of instances to enterprise environments with many thousands of instances.

Ansible manages machines in an agent-less manner. There is never a question of how to upgrade remote daemons or the problem of not being able to manage systems because daemons are uninstalled. Because OpenSSH is one of the most peer-reviewed open source components, security exposure is greatly reduced. Ansible is decentralized–it relies on your existing OS credentials to control access to remote machines. If needed, Ansible can easily connect with Kerberos, LDAP, and other centralized authentication management systems.

This documentation covers the version of Ansible noted in the upper left corner of this page. We maintain multiple versions of Ansible and of the documentation, so please be sure you are using the version of the documentation that covers the version of Ansible you’re using. For recent features, we note the version of Ansible where the feature was added.

Ansible releases a new major release of Ansible approximately three to four times per year. The core application evolves somewhat conservatively, valuing simplicity in language design and setup. However, the community around new modules and plugins being developed and contributed moves very quickly, adding many new modules in each release.

Ansible是一种IT自动化工具。 它可以配置系统，部署软件以及协调更高级的IT任务，例如连续部署或零停机滚动更新。

Ansible的主要目标是简单和易用。 它还非常关注安全性和可靠性，其特点是活动部件最少，使用OpenSSH进行运输（使用其他运输方式和拉动模式作为替代），以及一种围绕人（甚至是不熟悉的人）可审核性设计的语言。 该程序。

我们认为简单性与所有规模的环境有关，因此我们为各种类型的繁忙用户设计：开发人员，系统管理员，发布工程师，IT经理以及介于两者之间的每个人。 Ansible适用于管理所有环境，从具有少量实例的小型设置到具有数千个实例的企业环境。

Ansible以无代理的方式管理机器。 从来没有关于如何升级远程守护程序的问题，也没有因为卸载守护程序而无法管理系统的问题。 由于OpenSSH是最受同行评审的开源组件之一，因此可以大大降低安全风险。 Ansible是分散式的-它依靠您现有的OS凭据来控制对远程计算机的访问。 如果需要，Ansible可以轻松地与Kerberos，LDAP和其他集中式身份验证管理系统连接。

本文档涵盖了本页左上角注明的Ansible版本。 我们维护Ansible和文档的多个版本，因此请确保您使用的文档版本涵盖了您所使用的Ansible版本。 对于最新功能，我们注意到添加了该功能的Ansible版本。

Ansible每年大约发布三到四次Ansible的新主要版本。 核心应用程序有些保守地发展，重视语言设计和设置的简单性。 但是，围绕正在开发和贡献新模块和插件的社区非常迅速，在每个版本中都添加了许多新模块。



Installation, Upgrade & Configuration

- Installation Guide
  - [Installing Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
  - [Configuring Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html)
- Ansible Porting Guides
  - [Ansible 2.9 Porting Guide](https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.9.html)
  - [Ansible 2.8 Porting Guide](https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.8.html)
  - [Ansible 2.7 Porting Guide](https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.7.html)
  - [Ansible 2.6 Porting Guide](https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.6.html)
  - [Ansible 2.5 Porting Guide](https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.5.html)
  - [Ansible 2.4 Porting Guide](https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.4.html)
  - [Ansible 2.3 Porting Guide](https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.3.html)
  - [Ansible 2.0 Porting Guide](https://docs.ansible.com/ansible/latest/porting_guides/porting_guide_2.0.html)

Using Ansible

- User Guide
  - [Ansible Quickstart Guide](https://docs.ansible.com/ansible/latest/user_guide/quickstart.html)
  - [Ansible concepts](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html)
  - [Getting Started](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html)
  - [How to build your inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
  - [Working with dynamic inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html)
  - [Patterns: targeting hosts and groups](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html)
  - [Introduction to ad-hoc commands](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html)
  - [Connection methods and details](https://docs.ansible.com/ansible/latest/user_guide/connection_details.html)
  - [Working with command line tools](https://docs.ansible.com/ansible/latest/user_guide/command_line_tools.html)
  - [Working With Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html)
  - [Understanding privilege escalation: become](https://docs.ansible.com/ansible/latest/user_guide/become.html)
  - [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html)
  - [Working With Modules](https://docs.ansible.com/ansible/latest/user_guide/modules.html)
  - [Working With Plugins](https://docs.ansible.com/ansible/latest/plugins/plugins.html)
  - [Ansible and BSD](https://docs.ansible.com/ansible/latest/user_guide/intro_bsd.html)
  - [Windows Guides](https://docs.ansible.com/ansible/latest/user_guide/windows.html)
  - [Using collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html)

Contributing to Ansible

- Ansible Community Guide
  - [Getting started](https://docs.ansible.com/ansible/latest/community/index.html#getting-started)
  - [Going deeper](https://docs.ansible.com/ansible/latest/community/index.html#going-deeper)
  - [Working with the Ansible repo](https://docs.ansible.com/ansible/latest/community/index.html#working-with-the-ansible-repo)
  - [Traditional Table of Contents](https://docs.ansible.com/ansible/latest/community/index.html#traditional-table-of-contents)

Extending Ansible

- Developer Guide
  - [Adding modules and plugins locally](https://docs.ansible.com/ansible/latest/dev_guide/developing_locally.html)
  - [Should you develop a module?](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules.html)
  - [Ansible module development: getting started](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html)
  - [Contributing your module to Ansible](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_checklist.html)
  - [Conventions, tips, and pitfalls](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_best_practices.html)
  - [Ansible and Python 3](https://docs.ansible.com/ansible/latest/dev_guide/developing_python_3.html)
  - [Debugging modules](https://docs.ansible.com/ansible/latest/dev_guide/debugging.html)
  - [Module format and documentation](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_documenting.html)
  - [Windows module development walkthrough](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general_windows.html)
  - [Developing Cisco ACI modules](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general_aci.html)
  - [Guidelines for Ansible Amazon AWS module development](https://docs.ansible.com/ansible/latest/dev_guide/platforms/aws_guidelines.html)
  - [OpenStack Ansible Modules](https://docs.ansible.com/ansible/latest/dev_guide/platforms/openstack_guidelines.html)
  - [oVirt Ansible Modules](https://docs.ansible.com/ansible/latest/dev_guide/platforms/ovirt_dev_guide.html)
  - [Guidelines for VMware module development](https://docs.ansible.com/ansible/latest/dev_guide/platforms/vmware_guidelines.html)
  - [Information for submitting a group of modules](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_in_groups.html)
  - [Testing Ansible](https://docs.ansible.com/ansible/latest/dev_guide/testing.html)
  - [The lifecycle of an Ansible module](https://docs.ansible.com/ansible/latest/dev_guide/module_lifecycle.html)
  - [Developing plugins](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html)
  - [Developing dynamic inventory](https://docs.ansible.com/ansible/latest/dev_guide/developing_inventory.html)
  - [Developing the Ansible Core Engine](https://docs.ansible.com/ansible/latest/dev_guide/developing_core.html)
  - [Ansible module architecture](https://docs.ansible.com/ansible/latest/dev_guide/developing_program_flow_modules.html)
  - [Python API](https://docs.ansible.com/ansible/latest/dev_guide/developing_api.html)
  - [Rebasing a pull request](https://docs.ansible.com/ansible/latest/dev_guide/developing_rebasing.html)
  - [Using and Developing Module Utilities](https://docs.ansible.com/ansible/latest/dev_guide/developing_module_utilities.html)
  - [Developing collections](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections.html)
  - [Collection Galaxy metadata structure](https://docs.ansible.com/ansible/latest/dev_guide/collections_galaxy_meta.html)
  - [Ansible architecture](https://docs.ansible.com/ansible/latest/dev_guide/overview_architecture.html)

Common Ansible Scenarios

- [Public Cloud Guides](https://docs.ansible.com/ansible/latest/scenario_guides/cloud_guides.html)
- [Network Technology Guides](https://docs.ansible.com/ansible/latest/scenario_guides/network_guides.html)
- [Virtualization and Containerization Guides](https://docs.ansible.com/ansible/latest/scenario_guides/virt_guides.html)

Ansible for Network Automation

- Ansible for Network Automation
  - [Getting Started with Ansible for Network Automation](https://docs.ansible.com/ansible/latest/network/getting_started/index.html)
  - [Advanced Topics with Ansible for Network Automation](https://docs.ansible.com/ansible/latest/network/user_guide/index.html)
  - [Developer Guide for Network Automation](https://docs.ansible.com/ansible/latest/network/dev_guide/index.html)

Ansible Galaxy

- Galaxy User Guide
  - [Finding collections on Galaxy](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#finding-collections-on-galaxy)
  - [Installing collections](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#installing-collections)
  - [Finding roles on Galaxy](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#finding-roles-on-galaxy)
  - [Installing roles from Galaxy](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#installing-roles-from-galaxy)
- Galaxy Developer Guide
  - [Creating collections for Galaxy](https://docs.ansible.com/ansible/latest/galaxy/dev_guide.html#creating-collections-for-galaxy)
  - [Creating roles for Galaxy](https://docs.ansible.com/ansible/latest/galaxy/dev_guide.html#creating-roles-for-galaxy)

Reference & Appendices

- [Module Index](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html)
- [Playbook Keywords](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html)
- [Return Values](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html)
- [Ansible Configuration Settings](https://docs.ansible.com/ansible/latest/reference_appendices/config.html)
- [Controlling how Ansible behaves: precedence rules](https://docs.ansible.com/ansible/latest/reference_appendices/general_precedence.html)
- [YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html)
- [Python 3 Support](https://docs.ansible.com/ansible/latest/reference_appendices/python_3_support.html)
- [Interpreter Discovery](https://docs.ansible.com/ansible/latest/reference_appendices/interpreter_discovery.html)
- [Release and maintenance](https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html)
- [Testing Strategies](https://docs.ansible.com/ansible/latest/reference_appendices/test_strategies.html)
- [Sanity Tests](https://docs.ansible.com/ansible/latest/dev_guide/testing/sanity/index.html)
- [Frequently Asked Questions](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html)
- [Glossary](https://docs.ansible.com/ansible/latest/reference_appendices/glossary.html)
- [Ansible Reference: Module Utilities](https://docs.ansible.com/ansible/latest/reference_appendices/module_utils.html)
- [Special Variables](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html)
- [Red Hat Ansible Tower](https://docs.ansible.com/ansible/latest/reference_appendices/tower.html)
- [Logging Ansible output](https://docs.ansible.com/ansible/latest/reference_appendices/logging.html)



Roadmaps

- Ansible Roadmap
  - [Ansible 2.9](https://docs.ansible.com/ansible/latest/roadmap/ROADMAP_2_9.html)
  - [Ansible 2.8](https://docs.ansible.com/ansible/latest/roadmap/ROADMAP_2_8.html)
  - [Ansible 2.7](https://docs.ansible.com/ansible/latest/roadmap/ROADMAP_2_7.html)
  - [Ansible 2.6](https://docs.ansible.com/ansible/latest/roadmap/ROADMAP_2_6.html)
  - [Ansible 2.5](https://docs.ansible.com/ansible/latest/roadmap/ROADMAP_2_5.html)
  - [Ansible 2.4](https://docs.ansible.com/ansible/latest/roadmap/ROADMAP_2_4.html)
  - [Ansible 2.3](https://docs.ansible.com/ansible/latest/roadmap/ROADMAP_2_3.html)
  - [Ansible 2.2](https://docs.ansible.com/ansible/latest/roadmap/ROADMAP_2_2.html)
  - [Ansible 2.1](https://docs.ansible.com/ansible/latest/roadmap/ROADMAP_2_1.html)