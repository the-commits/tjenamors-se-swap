[![sourcehut](https://img.shields.io/badge/sourcehut-~the--commits/tjenamors--se--swap-2d6b9e?logo=sourcehut)](https://git.sr.ht/~the-commits/tjenamors-se-swap)
[![GitHub mirror](https://img.shields.io/badge/GitHub-the--commits/tjenamors--se--swap-181717?logo=github)](https://github.com/the-commits/tjenamors-se-swap)

# Swap

Manage swap file with distro-specific best-practice sizing.

## Requirements

- Ansible 2.14+
- Root/sudo access on target hosts
- Linux (RHEL 9+, Ubuntu 24.04+, Debian 12+)

## Supported Distros

| Family | Sizing Strategy |
|---|---|
| **RHEL** (9, 10) | Red Hat KB: 2× RAM (≤2 GB), 1× RAM (2–8 GB), 0.5× RAM (8–64 GB), 4 GB (>64 GB) |
| **Ubuntu** (24.04, 26.04) | Canonical guidance: 1 GB (2–4 GB RAM), 2 GB (4–8 GB), 2 GB (>8 GB) |
| **Debian** (12, 13) | Debian wiki: 2× RAM (<2 GB), 1× RAM (2–8 GB), 4 GB (>8 GB) |

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `swap_enabled` | `true` | Enable or disable swap entirely |
| `swap_file_path` | `/swapfile` | Path to the swap file |
| `swap_swappiness` | `60` | vm.swappiness value |
| `swap_file_size` | `0` | Force a specific size (MB). 0 = auto |
| `swap_size_mode` | `auto` | `auto` uses distro table, or set a size in MB |

When `swap_size_mode` is `auto`, the role calculates swap size based on
`ansible_memtotal_mb` and the distro-specific table from `vars/`.

## Example Playbook

```yaml
- hosts: all
  become: true
  vars:
    swap_size_mode: auto
    swap_swappiness: 10
  roles:
    - role: swap
```

## License

AGPL-3.0 — see [LICENSE](LICENSE) for details.
