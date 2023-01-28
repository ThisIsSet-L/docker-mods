# Dashboard Docker mod for SWAG

This mod adds a dashboard to SWAG powered by [Goaccess](https://goaccess.io/).

**Currently only works with a subdomain, not a subfolder.**

# Enable

In the container's docker arguments, set an environment variable DOCKER_MODS=linuxserver/mods:swag-dashboard

If adding multiple mods, enter them in an array separated by |, such as DOCKER_MODS=linuxserver/mods:swag-dashboard|linuxserver/mods:swag-mod2

## Internal access using `<server-ip>:81`

Add a mapping of `81:81` to swag's docker run command or compose

## Internal access using `dashboard.domain.com`

Requires an internal DNS, add a rewrite of `dashboard.domain.com` to your server's IP address

## External access using `dashboard.domain.com`

Remove the allow/deny lines in `/config/nginx/proxy-confs/dashboard.subdomain.com`, and instead secure it some other way (like Authelia for example).

## Notes 
- The application discovery scans all files, irrespective of extension, under the nginx config directories and looks for the following structure in accordance with the samples:
  ```yaml
    set $upstream_app <container/address>;
    set $upstream_port <port>;
    set $upstream_proto <protocol>;
    proxy_pass $upstream_proto://$upstream_app:$upstream_port;
    ```
  The following directories are scanned for configurations:
  - `/config/nginx`
  - `/etc/nginx/http.d` 
- Either [Swag Maxmind mod](https://github.com/linuxserver/docker-mods/tree/swag-maxmind) or [Swag DBIP mod](https://github.com/linuxserver/docker-mods/tree/swag-dbip) are required to enable the geo location graph.
- The host's fail2ban can be supported by mounting it to swag `- /path/to/host/fail2ban.sqlite3:/dashboard/fail2ban.sqlite3:ro`
- The host's logs can be supported by mounting it to swag `- /path/to/host/logs:/dashboard/logs:ro`

# Example
![Example](.assets/example.png)
