# Roblox Games and AWS Identity and Access Management

If you already know about Identity and Access Management, the principle of least privilege, and compartmentalization, then you can skip the first two sections to *How does IAM apply to Roblox Games?*

## What is Identity and Access Management?
One important of any Cloud Computing/Infrastructure-as-a-Service provider (such as Amazon Web Services, but also includes alternatives such as Google Cloud Platform, Microsoft Azure, and IBM Cloud, among numerous others) is **Identity and Access Management**, or IAM.

Identity and Access Management solves the question of "who am I?". For instance, if you are logging into the AWS Management Console, you are effectively asked the following question:

> Who are you?

> **I am** user "tycoonlover1359" of organization "example-organization" and I am using this password to assert that.

Much is the same when accessing AWS programmatically. Instead of logging in using a username and password, you instead use an **Access Key ID** and **Secret Access Key**. When a server asks to access AWS services, a similar dialogue occurs:

> Who are you?

> **I am** user "tycoonlover1359" and I am using Access Key ID "A1234567" and Secret Access Key "/124567890abcdefgh" to assert that.

Additionally, whenever setting up an IAM User within any cloud-services provider, it is incredibly important to follow the **Principle of Least Privilege** when assigning access policies to IAM Users.

## Principle of Least Privilege and Compartmentalization
The principle of least privilege essentially says that not all users should have access to all resources; instead users should only have access to the resources required to complete their task successfully. Ideally, users should only have access to the bare minimum required to complete their task; they should require permission from a higher-privileged user to access any resource they don't already have access to (for instance, a user may require additional resources to complete a one-off assignment).

A real-world example of this is access control for many companies and their campuses. For instance, take an AWS/GCP/Microsoft Azure datacenter. Firstly, to even enter the datacenter campus, a person must either be an employee that works at the datacenter, or be a guest who has received permission to enter the campus.

Next, many datacenters have increasingly stricter "rings" of security as a person gets closer to the physical servers running customer's programs. Subsequently, only authorized employees are granted access to these inner rings. For instance, a service technician who must replace broken down servers inside the datacenter would have access to the physical servers; on the other hand, a security officer managing access to the data likely wouldn't need physical access to the areas containing servers (at least, on a daily basis).

As an additional layer to the principle of least privilege, **compartmentalization** is also often implemented. Compartmentalization is the act restricting access to a given **sub**set of resources to authorized users. 

For instance, if you have documents filed under the "documents" resource, two different employees may have access to the documents resource but each would only have access to documents required to complete their job; as far as they are concerned, the other documents their coworker accessed doesn't exist.

## How does IAM apply to Roblox Games?
In a similar way to you accessing AWS services via the AWS Management Console or the AWS Command Line Interface (CLI), servers and programs must use programmatic access to interface with and utilize AWS services. Roblox games are no different; at their heart, they are still servers executing arbitrary code. As such, Roblox games must use programmatic access to utilize AWS services.

To use the Roblo3 SDK whilst still taking into account the principle of least privilege and compartmentalization, it is best to treat each game you make and integrate AWS into as different IAM Users. This has two benefits:

- Specific games can be assigned to only have access to the specific resources required to function; and
- Revocation of compromised security credentials becomes incredibly easy to do without affect any games utilizing credentials that haven't been compromised; in other words, revoking one game's credentials doesn't affect other games' ability to function with AWS.

Doing this means that each game has access to only the resources required for it to function, and (more importantly) that **each game** has a **different set** of security credentials (Access Key ID and Security Access Key).