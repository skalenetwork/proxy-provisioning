# SKALE Proxy Provisioning

[![Discord](https://img.shields.io/discord/534485763354787851.svg)](https://discord.gg/vvUtWJB)

A set of ansible playbooks to setup SKALE Proxy and Block explorer in the cloud

## 1. Installation

1. Clone this repo
2. Create a venv and install the requirements with [uv](https://docs.astral.sh/uv/) (Python 3.14 from `.python-version`):

```bash
uv venv
source .venv/bin/activate
uv pip install -r requirements.txt
```

## 2. Setup

1. Create inventory for your network:

   ```bash
   cp inventory-template inventory
   ```

2. Fill in the variables in `inventory`.

   > If you need only proxy - remove `[explorer]` section.

3. Put the SSH public keys for the hosts in `files/authorized_keys`
4. For the explorer, copy SKALE Manager ABI file for your network to `files/abi.json`
5. Configure SSL.
   You can use your own certificates by placing them to `files/ssl/proxy` / `files/ssl/explorer` (if explorer is necessary) on the host machine and setting `cert_mode=custom` in inventory.
   Alternatively if you have DigitalOcean account you can put `cert_mode=certbot` and add `do_token` and `email` to inventory to issue free Let's encrypt certificates using certbot.
   > Note: Right now only DigitalOcean can be used to complete ACME challenge for 'certbot' mode.

## 3. Usage

Once everything is in place, run:

```bash
ansible-playbook -i inventory main.yaml
```

## Contributing

**If you have any questions please ask our development community on [Discord](https://discord.gg/vvUtWJB).**

[![Discord](https://img.shields.io/discord/534485763354787851.svg)](https://discord.gg/vvUtWJB)

## License

All contributions are made under the [GNU Affero General Public License v3](LICENSE).
