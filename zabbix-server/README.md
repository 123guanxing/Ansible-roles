# Ansible Role: zabbix-server

��װzabbix server

## ����
zabbix����ͬ z?bix����һ������WEB������ṩ�ֲ�ʽϵͳ�����Լ�������ӹ��ܵ���ҵ���Ŀ�Դ���������
zabbix server����ͨ��SNMP��zabbix agent��ping���˿ڼ��ӵȷ����ṩ��Զ�̷�����/����״̬�ļ��ӣ������ռ��ȹ��ܣ�������������Linux��Solaris��HP-UX��AIX��Free BSD��Open BSD��OS X��ƽ̨�ϡ�

�ٷ���ַ�� https://www.zabbix.com
�ٷ��ĵ���ַ��https://www.zabbix.com/documentation/3.2/manual

## Ҫ��

�˽�ɫ����RHEL����������Ʒ�����С�

## ���Ի���

ansible `2.3.0.0`
os `Centos 6.7 X64`
python `2.6.6`

## ��ɫ����
	software_files_path: "/opt/software"
	software_install_path: "/usr/local"

	zabbix_server_version: "3.2.6"

	zabbix_server_file: "zabbix-{{ zabbix_server_version }}.tar.gz"
	zabbix_server_file_path: "{{ software_files_path }}/{{ zabbix_server_file }}"
	zabbix_server_file_url: "https://jaist.dl.sourceforge.net/project/zabbix/ZABBIX%20Latest%20Stable/{{ zabbix_server_version }}/{{ zabbix_server_file }}"

	zabbix_server_repo_url: "http://repo.zabbix.com/zabbix/3.2/rhel/6/x86_64/zabbix-release-3.2-1.el6.noarch.rpm"
	zabbix_server_packages:
	  - "zabbix-server-mysql-{{ zabbix_server_version }}-1.el6"
	  - "zabbix-web-mysql-{{ zabbix_server_version }}-1.el6"
	  - "zabbix-agent-{{ zabbix_server_version }}-1.el6"

	zabbix_server_user: zabbix
	zabbix_server_group: zabbix
	zabbix_server_hostanme: "Zabbix server"

	zabbix_server_install_from_source: false

	zabbix_server_source_dir: "/tmp/{{ zabbix_server_file | replace('.tar.gz','') }}"
	zabbix_server_source_configure_command: >
	  ./configure
	  --prefix={{ software_install_path }}/zabbix-{{ zabbix_server_version }}
	  --enable-server
	  --enable-agent
	  --with-mysql
	  --enable-ipv6
	  --with-net-snmp
	  --with-libcurl
	  --with-libxml2

	zabbix_server_conf_path: "/etc/zabbix" 
	zabbix_server_logs_path: "/var/log/zabbix"
	zabbix_server_webroot: "/var/www/html/zabbix"
	zabbix_server_webserver: "httpd"
	zabbix_server_webserver_user: "apache"

	zabbix_server_db: zabbix
	zabbix_server_db_host: "127.0.0.1"
	zabbix_server_db_port: "3306"
	zabbix_server_db_user: "zabbix"
	zabbix_server_db_password: "zabbix"

## ����

None

## github��ַ
https://github.com/kuailemy123/Ansible-roles/tree/master/zabbix-server

## Example Playbook

	Դ�밲װ
	---
    # ����web��������mysql������
    - hosts: node2
      vars:
       - zabbix_server_db: zabbix
       - zabbix_server_db_user: zabbix
       - zabbix_server_db_password: zabbix
    
      roles: 
       - { role: php, php_install_from_source: true }
       - mysql
    
      # ����zabbix���ݿ�,����п�ʡ�Դ˲���
      tasks:
       - name: configure_db | Create zabbix database.
    	 shell: mysql -uroot -p123456 -h192.168.77.130 -P3306 -e "{{ item }}"
    	 with_items:
    	   - "create database {{ zabbix_server_db }} character set utf8 collate utf8_bin;"
    	   - "grant all privileges on {{  zabbix_server_db }}.* to '{{ zabbix_server_db_user }}'@'%' identified by '{{ zabbix_server_db_password }}';"
    
    # ����zabbix-server
    - hosts: node2
      vars:
       - zabbix_server_db: zabbix
       - zabbix_server_db_user: zabbix
       - zabbix_server_db_password: zabbix
       - zabbix_server_db_host: 192.168.77.130
    
      roles:
       - { role: zabbix-server, zabbix_server_install_from_source: true }
   
   rpm��ʽ��װ
   ---
   # ����web��������mysql������
   - hosts: node2
     vars:
      - zabbix_server_db: zabbix
      - zabbix_server_db_user: zabbix
      - zabbix_server_db_password: zabbix
   
     roles: 
      - php
      - mysql
   
     # ����zabbix���ݿ�,����п�ʡ�Դ˲���
     tasks:
      - name: configure_db | Create zabbix database.
        shell: mysql -uroot -p123456 -h192.168.77.130 -P3306 -e "{{ item }}"
        with_items:
          - "create database {{ zabbix_server_db }} character set utf8 collate utf8_bin;"
          - "grant all privileges on {{  zabbix_server_db }}.* to '{{ zabbix_server_db_user }}'@'%' identified by '{{ zabbix_server_db_password }}';"
   
   # ����zabbix-server
   - hosts: node2
     vars:
      - zabbix_server_db: zabbix
      - zabbix_server_db_user: zabbix
      - zabbix_server_db_password: zabbix
      - zabbix_server_db_host: 192.168.77.130
   
     roles:
      - zabbix-server
   
## ʹ��

```
~]# /etc/init.d/zabbix-server 
Usage: /etc/init.d/zabbix-server {start|stop|status|restart|try-restart|force-reload}

```
