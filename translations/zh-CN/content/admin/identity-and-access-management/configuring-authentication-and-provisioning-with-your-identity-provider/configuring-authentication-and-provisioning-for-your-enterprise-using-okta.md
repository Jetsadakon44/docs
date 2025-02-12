---
title: Configuring authentication and provisioning for your enterprise using Okta
shortTitle: Configuring with Okta
intro: 'You can use Okta as an identity provider (IdP) to centrally manage authentication and user provisioning for {% data variables.product.prodname_ghe_managed %}.'
permissions: 'Enterprise owners can configure authentication and provisioning for {% data variables.product.prodname_ghe_managed %}.'
versions:
  ghae: '*'
redirect_from:
  - /admin/authentication/configuring-authentication-and-provisioning-with-your-identity-provider/configuring-authentication-and-provisioning-for-your-enterprise-using-okta
type: how_to
topics:
  - Accounts
  - Authentication
  - Enterprise
  - Identity
  - SSO
miniTocMaxHeadingLevel: 3
---

{% data reusables.saml.okta-ae-sso-beta %}

## 关于 SAML 和 SCIM 与 Octa

You can use Okta as an Identity Provider (IdP) for {% data variables.product.prodname_ghe_managed %}, which allows your Okta users to sign in to {% data variables.product.prodname_ghe_managed %} using their Okta credentials.

To use Okta as your IdP for {% data variables.product.prodname_ghe_managed %}, you can add the {% data variables.product.prodname_ghe_managed %} app to Okta, configure Okta as your IdP in {% data variables.product.prodname_ghe_managed %}, and provision access for your Okta users and groups.

The following provisioning features are available for all Okta users that you assign to your {% data variables.product.prodname_ghe_managed %} application.

| 功能       | 描述                                                                                                                                                                         |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 推送新用户    | When you create a new user in Okta, the user is added to {% data variables.product.prodname_ghe_managed %}.                                                              |
| 推送用户停用   | When you deactivate a user in Okta, it will suspend the user from your enterprise on {% data variables.product.prodname_ghe_managed %}.                                  |
| 推送个人资料更新 | When you update a user's profile in Okta, it will update the metadata for the user's membership in your enterprise on {% data variables.product.prodname_ghe_managed %}. |
| 重新激活用户   | When you reactivate a user in Okta, it will unsuspend the user in your enterprise on {% data variables.product.prodname_ghe_managed %}.                                  |

## 在 Okta 中添加 {% data variables.product.prodname_ghe_managed %} 应用程序

{% data reusables.saml.okta-ae-applications-menu %}
1. Click **Browse App Catalog**

  !["Browse App Catalog"](/assets/images/help/saml/okta-ae-browse-app-catalog.png)

1. In the search field, type "GitHub AE", then click **GitHub AE** in the results.

  !["Search result"](/assets/images/help/saml/okta-ae-search.png)

1. 单击 **Add（添加）**。

  !["Add GitHub AE app"](/assets/images/help/saml/okta-ae-add-github-ae.png)

1. For "Base URL", type the URL of your enterprise on {% data variables.product.prodname_ghe_managed %}.

  !["Configure Base URL"](/assets/images/help/saml/okta-ae-configure-base-url.png)

1. 单击 **Done（完成）**。

## Enabling SAML SSO for {% data variables.product.prodname_ghe_managed %}

To enable single sign-on (SSO) for {% data variables.product.prodname_ghe_managed %}, you must configure {% data variables.product.prodname_ghe_managed %} to use the sign-on URL, issuer URL, and public certificate provided by Okta. You can find locate these details in the "GitHub AE" app.

{% data reusables.saml.okta-ae-applications-menu %}
{% data reusables.saml.okta-ae-configure-app %}
1. Click **Sign On**.

  ![Sign On tab](/assets/images/help/saml/okta-ae-sign-on-tab.png)

1. Click **View Setup Instructions**.

  ![Sign On tab](/assets/images/help/saml/okta-ae-view-setup-instructions.png)

1. Take note of the "Sign on URL", "Issuer", and "Public certificate" details.
1. Use the details to enable SAML SSO for your enterprise on {% data variables.product.prodname_ghe_managed %}. 更多信息请参阅“[配置企业的 SAML 单点登录](/admin/authentication/managing-identity-and-access-for-your-enterprise/configuring-saml-single-sign-on-for-your-enterprise)”。

{% note %}

**Note:** To test your SAML configuration from {% data variables.product.prodname_ghe_managed %}, your Okta user account must be assigned to the {% data variables.product.prodname_ghe_managed %} app.

{% endnote %}

## Enabling API integration

The "GitHub AE" app in Okta uses the {% data variables.product.product_name %} API to interact with your enterprise for SCIM and SSO. This procedure explains how to enable and test access to the API by configuring Okta with a personal access token for {% data variables.product.prodname_ghe_managed %}.

1. In {% data variables.product.prodname_ghe_managed %}, generate a personal access token with the `admin:enterprise` scope. For more information, see "[Creating a personal access token](/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token)".
{% data reusables.saml.okta-ae-applications-menu %}
{% data reusables.saml.okta-ae-configure-app %}
{% data reusables.saml.okta-ae-provisioning-tab %}
1. 单击 **Configure API Integration（配置 API 集成）**。

1. 选择 **Enable API integration（启用 API 集成）**。

  ![Enable API integration](/assets/images/help/saml/okta-ae-enable-api-integration.png)

1. For "API Token", type the {% data variables.product.prodname_ghe_managed %} personal access token you generated previously.

1. 单击 **Test API Credentials（测试 API 凭据）**。

{% note %}

**Note:** If you see `Error authenticating: No results for users returned`, confirm that you have enabled SSO for {% data variables.product.prodname_ghe_managed %}. For more information see "[Enabling SAML SSO for {% data variables.product.prodname_ghe_managed %}](#enabling-saml-sso-for-github-ae)."

{% endnote %}

## Configuring SCIM provisioning settings

This procedure demonstrates how to configure the SCIM settings for Okta provisioning. These settings define which features will be used when automatically provisioning Okta user accounts to {% data variables.product.prodname_ghe_managed %}.

{% data reusables.saml.okta-ae-applications-menu %}
{% data reusables.saml.okta-ae-configure-app %}
{% data reusables.saml.okta-ae-provisioning-tab %}
1. Under "Settings", click **To App**.

  !["To App" settings](/assets/images/help/saml/okta-ae-to-app-settings.png)

1. 在“Provisioning to App（配置到 App）”的右侧，单击 **Edit（编辑）**。
1. 在“Create Users（创建用户）”的右侧，选择 **Enable（启用）**。
1. 在“Update User Attributes（更新用户属性）”的右侧，选择 **Enable（启用）**。
1. 在“Deactivate Users（停用用户）”的右侧，选择 **Enable（启用）**。
1. 单击 **Save（保存）**。

## Allowing Okta users and groups to access {% data variables.product.prodname_ghe_managed %}

You can provision access to {% data variables.product.product_name %} for your individual Okta users, or for entire groups.

### Provisioning access for Okta users

Before your Okta users can use their credentials to sign in to {% data variables.product.prodname_ghe_managed %}, you must assign the users to the "GitHub AE" app in Okta.

{% data reusables.saml.okta-ae-applications-menu %}
{% data reusables.saml.okta-ae-configure-app %}

1. Click **Assignments**.

  ![Assignments（分配）选项卡](/assets/images/help/saml/okta-ae-assignments-tab.png)

1. Select the Assign drop-down menu and click **Assign to People**.

  !["Assign to People" button](/assets/images/help/saml/okta-ae-assign-to-people.png)

1. To the right of the required user account, click **Assign**.

  ![List of users](/assets/images/help/saml/okta-ae-assign-user.png)

1. To the right of "Role", click a role for the user, then click **Save and go back**.

  ![Role selection](/assets/images/help/saml/okta-ae-assign-role.png)

1. 单击 **Done（完成）**。

### Provisioning access for Okta groups

You can map your Okta group to a team in {% data variables.product.prodname_ghe_managed %}. Members of the Okta group will then automatically become members of the mapped {% data variables.product.prodname_ghe_managed %} team. 更多信息请参阅“[将 Okta 组映射到团队](/admin/authentication/configuring-authentication-and-provisioning-with-your-identity-provider/mapping-okta-groups-to-teams)”。

## 延伸阅读

- Okta 文档中的[了解 SAML](https://developer.okta.com/docs/concepts/saml/).
- Okta 文档中的[了解 SCIM](https://developer.okta.com/docs/concepts/scim/).
