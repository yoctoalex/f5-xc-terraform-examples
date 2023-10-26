# F5 Distributed Cloud API Security CICD Deployment

## Prerequisites

### For CI/CD

* [F5 Distributed Cloud Account (F5XC)](https://console.ves.volterra.io/signup/usage_plan)
  * [F5XC API certificate](https://docs.cloud.f5.com/docs/how-to/user-mgmt/credentials)
  * [User Domain delegated](https://docs.cloud.f5.com/docs/how-to/app-networking/domain-delegation)
  * API Deployed and [Added as a pool in F5 Distributed Cloud](https://docs.cloud.f5.com/docs/how-to/app-networking/origin-pools)
* [Terraform Cloud Account](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started)
* [GitHub Account](https://github.com)

## Assets

* **xc:**        F5 Distributed Cloud WAAP

## Tools

* **IAC:** Terraform
* **IAC State:** Terraform Cloud
* **CI/CD:** GitHub Actions

## Terraform Cloud

* **Workspaces:** Create a CLI or API workspace for each asset in the workflow chosen. Check your work-flow article for more details


  | **Workflow** | **Assets/Workspaces** |
  | ------------ | --------------------- |
  | xcapi        | xc-api                |
  

* **Workspace Sharing:** Under the settings for each Workspace, set the **Remote state sharing** to share with each Workspace created.
  
* **Variable Set:** Create a Variable Set with the following values:

  | **Name**              | **Type**    | **Description**                                                                |
  | --------------------- | ----------- | ------------------------------------------------------------------------------ |
  | VOLT_API_P12_FILE     | Environment | Your F5XC API certificate. Set this to **api.p12**                             |
  | VES_P12_PASSWORD      | Environment | Set this to the password you supplied when creating your F5 XC API certificate |
  | tf_cloud_organization | Terraform   | Your Terraform Cloud Organization name                                         |

## GitHub

* **Fork and Clone Repo. Navigate to `Actions` tab and enable it.**

* **Actions Secrets:** Create the following GitHub Actions secrets in your forked repo
  *  P12: The linux base64 encoded F5XC API certificate
  *  TF_API_TOKEN: Your Terraform Cloud API token
  *  TF_CLOUD_ORGANIZATION: Your Terraform Cloud Organization name
  *  TF_CLOUD_WORKSPACE_*\<Workspace Name\>*: Create for each workspace in your workflow
      * EX: TF_CLOUD_WORKSPACE_XC_API would be created with the value `xc-api`

## Workflow Runs

**STEP 1:** Check out a branch for the workflow you wish to run using the following naming convention. 

  **DEPLOY**
  
  | Workflow | Branch Name  |
  | -------- | ------------ |
  | xc-api   | deploy-xcapi |
 
  **DESTROY**
  
  | Workflow | Branch Name   |
  | -------- | ------------- |
  | xc-api   | destroy-xcapi |


**STEP 2:** Rename `terraform.tfvars.examples` to `terraform.tfvars` and add/modify the following data:
  * project_prefix  = "Your project identifier name in **lower case** letters only - this will be applied as a prefix to all assets"
  * resource_owner = "Your-name"
  * api_url         = "Your F5XC tenant"
  * xc_tenant       = "Your tenant id available in F5 XC `Administration` section `Tenant Overview` menu"
  * xc_namespace    = "The existing XC namespace where you want to deploy resources"
  * app_domain      = "the FQDN of your app (cert will be autogenerated)"
  * xc_waf_blocking = "Set to true to enable blocking"
  * Also update assets boolean value as per your work-flow based on the values in the tfvars example.  The values in the example deploy what is done in the ClickOps version of the workshop.

**STEP 3:** Commit and push your build branch to your forked repo
  * Build will run and can be monitored in the GitHub Actions tab and TF Cloud console


**STEP 4:** Once the pipeline completes, verify your assets were deployed or destroyed based on your workflow.  
            **NOTE:**  The autocert process takes time.  It may be 5 to 10 minutes before Let's Encrypt has provided the cert.


## Development

Outline any requirements to setup a development environment if someone would like to contribute.  You may also link to another file for this information.

## Support

For support, please open a GitHub issue.  Note, the code in this repository is community supported and is not supported by F5 Networks.  

## Community Code of Conduct

Please refer to the [F5 DevCentral Community Code of Conduct](code_of_conduct.md).

## License

[Apache License 2.0](LICENSE)

## Copyright

Copyright 2014-2020 F5 Networks Inc.

### F5 Networks Contributor License Agreement

Before you start contributing to any project sponsored by F5 Networks, Inc. (F5) on GitHub, you will need to sign a Contributor License Agreement (CLA).

If you are signing as an individual, we recommend that you talk to your employer (if applicable) before signing the CLA since some employment agreements may have restrictions on your contributions to other projects.
Otherwise by submitting a CLA you represent that you are legally entitled to grant the licenses recited therein.

If your employer has rights to intellectual property that you create, such as your contributions, you represent that you have received permission to make contributions on behalf of that employer, that your employer has waived such rights for your contributions, or that your employer has executed a separate CLA with F5.

If you are signing on behalf of a company, you represent that you are legally entitled to grant the license recited therein.
You represent further that each employee of the entity that submits contributions is authorized to submit such contributions on behalf of the entity pursuant to the CLA.