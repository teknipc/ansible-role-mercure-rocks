Ansible Role: Mercure
=========

An Ansible role to deploy Mercure on Linux amd64 with systemd.

Mercure is a protocol allowing to push data updates to web browsers and other HTTP clients in a convenient, fast, reliable and battery-efficient way. It is especially useful to publish async and real-time updates of resources served through web APIs, to reactive web and mobile apps.

Visit [Mercure github repository](https://github.com/dunglas/mercure)

Requirements
------------

Linux OS with amd64 architecture and systemd as service manager.

Role Variables
--------------

### Version release ###

You can define the version to install with the `mercure_rocks_release` variable. i.e:

	mercure_rocks_release: "0.9.0"

If you don't define this variable, the latest release will be installed. In this case, if you replay your playbook and a new release is out, Mercure will be updated.

### Predefined Variables ###

    mercure_rocks_user: mercure

System user and group

    mercure_rocks_addr: "127.0.0.1:3000"

The address and port to listen on

    mercure_rocks_compress: "true"

HTTP compression support. Set to `"false"` to disable

    mercure_rocks_cors_allowed_origins: "*"

A list of allowed CORS origins

    mercure_rocks_debug: "false"

Set to `"true"` to enable the debug mode

    mercure_rocks_demo: "false"

Set to `"true"` to enable the demo mode (automatically enabled when debug=true)

    mercure_rocks_log_format: "TEXT"

The log format, can be `JSON`, `FLUENTD` or `TEXT` (default)

    mercure_rocks_transport_url: "null://"

URL representation of the history database. Provided database are `"null://"` to disabled history, `"bolt://"` to use bbolt (example `"bolt:///var/run/mercure.db?size=100&cleanup_frequency=0.4"`)

### Security ###

Security in Mercure is based on [JWT](https://jwt.io/).

You have 3 options:

* Define the same JWT secret key for *subscribers* and *publishers*
* Define two different keys for *subscribers* and *publishers*
* Define a key for *publishers* and allow anonymous connection for *subscribers* without any auth

To generate secret keys you can use online generators like [https://www.allkeysgenerator.com/Random/Security-Encryption-Key-Generator.aspx](https://www.allkeysgenerator.com/Random/Security-Encryption-Key-Generator.aspx)

#### Define the same JWT secret key for subscribers and publishers ####

Define `mercure_rocks_jwt_key` and `mercure_rocks_jwt_algorithm` variables, for example :

    mercure_rocks_jwt_key: "n2r5u8x!A%D*G-KaPdSgVkYp3s6v9y$B"
    mercure_rocks_jwt_algorithm: "HS256"

`mercure_rocks_jwt_algorithm` can be set to `"HS256"` or `"RS512"`

#### Define two different keys for subscribers and publishers ####

Define `mercure_rocks_publisher_jwt_key`, `mercure_rocks_publisher_jwt_algorithm`, `mercure_rocks_subscriber_jwt_key` and `mercure_rocks_subscriber_jwt_algorithm` variables based on the same schema than earlier

#### Define a key for publishers and allow anonymous connection for subscribers ####

Define `mercure_rocks_publisher_jwt_key` and `mercure_rocks_publisher_jwt_algorithm` variables and set `mercure_rocks_allow_anonymous` to `"true"`

See all options on [https://mercure.rocks/docs/hub/config](https://mercure.rocks/docs/hub/config). Just prefix them with `mercure_rocks_` in the role.

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
        - { role: teknipc.mercure }

in `vars/main.yml`

    mercure_rocks_jwt_key: "n2r5u8x!A%D*G-KaPdSgVkYp3s6v9y$B"
    mercure_rocks_jwt_algorithm: "HS256"
    mercure_rocks_release: "0.9.0"

License
-------

BSD

Author Information
------------------

This role was created by Denis Soriano from TeKniPC.

Mercure credits :

* Lead developper: [Kevin Dunglas](https://dunglas.fr/)
* [Mercure website](https://mercure.rocks/)
* [Mercure github repository](https://github.com/dunglas/mercure)
* Mercure is sponsored by [Les-Tilleuls.coop](https://les-tilleuls.coop/)
