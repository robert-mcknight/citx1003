# Lab 1: Setting up your laptop

## 1. Virtual Machines

The first tool we will set up on our devices is a Virtual Machine Manager (or Hypervisor). This software allows you to create and run virtual computer systems and other operating systems. There are many different Virtual Machine Managers, for this unit we recommend using VMWare, however, VirtualBox, Hyper-V, UTM or similar will also work.

You can find the files used in the following steps on [Microsoft Teams](https://teams.microsoft.com/l/channel/19%3AVJsk9zd2hT8RGZJG9gsXHYLcEmnPZ99aahe9QjBlF4g1%40thread.tacv2/General?groupId=657669ea-dcc0-4fec-bae1-b5cb9f7f1cfd&tenantId=05894af0-cb28-46d8-8716-74cdb46e2226) under "Files". 

### Step 1: Install a Virtual Machine Manager

If you're using a Windows computer, download the "VMWare Workstation" install file from Teams and install the software.


If you're using an Apple Mac computer, go to the [UTM](https://mac.getutm.app/) website and install the UTM virtualization software.

### Step 2: Extract the Virtual Machine files

If you're using a Windows computer, Download the kali-linux-2024.3-vmware-amd64.7z file from the "VMWare Workstation" folder on Teams. If the file fails to download, you can also download it from the [Kali Linux](https://www.kali.org/get-kali/#kali-virtual-machines) website.

 - Create a folder somewhere on your computer (eg. Documents\\Kali\\)

 - Extract the files from the .7z archive into the folder. On Windows, you might need to install the [7-zip software](https://www.7-zip.org/download.html) to perform the extraction.


If you're using an Apple Mac computer, Download the kali-linux-2023-arm64-utm.zip file from the "UTM" folder on Teams. If the file fails to download, you can also download it from the [UTM Virtual Machine Gallery](https://mac.getutm.app/gallery/kali-2023) website.

 - Create a folder somewhere on your computer (eg. ~/Documents/Kali/)

 - Extract the files from the .zip archive into the folder.


### Step 3: Open the Virtual Machine in VMWare

If you are using a Windows computer, the vm image (.vmx file) should open in VMWare. You can then run your virtual machine.


If you're using an Apple Mac computer, the vm image  should open in UTM. You can then run your virtual machine.


The default username and password for the kali VMs is:

**username:** kali  \
**password:** kali


**If you get stuck installing VMware or Kali, you can contact your Unit Coordinator.**
**There are several other alternatives to VMWare such as [Virtual Box](https://www.virtualbox.org/wiki/Downloads).**

 - If you want to try an alternative for Windows, you can install Virtual Box for free and download the virtual box vm image from [here](https://www.kali.org/get-kali/#kali-virtual-machines).

 - If you want to try an alternative for Apple Mac, you can try installing VMWare Fusion and creating a Kali VM through that.

## 2. Installing and running Docker

We will be using a technology called _Docker_ to run different environments on your laptop. 

You can get a comprehensive overview of what Docker is from here [https://docs.docker.com/get-started/overview/](https://docs.docker.com/get-started/overview/). To summarise though, Docker allows you to "package and run applications in a loosely isolated environment called a container". Containers are a way of virtualizing an environment by using the native operating system's functionality to isolate application environments.

### 2.1. Installing Docker (Windows/Mac/Linux)

Although it is possible to install Docker on your host computer, it's reccommended to install it inside your Kali Linux VM instead.

Docker on Kali linux can be installed by running the following commands in a terminal:

```bash
sudo apt update
sudo apt install docker.io
```

This uses the "Aptitude Package Manager" to download and install the docker application. Apt can be used to install many different programs and packages from the command-line terminal.

#### Apple Errors
If you are seeing errors installing applications in Kali on UTM on an Apple Mac, it may be that the signatures for the repositories are out of date (The Apple UTM version is the 2023 version of kali).

To fix those errors, use the following commands before installing docker:
```bash
wget http://http.kali.org/kali/pool/main/k/kali-archive-keyring/kali-archive-keyring_2024.1_all.deb
sudo dpkg -i kali-archive-keyring_2024.1_all.deb
sudo apt-get update
```
This will download the current Kali GPG Keyring and install it, allowing the APT package manager to update correctly.

### 4.2. Testing Docker

To test the environment, we will run a simple container that allows you to access a bash terminal. This allows you to enter commands that get executed within the container. You can only do what the container will let you do as it is a constrained environment.

To start with, make sure that your Docker Desktop application is running. Once it is, open a terminal window, PowerShell or Command prompt and run the following command (please note, the process may take a while on your machine).

{% hint style="warning" %}
All commands are treated as running from the VM. If running from the host, remove `sudo` at the beginning if it complains it cannot find `sudo`.
{% endhint %}

```bash
sudo docker pull uwacyber/cits1003-labs:bash
```

```bash
bash: Pulling from uwacyber/cits1003-labs
a31c7b29f4ad: Pull complete
56dc59d71033: Pull complete
2bfc36697d0c: Pull complete
9f3f7e1eed32: Pull complete
6f99373aa497: Pull complete
2bd679cc1668: Pull complete
312a9631755e: Pull complete
Digest: sha256:3aa1540adfa7a7bdd8e0955845e24372d2a7a28d5a9aa45f957abc9714a29aa2
Status: Downloaded newer image for uwacyber/cits1003-labs:bash
docker.io/uwacyber/cits1003-labs:bash
```

```bash
sudo docker run -it uwacyber/cits1003-labs:bash
```

Once the container is running, you can try the below command (first line only after the `#`, second line is the expected output) in the terminal:

```bash
root@9215e663eb9d:/# whoami
root
```

The `docker pull` command downloads the docker image to your machine. The image contains all of the files and configurations needed to run the container. You run a container using the `docker run` command as shown above.

In the case of the bash container, to stop it, you simply type `exit`. Other containers can be stopped using the `docker stop` command from another terminal. To do this, you need to provide the Container ID which you can do as follows:

```bash
0x4447734D4250:~$ sudo docker ps -a
CONTAINER ID   IMAGE                         COMMAND       CREATED         STATUS         PORTS     NAMES
45fe3a838ef0   uwacyber/cits1003-labs:bash   "/bin/bash"   3 minutes ago   Up 3 minutes             hungry_hodgkin
0x4447734D4250:~$ docker stop 45fe3a838ef0
45fe3a838ef0
```

By simply quitting with command `exit`, it saves the container. If you wish to remove the container automatically when you finish the session, add the `--rm` flag (this will be added in the examples by default):

```bash
sudo docker run -it --rm uwacyber/cits1003-labs:bash
```

This will automatically remove the container so you don't have to go to GUI to do it (of course, nothing you do in this container will be saved).

If you saved the container (i.e., not using the `--rm` flag) and want to restart that container that has stopped, first find the container ID you want to restart:

```bash
sudo docker ps -a
```

Next, restart the container:

```bash
sudo docker start -ai container_id
```

Here, the container ID is retrieved from the first column from the previous step (copy and paste).

Finally, once you have finished with a container, you can remove the container that was saved by:

```bash
sudo docker rm container_id
```

Remember that anything you have done in the container will be lost when you remove the container.

You can also delete the image downloaded from the Docker Desktop GUI, or from the command line find the image ID (column `IMAGE ID`):

```bash
sudo docker image ls
```

Delete the docker image:

```bash
sudo docker rmi image_id
```

We will be using containers in the various labs and so you will learn more about using Docker and how containers work generally as we proceed.

### Question 1. Find your first flag

Go back to the bash docker container. There is a file called flag.txt hidden somewhere. Can you find it?

{% tabs %}
{% tab title="" %}
Click on the Hint tab to reveal the solution
{% endtab %}

{% tab title="Hint" %}
{% hint style="info" %}
To find the file, we will first go to the home directory of the user root by using the

> cd /root

command. This will change the current directory to /root

Once there, we can list the contents of that directory by using the **ls** command (don't worry about the meaning of "-al" flag for now)

> ls -al

There will be a file called flag.txt in the directory. We can view the contents of the file by using the **cat** command:

> cat flag.txt
{% endhint %}
{% endtab %}
{% endtabs %}

**Flag: Submit the flag to the LMS**

## Case study: Mainstream cyber attacks: data breach

A data breach is an incident in which sensitive, protected, or confidential information is accessed, disclosed, or stolen without authorization. This can include personal information such as names, addresses, and Social Security numbers, as well as financial information such as credit card numbers and bank account information.

Data breaches can occur in various ways, such as hacking into a computer system, stealing physical devices containing data, or social engineering techniques like phishing emails or phone calls. The consequences of a data breach can be severe, including financial loss, damage to reputation, and potential legal liability.

Read through the following article and answer the questions below: [https://webo.digital/blog/optus-medibank-data-breaches-cyber-security](https://webo.digital/blog/optus-medibank-data-breaches-cyber-security)

### Question 2. CIA

Which aspect of cybersecurity do the cyber attacks primarily violate?

1. Confidentiality
2. Integrity
3. Availability
4. Authentication
5. Non-repudiation

{% hint style="info" %}
Submit your flag with the correct answer (e.g., CITS1003{1} if option 1 is the correct answer).
{% endhint %}

### Question 3. How was Optus compromised?

Which is the reason given by third-parties about how Optus was compromised?

1. Advanced Persistent Threat (APT) attacks
2. Scams such as spam text messages, phishing emails, etc.
3. Lack of authentication and authorization
4. None of the above

{% hint style="info" %}
Submit your flag with the correct answer (e.g., CITS1003{1} if option 1 is the correct answer).
{% endhint %}

### Question 4. Mitigation

What should be done to safeguard businesses from data breaches?

1. Implement, monitor and update customized IT policies and protocols
2. Enforce mandatory multi-factor authentication
3. Encrypt user data in an end-to-end way
4. All of the above

{% hint style="info" %}
Submit your flag with the correct answer (e.g., CITS1003{1} if option 1 is the correct answer).
{% endhint %}
