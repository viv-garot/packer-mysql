# packer-bionic64

## Description
Create a Vagrant ubuntu lts box via Packer

## Pre-requirements

* [Packer](https://www.packer.io/downloads)
* [Vagrant](https://www.vagrantup.com/downloads)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## How to use this repo

- Clone
- Build
- Vagrant usage
- Publish
- Consume 

---

### Clone the repo

```
git clone https://github.com/viv-garot/packer-bionic64
```

### Change directory

```
cd packer-bionic64
```

### Build the box with Packer

```
packer build bionic64.json
```

### How to use the box with Vagrant

#### Add the box to Vagrant

```
vagrant box add --name bionic64 bionic64-vbox.box
```

#### Initialize the box to create a VagrantFile

```
vagrant init -m bionic64
```

#### Start the box

```
vagrant up
```

### Access the box

```
vagrant ssh
```
or 
```
ssh vagrant@127.0.0.1 -p 2222 
```
*(password = vagrant)*
*(port 2222 is hardcoded in bionic64.json)*

### Destroy the box

```
vagrant destroy
```

### Remove the box

```
vagrant box remove bionic64
```


## Publish to Vagrant Cloud

### Login to Vagrant Cloud

```
vagrant cloud auth login
```

*Note : If you have Two-Factor authentication enabled the login will fail with the bellow error*
```
 - Two-factor authentication is enabled! Vagrant Cloud does not support creating (VagrantCloud::Error::ClientError::RequestError)
access tokens via the API when the user has two-factor authentication
enabled. Please create an access token using the Vagrant Cloud website and store
the result in the 'ATLAS_TOKEN' environment variable.
```

*- Create a token on Vagrant Cloud if not performed already*

![image](https://user-images.githubusercontent.com/85481359/124562546-6f819000-de3f-11eb-9609-8c5ed40dc159.png)

*- And export the result in the 'ATLAS_TOKEN' environment variable*

```
export ATLAS_TOKEN=<generated-token>
```

*- Then try again to login*

```
vagrant cloud auth login
```

### Create the box in Vagrant Cloud

```
vagrant cloud box create <user>/<box> --no-private
```
e.g.
```
vagrant cloud box create vivien/bionic64 --no-private
```

### Publish the box to Vagrant Cloud

```
vagrant cloud publish --box-version `date +%y.%m.%d` \
  --force --no-private --release <user>/<box>   \
  `date +%y.%m.%d` virtualbox bionic64-vbox.box
```

e.g.
```
vagrant cloud publish --box-version `date +%y.%m.%d` \
  --force --no-private --release vivien/bionic64   \
  `date +%y.%m.%d` virtualbox bionic64-vbox.box
```

## Consume the box from the Vagrant Cloud

### Create a sub directory

```
mkdir myboxes
```

### Change directory

```
cd myboxes
```

### Initialize the box to create a VagrantFile

```
vagrant init -m <user>/<box>
```

e.g.
```
vagrant init -m vivien/bionic64
```

### Start the box

```
vagrant up
```
