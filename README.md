# Prometheus Node-Exporter Grafana deployment using Ansible roles

Prometheus is an open-source monitoring system which is very lightweight and has a good alerting mechanism. Prometheus can monitor hundreds of instances and display metrics and statistics from all of them to you in one simple dashboard.

Grafana is a web application that gives you visualization tools. In a nutshell, we will be able to create and use dashboards from Grafana to visualize all the statistics and metrics presented by Prometheus.

# Project Scope

In this demo, we will be creating an Ansible playbook to automate the installation of grafana, node_exporter, and Prometheus into an EC2 instance. Here, we will be making use of ansible roles to deploy each components.


# Architecture

This is a monitoring project which consists of the following components:

- Node Exporter : Node exporter produces metrics about infrastructure which include CPU, memory, disc, network stats etc.
- Prometheus : It helps monitor servers which are called targets. It can be a single target or multiple targets monitored for different metrics.
- Grafana : Is a tool for visualising the metrics by connecting with the prometheus server.

In this module, we will see how each of these components can be automatically deployed using ansible playbook.

![image](https://github.com/Jisjo/ansible_prometheus-installation_role/blob/main/Diagram-1.png)

We will be deploying this project using 3 instances.

- In the first instance, we will be setting this up as a monitoring server in which we will be installing prometheus(port:9090) and Grafana(port:3000) application. This instance connects to other 2 instances and reads the metrices like cpu usage,memory, stats etc in a timely manner.

- In the other 2 nodes, we will be setting up the node exporter which will be working on port 9100.

# Requirements

- Install Ansible on your machine (ansible master).
- Ansible-galaxy
- 3 instances for installing node exporter, prometheus and grafana applications.
- community.grafana (auto configure grafana with prometheus)
```
ansible-galaxy collection install community.grafana
```
# Variables used

### 02-prometheus.main.yml
> node and grafana instance or server IPs.
```
- node_ip: "172.31.38.242"
- grafana_ip: "172.31.38.234"
```
### role/grafana/vars/main.yml
> grafana dashboard admin login details
```
grafana_user: admin
grafana_password: admin123
```
### role/node-exporter/vars/main.yml
> binary for node-exporter
```
url_node_exporter: "https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz"
```

# Provisioning

Clone the files and place the role under /etc/ansible/role/ (confirm the roles_path under the ansible conf file)
```
git clone https://github.com/Jisjo/ansible_prometheus-installation_role.git
```
We can run the playbooks using the command
```
ansible-playbook -i hosts 01-node-exporter-main.yml 
ansible-playbook -i hosts 02-prometheus.main.yml
```

# Result
> Grafana: http://<ip>:3000 
  
![screenshot](https://github.com/Jisjo/ansible_prometheus-installation_role/blob/main/Screenshot-grafana-1.png)

> prometheus: http://<ip>:9090
  
![image](https://github.com/Jisjo/ansible_prometheus-installation_role/blob/main/Screenshot-prom-1.png)

