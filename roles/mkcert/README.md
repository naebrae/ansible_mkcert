# Ansible role: mkcert.

Creates a `Test Lab Certificate Authority` in the **$HOME/cacerts** folder of the operating account if it doesn't already exist. Those details default to:
```
HomeLabRoot.crt: "/C=AU/O=Home Lab/OU=www.lab.home/CN=Home Lab Root"
HomeLabCA.crt: "/C=AU/O=Home Lab/CN=Home Lab CA"
```
After the `Test Lab Certificate Authority` is created or verified to exist, it creates the requested `server certifcate`, signed by the certificate authority. It creates the certificate in a **certs** folder in the **playbook base** folder. It will reuse a certificate with the file name calculated from the requested `Common Name` in the Subject. Once the certificate is created or resued, it is copied to the remote server:

- RedHat family:
   ```
   .crt to /etc/pki/tls/certs/
   .key to /etc/pki/tls/private/
   HomeLabCA.crt to /etc/pki/tls/certs/
   ```
- Debian family:
   ```
   .crt to /etc/ssl/certs/
   .key to /etc/ssl/private/
   HomeLabCA.crt to /etc/ssl/certs/
   ```

## Example Usage

- Create localhost certificate with SAN containging DNS name, IPv4, and IPv6 address.
   ```
   - { role: mkcert,
        site_name: localhost,
        cert_org: "/C=AU/ST=Western Australia/L=Perth/O=Home Lab",
        cert_san: "DNS:localhost.localdomain,IP:127.0.0.1,IP:::1",
        cert_return: localhost_cert
     }
   ```

   Returns:
   ```
   {{ localhost_cert.ca }}: "<<$HOME>>/cacerts/HomeLabCA.crt",
   {{ localhost_cert.crt }}: "<<playbook_dir>>/certs/localhost.crt",
   {{ localhost_cert.key }}: "<<playbook_dir>>/certs/localhost.key",
   {{ localhost_cert.root }}: "<<$HOME>>/cacerts/HomeLabRoot.crt"
   ```


- Create Windows style certificate with SAN containing all remote host IP addresses.
   ```
  - { role: mkcert,
       site_name: "dc1.ad.site.name",
       cert_org: "/DC=name/DC=site/DC=ad",
       cert_san: "DNS:{{ ansible_fqdn }},DNS:{{ ansible_hostname }}
         {% for ipv4_address in ansible_all_ipv4_addresses %},IP:{{ ipv4_address }}{% endfor %}
         {% for ipv6_address in ansible_all_ipv6_addresses %},IP:{{ ipv6_address }}{% endfor %}",
       cert_return: dc1_cert
    }
   ```

   Returns:
   ```
    {{ dc1_cert.ca }}: "<<$HOME>>/cacerts/HomeLabCA.crt",
    {{ dc1_cert.crt }}: "<<playbook_dir>>/certs/dc1_ad_site_name.crt",
    {{ dc1_cert.key }}: "<<playbook_dir>>/certs/dc1_ad_site_name.key",
    {{ dc1_cert.root }}: "<<$HOME>>/cacerts/HomeLabRoot.crt"
   ```


- Create wildcard certificate
   ```
  - { role: mkcert, site_name: "*.site.name", cert_org: "/C=AU/O=Home Lab/ST=Western Australia/L=Perth", cert_return: star_cert }
   ```

   Returns:
   ```
    {{ star_cert.ca }}: "<<$HOME>>/cacerts/HomeLabCA.crt"
    {{ star_cert.crt }}: "<<playbook_dir>>/certs/star_site_name.crt"
    {{ star_cert.key }}: "<<playbook_dir>>/certs/star_site_name.key"
    {{ star_cert.root }}: "<<$HOME>>/cacerts/HomeLabRoot.crt"
   ```


- Create certificate using remote hosts FQDN.
   ```
  - mkcert
   ```

## License

GNU General Public License v2.0 or later

