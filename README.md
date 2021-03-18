# Haproxy-By-Ansible

👉 HAProxy, which stands for High Availability Proxy, is a popular open-source software TCP/HTTP Load Balancer and proxying solution which can be run on Linux, Solaris, and FreeBSD.

# 👉 LoadBalancer
A load balancer is a device that acts as a reverse proxy and distributes network or application traffic across a number of servers. Load balancers are used to increase capacity (concurrent users) and reliability of applications. So load balancer is work as proxy so we can protect our server by Proxy .

In load balancer we use proxy ip and our request come from backend server ips so we can access web page from server without knowing actual ip.

In this blog you will you how to make load balancer .

Here i use three Machine one loadbalancer (127.0.0.1) two webserver
(192.168.0.118)& .119

# 👉Now We Start make LoadBalancer by Ansible👈

👉 Create Inventory

![image](https://user-images.githubusercontent.com/70094412/111656818-e2cfa980-8830-11eb-97f1-cfc571610849.png)

In this practical Our control node will Loadbalncer so i use 127.0.0.1

Now make **jinja code in haproxy.cfg**  file copy from /etc/haproxy/


![image](https://user-images.githubusercontent.com/70094412/111656924-f844d380-8830-11eb-8195-473e93727003.png)

Now Create the Ansible File for LoadBalncer and Webserver

haproxy.yml
```

- hosts: webserver
  tasks:
          - name: "installing the httpd server"
            package:
                    name: "httpd"
                    state: present

          - name: "installing the php on the webserver"
            package:
                    name: "php"
                    state: present


          - name: "copying the content into webserver"
            copy:
                    dest: "/var/www/html/index.php"
                    src: "/root/index.php"

          - name: "starting the httpd"
            service:
                    name: "httpd"
                    state: started
- hosts: loadbalancer
  tasks:
          - name: "installing the haproxy to loadbalancer"
            package:
                    name: "haproxy"
                    state: present

          - name: "configuration haproxy.cfg file to the loadBalancer"
            template:
                    dest: "/etc/haproxy/haproxy.cfg"
                    src: "/root/haproxy.cfg"

          - name: "restarting the loadbalancer"
            service:
                    name: "haproxy"
                    state: restarted
```

index.php
```
<pre>
<h1>hello</h1>
<h2>
<?php
print `/usr/sbin/ip addr`
?>
</h2>
</pre>
```

Now run the playbook using the below command

```
ansible-playbook <<playbookname.yml>>
```

**Output:**

![image](https://user-images.githubusercontent.com/70094412/111657301-4eb21200-8831-11eb-8893-e8cb66d133b7.png)

Now See haproxy.cfg Config or not

![image](https://user-images.githubusercontent.com/70094412/111657367-5f628800-8831-11eb-8502-73d78aaf7e65.png)

So you can see in the above screenshot, the haproxy.cfg file is updated

Now Put ip of LoadBalncer (192.168.0.113) in chrome So you can access web page from server


![image](https://user-images.githubusercontent.com/70094412/111657419-6c7f7700-8831-11eb-8780-0083d5c8598f.png)


So you can See our web page from .118 server ip .

>Thank you

