# Ansible Execution Environment (EE) Builder

Tested on Fedora 39 and upstream ansible-builder. 

!!! This would not result in a Red Hat supported execution environment !!!

From a bash terminal, follow these instructions.

Install virtualenv and podman if not yet installed:

```
sudo dnf install python3-virtualenv podman
```

Create a Python virtual environment, activate it and install ansible-builder within it:

```
virtualenv builder
. builder/bin/activate
pip install ansible-builder
```

Clone this repository:

```
git clone https://github.com/sebw/ansible-ee-builder.git
```

Have a look at the content of the repository. For the sake of the example we are including an Ansible collection (`requirements.yml`), some Python dependencies (`requirements.txt`) and add a RPM on top of it (`bindep.txt`).

Feel free to modify those to your liking.

Now build your image:

```
cd ansible-ee-builder/
ansible-builder build --tag my_first_ee_image --container-runtime podman
```

This can take a couple of minutes.

Wait for the "Complete! The build context can be found at..." message.

If you are curious, you can check the `context/Containerfile` that has been used to build your new EE.

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

In a web browser, log into your Quay account, create a new repository called `my_ee_image` and make sure your account has admin permissions.

From the CLI, log into quay.io and push your image:

```
podman login quay.io
podman push quay.io/YOUR-QUAY-USER/my_ee_image:0.0.1
```

From this point on, you can create a new execution environment in AAP 2 under Administration > Execution Environment > Add.

You can then configure your project or job template to use your newly published execution environment.

# More info

https://www.ansible.com/blog/introduction-to-ansible-builder


