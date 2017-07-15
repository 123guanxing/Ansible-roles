# Ansible Role: grafana

��װgrafana

## ����
Grafana �� Graphite �� InfluxDB �Ǳ��̺�ͼ�α༭����Grafana �ǿ�Դ�ģ�������ȫ�Ķ����Ǳ��̺�ͼ�α༭����֧�� Graphite��InfluxDB �� OpenTSDB��

�ٷ���ַ��https://grafana.com/
github: https://github.com/grafana/grafana
�ٷ��ĵ���ַ��http://docs.grafana.org/

## Ҫ��

�˽�ɫ����RHEL����������Ʒ�����С�

## ���Ի���

ansible `2.3.0.0`
os `Centos 6.7 X64`
python `2.6.6`

## ��ɫ����
	software_files_path: "/opt/software"
	software_install_path: "/usr/local"

	grafana_version: "4.4.1"
	grafana_package_url: "https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana-{{ grafana_version }}-1.x86_64.rpm"

	grafana_plugins: []

	grafana_http_port: 3000
	grafana_admin_user: "admin"
	grafana_admin_password: "admin"

## ����

None

## github��ַ
https://github.com/kuailemy123/Ansible-roles/tree/master/grafana

## Example Playbook
   - hosts: node1
     roles:
      - grafana
		
   - hosts: node1
     vars:
      - grafana_plugins:
         - alexanderzobnin-zabbix-app
      - grafana_admin_password: '123456'
     roles:
      - { role: grafana }
      - { role: iptables, iptables_allowed_tcp_ports: [ "3000"]}

## ʹ��

```
~]# /etc/init.d/grafana-server 
Usage: /etc/init.d/grafana-server {start|stop|restart|force-reload|status}

```
