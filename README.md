# GrafanaNGINX-PortForward
Files to port forward port 3000 to port 80 (works with TCP, probably UDP too), and works with web servers (apache)

This is a basic HTTP nginx (as a reverse proxy) portforward

Requirements:
- nginx (engine-ex) and basic commands: http://nginx.org/en/docs/windows.html
- IPv4 address (ipconfig in command line, this is the IP on the network)
- Public IP (google "my public IP address", should be different from IPv4)
  - this is the address other people will use
- Grafana (this is our main service) and it's port (3000 by default)
- Router username and password (should be on device)

1. open up windows firewall port, by setting up an inbound rule (in this case, port 80 will be accessed)
 - Control Panel > System and Security > Windows Defender Firewall > Advanced Settings > Inbound Rules > New Rule...
 - Make sure the "Port" bubble is filled in, click next
 - Apply to TCP
 - Specific local Ports: 80, then next
 - Allow the connection
 - Check mark everything, then next
 - give it whatever name/description to distinguish


2. start Grafana and nginx

3. Make sure your custom.ini is the same as the one in the repo, colons don't matter so much

;http_addr =

http_port = 3000

domain = localhost

;enforce_domain = false

root_url = %(protocol)s://%(domain)s:%(http_port)s

serve_from_sub_path = false

4. Configure nginx, make sure it looks like the one in the repo, specifically:

server {
  listen 80;
  root /usr/share/nginx/html;
  index index.html index.htm;

  location / {
   proxy_pass http://localhost:3000/;
  }

3. Reload nginx and grafana, nginx through command line (administrator) and grafana through "Services"
