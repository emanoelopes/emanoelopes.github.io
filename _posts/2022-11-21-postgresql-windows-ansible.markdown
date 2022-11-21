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

Antes de começar, é preciso baixar o instalador diretamente do [sítio](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads) e mover para o mesmo diretório da playbook.

Veremos, como exemplo esta playbook:

```
---
- name: Install Postgresql 
  hosts: '{{ local }}'

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

  tasks:
    - name: Install Postgres and set postgres password
      win_shell: 'postgresql-15.1-1-windows-x64.exe --unattendedmodeui none --mode unattended --superpassword ufc123'
      args:
        chdir: '{{ temp_dir }}'
        executable: cmd
      tags: postgresql_install
```

Em vars temos:

- temp_dir contém o diretório/pasta onde iremos guardar o instalador no host de destino. 
- old_install_dir o diretório/pasta onde havia uma versão anterior do postgresql.
- install_file é o instalador que será copiado do controlador ansible para todos os hosts.

Nas duas pré-tarefas faremos uma remoção da pasta da antiga instalação, se houver. Depois realizar a cópia do instalador para os hosts.

Agora iremos para a tarefa propriamente dita: instalação do postgresql e do pgadmin4. 

Para isso, basta chamar o instalador com o módulo win_shell já com os parâmetros de instalação sem GUI e atendimento e alterando a senha do 'postgres' que é o usuário administrador do banco.

Para testar, basta chamar o 'pgadmin' e acessar o banco.

