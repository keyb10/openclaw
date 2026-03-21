# OpenClaw - Documentacao

Documentacao do projeto OpenClaw instalado em VPS Hostinger.

## Informacoes do Servidor

- **VPS**: Hostinger
- **IP**: 187.77.235.201
- **SSH**: `root@187.77.235.201`
- **Container**: `openclaw-o3s8-openclaw-1`
- **Imagem**: `ghcr.io/hostinger/hvps-openclaw:latest`

## URLs

- **Interface Web**: https://openclaw.lctconsulting.com.br
- **Gateway Token**: `19b1f8c7c976426eda676bd89ed6b16b69846b4310650c42455d2b15d87f535f`

## Componentes

| Componente | Status |
|-----------|--------|
| Container OpenClaw | Ativo |
| Cloudflare Tunnel | Ativo |
| DNS (openclaw.lctconsulting.com.br) | Configurado |
| OpenRouter API | Configurado |
| Telegram Bot | Conectado |

## Modelo

- **Provider**: OpenRouter
- **Modelo**: openrouter/auto

## Telegram

- **Bot Token**: Configurado
- **Group Policy**: open
- **DM Policy**: open

## Cloudflare Tunnel

- **Conta**: leandro@lctconsulting.com.br
- **Dominio**: lctconsulting.com.br
- **Tunnel ID**: d240a966-fdf4-44ed-9ec4-76603236a066
- **DNS Record**: openclaw.lctconsulting.com.br -> tunnel

## Troubleshooting

### Gateway Lock
Se o gateway travar com "gateway already running (pid XX)":
```bash
docker exec openclaw-o3s8-openclaw-1 rm -f /tmp/openclaw-1000/gateway.*.lock
docker restart openclaw-o3s8-openclaw-1
```

### Cloudflared Morreu
```bash
docker cp /usr/local/bin/cloudflared openclaw-o3s8-openclaw-1:/usr/local/bin/cloudflared
docker exec openclaw-o3s8-openclaw-1 chmod +x /usr/local/bin/cloudflared
TOKEN=$(cat /root/tunnel_token.txt)
docker exec -d openclaw-o3s8-openclaw-1 /usr/local/bin/cloudflared tunnel run --token "$TOKEN"
```

### Container Reiniciou
Se o container foi recriado, reconfigurar:
```bash
docker cp /usr/local/bin/cloudflared openclaw-o3s8-openclaw-1:/usr/local/bin/cloudflared
docker exec openclaw-o3s8-openclaw-1 chmod +x /usr/local/bin/cloudflared

# Reconfigurar modelo
docker exec openclaw-o3s8-openclaw-1 openclaw config set agents.defaults.model openrouter/auto

# Reconfigurar Telegram
docker exec openclaw-o3s8-openclaw-1 openclaw channels add --channel telegram --token <TOKEN>
docker exec openclaw-o3s8-openclaw-1 openclaw config set channels.telegram.groupPolicy open
docker exec openclaw-o3s8-openclaw-1 openclaw config set channels.telegram.dmPolicy open
```
