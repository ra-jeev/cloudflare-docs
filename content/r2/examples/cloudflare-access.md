---
title: Protect an R2 Bucket with Cloudflare Access
pcx_content_type: configuration
---

# Protect an R2 bucket with Cloudflare Access

You can secure access to R2 buckets using [Cloudflare Access](/cloudflare-one/applications/configure-apps/).

Access allows you to only allow specific users, groups or applications within your organization to access objects within a bucket, or specific sub-paths, based on policies you define.

{{<Aside type="note">}}

For providing secure access to bucket objects for anonymous users, we recommend using [pre-signed URLs](/r2/api/s3/presigned-urls/) instead.

Pre-signed URLs do not require users to be a member of your organization and enable programmatic application directly.

{{</Aside>}}

## 1. Create a bucket

_If you have an existing R2 bucket, you can skip this step._

You will need to create an R2 bucket. Follow the [R2 get started guide](/r2/get-started/) to create a bucket before returning to this guide.

## 2. Create an Access application

Within the **Zero Trust** section of the Cloudflare Dashboard, you will need to create an Access application and a policy to restrict access to your R2 bucket.

If you have not configured Cloudflare Access before, we recommend:

* Configuring an [identity provider](/cloudflare-one/identity/) first to enable Access to use your organization's single-sign on (SSO) provider as an authentication method.
* Creating an [Access group](/cloudflare-one/identity/users/groups/#access-groups) that defines which users or groups within your organization can access specific resources.

To create an Access application for your R2 bucket:

1. Go to [**Access**](https://one.dash.cloudflare.com/?to=/:account/access/apps) and select **Add an application**
2. Select **Self-hosted**
3. Enter an **Application name**
4. Enter the **Application domain**. The **Domain** must be a domain hosted on Cloudflare, and the **Subdomain** part of the custom domain you will connect to your R2 bucket. For example, if you want to serve files from `behind-access.example.com` and `example.com` is a domain within your Cloudflare account, then enter `behind-access` in the subdomain field and select `example.com` from the **Domain** list.
5. (Optional) Configure the block page policy. This can be changed later.
6. Configure the **Identity providers** that will be used to protect this domain using Access.
7. Click **Next**
8. Enter a **Policy name** and an **Action**. This should be **Allow**, and will enable the group(s) you select to access objects within the bucket behind this Access application.
9. To **Assign a group** (or groups) and allow access to your bucket, select one or more groups. If you have not created any groups, you will [need to do this first](/cloudflare-one/identity/users/groups/#access-groups). You should **ensure that this group only contains the users within your organization that need access to this R2 bucket**.
10. Click **Next** and then **Add an application**.

Review the [Cloudflare Access documentation](/cloudflare-one/applications/configure-apps/self-hosted-apps/) to understand how to configure additional Access application options.

## 3. Connect a custom domain

{{<Aside type="warning">}}

You should create an Access application before connecting a custom domain to your bucket, as connecting a custom domain will otherwise make your bucket public by default.

{{</Aside>}}

You will need to [connect a custom domain](/r2/buckets/public-buckets/#connect-a-bucket-to-a-custom-domain) to your bucket in order to configure it as an Access application. Make sure the custom domain **is the same domain** you entered when configuring your Access policy.

{{<render file="_custom-domain-steps.md">}}

## 4. Test your Access policy

Visit the custom domain you connected to your R2 bucket, which should present a Cloudflare Access authentication page with your selected identity provider(s) and/or authentication methods.

For example, if you connected Google and/or GitHub identity providers, you can log in with those providers. If the login is successful and your account is a member of the [Access group](/cloudflare-one/identity/users/groups/#access-groups) you associated with the Access application you created in this guide, you will be able to access (read/download) objects within the R2 bucket.

If you cannot authenticate or receive a block page after authenticating, check that you have an [Access policy](/cloudflare-one/applications/configure-apps/self-hosted-apps/#2-add-an-access-policy) configured within your Access application that explicitly allows the group your user account is associated with.

## Next steps

* Learn more about [Access applications](/cloudflare-one/applications/configure-apps/) and how to configure them.
* Understand how to use [pre-signed URLs](/r2/api/s3/presigned-urls/) to issue time-limited and prefix-restricted access to objects for users not within your organization.
* Review the [documentation on using API tokens to authenticate](/r2/api/s3/tokens/) against R2 buckets.