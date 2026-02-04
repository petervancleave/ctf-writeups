# Cloud Computing Fundamentals




https://tryhackme.com/room/cloudcomputingfundamentals

---

# Introduction

Cloud computing overcomes local limitations like lag, limited access, and downtime by leveraging virtualization and containers to provide scalable, reliable, and efficient shared infrastructure.

## Learning Objectives

- What is cloud computing
- Service models of cloud (IaaS, PaaS, SaaS)
- Cloud Types (Private/Public/Hybrid)
- Benefits of cloud computing
- How big companies are using the cloud

# Cloud Computing Overview

Cloud computing solves the critical problems of scalability, accessibility, and reliability by allowing applications to use flexible, on-demand internet-based resources instead of relying on a single local computer. It evolved from physical servers to offer key benefits like scalability, cost-effectiveness, and high availability. Available in public, private, and hybrid deployment models, along with service models like IaaS, PaaS, and SaaS, the cloud is provided by major vendors such as AWS, Azure, and Google Cloud. It is widely adopted by companies like Netflix and Spotify to handle global user demand efficiently and focus on their core products rather than infrastructure management.

### Cloud Benefits and Characteristics

- **Scalability:** Easily scale up or down as your application's needs change.
- **On-demand self-service:** Create or remove servers and storage instantly, without waiting for hardware.
- **Pay only for what you use:** You are charged based on usage, not upfront costs.
- **Security:** Cloud providers protect the infrastructure with strong security measures.
- **High availability:** Applications keep running even if part of the system fails.
- **Global access:** Your application can be accessed by users anywhere in the world.

### Types of Cloud

- **Public Cloud:** Used by startups, websites, and global apps because it is affordable, easy to scale, and requires no infrastructure management. Public cloud services are preferable for nearly every use case.
- **Private Cloud:** Used by banks, healthcare, and government organizations because it offers greater control, customization, and compliance for sensitive data.
- **Hybrid Cloud:** Used by companies like e-commerce platforms that need to keep sensitive data private while still scaling publicly during high demand.

- **Infrastructure as a Service (IaaS):** You rent basic computing resources such as virtual servers, storage, and networking. You are responsible for managing the operating system and your application, while the provider manages the physical hardware.
- **Platform as a Service (PaaS):** The cloud provider manages the infrastructure and the operating system. You focus on building, deploying, and running your application without worrying about servers.
- **Software as a Service (SaaS):** You use a complete application over the internet. The provider manages everything, and you access the software through a browser or app, for example, Gmail or Zoom.


### Cloud Vendors

- **Microsoft Azure:** A strong competitor, especially in enterprise and hybrid cloud environments.
- **Google Cloud Platform (GCP):** Known for powerful data analytics, AI, and machine learning tools.
- **Alibaba Cloud:** A major player in Asia, offering competitive cloud services globally.
- **IBM Cloud:** Focuses on hybrid cloud and AI-driven solutions for businesses.
- **Oracle Cloud:** Focuses on enterprise applications and databases.


### How companies use cloud

- **Netflix** runs its entire platform on AWS so it can scale globally, stay online during peak demand, and stream content reliably to millions of users at once.
- **Spotify** uses the cloud to handle millions of songs and users, scaling quickly when new music or features are released.
- **Instagram** relies on the cloud to store massive amounts of photos and videos and deliver them fast to users around the world.
- **Online stores** use the cloud to handle traffic spikes during black friday without buying permanent infrastructure.

---

Q: What is the characteristic of cloud environments that enables you to handle an unexpected increase in access to your application?

A: Scalability

Q: What is the most common type of cloud deployment used?

A: Public Cloud

Q: Suppose you want to deploy an application to the internet, focusing only on application development and leaving infrastructure to others. What type of cloud service is the best?

A: PaaS

# Deploying a Cloud Instance


### Basic Cloud Terminology

- **EC2 (Virtual Computer / Server):** EC2 represents a virtual computer in the cloud. Just like a real computer, it has a CPU and memory (RAM) and can run applications. Whenever you add an EC2 instance, you are adding a computer to your environment.
- **Instance Type (for example: t2, t3, m5):** Instance types describe how powerful the virtual computer is. Some have more CPU and RAM and are therefore more expensive. You choose the Instance Type based on your needs, knowing that:
    - Bigger instances = more power + higher cost
    - Minor instances = less power + lower cost

### Deploying

You will create three virtual computers (EC2 instances) to host your cyber security training application. This aligns with the Infrastructure as a Service (IaaS) model you previously learned, as cyber security practices often require full access to the operating system. This allows you to install tools, configure the system, and safely simulate attacks and defenses, just like in real-world environments!

First, you need to choose a region where your resources will live. You can do it in the top right of your screen:


## Creating Virtual Machines

Now, go to the `Create Virtual Machine` block on the right side of the page to create the virtual machines for your application.  
First, let's create your application interface machine. Set the following configuration:

- Instance Name: `application-interface`
- Instance Type: `t3.micro`
- Status: `running`

Machine 1:

- Instance Name: `study-machine-1`
- Instance Type: `m5.large`
- Status: `running`

Machine 2:

- Instance Name: `study-machine-2`
- Instance Type: `m5.large`
- Status: `running`

## Billing Analysis

Let's analyze how much credit is costing our environment by navigating to the `Billing` section at the bottom of the page.  
There, you can check how much each type of instance is costing you, as well as the total.

Currently, you are still developing your application, so users have not yet accessed your platform. We can optimize costs by stopping the two study machines and reviewing the new cost of your environment.

Go to the `Instances` block and click the `Stop` button for both `study-machine-1` and `study-machine-2`.


---

Q: What is the total cost of credits of the entire environment if `study-machine-1` and `study-machine-2`  are stopped?

A: 30

Q: What is the total cost of credits of the entire environment if `study-machine-1` and `study-machine-2`  are stopped?

A: 70

Q: What is the total cost of credits if **only** **the new instances** we created are running?

A: 150

Q: What is the total cost of credits if **only** **the new instances** we created are running?

A: 188

