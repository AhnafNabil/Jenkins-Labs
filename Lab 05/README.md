## Running Jenkins on Port 80: Two Different Methods

By default, Jenkins runs on port 8080 after installation. However, there are scenarios where you might want to access Jenkins on port 80 or any other port, especially in production environments where port 80 is used for HTTP traffic. This documentation will guide you through two different methods to run Jenkins on port 80.

### Methods to Run Jenkins on Port 80
1. **Using IP Table Forwarding Rule**
2. **Using Nginx as a Reverse Proxy**

### Method 1: Running Jenkins on Port 80 Using IP Table Forwarding Rule
This method involves creating an IP table forwarding rule that redirects traffic from port 80 to Jenkins's default port 8080. This is the simplest method and doesn't require additional software.

#### Steps:
1. **Create the IP Table Forwarding Rule:**

    ```bash
    sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
    ```

2. **Save the IP Table Rules:**

    - For RedHat-based systems:

      ```bash
      sudo iptables-save > /etc/sysconfig/iptables
      ```

    - For Ubuntu-based systems:

      ```bash
      sudo sh -c "iptables-save > /etc/iptables.rules"
      ```

Now, when you access Jenkins on port 80, the IP table rule will automatically forward the requests to port 8080.

### Method 2: Running Jenkins Behind an Nginx Reverse Proxy
Using Nginx as a reverse proxy is a more robust solution, especially for production environments. Nginx will handle incoming traffic on port 80 and forward it to Jenkins on port 8080.

#### Steps:
1. **Install Nginx:**

    - For RedHat-based systems:

      ```bash
      sudo yum install nginx
      ```

    - For Ubuntu-based systems:

      ```bash
      sudo apt-get install nginx
      ```

2. **Configure Nginx:**

    - Open the Nginx configuration file:

      ```bash
      sudo vi /etc/nginx/nginx.conf
      ```

    - Locate the following block:

      ```nginx
      location / {
      }
      ```

    - Modify it to include the Jenkins proxy settings:

      ```nginx
      location / {
          proxy_pass http://127.0.0.1:8080;
          proxy_redirect off;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
      ```

    - Note: If Nginx is running on a different server than Jenkins, replace `127.0.0.1` with your Jenkins server's IP address.

3. **Set SELinux to Allow Nginx Connections:**

    ```bash
    sudo setsebool httpd_can_network_connect 1 -P
    ```

4. **Restart Nginx:**

    ```bash
    sudo systemctl restart nginx
    ```

Now, Nginx will forward all requests on port 80 to Jenkins on port 8080.

### Conclusion
You can choose any of the methods based on your environment and requirements. For a simple setup, the IP table forwarding rule is sufficient. In production environments, using Nginx as a reverse proxy offers more flexibility and scalability.