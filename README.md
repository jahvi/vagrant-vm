# Vagrant VM

Base local development config for Vagrant VM

## LAMP Stack

- Apache 2.4
- PHP 7.0
- MariaDB 10.1

## PHP Modules

- cli
- intl
- mcrypt
- gd
- curl
- mbstring
- soap
- zip

## Misc Modules

- XDebug
- Redis
- Mailhog (accesible at servername:8025)

## Extending config settings

The main [config file](https://github.com/jahvi/vagrant-vm/blob/master/puphpet/config.yaml) can be found inside the puphet directory, to extend or override any of its settings create a `config-custom.yaml` file with the options that need changing. For example to change the VM id and hostname the `config-custom.yaml` file should look like this:

```
vagrantfile:
    vm:
        provider:
            local:
                machines:
                    vflm_vnuunqc5363d:
                        id: MyMachine
                        hostname: my-machine.dev
```

### Adding additional virtual hosts

Like changes to the config file additional virtual hosts can be added in the `config-custom.yaml` under the `apache` key. For example the following will create a new virtual host accessible at `project.dev` with the base directory named `project` and redirect all PHP request to the `index.php` file:

```
apache:
    vhosts:
        RandomUniqueString:
            servername: project.dev
            docroot: /var/www/project
            port: '80'
            setenvif:
                - 'Authorization "(.*)" HTTP_AUTHORIZATION=$1'
            custom_fragment: ''
            ssl: '0'
            ssl_cert: ''
            ssl_key: ''
            ssl_chain: ''
            ssl_certs_dir: ''
            ssl_protocol: ''
            ssl_cipher: ''
            directories:
                RandomUniqueString:
                    path: /var/www/project
                    options:
                        - Indexes
                        - FollowSymlinks
                        - MultiViews
                    allow_override:
                        - All
                    require:
                        - 'all granted'
                    custom_fragment: ''
                    files_match:
                        RandomUniqueString:
                            path: \.php$
                            sethandler: 'proxy:fcgi://127.0.0.1:9000'
                            custom_fragment: ''
                            provider: filesmatch
                    provider: directory
```

## Recommended workflow

1. Clone this repo into a directory of choice.
2. Create a new directory inside it for your project.
3. Create a new virtual host and give it a unique `servername`, the document root should point to the directory created in the previous step.
4. Make sure to add the `servername` to the `/etc/hosts` file and point it to the VMs IP (192.168.56.101 by default).
5. Provision VM to apply changes.
