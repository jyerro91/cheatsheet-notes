# Nginx Configuration

```
http {
  # Hide Web Server Information
  # This will remove showing web server version
  server_tokens off;
  
  # Protection against XSS 
  add_header X-XSS-Protection "1; mode=block";
  
  # Protection against Clickjacking Attacks
  add_header X-Frame-Options "SAMEORIGIN";
  
  # Protection against Content-Type Sniffing
  add_header X-Content-Type-Options nosniff;
}
```

### Optimize SSL/TLS Settings 

Create a ssl.conf under `/etc/nginx/conf.d` directory and add below given code in it to disable weak SSL protocols and ciphers

```conf 
ssl_protocols TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers ECDH+AESGCM:ECDH+AES256:ECDH+AES128:!ADH:!AECDH:!MD5;
```
