Role Name
=========

    Deploy Graylog v2 standalone or cluster
    Cluster can be with 2+ graylog nodes (or 3+ with etcd) and 3 nodes for ElasticSearch/MongoDB

Requirements
------------

    Apache2
    Elasticsearch 2.1.x or later (support 2.3.1)
    MongoDB 2.4 or newer. MongoDB recommend using MongoDB 3.x and the WiredTiger storage engine.
    Oracle Java SE 8 or later

Role Variables
--------------

    ## Package ##

    graylog_apt_key: "http://mirror.services.local/pubkey/ael.scs.systeme.key"
    graylog_apt_repo: "deb [arch=amd64] http://mirror.services.local/graylog_2.0/ trusty main"

    ## LV ##
    graylog_lv_name: "lv_graylog"
    graylog_vg_root: "vg_vroot"
    graylog_lvsize: "10G"
    graylog_lv_create: yes

    ## JVM ##
    graylog_xms: "1G"
    graylog_xmx: "1G"
    graylog_extra_java_args: ""

    ## Templates ##
    graylog_config_template_file: "server.conf"
    graylog_default_config_template_file: "default_graylog-server"
    graylog_master_node_mode: "true"

    ## Configuration ##
    graylog_cluster: no
    ## Example : pwgen -N 1 -s 96 #
    graylog_password_secret: "c4b1aGGc3OC6FzuYPzECSoFf2q3wWqN7cfmxDrVElzNDkvlnaFQhGvgOVII9ivDB9mjImFwqhHWsT7Tu9q61MRfCpiScHg5A"
    ## Example : echo -n yourpassword | shasum -a 256 ##
    graylog_root_password_sha2: "68bb07c845116797bd6038055d19c57a53d6aa9f027e5b02c7282644de46d756"
    graylog_rest_listen_uri: "http://127.0.0.1:12900/"
    graylog_web_listen_uri: "http://127.0.0.1:9000/"

    graylog_elasticsearch_shards: "1"
    graylog_elasticsearch_replicas: "1"
    graylog_elasticsearch_max_number_of_indices: 20
    graylog_elasticsearch_max_docs_per_index: 20000000
    # It supports multiple rotation strategies:
    #   - "count" of messages per index, use elasticsearch_max_docs_per_index below to configure
    #   - "size" per index, use elasticsearch_max_size_per_index below to configure
    # valid values are "count", "size" and "time", default is "count"
    graylog_rotation_strategy: count
    graylog_elasticsearch_max_time_per_index: 1d
    graylog_elasticsearch_max_size_per_index: 1073741824
    graylog_elasticsearch_cluster_name: "graylog"
    graylog_elasticsearch_discovery_zen_ping_multicast_enabled: "false"
    graylog_elasticsearch_discovery_zen_ping_unicast_hosts: "127.0.0.1:9300"

    graylog_mongodb_user: "grayloguser"
    graylog_mongodb_password: "monpassword"
    graylog_mongodb_bdd: "graylog"
    graylog_mongodb_uri: "{{ graylog_mongodb_user }}:{{ graylog_mongodb_password }}@localhost:27017/{{ graylog_mongodb_bdd }}"
    graylog_mongodb_master_host: "localhost"

    graylog_rest_api_listen_port: "12900"

    graylog_root_timezone: "Europe/Paris"

    java_bin: "/usr/lib/jvm/jre-8-oracle-x64/bin/java"
    graylog_apache_server_cert: ""
    graylog_apache_server_key: ""


Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

    graylog_mongodb_admin_password: "osef"
    graylog_password_secret: "c4b1aGGc3OC6FzuYPzECSoFf2q3wWqN7cfmxDrVElzNDkvlnaFQhGvgOVII9ivDB9mjImFwqhHWsT7Tu9q61MRfCpiScHg5A"
    graylog_root_password_sha2: "68bb07c845116797bd6038055d19c57a53d6aa9f027e5b02c7282644de46d756"
    graylog_rest_listen_uri: "http://127.0.0.1:12900/"
    graylog_web_listen_uri: "http://127.0.0.1:12900/"
    graylog_elasticsearch_shards: "1"
    graylog_elasticsearch_replicas: "0"
    graylog_elasticsearch_cluster_name: "graylog"
    graylog_elasticsearch_discovery_zen_ping_multicast_enabled: "false"
    graylog_elasticsearch_discovery_zen_ping_unicast_hosts: "127.0.0.1:9300"
    graylog_mongodb_user: "grayloguser"
    graylog_mongodb_password: "osef"
    graylog_mongodb_bdd: "graylog"
    graylog_mongodb_uri: "{{ graylog_mongodb_user }}:{{ graylog_mongodb_password }}@localhost:27017/{{ graylog_mongodb_bdd }}"

    ## OPTIONNEL ##
    graylog_root_email: jlesaux.externe@pagesjaunes.fr
    ## voir http://www.joda.org/joda-time/timezones.html ##
    graylog_root_timezone: "Europe/Paris"

    ## APACHE ##
    graylog_apache_fqdn: "jlsmongo01l.btsys.local"
    graylog_rest_api_listen_port: "12900"
    graylog_apache_server_cert: |
      -----BEGIN CERTIFICATE-----
      MIIDoTCCAomgAwIBAgIJAMoE/oULXStmMA0GCSqGSIb3DQEBCwUAMGcxCzAJBgNV
      BAYTAkZSMRMwEQYDVQQIDApTb21lLVN0YXRlMSEwHwYDVQQKDBhJbnRlcm5ldCBX
      aWRnaXRzIFB0eSBMdGQxIDAeBgNVBAMMF2psc21vbmdvMDFsLmJ0c3lzLmxvY2Fs
      AUrs7PpTVZGo3F/aCfZ81tRlE8on
      -----END CERTIFICATE-----
    graylog_apache_server_key: |
      -----BEGIN PRIVATE KEY-----
      MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQDa3VAs1mb6gajR
      FzvX7hL1EgDG4MhZtPZrwI/2Qwk2JIwYUt2dqefhUficdHUDJp41/1vEgj5eLaVT
      v09url4kI+cSfbte49kDT4P8Gf4/St7kecToKuVolPtAG8JfBYDSKmAbanGXM0bz
      AsEIZmite0aLn78kXvP/uPY=
      -----END PRIVATE KEY-----


Example Playbook for complete install with mongo, apache and elasticsearch
----------------------------------------------------------------------------
    - hosts: jlsmongo
      become: yes
      roles:
        - { role: ansible-apache }
        - { role: ansible-mongodb }
        - { role: ansible-java }
        - { role: ansible-elasticsearch }
        - { role: ansible-graylog }
      vars:
      ## APACHE ##
        apache_mod_list:
          - headers
          - ssl
          - rewrite
          - proxy
          - proxy_http
        apache_mpm_list:
          - worker
        apache_use_lvm: true
        apache_lv_size: 1g

        ## JAVA ##
        java_version: 8
        install_jdk: 0
        install_jre: 1
        install_openjdk: 0

        ## ELASTICSEARCH ##
        es_version: "2.3.3"
        es_lv_create: yes
        es_lvsize: "2G"
        es_cluster_name: "graylog"
        es_localhost_only: 0
        es_heap_size: "2G"
        es_max_locked_memory: "unlimited"
        es_cluster_group: "trusty"
        es_firewall: no
        es_unicast_hostlist:
          - jlsmongo01l.btsys.local

        ## MONGODB ##
        mongodb_major_version: "3.2"
        mongodb_minor_version: "latest"
        mongodb_engine: "wiredTiger"
        mongodb_cluster: "false"
        mongodb_bind_ip: 0.0.0.0
        mongodb_cluster_name: "mongograylog"
        mongodb_admin_password: "osef"
        mongodb_backup_password: "osef"
        mongodb_cluster_key: clusterkeyyyyy

        ## GRAYLOG ##
        ## A METTRE DANS LE ROLE GRAYLOG ##
        graylog_mongodb_admin_password: "osef"
        graylog_password_secret: "c4b1aGGc3OC6FzuYPzECSoFf2q3wWqN7cfmxDrVElzNDkvlnaFQhGvgOVII9ivDB9mjImFwqhHWsT7Tu9q61MRfCpiScHg5A"
        graylog_root_password_sha2: "68bb07c845116797bd6038055d19c57a53d6aa9f027e5b02c7282644de46d756"
        graylog_rest_listen_uri: "http://127.0.0.1:12900/"
        graylog_web_listen_uri: "http://127.0.0.1:9000/"
        graylog_elasticsearch_shards: "1"
        graylog_elasticsearch_replicas: "0"
        graylog_elasticsearch_cluster_name: "graylog"
        graylog_elasticsearch_discovery_zen_ping_multicast_enabled: "false"
        graylog_elasticsearch_discovery_zen_ping_unicast_hosts: "127.0.0.1:9300"
        graylog_mongodb_user: "grayloguser"
        graylog_mongodb_password: "osef"
        graylog_mongodb_bdd: "graylog"
        graylog_mongodb_uri: "{{ graylog_mongodb_user }}:{{ graylog_mongodb_password }}@localhost:27017/{{ graylog_mongodb_bdd }}"

        ## OPTIONNEL ##
        graylog_root_email: jlesaux.externe@pagesjaunes.fr
        ## voir http://www.joda.org/joda-time/timezones.html ##
        graylog_root_timezone: "Europe/Paris"

        ## APACHE ##
        graylog_apache_fqdn: "jlsmongo01l.btsys.local"
        graylog_apache_server_cert: |
          -----BEGIN CERTIFICATE-----
          MIIDoTCCAomgAwIBAgIJAMoE/oULXStmMA0GCSqGSIb3DQEBCwUAMGcxCzAJBgNV
          YEv4Hci8inbIa86fVCx4I53DvQJEle8Ewvla2oA2lNK8YRGsbpHgru/FfeP1iOPJ
          AUrs7PpTVZGo3F/aCfZ81tRlE8on
          -----END CERTIFICATE-----
        graylog_apache_server_key: |
          -----BEGIN PRIVATE KEY-----
          MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQDa3VAs1mb6gajR
          FzvX7hL1EgDG4MhZtPZrwI/2Qwk2JIwYUt2dqefhUficdHUDJp41/1vEgj5eLaVT
          XaE6UFwKJkxs5XU2UmfYv6QSN8+PWE+xVOdUhj+erbqIUaQi93BFWSpU8PlLtiCb
          AsEIZmite0aLn78kXvP/uPY=
          -----END PRIVATE KEY-----

Playbook for full cluster installation (3 graylog servers with etcd and 3 mongo/es servers)
----------------------------------------------------------------------------------------------
    - name: Deploy MongoDB and ElasticSearch Cluster
      hosts: trusty
      become: yes
      roles:
        - { role: ansible-mongodb }
        - { role: ansible-java }
        - { role: ansible-elasticsearch }
      vars:
        ## JAVA ##
        java_version: 8
        install_jdk: 0
        install_jre: 1
        install_openjdk: 0
        ## ELASTICSEARCH ##
        es_version: "2.3.3"
        es_lv_create: yes
        es_lvsize: "10G"
        es_cluster_name: "graylog"
        es_localhost_only: 0
        es_heap_size: "2G"
        es_max_locked_memory: "unlimited"
        es_cluster_group: "trusty"
        es_prod_interface: "eth0"
        es_firewall: no
        es_discovery_zen_minimum_master_nodes: 2
        es_unicast_hostlist:
          - "jlstrusty01l.btsys.local"
          - "jlstrusty02l.btsys.local"
          - "jlstrusty03l.btsys.local"
        ## MONGODB ##
        mongodb_major_version: "3.2"
        mongodb_minor_version: "latest"
        mongodb_engine: "wiredTiger"
        #mongodb_cluster: "false"
        mongodb_cluster: "true"
        mongodb_bind_ip: 0.0.0.0
        mongodb_cluster_name: "mongograylog"
        mongodb_admin_password: "osef"
        mongodb_backup_password: "osef"
        mongodb_cluster_key: clusterkeyyyyy
        mongodb_lv_size: 2g

    - name: Deploy Two Graylog Servers and keepalived
      hosts: graylogserver
      become: yes
      roles:
        - { role: ansible-apache }
        - { role: ansible-java }
        - { role: ansible-graylog }
        - { role: ansible-etcd }
        - { role: ansible-keepalived }

      vars:
        ## APACHE ##
        apache_mod_list:
          - headers
          - ssl
          - rewrite
          - proxy
          - proxy_http
        apache_mpm_list:
          - worker
        apache_use_lvm: true
        apache_lv_size: 1g

        ## JAVA ##
        java_version: 8
        install_jdk: 0
        install_jre: 1
        install_openjdk: 0

        ## GRAYLOG ##
        graylog_cluster: yes
        graylog_xms: "2G"
        graylog_xmx: "2G"
        graylog_mongodb_admin_password: "osef"
        graylog_password_secret: "c4b1aGGc3OC6FzuYPzECSoFf2q3wWqN7cfmxDrVElzNDkvlnaFQhGvgOVII9ivDB9mjImFwqhHWsT7Tu9q61MRfCpiScHg5A"
        graylog_root_password_sha2: "68bb07c845116797bd6038055d19c57a53d6aa9f027e5b02c7282644de46d756"
        graylog_rest_listen_uri: "http://127.0.0.1:12900/"
        graylog_web_listen_uri: "http://127.0.0.1:9000/"
        graylog_elasticsearch_shards: "1"
        graylog_elasticsearch_replicas: "1"
        graylog_elasticsearch_max_number_of_indices: 42
        graylog_elasticsearch_max_docs_per_index: 20000000
        graylog_rotation_strategy: time
        graylog_elasticsearch_max_time_per_index: 42d
        graylog_elasticsearch_max_size_per_index: 1073741824
        graylog_elasticsearch_cluster_name: "graylog"
        graylog_elasticsearch_discovery_zen_ping_multicast_enabled: "false"
        graylog_elasticsearch_discovery_zen_ping_unicast_hosts: "jlstrusty01l.btsys.local:9300,jlstrusty02l.btsys.local:9300,jlstrusty03l.btsys.local:9300"
        graylog_mongodb_user: "grayloguser"
        graylog_mongodb_password: "osef"
        graylog_mongodb_bdd: "graylog"
        graylog_elasticsearch_network_host: "0.0.0.0"
        graylog_mongodb_uri: "{{ graylog_mongodb_user }}:{{ graylog_mongodb_password }}@jlstrusty01l.btsys.local:27017,jlstrusty02l.btsys.local:27017,jlstrusty03l.btsys.local:27017/{{ graylog_mongodb_bdd }}"
        graylog_mongodb_master_host: "jlstrusty01l.btsys.local"

        ## OPTIONNEL ##
        graylog_root_email: jlesaux.externe@pagesjaunes.fr
        ## voir http://www.joda.org/joda-time/timezones.html ##
        graylog_root_timezone: "Europe/Paris"

        ## APACHE ##
        graylog_apache_fqdn: "jlstrustyvip01l.btsys.local"
        graylog_rest_api_listen_port: "12900"
        graylog_apache_server_cert: |
          -----BEGIN CERTIFICATE-----
          MIID9TCCAt2gAwIBAgIBejANBgkqhkiG9w0BAQ0FADBeMQswCQYDVQQGEwJGUjER
          MA8GA1UECAwIQnJldGFnbmUxFjAUBgNVBAoMDVNvbG9jYWwgR3JvdXAxETAPBgNV
          BAsMCFN5c3RlbWVzMREwDwYDVQQDDAhJbmZyYSBDQTAgFw0xNjA1MTcxMjU3MTNa
          GA8yMTE2MDQyMzEyNTcxM1owcTELMAkGA1UEBhMCRlIxETAPBgNVBAgMCEJyZXRh
          Z25lMRYwFAYDVQQKDA1Tb2xvY2FsIEdyb3VwMREwDwYDVQQLDAhTeXN0ZW1lczEk
          MCIGA1UEAwwbamxzdHJ1c3R5dmlwMDFsLmJ0c3lzLmxvY2FsMIIBIjANBgkqhkiG
          9w0BAQEFAAOCAQ8AMIIBCgKCAQEA4I+aNHNTEqDtSpNdUjwTk7qF2t1jjY8xwkYn
          Sl71RiMjMP0OK8kCnKsIVWrJVhpX/XPtGO7kf3rqilDCuJMeuGoXX76dTEz4rND2
          r0XygDWGayE/qJdHVKC1LToQMzVki+VfR2dzAmXw7DLsA7thzHzBBqxP74xtWzpM
          K//1O5hbpdtxMfPaTtfJpfvJQ7scvQzQcZj12g9blwwzOI72e11duSKzxuC0T5Mk
          14tIFgXen3xBPPZRuPG3jJL+/+F+59PbYUnhuSVfQUVxkUytKBOUxTW1R1zpiA/f
          zNDw9Gi/YiXQeFkrr4xb3EujfWUoh0O1trPxE6ox6XK9f+RbjwIDAQABo4GoMIGl
          MAkGA1UdEwQCMAAwHQYDVR0OBBYEFHNct/GrnJjUpopLCIioxe2nN/HKMB8GA1Ud
          IwQYMBaAFLnOX8LmY7aXmZsoGzc6o9uB8HKZMDYGA1UdHwQvMC0wK6ApoCeGJWh0
          dHA6Ly9jYS5zZXJ2aWNlcy5sb2NhbC9JbmZyYV9DQS5jcmwwCwYDVR0PBAQDAgXg
          MBMGA1UdJQQMMAoGCCsGAQUFBwMBMA0GCSqGSIb3DQEBDQUAA4IBAQABl7CQ+e2T
          kvFf2C2GWJSqosYWqlzE7TipSCUZmX3Ov2QEfxR67SuRSdfgV3h6VjIQJaLckXNa
          Xq8zF0eRdzLAjWCgPLYtA55zvilod7ebPo0kEVZZWAVbKAugrSuQN2GPSf7xbSGo
          IKfNVDZbfX22y3pJZ8RLNcMnLVR8LxTOvyq4HcyZ2K7YL/uUqmUrU3+oJ7as8h2Q
          pHzb0HMpVNeP7hXfYQY0f7RQGdF3PVLTAbGY0XJQlRjzxusbvJwQt+GjHMhRY2l4
          DcBOTCxdcl19nR05wknpGkinp141oXW5zMtgKVfJdnhRMf+il1oZj/zK+MS6WDvp
          r8UDvQb3j6Gm
          -----END CERTIFICATE-----
        graylog_apache_server_key: |
          -----BEGIN PRIVATE KEY-----
          MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDgj5o0c1MSoO1K
          k11SPBOTuoXa3WONjzHCRidKXvVGIyMw/Q4ryQKcqwhVaslWGlf9c+0Y7uR/euqK
          UMK4kx64ahdfvp1MTPis0PavRfKANYZrIT+ol0dUoLUtOhAzNWSL5V9HZ3MCZfDs
          MuwDu2HMfMEGrE/vjG1bOkwr//U7mFul23Ex89pO18ml+8lDuxy9DNBxmPXaD1uX
          DDM4jvZ7XV25IrPG4LRPkyTXi0gWBd6ffEE89lG48beMkv7/4X7n09thSeG5JV9B
          RXGRTK0oE5TFNbVHXOmID9/M0PD0aL9iJdB4WSuvjFvcS6N9ZSiHQ7W2s/ETqjHp
          cr1/5FuPAgMBAAECggEAF2uCpqO8bz3vYY669d+kHw0d9rSONG5RvzQ31s7Of9Ec
          U3ig6LofFp3T0azAcMVwldNoT+hiVlVIbsJ7fGqDkrIl2+tyVknUyZlFkQQXhX2P
          lk6yZ+/XFaFpI91hYSXZZam3ZSgJ258sIEYzTTRWv8/nBb4k3BPmN9R9qz0Xo25P
          XzMrQW7pe/uXU2wLvWEzf91ZybKPT2X5jMGjEFH7v16gi5U4gIeMQ7jFFVEsbxv/
          up10UIBfxU+OLaF75tzlSlVxFeBMmktXaODGaYDVWCkGngMSupj/Y/x35IdHAz5W
          mvtb+y8Ltfv6B/K0y+X3jqM9a87Xj60pWKxg9iXAaQKBgQD3aE3TpFDc1Wc3vU9/
          /oPIgmOQac+Zaclc7KCoouL7hodZt6WzhmkMetL/koB9bbvjrxrlKC+DYjLYJIFn
          +dQitmMmOQ/WLCCnWXUQPmk0Btn8ECJ49nvuOVobNbR41sOu0MLPSHC6iJK7eZ+Y
          KksQCl0DHw+r/qo52EaDpyZYTQKBgQDoXCunyw4v93h2FHFwgatnhACRrc7xBXFx
          leNdRD6S5SqEBETHZ9R5mjVPU91LQ8hukyZVmmzUpJnP5LqtEjt/gQDQ5hsD2boV
          RVSVOZkFDyh8/V+dVbh+eRN7qrsa8o8o3R7gbofQXCJp5GxrQoS/lhP6Ry4WeDBv
          Glv+bKjxSwKBgGeUpbDMBIbQWax+d8BQoH/cBy84/Y9vOLzM3N59g6ZmxlgLiTZG
          Ocjdy2TwwxbAUH+cmhgC4RGSlVLkxcDwWZ5G2e/wx+6U/v7Rdy9b0dPUYoMjhis6
          ltw/6relnm9RCxAvmsAJxhhygWw4GVctrcuDazmZUYhi0IXzRGJuIqGpAoGBAJYW
          UyyDAJsDIpBDDDM7zteCcEupFS6h8XEI/F/WIQUJebjkePjEnH4fmaev6BUhp2ml
          KvHIWdvQpnmeqOX6DOyDC1/kAjcugAAVVFk/ZxPZgrGZiBU8tXscAfWzhkAVxVsD
          2Vnmi1uO57u2jEGMKesGqcjUCXUCFWbug9WHomiZAoGBAOn7Mj9jvUcAfMPW6iL+
          EuVuihKfud4lMsZ5AZXDfhNxlkFOywEjcKUzoouMdNoTTRa7l6HvJPGMFYLVvIok
          z9kuXgQBvnjNkX591JQ6W44+5+k9RIEpgQQsqBy+bV8/Of3DU+7HdZMd4p4rdRrm
          OZyh3YGzduDYJNppKcZIw5qM
          -----END PRIVATE KEY-----

        keepalived_service_to_check: {graylogapi: 12900 , graylogkeepalived: 9000}
        keepalived_auth_passwd: "OseFDuPassWd"
        keepalived_interface: "eth1"
        keepalived_master_ip_vip1: "192.168.229.13"
        keepalived_master_ip_vip2: "192.168.229.18"
        keepalived_backup_ip: "192.168.229.103"
        keepalived_vip1_ip: "192.168.229.79"
        keepalived_vip2_ip: "192.168.229.58"
        keepalived_three_nodes: yes
        keepalived_with_etcd: True
        keepalived_nopreempt: False

        etcd_version: 3.0.3
        etcd_clustername: "MonClusterEtcd"
        etcd_inventory_clustergroup: "graylogserver"
        etcd_admin_password: "42xordcte"

Inventory example for full cluster installation (3 graylog servers with etcd and 3 mongo/es servers) :
------------------------
[trusty]
jlstrusty01l.btsys.local node.name=jlstrusty01l
jlstrusty02l.btsys.local node.name=jlstrusty02l
jlstrusty03l.btsys.local node.name=jlstrusty03l

[graylogserver]
jlsgraylog01l.btsys.local graylog_master_node_mode="true" keepalived_master=True keepalived_vip1=True
jlsgraylog02l.btsys.local graylog_master_node_mode="false" keepalived_master=True keepalived_vip2=True
jlsgraylog03l.btsys.local graylog_master_node_mode="false" keepalived_master=False


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
