# vps-kit

Root Linux box buat `sbx --cloud`, tanpa sshd/password (sbx udah kasih root shell + SSH native).

## Prasyarat

- `sbx` CLI >= 0.45
- Docker Agentic Platform subscription aktif (cloud access)

## Run

```bash
sbx login
sbx --cloud diagnose

# shell interaktif root
sbx --cloud run ./vps-kit --name myvps
# atau tanpa kit sama sekali:
sbx --cloud run shell --name myvps

# jalanin command (root = sudo)
sbx --cloud exec myvps whoami
sbx --cloud exec myvps sudo apt-get install -y something

# attach lagi
sbx --cloud attach myvps
```

## SSH

```bash
sbx --cloud setup ssh
ssh <sbx_id>@sbx_cloud     # id dari `sbx --cloud ls`
```

No password — auth pake login Docker. Ga ada sshd di dalam sandbox.

## TTL & biaya

```bash
sbx --cloud ttl myvps        # cek sisa waktu
sbx --cloud ttl myvps +12h   # perpanjang (max 24 jam dari create)
sbx --cloud rm myvps         # hapus
```

Default TTL 1 jam. Ini ephemeral, bukan VPS permanen.

## Catatan

- Kalau `sbx --cloud run ./vps-kit` (local dir) ditolak di cloud, push dulu:
  `sbx kit push ./vps-kit <docker-id>/vps-kit:1.0` lalu `sbx --cloud run <docker-id>/vps-kit:1.0`.
- Base image = image shell bawaan Docker (Debian-based). Kalau butuh `ubuntu:22.04` persis, perlu v3 Dockerfile.
- Ekspose service (bukan SSH): `sbx --cloud ports myvps --publish 8080`.