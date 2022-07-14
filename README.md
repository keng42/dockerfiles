# dockerfiles

## speedtest

```sh
docker run --rm keng42/speedtest --share
```

## shadowsocks-libev with v2ray-plugin

```sh
docker run -d --name ss \
  -p 8880:8880 \
  -v $PWD/config.json:/etc/shadowsocks-libev/config.json \
  keng42/ss-libev-v2ray
```

### Links

https://github.com/shadowsocks/shadowsocks-libev

https://github.com/shadowsocks/v2ray-plugin

https://github.com/teddysun/shadowsocks_install
