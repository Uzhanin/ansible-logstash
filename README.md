# Ansible role: logstash
Разворачивает логстэш в докер контейнере
## Переменные
При использовании этой роли в плейбуках, должны быть заданы переменные:

    logstash_system_password:  # пароль от учетной записи, с которой логстэш подключается к эластику (зашифрованный vars/vault.yml в примере)
    logstash_writer_password: # пароль от учетной записи, используемой в пайплайнах. Эта учетка будет создавать индексы и писать в них
    elastic_hostname: # имя хоста с эластиком (в примере задано через группу хостов elastic в инвентарнике)
    
## Сертификаты
Для подключения к эластику с использованием SSL, логстэш должен иметь сертификат удостоверяющего центра `ca.crt`. В плейбуке он кладется в директорию `files/certs`.

## Пример плейбука

    ---
    - hosts: logstash
      become: true
      vars:
        - elastic_hostname: "{{ groups['elastic'][0] }}"
      vars_files:
        - vars/vault.yml
      roles:
        - logstash
      tags:
        - logstash
