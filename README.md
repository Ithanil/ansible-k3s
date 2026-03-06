# K3s Setup Role

This role provisions K3s using upstream `k3s.orchestration` roles and applies
source-restricted UFW rules for K3s traffic before and after provisioning.

## Dependency

This role depends on the `k3s.orchestration` collection from:
https://github.com/k3s-io/k3s-ansible

One way to install the collection would be to include it as a submodule at `collections/ansible_collections/k3s/orchestration`
and set `collections_path = ./collections:~/.ansible/collections:/usr/share/ansible/collections` in your `ansible.cfg`.

## What It Does

- Reconciles UFW rules for K3s ports
- Runs `k3s.orchestration.prereq`
- Runs `k3s.orchestration.k3s_server` with one retry on failure
- Runs `k3s.orchestration.k3s_agent` with one retry on failure
- Reconciles UFW rules again after provisioning

## Expected Inventory Groups

- `k3s_cluster` for all K3s nodes
- `server` for K3s server/control-plane nodes
- `agent` for K3s agent nodes

## Usage

```yaml
- name: Setup K3s cluster
  hosts: k3s_cluster
  tasks:
    - name: Setup K3s
      ansible.builtin.include_role:
        name: ansible-k3s
```
