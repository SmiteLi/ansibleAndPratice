# [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id6)

Topics

- Ansible Vault
  - What Can Be Encrypted With Vault
    - [File-level encryption](https://docs.ansible.com/ansible/latest/user_guide/vault.html#file-level-encryption)
    - [Variable-level encryption](https://docs.ansible.com/ansible/latest/user_guide/vault.html#variable-level-encryption)
  - [Vault IDs and Multiple Vault Passwords](https://docs.ansible.com/ansible/latest/user_guide/vault.html#vault-ids-and-multiple-vault-passwords)
  - [Creating Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#creating-encrypted-files)
  - [Editing Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#editing-encrypted-files)
  - [Rekeying Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#rekeying-encrypted-files)
  - [Encrypting Unencrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#encrypting-unencrypted-files)
  - [Decrypting Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#decrypting-encrypted-files)
  - [Viewing Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#viewing-encrypted-files)
  - [Use encrypt_string to create encrypted variables to embed in yaml](https://docs.ansible.com/ansible/latest/user_guide/vault.html#use-encrypt-string-to-create-encrypted-variables-to-embed-in-yaml)
  - Providing Vault Passwords
    - [Labelling Vaults](https://docs.ansible.com/ansible/latest/user_guide/vault.html#labelling-vaults)
    - [Multiple Vault Passwords](https://docs.ansible.com/ansible/latest/user_guide/vault.html#multiple-vault-passwords)
  - [Vault Password Client Scripts](https://docs.ansible.com/ansible/latest/user_guide/vault.html#vault-password-client-scripts)
  - [Speeding Up Vault Operations](https://docs.ansible.com/ansible/latest/user_guide/vault.html#speeding-up-vault-operations)
  - [Vault Format](https://docs.ansible.com/ansible/latest/user_guide/vault.html#vault-format)
  - [Vault Payload Format 1.1 - 1.2](https://docs.ansible.com/ansible/latest/user_guide/vault.html#vault-payload-format-1-1-1-2)

Ansible Vault is a feature of ansible that allows you to keep sensitive data such as passwords or keys in encrypted files, rather than as plaintext in playbooks or roles. These vault files can then be distributed or placed in source control.

To enable this feature, a command line tool - [ansible-vault](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault) - is used to edit files, and a command line flag ([`--ask-vault-pass`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-ask-vault-pass), [`--vault-password-file`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-password-file) or [`--vault-id`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-id)) is used. Alternately, you may specify the location of a password file or command Ansible to always prompt for the password in your ansible.cfg file. These options require no command line flag usage.

For best practices advice, refer to [Variables and Vaults](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#best-practices-for-variables-and-vaults).

Ansible Vault是ansible的一项功能，它使您可以将敏感数据（例如密码或密钥）保留在加密文件中，而不是将其保留为剧本或角色中的纯文本。 然后，可以将这些保管库文件分发或放置在源代码管理中。



要启用此功能，可使用命令行工具ansible-vault编辑文件，并使用命令行标志（--ask-vault-pass，-vault-password-file或--vault-id）。 。 或者，您可以指定密码文件或Ansible命令的位置，以始终提示您在ansible.cfg文件中输入密码。 这些选项不需要使用命令行标志。



有关最佳做法的建议，请参阅变量和保管库。

## [What Can Be Encrypted With Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id7)

### [File-level encryption](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id8)

Ansible Vault can encrypt any structured data file used by Ansible.

This can include “group_vars/” or “host_vars/” inventory variables, variables loaded by “include_vars” or “vars_files”, or variable files passed on the ansible-playbook command line with `-e @file.yml` or `-e @file.json`. Role variables and defaults are also included.

Ansible tasks, handlers, and so on are also data so these can be encrypted with vault as well. To hide the names of variables that you’re using, you can encrypt the task files in their entirety.

Ansible Vault can also encrypt arbitrary files, even binary files. If a vault-encrypted file is given as the `src` argument to the [copy](https://docs.ansible.com/ansible/latest/modules/copy_module.html#copy-module), [template](https://docs.ansible.com/ansible/latest/modules/template_module.html#template-module), [unarchive](https://docs.ansible.com/ansible/latest/modules/unarchive_module.html#unarchive-module), [script](https://docs.ansible.com/ansible/latest/modules/script_module.html#script-module) or [assemble](https://docs.ansible.com/ansible/latest/modules/assemble_module.html#assemble-module) modules, the file will be placed at the destination on the target host decrypted (assuming a valid vault password is supplied when running the play).

Ansible Vault可以加密Ansible使用的任何结构化数据文件。



这可以包括“ group_vars /”或“ host_vars /”库存变量，由“ include_vars”或“ vars_files”加载的变量，或通过-e @ file.yml或-e @file在ansible-playbook命令行上传递的变量文件。 json。 角色变量和默认值也包括在内。



Ansible任务，处理程序等也是数据，因此也可以使用Vault对其进行加密。 要隐藏您正在使用的变量的名称，您可以对任务文件进行整体加密。



Ansible Vault也可以加密任意文件，甚至二进制文件。 如果将保管库加密文件作为副本，模板，未归档，脚本或汇编模块的src参数提供，则该文件将被解密后放置在目标主机上的目标位置（假设在运行播放时提供了有效的保管库密码） ）。

Note

The advantages of file-level encryption are that it is easy to use and that password rotation is straightforward with [rekeying](https://docs.ansible.com/ansible/latest/user_guide/vault.html#rekeying-files). The drawback is that the contents of files are no longer easy to access and read. This may be problematic if it is a list of tasks (when encrypting a variables file, [best practice](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#best-practices-for-variables-and-vaults) is to keep references to these variables in a non-encrypted file).

文件级加密的优点是易于使用，并且通过重新输入密钥即可轻松进行密码轮换。 缺点是文件内容不再易于访问和读取。 如果它是任务列表，则可能会出现问题（在加密变量文件时，最佳实践是将对这些变量的引用保留在未加密的文件中）。

### [Variable-level encryption](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id9)

Ansible also supports encrypting single values inside a YAML file, using the !vault tag to let YAML and Ansible know it uses special processing. This feature is covered in more detail [below](https://docs.ansible.com/ansible/latest/user_guide/vault.html#encrypt-string-for-use-in-yaml).

Ansible还支持使用！vault标记加密YAML文件中的单个值，以使YAML和Ansible知道它使用了特殊处理。 该功能将在下面详细介绍。

Note

The advantage of variable-level encryption is that files are still easily legible even if they mix plaintext and encrypted variables. The drawback is that password rotation is not as simple as with file-level encryption: the [rekey](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault-rekey) command does not work with this method.

可变级别加密的优点是，即使文件混合了纯文本和加密变量，它们仍然易于辨认。 缺点是密码旋转不像使用文件级加密那样简单：rekey命令不适用于此方法。

## [Vault IDs and Multiple Vault Passwords](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id10)

A vault ID is an identifier for one or more vault secrets; Ansible supports multiple vault passwords.

Vault IDs provide labels to distinguish between individual vault passwords.

To use vault IDs, you must provide an ID *label* of your choosing and a *source* to obtain its password (either `prompt` or a file path):

保管库ID是一个或多个保管库秘密的标识符； Ansible支持多个保管库密码。



保管库ID提供标签以区分各个保管库密码。



要使用文件库ID，必须提供您选择的ID标签和获取其密码的来源（提示或文件路径）：

```
--vault-id label@source
```

This switch is available for all Ansible commands that can interact with vaults: [ansible-vault](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault), [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#ansible-playbook), etc.

Vault-encrypted content can specify which vault ID it was encrypted with.

For example, a playbook can now include a vars file encrypted with a ‘dev’ vault ID and a ‘prod’ vault ID.

此开关适用于可与Vault交互的所有Ansible命令：ansible-vault，ansible-playbook等。



保管库加密的内容可以指定用于加密的保管库ID。



例如，剧本现在可以包含使用“开发”文件库ID和“产品”文件库ID加密的vars文件

## [Creating Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id11)

To create a new encrypted data file, run the following command:

要创建新的加密数据文件，请运行以下命令：

```
ansible-vault create foo.yml
```

First you will be prompted for a password. After providing a password, the tool will launch whatever editor you have defined with $EDITOR, and defaults to vi. Once you are done with the editor session, the file will be saved as encrypted data.

The default cipher is AES (which is shared-secret based).

To create a new encrypted data file with the Vault ID ‘password1’ assigned to it and be prompted for the password, run:

首先，将提示您输入密码。 提供密码后，该工具将启动您用$ EDITOR定义的任何编辑器，默认为vi。 完成编辑器会话后，文件将另存为加密数据。



默认密码是AES（基于共享秘密）。



要创建一个新的加密数据文件，并为其分配了保管箱ID“ password1”，并提示输入密码，请运行：

```
ansible-vault create --vault-id password1@prompt foo.yml
```



## [Editing Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id12)

To edit an encrypted file in place, use the [ansible-vault edit](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault-edit) command. This command will decrypt the file to a temporary file and allow you to edit the file, saving it back when done and removing the temporary file:

要在适当位置编辑加密文件，请使用ansible-vault编辑命令。 此命令会将文件解密为一个临时文件，并允许您编辑该文件，完成后将其保存回去并删除该临时文件

```
ansible-vault edit foo.yml
```

To edit a file encrypted with the ‘vault2’ password file and assigned the ‘pass2’ vault ID:

要编辑使用“ vault2”密码文件加密并分配“ pass2”文件库ID的文件，请执行以下操作：

```
ansible-vault edit --vault-id pass2@vault2 foo.yml
```



## [Rekeying Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id13)

Should you wish to change your password on a vault-encrypted file or files, you can do so with the rekey command:

如果您希望在一个或多个文件库加密的文件上更改密码，可以使用rekey命令:

```
ansible-vault rekey foo.yml bar.yml baz.yml
```

This command can rekey multiple data files at once and will ask for the original password and also the new password.

To rekey files encrypted with the ‘preprod2’ vault ID and the ‘ppold’ file and be prompted for the new password:

此命令可以一次重新密钥多个数据文件，并要求输入原始密码和新密码。



要为使用“ preprod2”文件库ID和“ ppold”文件加密的文件重新输入密钥，并提示输入新密码：

```
ansible-vault rekey --vault-id preprod2@ppold --new-vault-id preprod2@prompt foo.yml bar.yml baz.yml
```

A different ID could have been set for the rekeyed files by passing it to `--new-vault-id`.

可以通过将重新传递给--new-vault-id的文件设置一个不同的ID。

## [Encrypting Unencrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id14)

If you have existing files that you wish to encrypt, use the [ansible-vault encrypt](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault-encrypt) command. This command can operate on multiple files at once:

如果您有要加密的现有文件，请使用ansible-vault crypto命令。 此命令可以一次对多个文件进行操作：

```
ansible-vault encrypt foo.yml bar.yml baz.yml
```

To encrypt existing files with the ‘project’ ID and be prompted for the password:

要使用“项目” ID加密现有文件并提示输入密码，请执行以下操作：

```
ansible-vault encrypt --vault-id project@prompt foo.yml bar.yml baz.yml
```

Note

It is technically possible to separately encrypt files or strings with the *same* vault ID but *different* passwords, if different password files or prompted passwords are provided each time. This could be desirable if you use vault IDs as references to classes of passwords (rather than a single password) and you always know which specific password or file to use in context. However this may be an unnecessarily complex use-case. If two files are encrypted with the same vault ID but different passwords by accident, you can use the [rekey](https://docs.ansible.com/ansible/latest/user_guide/vault.html#rekeying-files) command to fix the issue.

如果每次都提供不同的密码文件或提示的密码，则从技术上来说，可以使用相同的保管库ID但使用不同的密码分别加密文件或字符串。 如果您将保管库ID用作密码类（而不是单个密码）的引用，并且始终知道在上下文中使用哪个特定密码或文件，则这可能是理想的。 但是，这可能是不必要的复杂用例。 如果两个文件使用相同的保管库ID加密但偶然使用了不同的密码加密，则可以使用rekey命令解决该问题。

## [Decrypting Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id15)

If you have existing files that you no longer want to keep encrypted, you can permanently decrypt them by running the [ansible-vault decrypt](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault-decrypt) command. This command will save them unencrypted to the disk, so be sure you do not want [ansible-vault edit](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault-edit) instead:

如果您有不再需要加密的现有文件，则可以通过运行ansible-vault解密命令将其永久解密。 此命令会将它们未加密保存到磁盘，因此请确保您不希望使用ansible-vault编辑：

```
ansible-vault decrypt foo.yml bar.yml baz.yml
```



## [Viewing Encrypted Files](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id16)

If you want to view the contents of an encrypted file without editing it, you can use the [ansible-vault view](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault-view) command:

如果要在不编辑的情况下查看加密文件的内容，可以使用ansible-vault view命令：

```
ansible-vault view foo.yml bar.yml baz.yml
```



## [Use encrypt_string to create encrypted variables to embed in yaml](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id17)

The [ansible-vault encrypt_string](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault-encrypt-string) command will encrypt and format a provided string into a format that can be included in [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#ansible-playbook) YAML files.

To encrypt a string provided as a cli arg:

ansible-vault crypto_string命令将加密提供的字符串并将其格式化为可以包含在ansible-playbook YAML文件中的格式。



加密作为客户端提供的字符串：

```
ansible-vault encrypt_string --vault-password-file a_password_file 'foobar' --name 'the_secret'
```

Result:

```
the_secret: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      62313365396662343061393464336163383764373764613633653634306231386433626436623361
      6134333665353966363534333632666535333761666131620a663537646436643839616531643561
      63396265333966386166373632626539326166353965363262633030333630313338646335303630
      3438626666666137650a353638643435666633633964366338633066623234616432373231333331
      6564
```

To use a vault-id label for ‘dev’ vault-id:

要将“ vault-id”标签用于“ dev” vault-id：

```
ansible-vault encrypt_string --vault-id dev@a_password_file 'foooodev' --name 'the_dev_secret'
```

Result:

```
the_dev_secret: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          30613233633461343837653833666333643061636561303338373661313838333565653635353162
          3263363434623733343538653462613064333634333464660a663633623939393439316636633863
          61636237636537333938306331383339353265363239643939666639386530626330633337633833
          6664656334373166630a363736393262666465663432613932613036303963343263623137386239
          6330
```

To encrypt a string read from stdin and name it ‘db_password’:

要加密从stdin读取的字符串并将其命名为“ db_password”，请执行以下操作：

```
echo -n 'letmein' | ansible-vault encrypt_string --vault-id dev@a_password_file --stdin-name 'db_password'
```

Warning

This method leaves the string in your shell history. Do not use it outside of testing.

此方法将字符串保留在您的外壳历史记录中。 不要在测试之外使用它。

Result:

```
Reading plaintext input from stdin. (ctrl-d to end input)
db_password: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          61323931353866666336306139373937316366366138656131323863373866376666353364373761
          3539633234313836346435323766306164626134376564330a373530313635343535343133316133
          36643666306434616266376434363239346433643238336464643566386135356334303736353136
          6565633133366366360a326566323363363936613664616364623437336130623133343530333739
          3039
```

To be prompted for a string to encrypt, encrypt it, and give it the name ‘new_user_password’:

在提示您进行字符串加密时，请对其进行加密，并将其命名为“

```
ansible-vault encrypt_string --vault-id dev@a_password_file --stdin-name 'new_user_password'
```

Output:

```
Reading plaintext input from stdin. (ctrl-d to end input)
```

User enters ‘hunter2’ and hits ctrl-d.

用户输入“ hunter2”并按ctrl-d。

Warning

Do not press Enter after supplying the string. That will add a newline to the encrypted value.

提供字符串后，请勿按Enter。 这将在加密值中添加换行符。

Result:

```
new_user_password: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          37636561366636643464376336303466613062633537323632306566653533383833366462366662
          6565353063303065303831323539656138653863353230620a653638643639333133306331336365
          62373737623337616130386137373461306535383538373162316263386165376131623631323434
          3866363862363335620a376466656164383032633338306162326639643635663936623939666238
          3161
```

See also [Single Encrypted Variable](https://docs.ansible.com/ansible/latest/user_guide/playbooks_vault.html#single-encrypted-variable)

After you added the encrypted value to a var file (vars.yml), you can see the original value using the debug module.

将加密的值添加到var文件（vars.yml）之后，可以使用调试模块查看原始值。

```
ansible localhost -m debug -a var="new_user_password" -e "@vars.yml" --ask-vault-pass
Vault password:

localhost | SUCCESS => {
    "new_user_password": "hunter2"
}
```



## [Providing Vault Passwords](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id18)

When all data is encrypted using a single password the [`--ask-vault-pass`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-ask-vault-pass) or [`--vault-password-file`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-password-file) cli options should be used.

For example, to use a password store in the text file `/path/to/my/vault-password-file`:

当使用单个密码对所有数据进行加密时，应使用--ask-vault-pass或--vault-password-file cli选项。



例如，要在文本文件/ path / to / my / vault-password-file中使用密码存储，请执行以下操作：

```
ansible-playbook --vault-password-file /path/to/my/vault-password-file site.yml
```

To prompt for a password:

提示输入密码：

```
ansible-playbook --ask-vault-pass site.yml
```

To get the password from a vault password executable script `my-vault-password.py`:

要从Vault密码可执行脚本my-vault-password.py获取密码，请执行以下操作：

```
ansible-playbook --vault-password-file my-vault-password.py
```

The config option [DEFAULT_VAULT_PASSWORD_FILE](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#default-vault-password-file) can be used to specify a vault password file so that the [`--vault-password-file`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-password-file) cli option does not have to be specified every time.

可以使用配置选项DEFAULT_VAULT_PASSWORD_FILE指定文件库密码文件，这样就不必每次都指定--vault-password-file cli选项。

### [Labelling Vaults](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id19)

Since Ansible 2.4 the [`--vault-id`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-id) can be used to indicate which vault ID (‘dev’, ‘prod’, ‘cloud’, etc) a password is for as well as how to source the password (prompt, a file path, etc).

By default the vault-id label is only a hint, any values encrypted with the password will be decrypted. The config option [DEFAULT_VAULT_ID_MATCH](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#default-vault-id-match) can be set to require the vault id to match the vault ID used when the value was encrypted. This can reduce errors when different values are encrypted with different passwords.

For example, to use a password file `dev-password` for the vault-id ‘dev’:

从Ansible 2.4开始，--vault-id可用于指示密码用于哪个库ID（“ dev”，“ prod”，“ cloud”等），以及如何获取密码（提示，文件路径） 等）。



默认情况下，vault-id标签只是提示，使用密码加密的任何值都将被解密。 可以将配置选项DEFAULT_VAULT_ID_MATCH设置为要求文件库ID匹配值加密时使用的文件库ID。 当使用不同的密码加密不同的值时，可以减少错误。



例如，要将密码文件dev-password用于文件库ID“ dev”：

```
ansible-playbook --vault-id dev@dev-password site.yml
```

To prompt for the password for the ‘dev’ vault ID:

要提示输入“ dev”文件库ID的密码，请执行以下操作：

```
ansible-playbook --vault-id dev@prompt site.yml
```

To get the ‘dev’ vault ID password from an executable script `my-vault-password.py`:

要从可执行脚本my-vault-password.py获取“ dev”文件库ID密码，请执行以下操作：

```
ansible-playbook --vault-id dev@my-vault-password.py
```

The config option [DEFAULT_VAULT_IDENTITY_LIST](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#default-vault-identity-list) can be used to specify a default vault ID and password source so that the [`--vault-id`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-id) cli option does not have to be specified every time.

The [`--vault-id`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-id) option can also be used without specifying a vault-id. This behaviour is equivalent to [`--ask-vault-pass`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-ask-vault-pass) or [`--vault-password-file`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-password-file) so is rarely used.

For example, to use a password file `dev-password`:

配置选项DEFAULT_VAULT_IDENTITY_LIST可用于指定默认文件库ID和密码源，因此不必每次都指定--vault-id cli选项。



--vault-id选项也可以在不指定vault-id的情况下使用。 此行为等效于--ask-vault-pass或--vault-password-file，因此很少使用。



例如，使用密码文件dev-password：

```
ansible-playbook --vault-id dev-password site.yml
```

To prompt for the password:

提示输入密码：

```
ansible-playbook --vault-id @prompt site.yml
```

To get the password from an executable script `my-vault-password.py`:

要从可执行脚本my-vault-password.py获取密码，请执行以下操作：

```
ansible-playbook --vault-id my-vault-password.py
```

Note

Prior to Ansible 2.4, the [`--vault-id`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-id) option is not supported so [`--ask-vault-pass`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-ask-vault-pass) or [`--vault-password-file`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-password-file) must be used.

在Ansible 2.4之前，不支持--vault-id选项，因此必须使用--ask-vault-pass或--vault-password-file。

### [Multiple Vault Passwords](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id20)

Ansible 2.4 and later support using multiple vault passwords, [`--vault-id`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-id) can be provided multiple times.

For example, to use a ‘dev’ password read from a file and to be prompted for the ‘prod’ password:

Ansible 2.4及更高版本支持使用多个保险库密码，可以多次提供--vault-id。



例如，要使用从文件读取的“开发”密码并提示输入“产品”密码，请执行以下操作：

```
ansible-playbook --vault-id dev@dev-password --vault-id prod@prompt site.yml
```

By default the vault ID labels (dev, prod etc.) are only hints, Ansible will attempt to decrypt vault content with each password. The password with the same label as the encrypted data will be tried first, after that each vault secret will be tried in the order they were provided on the command line.

Where the encrypted data doesn’t have a label, or the label doesn’t match any of the provided labels, the passwords will be tried in the order they are specified.

In the above case, the ‘dev’ password will be tried first, then the ‘prod’ password for cases where Ansible doesn’t know which vault ID is used to encrypt something.

To add a vault ID label to the encrypted data use the [`--vault-id`](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-create-vault-id) option with a label when encrypting the data.

The [DEFAULT_VAULT_ID_MATCH](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#default-vault-id-match) config option can be set so that Ansible will only use the password with the same label as the encrypted data. This is more efficient and may be more predictable when multiple passwords are used.

The config option [DEFAULT_VAULT_IDENTITY_LIST](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#default-vault-identity-list) can have multiple values which is equivalent to multiple [`--vault-id`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-id) cli options.

The [`--vault-id`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-id) can be used in lieu of the [`--vault-password-file`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-vault-password-file) or [`--ask-vault-pass`](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-ask-vault-pass) options, or it can be used in combination with them.

When using [ansible-vault](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault) commands that encrypt content ([ansible-vault encrypt](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault-encrypt), [ansible-vault encrypt_string](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#ansible-vault-encrypt-string), etc) only one vault-id can be used.

默认情况下，保管库ID标签（dev，prod等）只是提示，Ansible将尝试使用每个密码解密保管库内容。 将首先尝试使用与加密数据具有相同标签的密码，然后将按照在命令行中提供密码的顺序尝试每个保管库密钥。



如果加密的数据没有标签，或者标签与提供的任何标签都不匹配，则会按指定的顺序尝试输入密码。



在上述情况下，将首先尝试使用“开发”密码，然后在Ansible不知道使用哪个库ID加密某些内容的情况下，再尝试使用“产品”密码。



要将Vault ID标签添加到加密的数据，请在加密数据时使用带有标签的--vault-id选项。



可以设置DEFAULT_VAULT_ID_MATCH配置选项，以便Ansible将仅使用与加密数据具有相同标签的密码。 当使用多个密码时，这更有效，并且可能更可预测。



配置选项DEFAULT_VAULT_IDENTITY_LIST可以具有多个值，这些值等效于多个--vault-id cli选项。



--vault-id可以代替--vault-password-file文件或--ask-vault-pass选项使用，也可以与它们结合使用。



当使用对内容加密的ansible-vault命令（ansible-vault加密，ansible-vault crypto_string等）时，只能使用一个vault-id。

## [Vault Password Client Scripts](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id21)

When implementing a script to obtain a vault password it may be convenient to know which vault ID label was requested. For example a script loading passwords from a secret manager may want to use the vault ID label to pick either the ‘dev’ or ‘prod’ password.

Since Ansible 2.5 this is supported through the use of Client Scripts. A Client Script is an executable script with a name ending in `-client`. Client Scripts are used to obtain vault passwords in the same way as any other executable script. For example:

在实施脚本以获取文件库密码时，可以方便地知道请求了哪个文件库ID标签。 例如，从机密管理人员加载密码的脚本可能要使用库ID标签来选择“开发”或“生产”密码。



从Ansible 2.5开始，这通过使用客户端脚本来支持。 客户端脚本是名称以-client结尾的可执行脚本。 客户端脚本用于获取保管库密码的方式与其他任何可执行脚本相同。 例如：

```
ansible-playbook --vault-id dev@contrib/vault/vault-keyring-client.py
```

The difference is in the implementation of the script. Client Scripts are executed with a `--vault-id` option so they know which vault ID label was requested. So the above Ansible execution results in the below execution of the Client Script:

区别在于脚本的实现。 客户端脚本使用--vault-id选项执行，因此它们知道请求了哪个Vault ID标签。 因此，上面的Ansible执行导致下面的客户端脚本执行：

```
contrib/vault/vault-keyring-client.py --vault-id dev
```

`contrib/vault/vault-keyring-client.py` is an example of Client Script that loads passwords from the system keyring.

contrib / vault / vault-keyring-client.py是客户端脚本的示例，该脚本从系统密钥环加载密码。

## [Speeding Up Vault Operations](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id22)

If you have many encrypted files, decrypting them at startup may cause a perceptible delay. To speed this up, install the cryptography package:

```
pip install cryptography
```

如果您有许多加密文件，则在启动时对其进行解密可能会导致明显的延迟。 为了加快速度，请安装加密软件包：

## [Vault Format](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id23)

A vault encrypted file is a UTF-8 encoded txt file.

The file format includes a newline terminated header.

For example:

保管库加密文件是UTF-8编码的txt文件。



文件格式包括换行符终止的标头。



例如：

```
$ANSIBLE_VAULT;1.1;AES256
```

or:

```
$ANSIBLE_VAULT;1.2;AES256;vault-id-label
```

The header contains the vault format id, the vault format version, the vault cipher, and a vault-id label (with format version 1.2), separated by semi-colons ‘;’

The first field `$ANSIBLE_VAULT` is the format id. Currently `$ANSIBLE_VAULT` is the only valid file format id. This is used to identify files that are vault encrypted (via vault.is_encrypted_file()).

The second field (`1.X`) is the vault format version. All supported versions of ansible will currently default to ‘1.1’ or ‘1.2’ if a labeled vault-id is supplied.

The ‘1.0’ format is supported for reading only (and will be converted automatically to the ‘1.1’ format on write). The format version is currently used as an exact string compare only (version numbers are not currently ‘compared’).

The third field (`AES256`) identifies the cipher algorithm used to encrypt the data. Currently, the only supported cipher is ‘AES256’. [vault format 1.0 used ‘AES’, but current code always uses ‘AES256’]

The fourth field (`vault-id-label`) identifies the vault-id label used to encrypt the data. For example using a vault-id of `dev@prompt` results in a vault-id-label of ‘dev’ being used.

Note: In the future, the header could change. Anything after the vault id and version can be considered to depend on the vault format version. This includes the cipher id, and any additional fields that could be after that.

The rest of the content of the file is the ‘vaulttext’. The vaulttext is a text armored version of the encrypted ciphertext. Each line will be 80 characters wide, except for the last line which may be shorter.

标头包含保管库格式ID，保管库格式版本，保管库密码和保管库ID标签（格式版本为1.2），以分号“;”分隔



第一个字段$ ANSIBLE_VAULT是格式ID。 当前$ ANSIBLE_VAULT是唯一有效的文件格式ID。 这用于标识通过Vault加密的文件（通过vault.is_encrypted_file（））。



第二个字段（1.X）是库格式版本。 如果提供了带标签的库ID，则所有支持的ansible版本当前默认为“ 1.1”或“ 1.2”。



“ 1.0”格式仅支持读取（写入时会自动转换为“ 1.1”格式）。 格式版本当前仅用作精确字符串比较（当前版本号未“比较”）。



第三个字段（AES256）标识用于加密数据的密码算法。 目前，唯一受支持的密码是“ AES256”。 [vault格式1.0使用'AES'，但当前代码始终使用'AES256']



第四个字段（文件库ID标签）标识用于加密数据的文件库ID标签。 例如，使用dev @ prompt的vault-id会导致使用“ dev”的vault-id-label。



注意：将来，标题可能会更改。 保管库ID和版本之后的任何内容都可以视为取决于保管库格式版本。 这包括密码ID，以及之后的所有其他字段。



文件的其余内容是“ vaulttext”。 Vault文本是加密密文的文本防护版本。 每行将有80个字符宽，但最后一行可能会更短

## [Vault Payload Format 1.1 - 1.2](https://docs.ansible.com/ansible/latest/user_guide/vault.html#id24)

The vaulttext is a concatenation of the ciphertext and a SHA256 digest with the result ‘hexlifyied’.

‘hexlify’ refers to the `hexlify()` method of the Python Standard Library’s [binascii](https://docs.python.org/3/library/binascii.html) module.

hexlify()’ed result of:

- hexlify()’ed string of the salt, followed by a newline (`0x0a`)

- hexlify()’ed string of the crypted HMAC, followed by a newline. The HMAC is:

  - a RFC2104 style HMAC

    - inputs are:
  - The AES256 encrypted ciphertext
      - A PBKDF2 key. This key, the cipher key, and the cipher IV are generated from:
    - the salt, in bytes
        - 10000 iterations
    - SHA256() algorithm
        - the first 32 bytes are the cipher key
    - the second 32 bytes are the HMAC key
        - remaining 16 bytes are the cipher IV
  
- hexlify()’ed string of the ciphertext. The ciphertext is:

> - AES256 encrypted data. The data is encrypted using:
>   - AES-CTR stream cipher
>   - cipher key
>   - IV
>   - a 128 bit counter block seeded from an integer IV
>   - the plaintext
>     - the original plaintext
>     - padding up to the AES256 blocksize. (The data used for padding is based on [RFC5652](https://tools.ietf.org/html/rfc5652#section-6.3))