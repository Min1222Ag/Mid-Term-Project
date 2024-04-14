# Use Apache HTTP Server official image
FROM httpd:2.4

# Remove the default index.html file provided by Apache
RUN rm /usr/local/apache2/htdocs/index.html

# Copy the HTML files into the container
COPY ./forty-jekyll-theme /usr/local/apache2/htdocs/

# Install OpenSSL (if not included)
RUN apt-get update && apt-get install -y openssl

# Enable SSL and required modules in Apache
RUN sed -i '/LoadModule ssl_module modules\/mod_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf \
 && sed -i '/Include conf\/extra\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf \
 && sed -i '/LoadModule socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' /usr/local/apache2/conf/httpd.conf

# Copy SSL certificate and key into the container
COPY server.key /usr/local/apache2/conf/
COPY server.crt /usr/local/apache2/conf/

# Update SSL Configuration to use copied certificates
RUN echo "SSLEngine on" >> /usr/local/apache2/conf/extra/httpd-ssl.conf \
 && echo "SSLCertificateFile /usr/local/apache2/conf/server.crt" >> /usr/local/apache2/conf/extra/httpd-ssl.conf \
 && echo "SSLCertificateKeyFile /usr/local/apache2/conf/server.key" >> /usr/local/apache2/conf/extra/httpd-ssl.conf

# Expose port 443 for SSL
EXPOSE 443

# Start Apache in the foreground
CMD ["httpd-foreground"]
