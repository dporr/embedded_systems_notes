# Serve kenel binaries and source from Host over TFTP

## install & configure TFTP
```shell
sudo apt install tftpd-hpa
sudo systemctl start tftpd-hpa
sudo cp -r <KERNEL_DIR> /srv/tftp/
sudo chmod -R 777 /srv/tftp/
```

# ALWAYS stop and cleanup the tftp server
```shell
sudo systemctl start tftpd-hpa
sudo rm -rf /srv/tftp/
```