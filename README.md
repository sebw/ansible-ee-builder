# Ansible Execution Environment Builder

Tested on Fedora 34.

From a bash terminal, follow these instructions.

Install virtualenv:

```
sudo dnf install python3-virtualenv
```

Create a Python virtual environment, activate it and install ansible-builder:

```
virtualenv builder
. builder/bin/activate
pip install ansible-builder
```

Clone this repository:

```
git clone https://github.com/RedHatBelux/ansible-ee-builder.git
```

Build the image:

```
cd ansible-ee-builder
ansible-builder build --tag my_first_ee_image --container-runtime podman
```

Confirm that the image is available locally:

```
(builder) bash-5.1$ podman images my_first_ee_image
REPOSITORY                   TAG         IMAGE ID      CREATED         SIZE
localhost/my_first_ee_image  latest      bddfc827faf6  19 minutes ago  751 MB
```

Tag your image:

```
podman tag localhost/my_first_ee_image quay.io/YOUR-QUAY-USER/my_ee_image:0.0.1
```

Log into quay and push your image:

```
podman login quay.io
podman push quay.io/YOUR-QUAY-USER/my_ee_image:0.0.1
```

If you get an error, just make sure the Quay repository is public.

From this point on, you can create a new execution environment in AAP 2 under Administration > Execution Environment > Add.

You can then configure your project or job template to use your newly published execution environment.

# More info

https://www.ansible.com/blog/introduction-to-ansible-builder


