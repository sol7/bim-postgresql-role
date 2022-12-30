# Ansible Role: PostgreSQL

Instala e configura o servidor PostgreSQL nos servidores RHEL/CENTOS ou Debian/Ubuntu.

## Requisitos

Nenhum requesito especial

    - hosts: database
      collections:
        - community.postgresql
      roles:
        - role: bim-postgresql-role
      become: true
      gather_facts: true

## Role Variáveis

As variáveis disponíveis estão listadas abaixo, juntamente com os valores padrão (veja `defaults/main.yml`):

    postgresql_global_config_options:
      - option: unix_socket_directories
        value: '{{ postgresql_unix_socket_directories | join(",") }}'
      - option: log_directory
        value: 'log'

Opções de configuração global que serão definidas em `postgresql.conf`.

    postgresql_hba_entries:
      - { type: local, database: all, user: postgres, auth_method: peer }
      - { type: local, database: all, user: all, auth_method: peer }
      - { type: host, database: all, user: all, address: '127.0.0.1/32', auth_method: md5 }
      - { type: host, database: all, user: all, address: '::1/128', auth_method: md5 }

configura [host based authentication](https://www.postgresql.org/docs/current/static/auth-pg-hba-conf.html) entradas definidas no `pg_hba.conf`. As opções incluem:

  - `type` (required)
  - `database` (required)
  - `user` (required)
  - `address` (one of this or the following two are required)
  - `ip_address`
  - `ip_mask`
  - `auth_method` (required)
  - `auth_options` (optional)

    postgresql_databases:
      - name: exampledb # required; the rest are optional
        lc_collate: # defaults to 'en_US.UTF-8'
        lc_ctype: # defaults to 'en_US.UTF-8'
        encoding: # defaults to 'UTF-8'
        template: # defaults to 'template0'
        login_host: # defaults to 'localhost'
        login_password: # defaults to not set
        login_user: # defaults to 'postgresql_user'
        login_unix_socket: # defaults to 1st of postgresql_unix_socket_directories
        port: # defaults to not set
        owner: # defaults to postgresql_user
        state: # defaults to 'present'

Bancos de dados que devem existir no servidor. Somente o `name` é necessário. Todas as outras propriedades são opcionais.

    postgresql_users:
      - name: jdoe #required; the rest are optional
        password: # defaults to not set
        encrypted: # defaults to not set
        priv: # defaults to not set
        role_attr_flags: # defaults to not set
        db: # defaults to not set
        login_host: # defaults to 'localhost'
        login_password: # defaults to not set
        login_user: # defaults to '{{ postgresql_user }}'
        login_unix_socket: # defaults to 1st of postgresql_unix_socket_directories
        port: # defaults to not set
        state: # defaults to 'present'

Lista de usuários que devem existir no servidor. Somente o `name` é necessário. Todas as outras propriedades são opcionais.

    postgresql_version: [OS-specific]
    postgresql_data_dir: [OS-specific]
    postgresql_bin_path: [OS-specific]
    postgresql_config_path: [OS-specific]
    postgresql_daemon: [OS-specific]

Variáveis específicas por Sistema Operacional definidas ao incluir os arquivos do diretório `vars`.

## Exemplo de manual

    - hosts: database
      become: yes
      vars_files:
        - vars/main.yml
      roles:
        - bim-postgresql-role

*Inside `vars/main.yml`*:

    postgresql_databases:
      - name: example_db
    postgresql_users:
      - name: example_user
        password: supersecure

## License

Proprietária

## Author Information

FelipeDie
