# Opensuse 15.1 Cheatsheet

## Update package manager
```bash
sudo zypper up
```

## Search package
```bash
sudo zypper search "PackageName"
```

## Install package
```bash
sudo zypper in "Packagename"
```

## Enabling application on server boot
```bash
sudo systemctl enable "PackageName"
```

## Starting the application
```bash
sudo systemctl start "PackageName"
```

## Checking application status
```bash
sudo systemctl status "PackageName"
```

## Restarting the application
```bash
sudo systemctl restart "PackageName"
```

## Installing PHP
```bash
sudo zypper in php7 php7-mysql php7-fpm php7-gd php7-mbstring php7-bcmath php7-zip php7-ldap php7-pdo
```

## Official repository of PHP 7.4
```bash
sudo zypper ar http://download.opensuse.org/repositories/devel:/languages:/php/openSUSE_Leap_15.1/ php
```

## Set repository priority
```bash
sudo zypper mr -p 70 php
```

## Refresh repository
```bash
sudo zypper refresh
```
