

# Jenkins by Zasim

This guide is crafted and documented by **Zasim Neupane**, a DevOps enthusiast, to help beginners quickly get started with Jenkins on AWS EC2 with real-world setup instructions.

---

## Installation on AWS EC2 Instance

---

### Step 1: Launch an EC2 Instance

1. Log in to your **AWS Console**
2. Navigate to **EC2 > Instances**
3. Click **Launch Instances**

![Launch Instance](https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png)

Use:

* Amazon Linux 2 / Ubuntu 22.04 LTS
* t2.micro (Free Tier)
* Allow TCP 8080 in **Inbound Rules**

---

### ☕ Step 2: Install Java and Jenkins

#### Install Java

```bash
sudo apt update
sudo apt install openjdk-17-jre -y
java -version
```

#### Install Jenkins

```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
```

---

### Step 3: Open Port 8080 for Jenkins

#### Add Security Group Rule:

* Go to **EC2 > Instance**
* Scroll to **Security > Security Groups**
* Edit Inbound Rules

  * Add Rule: **Custom TCP**, Port: `8080`, Source: `0.0.0.0/0` (for public access)

![Inbound Rules](https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png)

---

### Step 4: Access Jenkins

* Open your browser and go to:

```
http://<your-ec2-public-ip>:8080
```

* Retrieve the Jenkins Admin Password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

* Paste it into the browser and log in.

![Unlock Jenkins](https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png)

---

### Step 5: Install Suggested Plugins

* Click on **"Install suggested plugins"**

![Install Plugins](https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png)

Wait a few minutes...

![Installing Plugins](https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png)

---

### Step 6: Create Admin User

Fill out your admin credentials or skip the step.

![Create Admin User](https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png)

---

 Jenkins is now ready to use!

![Jenkins Dashboard](https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png)

---

## Setup Docker Pipeline Plugin

1. Go to **Manage Jenkins** > **Manage Plugins**
2. In the `Available` tab, search: `Docker Pipeline`
3. Click **Install**
4. Restart Jenkins when prompted

![Docker Plugin](https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png)

---

## Setup Docker as Jenkins Agent

### Step 1: Install Docker

```bash
sudo apt update
sudo apt install docker.io -y
```

### Step 2: Add Permissions

```bash
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
sudo systemctl restart docker
```

### Step 3: Restart Jenkins

```bash
http://<your-ec2-public-ip>:8080/restart
```

---

## You’re Done!

You’ve installed Jenkins, exposed it to the internet, installed Docker, and configured it to run inside EC2 with Docker support. From here, you can start building pipelines, trigger jobs, deploy apps, and automate workflows.

---

**Created with by [Zasim Neupane](https://github.com/iamzasem)**

---

