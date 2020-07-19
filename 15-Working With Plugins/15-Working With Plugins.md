# Working With Plugins

Plugins are pieces of code that augment Ansible’s core functionality. Ansible uses a plugin architecture to enable a rich, flexible and expandable feature set.

Ansible ships with a number of handy plugins, and you can easily write your own.

This section covers the various types of plugins that are included with Ansible:

# Action Plugins

- [Enabling action plugins](https://docs.ansible.com/ansible/latest/plugins/action.html#enabling-action-plugins)
- [Using action plugins](https://docs.ansible.com/ansible/latest/plugins/action.html#using-action-plugins)
- [Plugin list](https://docs.ansible.com/ansible/latest/plugins/action.html#plugin-list)

Action plugins act in conjunction with [modules](https://docs.ansible.com/ansible/latest/user_guide/modules.html#working-with-modules) to execute the actions required by playbook tasks. They usually execute automatically in the background doing prerequisite work before modules execute.

The ‘normal’ action plugin is used for modules that do not already have an action plugin.



## [Enabling action plugins](https://docs.ansible.com/ansible/latest/plugins/action.html#id2)

You can enable a custom action plugin by either dropping it into the `action_plugins` directory adjacent to your play, inside a role, or by putting it in one of the action plugin directory sources configured in [ansible.cfg](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings).



## [Using action plugins](https://docs.ansible.com/ansible/latest/plugins/action.html#id3)

Action plugin are executed by default when an associated module is used; no action is required.

## [Plugin list](https://docs.ansible.com/ansible/latest/plugins/action.html#id4)

You cannot list action plugins directly, they show up as their counterpart modules:

Use `ansible-doc -l` to see the list of available modules. Use `ansible-doc <name>` to see specific documentation and examples, this should note if the module has a corresponding action plugin.

# Become Plugins

- [Enabling Become Plugins](https://docs.ansible.com/ansible/latest/plugins/become.html#enabling-become-plugins)
- [Using Become Plugins](https://docs.ansible.com/ansible/latest/plugins/become.html#using-become-plugins)
- [Plugin List](https://docs.ansible.com/ansible/latest/plugins/become.html#plugin-list)

*New in version 2.8.*

Become plugins work to ensure that Ansible can use certain privilege escalation systems when running the basic commands to work with the target machine as well as the modules required to execute the tasks specified in the play.

These utilities (`sudo`, `su`, `doas`, etc) generally let you ‘become’ another user to execute a command with the permissions of that user.



## [Enabling Become Plugins](https://docs.ansible.com/ansible/latest/plugins/become.html#id2)

The become plugins shipped with Ansible are already enabled. Custom plugins can be added by placing them into a `become_plugins` directory adjacent to your play, inside a role, or by placing them in one of the become plugin directory sources configured in [ansible.cfg](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings).



## [Using Become Plugins](https://docs.ansible.com/ansible/latest/plugins/become.html#id3)

In addition to the default configuration settings in [Ansible Configuration Settings](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings) or the `--become-method` command line option, you can use the `become_method` keyword in a play or, if you need to be ‘host specific’, the connection variable `ansible_become_method` to select the plugin to use.

You can further control the settings for each plugin via other configuration options detailed in the plugin themselves (linked below).



## [Plugin List](https://docs.ansible.com/ansible/latest/plugins/become.html#id4)

You can use `ansible-doc -t become -l` to see the list of available plugins. Use `ansible-doc -t become <plugin name>` to see specific documentation and examples.

- [doas – Do As user](https://docs.ansible.com/ansible/latest/plugins/become/doas.html)
- [dzdo – Centrify’s Direct Authorize](https://docs.ansible.com/ansible/latest/plugins/become/dzdo.html)
- [enable – Switch to elevated permissions on a network device](https://docs.ansible.com/ansible/latest/plugins/become/enable.html)
- [ksu – Kerberos substitute user](https://docs.ansible.com/ansible/latest/plugins/become/ksu.html)
- [machinectl – Systemd’s machinectl privilege escalation](https://docs.ansible.com/ansible/latest/plugins/become/machinectl.html)
- [pbrun – PowerBroker run](https://docs.ansible.com/ansible/latest/plugins/become/pbrun.html)
- [pfexec – profile based execution](https://docs.ansible.com/ansible/latest/plugins/become/pfexec.html)
- [pmrun – Privilege Manager run](https://docs.ansible.com/ansible/latest/plugins/become/pmrun.html)
- [runas – Run As user](https://docs.ansible.com/ansible/latest/plugins/become/runas.html)
- [sesu – CA Privileged Access Manager](https://docs.ansible.com/ansible/latest/plugins/become/sesu.html)
- [su – Substitute User](https://docs.ansible.com/ansible/latest/plugins/become/su.html)
- [sudo – Substitute User DO](https://docs.ansible.com/ansible/latest/plugins/become/sudo.html)



# doas – Do As user

*New in version 2.8.*

- [Synopsis](https://docs.ansible.com/ansible/latest/plugins/become/doas.html#synopsis)
- [Parameters](https://docs.ansible.com/ansible/latest/plugins/become/doas.html#parameters)
- [Status](https://docs.ansible.com/ansible/latest/plugins/become/doas.html#status)

## [Synopsis](https://docs.ansible.com/ansible/latest/plugins/become/doas.html#id1)

- This become plugins allows your remote/login user to execute commands as another user via the doas utility.

## [Parameters](https://docs.ansible.com/ansible/latest/plugins/become/doas.html#id2)

| Parameter          | Choices/Defaults    | Configuration                                                | Comments                                                     |
| ------------------ | ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **become_exe** -   | **Default:** "doas" | ini entries:[privilege_escalation] become_exe = doas[doas_become_plugin] executable = doasenv:ANSIBLE_BECOME_EXEenv:ANSIBLE_DOAS_EXEvar: ansible_become_exevar: ansible_doas_exe | Doas executable                                              |
| **become_flags** - | **Default:** null   | ini entries:[privilege_escalation] become_flags = None[doas_become_plugin] flags = Noneenv:ANSIBLE_BECOME_FLAGSenv:ANSIBLE_DOAS_FLAGSvar: ansible_become_flagsvar: ansible_doas_flags | Options to pass to doas                                      |
| **become_pass** -  |                     | ini entries:[doas_become_plugin] password = VALUEenv:ANSIBLE_BECOME_PASSenv:ANSIBLE_DOAS_PASSvar: ansible_become_passwordvar: ansible_become_passvar: ansible_doas_pass | password for doas prompt                                     |
| **become_user** -  |                     | ini entries:[privilege_escalation] become_user = VALUE[doas_become_plugin] user = VALUEenv:ANSIBLE_BECOME_USERenv:ANSIBLE_DOAS_USERvar: ansible_become_uservar: ansible_doas_user | User you 'become' to execute the task                        |
| **prompt_l10n** -  | **Default:** []     | ini entries:[doas_become_plugin] localized_prompts = []env:ANSIBLE_DOAS_PROMPT_L10Nvar: ansible_doas_prompt_l10n | List of localized strings to match for prompt detectionIf empty we'll use the built in one |



## [Status](https://docs.ansible.com/ansible/latest/plugins/become/doas.html#id3)

- This become is not guaranteed to have a backwards compatible interface. *[preview]*
- This become is [maintained by the Ansible Community](https://docs.ansible.com/ansible/latest/user_guide/modules_support.html#modules-support). *[community]*

### Authors

- ansible (@core)



# Cache Plugins

- [Enabling Fact Cache Plugins](https://docs.ansible.com/ansible/latest/plugins/cache.html#enabling-fact-cache-plugins)
- [Enabling Inventory Cache Plugins](https://docs.ansible.com/ansible/latest/plugins/cache.html#enabling-inventory-cache-plugins)
- [Using Cache Plugins](https://docs.ansible.com/ansible/latest/plugins/cache.html#using-cache-plugins)
- [Plugin List](https://docs.ansible.com/ansible/latest/plugins/cache.html#plugin-list)

Cache plugin implement a backend caching mechanism that allows Ansible to store gathered facts or inventory source data without the performance hit of retrieving them from source.

The default cache plugin is the [memory](https://docs.ansible.com/ansible/latest/plugins/cache/memory.html#memory-cache) plugin, which only caches the data for the current execution of Ansible. Other plugins with persistent storage are available to allow caching the data across runs.

You can use a separate cache plugin for inventory and facts. If an inventory-specific cache plugin is not provided and inventory caching is enabled, the fact cache plugin is used for inventory.



## [Enabling Fact Cache Plugins](https://docs.ansible.com/ansible/latest/plugins/cache.html#id2)

Only one fact cache plugin can be active at a time.

You can enable a cache plugin in the Ansible configuration, either via environment variable:

```
export ANSIBLE_CACHE_PLUGIN=jsonfile
```

or in the `ansible.cfg` file:

```
[defaults]
fact_caching=redis
```

If the cache plugin is in a collection use the fully qualified name:

```
[defaults]
fact_caching = namespace.collection_name.cache_plugin_name
```

You will also need to configure other settings specific to each plugin. Consult the individual plugin documentation or the Ansible [configuration](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings) for more details.

A custom cache plugin is enabled by dropping it into a `cache_plugins` directory adjacent to your play, inside a role, or by putting it in one of the directory sources configured in [ansible.cfg](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings).

## [Enabling Inventory Cache Plugins](https://docs.ansible.com/ansible/latest/plugins/cache.html#id3)

Inventory may be cached using a file-based cache plugin (like jsonfile). Check the specific inventory plugin to see if it supports caching. Cache plugins inside a collection are not supported for caching inventory. If an inventory-specific cache plugin is not specified Ansible will fall back to caching inventory with the fact cache plugin options.

The inventory cache is disabled by default. You may enable it via environment variable:

```
export ANSIBLE_INVENTORY_CACHE=True
```

or in the `ansible.cfg` file:

```
[inventory]
cache=True
```

or if the inventory plugin accepts a YAML configuration source, in the configuration file:

```
# dev.aws_ec2.yaml
plugin: aws_ec2
cache: True
```

Similarly with fact cache plugins, only one inventory cache plugin can be active at a time and may be set via environment variable:

```
export ANSIBLE_INVENTORY_CACHE_PLUGIN=jsonfile
```

or in the ansible.cfg file:

```
[inventory]
cache_plugin=jsonfile
```

or if the inventory plugin accepts a YAML configuration source, in the configuration file:

```
# dev.aws_ec2.yaml
plugin: aws_ec2
cache_plugin: jsonfile
```

Consult the individual inventory plugin documentation or the Ansible [configuration](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings) for more details.



## [Using Cache Plugins](https://docs.ansible.com/ansible/latest/plugins/cache.html#id4)

Cache plugins are used automatically once they are enabled.



## [Plugin List](https://docs.ansible.com/ansible/latest/plugins/cache.html#id5)

You can use `ansible-doc -t cache -l` to see the list of available plugins. Use `ansible-doc -t cache <plugin name>` to see specific documentation and examples.

- [jsonfile – JSON formatted files](https://docs.ansible.com/ansible/latest/plugins/cache/jsonfile.html)
- [memcached – Use memcached DB for cache](https://docs.ansible.com/ansible/latest/plugins/cache/memcached.html)
- [memory – RAM backed, non persistent](https://docs.ansible.com/ansible/latest/plugins/cache/memory.html)
- [mongodb – Use MongoDB for caching](https://docs.ansible.com/ansible/latest/plugins/cache/mongodb.html)
- [pickle – Pickle formatted files](https://docs.ansible.com/ansible/latest/plugins/cache/pickle.html)
- [redis – Use Redis DB for cache](https://docs.ansible.com/ansible/latest/plugins/cache/redis.html)
- [yaml – YAML formatted files](https://docs.ansible.com/ansible/latest/plugins/cache/yaml.html)

# Callback Plugins

- [Example callback plugins](https://docs.ansible.com/ansible/latest/plugins/callback.html#example-callback-plugins)
- [Enabling callback plugins](https://docs.ansible.com/ansible/latest/plugins/callback.html#enabling-callback-plugins)
- [Setting a callback plugin for `ansible-playbook`](https://docs.ansible.com/ansible/latest/plugins/callback.html#setting-a-callback-plugin-for-ansible-playbook)
- [Setting a callback plugin for ad-hoc commands](https://docs.ansible.com/ansible/latest/plugins/callback.html#setting-a-callback-plugin-for-ad-hoc-commands)
- [Plugin list](https://docs.ansible.com/ansible/latest/plugins/callback.html#plugin-list)

Callback plugins enable adding new behaviors to Ansible when responding to events. By default, callback plugins control most of the output you see when running the command line programs, but can also be used to add additional output, integrate with other tools and marshall the events to a storage backend.



## [Example callback plugins](https://docs.ansible.com/ansible/latest/plugins/callback.html#id2)

The [log_plays](https://docs.ansible.com/ansible/latest/plugins/callback/log_plays.html#log-plays-callback) callback is an example of how to record playbook events to a log file, and the [mail](https://docs.ansible.com/ansible/latest/plugins/callback/mail.html#mail-callback) callback sends email on playbook failures.

The [say](https://docs.ansible.com/ansible/latest/plugins/callback/say.html#say-callback) callback responds with computer synthesized speech in relation to playbook events.



## [Enabling callback plugins](https://docs.ansible.com/ansible/latest/plugins/callback.html#id3)

You can activate a custom callback by either dropping it into a `callback_plugins` directory adjacent to your play, inside a role, or by putting it in one of the callback directory sources configured in [ansible.cfg](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings).

Plugins are loaded in alphanumeric order. For example, a plugin implemented in a file named 1_first.py would run before a plugin file named 2_second.py.

Most callbacks shipped with Ansible are disabled by default and need to be whitelisted in your [ansible.cfg](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings) file in order to function. For example:

```
#callback_whitelist = timer, mail, profile_roles, collection_namespace.collection_name.custom_callback
```

## [Setting a callback plugin for `ansible-playbook`](https://docs.ansible.com/ansible/latest/plugins/callback.html#id4)

You can only have one plugin be the main manager of your console output. If you want to replace the default, you should define CALLBACK_TYPE = stdout in the subclass and then configure the stdout plugin in [ansible.cfg](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings). For example:

```
stdout_callback = dense
```

or for my custom callback:

```
stdout_callback = mycallback
```

This only affects [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#ansible-playbook) by default.

## [Setting a callback plugin for ad-hoc commands](https://docs.ansible.com/ansible/latest/plugins/callback.html#id5)

The [ansible](https://docs.ansible.com/ansible/latest/cli/ansible.html#ansible) ad hoc command specifically uses a different callback plugin for stdout, so there is an extra setting in [Ansible Configuration Settings](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings) you need to add to use the stdout callback defined above:

```
[defaults]
bin_ansible_callbacks=True
```

You can also set this as an environment variable:

```
export ANSIBLE_LOAD_CALLBACK_PLUGINS=1
```



## [Plugin list](https://docs.ansible.com/ansible/latest/plugins/callback.html#id6)

You can use `ansible-doc -t callback -l` to see the list of available plugins. Use `ansible-doc -t callback <plugin name>` to see specific documents and examples.

- [actionable – shows only items that need attention](https://docs.ansible.com/ansible/latest/plugins/callback/actionable.html)
- [aws_resource_actions – summarizes all “resource:actions” completed](https://docs.ansible.com/ansible/latest/plugins/callback/aws_resource_actions.html)
- [cgroup_memory_recap – Profiles maximum memory usage of tasks and full execution using cgroups](https://docs.ansible.com/ansible/latest/plugins/callback/cgroup_memory_recap.html)
- [cgroup_perf_recap – Profiles system activity of tasks and full execution using cgroups](https://docs.ansible.com/ansible/latest/plugins/callback/cgroup_perf_recap.html)
- [context_demo – demo callback that adds play/task context](https://docs.ansible.com/ansible/latest/plugins/callback/context_demo.html)
- [counter_enabled – adds counters to the output items (tasks and hosts/task)](https://docs.ansible.com/ansible/latest/plugins/callback/counter_enabled.html)
- [debug – formatted stdout/stderr display](https://docs.ansible.com/ansible/latest/plugins/callback/debug.html)
- [default – default Ansible screen output](https://docs.ansible.com/ansible/latest/plugins/callback/default.html)
- [dense – minimal stdout output](https://docs.ansible.com/ansible/latest/plugins/callback/dense.html)
- [foreman – Sends events to Foreman](https://docs.ansible.com/ansible/latest/plugins/callback/foreman.html)
- [full_skip – suppresses tasks if all hosts skipped](https://docs.ansible.com/ansible/latest/plugins/callback/full_skip.html)
- [grafana_annotations – send ansible events as annotations on charts to grafana over http api](https://docs.ansible.com/ansible/latest/plugins/callback/grafana_annotations.html)
- [hipchat – post task events to hipchat](https://docs.ansible.com/ansible/latest/plugins/callback/hipchat.html)
- [jabber – post task events to a jabber server](https://docs.ansible.com/ansible/latest/plugins/callback/jabber.html)
- [json – Ansible screen output as JSON](https://docs.ansible.com/ansible/latest/plugins/callback/json.html)
- [junit – write playbook output to a JUnit file](https://docs.ansible.com/ansible/latest/plugins/callback/junit.html)
- [log_plays – write playbook output to log file](https://docs.ansible.com/ansible/latest/plugins/callback/log_plays.html)
- [logdna – Sends playbook logs to LogDNA](https://docs.ansible.com/ansible/latest/plugins/callback/logdna.html)
- [logentries – Sends events to Logentries](https://docs.ansible.com/ansible/latest/plugins/callback/logentries.html)
- [logstash – Sends events to Logstash](https://docs.ansible.com/ansible/latest/plugins/callback/logstash.html)
- [mail – Sends failure events via email](https://docs.ansible.com/ansible/latest/plugins/callback/mail.html)
- [minimal – minimal Ansible screen output](https://docs.ansible.com/ansible/latest/plugins/callback/minimal.html)
- [nrdp – post task result to a nagios server through nrdp](https://docs.ansible.com/ansible/latest/plugins/callback/nrdp.html)
- [null – Don’t display stuff to screen](https://docs.ansible.com/ansible/latest/plugins/callback/null.html)
- [oneline – oneline Ansible screen output](https://docs.ansible.com/ansible/latest/plugins/callback/oneline.html)
- [osx_say – notify using software speech synthesizer](https://docs.ansible.com/ansible/latest/plugins/callback/osx_say.html)
- [profile_roles – adds timing information to roles](https://docs.ansible.com/ansible/latest/plugins/callback/profile_roles.html)
- [profile_tasks – adds time information to tasks](https://docs.ansible.com/ansible/latest/plugins/callback/profile_tasks.html)
- [say – notify using software speech synthesizer](https://docs.ansible.com/ansible/latest/plugins/callback/say.html)
- [selective – only print certain tasks](https://docs.ansible.com/ansible/latest/plugins/callback/selective.html)
- [skippy – Ansible screen output that ignores skipped status](https://docs.ansible.com/ansible/latest/plugins/callback/skippy.html)
- [slack – Sends play events to a Slack channel](https://docs.ansible.com/ansible/latest/plugins/callback/slack.html)
- [splunk – Sends task result events to Splunk HTTP Event Collector](https://docs.ansible.com/ansible/latest/plugins/callback/splunk.html)
- [stderr – Splits output, sending failed tasks to stderr](https://docs.ansible.com/ansible/latest/plugins/callback/stderr.html)
- [sumologic – Sends task result events to Sumologic](https://docs.ansible.com/ansible/latest/plugins/callback/sumologic.html)
- [syslog_json – sends JSON events to syslog](https://docs.ansible.com/ansible/latest/plugins/callback/syslog_json.html)
- [timer – Adds time to play stats](https://docs.ansible.com/ansible/latest/plugins/callback/timer.html)
- [tree – Save host events to files](https://docs.ansible.com/ansible/latest/plugins/callback/tree.html)
- [unixy – condensed Ansible output](https://docs.ansible.com/ansible/latest/plugins/callback/unixy.html)
- [yaml – yaml-ized Ansible screen output](https://docs.ansible.com/ansible/latest/plugins/callback/yaml.html)