**Deploying Flask App Behind HAProxy**

**Overview:**
This Ansible playbook is designed to deploy a Flask application behind an HAProxy load balancer on a group of web servers. The Flask application serves a simple response, and HAProxy is used to distribute incoming traffic across multiple backend servers.

**Prerequisites:**
- Ansible installed on your local machine.
- SSH access to the target hosts configured with SSH keys.
- Flask application files present in the current working directory.
- HAProxy configuration template file (haproxy.cfg.j2) present in the current working directory.

**Setup:**
1. Ensure that the Flask application files (application2.py) are located in the current working directory.
2. Make sure the HAProxy configuration template file (haproxy.cfg.j2) is also present in the current working directory.

**Usage:**
1. Update the `hosts` file with the IP addresses or hostnames of your web servers and HAProxy server.
2. Run the Ansible playbook using the following command:

```
ansible-playbook -i hosts site.yaml
```

**Description:**
- The playbook is divided into two main sections:
  - Deployment of Flask Application on Web Servers (`webservers` group)
  - Configuration of HAProxy Load Balancer (`HAproxy` group)

- The Flask application deployment tasks include:
  - Updating the package cache using `apt`.
  - Installing Python 3, pip3, and virtual environment packages.
  - Installing Flask and Gunicorn.
  - Copying the Flask application file (`application2.py`) to the remote server.
  - Running the Flask application using Gunicorn.

- The HAProxy configuration tasks include:
  - Updating the package cache using `apt`.
  - Installing the latest version of HAProxy.
  - Configuring HAProxy by copying the template file (`haproxy.cfg.j2`) to the destination directory.
  - Restarting the HAProxy service to apply the changes.

**Notes:**
- Customize the HAProxy configuration template (`haproxy.cfg.j2`) according to your requirements.
- Ensure that SSH keys are properly configured for passwordless authentication to the target hosts.
- Test the deployment thoroughly to ensure the Flask application is accessible through the HAProxy load balancer.

**References:**
- Flask Documentation: [https://flask.palletsprojects.com/](https://flask.palletsprojects.com/)
- HAProxy Documentation: [https://www.haproxy.com/documentation/](https://www.haproxy.com/documentation/)