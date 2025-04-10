# SKALE Proxy Provisioning

[![Discord](https://img.shields.io/discord/534485763354787851.svg)](https://discord.gg/vvUtWJB)

A set of ansible playbooks to setup SKALE Proxy and Block explorer in the cloud

## 1. Installation

> Make sure that you have Python 3.7+ installed

1. Clone this repo
2. Create venv & install requrements:

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## 2. Setup

1. Create inventory for your network:

   ```bash
   cp inventory-template inventory
   ```

2. Configure inventory

   > If you need only proxy - remove `[explorer]` section.

   ```bash
   [explorer]
   explorer-0 ansible_host= ansible_ssh_user=

   [proxy]
   proxy-0 ansible_host= ansible_ssh_user=

   [all:vars]
   ansible_ssh_user='' # username on machine(s) - e.g. ubuntu
   ansible_python_interpreter=/usr/bin/python3

   base_path= # home path on the machine(s) - e.g. /home/ubuntu
   proxy_domain='' # proxy domain name
   explorer_domain='' # explorer domain name (optional for proxy-only setup)
   eth_endpoint='' # Ethereum Mainnet endpoint

   cert_mode=  # 'certbot' - generate using certbot / custom - upload your own

   # Environment variable that determines if proxy is deployed behind AWS ALB.
   # When set to true, configures Nginx to use X-Forwarded-For header for client IP.
   # Set to true if deploying behind Application Load Balancer to ensure correct rate limiting.
   # Defaults to false for direct proxy deployments.
   use_alb=

   do_token= # optional - DigitalOcean token (for 'certbot' cert_mode)
   email= # optional - SSL cert email (for 'certbot' cert_mode)

   schain_names= # optional, run block explorer only for selected chains
   ```

1. Copy SKALE Manager ABI file for your network to `files/abi.json`
2. Configure SSL.
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

![GitHub](https://img.shields.io/github/license/skalenetwork/skale.py.svg)
All contributions are made under the [GNU Lesser General Public License v3](https://www.gnu.org/licenses/lgpl-3.0.en.html). See [LICENSE](LICENSE).
