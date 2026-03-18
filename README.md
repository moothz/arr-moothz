# Configuração da Stack de Mídia (Arr Stack)

Este repositório contém a configuração para uma stack completa de gerenciamento de mídia automatizada utilizando Docker Compose.

## 🚀 Serviços e Endereços Locais

Abaixo estão os endereços para acessar cada serviço na sua rede local:

| Serviço | Porta (Host) | Endereço Local | Descrição |
| :--- | :--- | :--- | :--- |
| **Jellyfin** | 8096 | [http://192.168.3.10:8096](http://192.168.3.10:8096) | Servidor de Streaming |
| **Jellyseerr** | 5055 | [http://192.168.3.10:5055](http://192.168.3.10:5055) | Solicitações de Mídia |
| **Sonarr** | 8991 | [http://192.168.3.10:8991](http://192.168.3.10:8991) | Gerenciamento de Séries |
| **Radarr** | 7878 | [http://192.168.3.10:7878](http://192.168.3.10:7878) | Gerenciamento de Filmes |
| **Readarr** | 8787 | [http://192.168.3.10:8787](http://192.168.3.10:8787) | Gerenciamento de Livros |
| **Jackett** | 9117 | [http://192.168.3.10:9117](http://192.168.3.10:9117) | Agregador de Indexadores (Trackers) |
| **qBittorrent** | 8080 | [http://192.168.3.10:8080](http://192.168.3.10:8080) | Cliente de Download |

### 🌐 Acesso Externo (Cloudflare Tunnel)
- **Jellyfin:** [https://jelly.moothz.win](https://jelly.moothz.win)
- **Jellyseerr:** [https://selly.moothz.win](https://selly.moothz.win)
- **qBittorrent:** [https://qb.moothz.win](https://qb.moothz.win)

---

## 📂 Estrutura de Diretórios e Volumes

Toda a stack está centralizada em `/root/arr` para as configurações e `/mnt/wd2/arr` para os dados de mídia.

### Configurações (Config Files)
As configurações de cada container estão localizadas na pasta `./config` no diretório da stack:
- `./config/jellyfin`
- `./config/jellyseerr`
- `./config/sonarr`
- `./config/radarr`
- `./config/readarr`
- `./config/jackett`
- `./config/qbittorrent`

### Armazenamento de Mídia e Downloads
Mapeado para o disco externo em `/mnt/wd2/arr`:
- **Downloads:** `/mnt/wd2/arr/downloads` (Mapeado como `/downloads` nos containers)
- **Filmes:** `/mnt/wd2/arr/media/movies` (Mapeado como `/movies` no Radarr)
- **Séries:** `/mnt/wd2/arr/media/tv` (Mapeado como `/tv` no Sonarr)
- **Livros:** `/mnt/wd2/arr/media/books` (Mapeado como `/books` no Readarr)
- **Biblioteca Geral:** `/mnt/wd2/arr/media` (Mapeado como `/media` no Jellyfin)

---

## ⚙️ Configurações Importantes Efetuadas

### 1. Permissões de Arquivo
O diretório `/mnt/wd2/arr` foi ajustado para o usuário `1000:1000` com permissões `775` para garantir que todos os serviços possam ler e escrever os arquivos de mídia.

### 2. qBittorrent WebUI
Para permitir o acesso via Cloudflare Tunnel e IP local simultaneamente, foram feitas as seguintes alterações no `qBittorrent.conf`:
- `WebUI\HostHeaderValidation=false`
- `WebUI\ServerDomains=*`
- `WebUI\CSRFProtection=false`
- `WebUI\Address=*`

### 3. Sonarr
A porta externa foi alterada para **8991** pois a porta padrão 8989 já estava em uso no sistema host. **Nota:** Internamente na rede do Docker, ele ainda responde na porta 8989.

---

## 🛠️ Manutenção
Para reiniciar a stack:
```bash
docker compose restart
```
Para ver os logs:
```bash
docker compose logs -f
```
