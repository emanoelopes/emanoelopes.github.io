---
title: "Instalando o PostgreSQL 15.1 no Windows 10 com Ansible"
layout: post
date: 2022-11-21 00:00 -0300
tag: 
- ansible
- windows
- laboratório
image: /assets/images/markdown.jpg
headerImage: false
projects: true
hidden: true # don't count this post in blog pagination
description: "Aprenda a instalar o PostgreSQL 15.1 no Windows 10 Utilizando o Ansible"
category: project
author: emanoel
externalLink: false
---
Veja como instalar o PostgreSQL 15.1 alterando a senha do usuário 'postgres' durante o processo.

```
---
- name: Install Postgresql 
  hosts: '{{ local}}' 

  vars:
    temp_dir: c:\Users\aluno\Downloads\
    old_install_dir: c:\Program Files\postgreSQL 
    install_file: postgresql-15.1-1-windows-x64.exe 

  pre_tasks: 
    - name: Remove any pre-existent installation directory 
      win_file: 
        path: '{{ old_install_dir }}'
        state: absent
  
    - name: Copy Postgresql 15 install file to hosts
      win_copy:
        src: '{{ install_file }}'
        dest: '{{ temp_dir }}'
      tags: postgresql_copy
```

```
  tasks:
    - name: Install Postgres and set postgres password
      win_shell: 'postgresql-15.1-1-windows-x64.exe --unattendedmodeui none --mode unattended --superpassword ufc123'
      args:
        chdir: '{{ temp_dir }}'
        executable: cmd
      tags: postgresql_install
```


<playbook here!>


