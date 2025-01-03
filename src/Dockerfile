FROM alpine:3.21
LABEL "com.example.vendor"="deninet.com"

# Update the package list and install Ansible.
RUN apk -U upgrade &&\
    apk add --update --no-cache ansible-core git && \
    rm -rf /tmp/* \
           /var/cache/apk/*

# Copy the Ansible configuration files
COPY ansible-hosts /etc/ansible/hosts
COPY ansible.cfg /etc/ansible/ansible.cfg
COPY ansible /ansible

# Run the build.
RUN ansible-galaxy install -fr /ansible/requirements.yml && \
    ansible-playbook -i /ansible/inventories/all.ini /ansible/build.yml && \
    apk del --no-interactive git

# Configure the runtime environment of the container.
EXPOSE 11211

# Switch to the service account
USER memcache

# Set the entrypoint and default command.
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
CMD ["docker-cmd.sh", "/etc/memcache/memcache-foreground.sh"]
