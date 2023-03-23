# Ansible Collection - hcs_company.proxmox

## Included content

- **Modules**:
  - [ceph_volume](plugins/modules/ceph_volume.py)
  - [collect_kernel_info](plugins/modules/collect_kernel_info.py)
  - [proxmox_acl](plugins/modules/proxmox_acl.py)
  - [proxmox_group](plugins/modules/proxmox_group.py)
  - [proxmox_query](plugins/modules/proxmox_query.py)
  - [proxmox_role](plugins/modules/proxmox_role.py)
  - [proxmod_storage](plugins/modules/proxmod_storage.py)
  - [proxmox_user](plugins/modules/proxmox_user.py)

## Tested with Ansible

- 2.9

## External requirements

Currently there are no external dependencies required for the Proxmox modules.

## Using this collection

### Installing the Collection from Ansible Galaxy

Before using the Proxmox collection, you need to install it with the Ansible Galaxy CLI:

```bash
ansible-galaxy collection install hcs_company.proxmox
```

You can also include it in a `requirements.yml` file and install it via `ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: hcs_company.proxmox

...

```

See [Ansible Using collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html) for more details.

## Contributing

See the [contribution guide](CONTRIBUTING.md).

## Licensing

<!-- Include the appropriate license information here and a pointer to the full licensing details. If the collection contains modules migrated from the ansible/ansible repo, you must use the same license that existed in the ansible/ansible repo. See the GNU license example below. -->

GNU General Public License v3.0 or later.

See [LICENSE](https://www.gnu.org/licenses/gpl-3.0.txt) to see the full text.
