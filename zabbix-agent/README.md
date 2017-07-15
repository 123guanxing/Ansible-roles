# Ansible Role: zabbix-agent

��װzabbix�ͻ���

## ����
zabbix����ͬ z?bix����һ������WEB������ṩ�ֲ�ʽϵͳ�����Լ�������ӹ��ܵ���ҵ���Ŀ�Դ���������
zabbix agent��Ҫ��װ�ڱ����ӵ�Ŀ��������ϣ�����Ҫ��ɶ�Ӳ����Ϣ�������ϵͳ�йص��ڴ棬CPU����Ϣ���ռ���zabbix agent����������Linux,Solaris,HP-UX,AIX,Free BSD,Open BSD, OS X, Tru64/OSF1, Windows NT4.0, Windows (2000/2003/XP/Vista)��ϵͳ֮�ϡ�

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

	zabbix_agent_version: "3.2.6"

	zabbix_agent_file: "zabbix-{{ zabbix_agent_version }}.tar.gz"
	zabbix_agent_file_path: "{{ software_files_path }}/{{ zabbix_agent_file }}"
	zabbix_agent_file_url: "https://jaist.dl.sourceforge.net/project/zabbix/ZABBIX%20Latest%20Stable/{{ zabbix_agent_version }}/{{ zabbix_agent_file }}"

	zabbix_agent_repo_url: "http://repo.zabbix.com/zabbix/3.2/rhel/6/x86_64/zabbix-release-3.2-1.el6.noarch.rpm"
	zabbix_agent_packages:
	  - "zabbix-agent-{{ zabbix_agent_version }}-1.el6"

	zabbix_agent_user: zabbix
	zabbix_agent_group: zabbix

	zabbix_agent_install_from_source: false

	zabbix_agent_source_dir: "/tmp/{{ zabbix_agent_file | replace('.tar.gz','') }}"
	zabbix_agent_source_configure_command: >
	  ./configure
	  --prefix={{ software_install_path }}/zabbix-{{ zabbix_agent_version }}
	  --enable-agent

	zabbix_agent_conf_path: "/etc/zabbix" 
	zabbix_agent_logs_path: "/var/log/zabbix"

	zabbix_agent_hostname: "{{ ansible_hostname | d() }}"
	zabbix_agent_server_host: "127.0.0.1"

## ����

None

## github��ַ
https://github.com/kuailemy123/Ansible-roles/tree/master/zabbix-agent

## Example Playbook
	---
	# Դ�밲װ
	- hosts: node2
	  roles:
	   - { role: zabbix-agent, zabbix_agent_install_from_source: true, zabbix_agent_server_host: "192.168.77.130" }
	   
	# rpm����װ
    - hosts: node3
	  roles:
	   - { role: zabbix-agent, zabbix_agent_server_host: "192.168.77.130" }


## ʹ��

```
~]# /etc/init.d/zabbix-agent 
Usage: /etc/init.d/zabbix-agent {start|stop|status|restart|help}

	start		- start zabbix_agentd
	stop		- stop zabbix_agentd
	status		- show current status of zabbix_agentd
	restart		- restart zabbix_agentd if running by sending a SIGHUP or start if not running
	help		- this screen
```
