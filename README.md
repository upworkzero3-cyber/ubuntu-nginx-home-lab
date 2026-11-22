# Ubuntu Server Home Lab

I deployed Nginx on Ubuntu Server in Oracle VirtualBox, served a custom
page to my Windows host, and tested a backup and restore.

## Setup

- Guest: Ubuntu Server
- Host: Windows
- Web server: Nginx
- VirtualBox NAT rule: Windows `127.0.0.1:8080` → Ubuntu port `80`

## Reproduce the setup

Run these commands in Ubuntu:

```bash
sudo apt update
sudo apt install nginx
systemctl is-active nginx
curl -I http://localhost
echo 'Ubuntu Server Home Lab' | sudo tee /var/www/html/lab.html
```

In VirtualBox, add a TCP port forwarding rule with host IP `127.0.0.1`,
host port `8080`, and guest port `80`. Leave the guest IP blank if the
VM gets its address automatically.

On the Windows host, open `http://127.0.0.1:8080/lab.html`.

## Backup and restore test

```bash
cp /var/www/html/lab.html ~/lab-backup.html
sudo cp ~/lab-backup.html /var/www/html/lab-restored.html
diff -s /var/www/html/lab.html /var/www/html/lab-restored.html
```

`diff` reported that the files were identical. I opened the restored
page at `http://127.0.0.1:8080/lab-restored.html` from Windows.
