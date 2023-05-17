This repo provides minimal example to setup SSL with from Google Domains with Traefik + acme.sh.

##### 1. Run acme as daemon

```
docker run --rm  -itd  \
  -v "$(pwd)/out":/acme.sh  \
  --net=host \
  --name=acme.sh \
  neilpang/acme.sh daemon
```

##### 2. Visit domains.google.com/registrar/example.com/security

##### 3. In the 'Advanced security features' section, expand 'Google Trust Services', then click on 'Get EAB Key', you will get EAB Key ID and EAB HMAC Key for account registration

##### 4. In the 'Advanced security features' section, scroll down to ACME DNS API and click on 'Create token', you will get an access token for issuing

##### 5. Register your account

```
docker exec acme.sh --register-account -m email@example.com --server google \
 --eab-kid your_eab_key_id \
 --eab-hmac-key your_eab_hmac_key

```

##### 6. Issue certificate

```
docker exec -e GOOGLEDOMAINS_ACCESS_TOKEN="your_access_token" acme.sh --issue --server google -d 'example.com' --dns dns_googledomains

```

##### 7. Modify docker-compose volume to point to your fullchain.cer & example.com.key

##### 8. Run docker compose and visit whoami.example.com

## References

1. https://github.com/acmesh-official/acme.sh/wiki/Google-Trust-Services-CA
2. https://github.com/acmesh-official/acme.sh/wiki/Run-acme.sh-in-docker#3-run-acmesh-as-a-docker-daemon
