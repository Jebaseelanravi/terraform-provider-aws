---
subcategory: "FMS (Firewall Manager)"
layout: "aws"
page_title: "AWS: aws_fms_policy"
description: |-
  Provides a resource to create an AWS Firewall Manager policy
---


<!-- Please do not edit this file, it is generated. -->
# Resource: aws_fms_policy

Provides a resource to create an AWS Firewall Manager policy. You need to be using AWS organizations and have enabled the Firewall Manager administrator account.

~> **NOTE:** Due to limitations with testing, we provide it as best effort. If you find it useful, and have the ability to help test or notice issues, consider reaching out to us on [GitHub](https://github.com/hashicorp/terraform-provider-aws).

## Example Usage

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { Fn, Token, TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { FmsPolicy } from "./.gen/providers/aws/fms-policy";
import { WafregionalRuleGroup } from "./.gen/providers/aws/wafregional-rule-group";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    const example = new WafregionalRuleGroup(this, "example", {
      metricName: "WAFRuleGroupExample",
      name: "WAF-Rule-Group-Example",
    });
    const awsFmsPolicyExample = new FmsPolicy(this, "example_1", {
      excludeResourceTags: false,
      name: "FMS-Policy-Example",
      remediationEnabled: false,
      resourceType: "AWS::ElasticLoadBalancingV2::LoadBalancer",
      securityServicePolicyData: {
        managedServiceData: Token.asString(
          Fn.jsonencode({
            defaultAction: {
              type: "BLOCK",
            },
            overrideCustomerWebACLAssociation: false,
            ruleGroups: [
              {
                id: example.id,
                overrideAction: {
                  type: "COUNT",
                },
              },
            ],
            type: "WAF",
          })
        ),
        type: "WAF",
      },
      tags: {
        Name: "example-fms-policy",
      },
    });
    /*This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.*/
    awsFmsPolicyExample.overrideLogicalId("example");
  }
}

```

## Argument Reference

This resource supports the following arguments:

* `name` - (Required, Forces new resource) The friendly name of the AWS Firewall Manager Policy.
* `deleteAllPolicyResources` - (Optional) If true, the request will also perform a clean-up process. Defaults to `true`. More information can be found here [AWS Firewall Manager delete policy](https://docs.aws.amazon.com/fms/2018-01-01/APIReference/API_DeletePolicy.html)
* `deleteUnusedFmManagedResources` - (Optional) If true, Firewall Manager will automatically remove protections from resources that leave the policy scope. Defaults to `false`. More information can be found here [AWS Firewall Manager policy contents](https://docs.aws.amazon.com/fms/2018-01-01/APIReference/API_Policy.html)
* `description` - (Optional) The description of the AWS Network Firewall firewall policy.
* `excludeMap` - (Optional) A map of lists of accounts and OU's to exclude from the policy.
* `excludeResourceTags` - (Required, Forces new resource) A boolean value, if true the tags that are specified in the `resourceTags` are not protected by this policy. If set to false and resource_tags are populated, resources that contain tags will be protected by this policy.
* `includeMap` - (Optional) A map of lists of accounts and OU's to include in the policy.
* `remediationEnabled` - (Required) A boolean value, indicates if the policy should automatically applied to resources that already exist in the account.
* `resourceTags` - (Optional) A map of resource tags, that if present will filter protections on resources based on the exclude_resource_tags.
* `resourceType` - (Optional) A resource type to protect. Conflicts with `resourceTypeList`. See the [FMS API Reference](https://docs.aws.amazon.com/fms/2018-01-01/APIReference/API_Policy.html#fms-Type-Policy-ResourceType) for more information about supported values.
* `resourceTypeList` - (Optional) A list of resource types to protect. Conflicts with `resourceType`. See the [FMS API Reference](https://docs.aws.amazon.com/fms/2018-01-01/APIReference/API_Policy.html#fms-Type-Policy-ResourceType) for more information about supported values. Lists with only one element are not supported, instead use `resourceType`.
* `securityServicePolicyData` - (Required) The objects to include in Security Service Policy Data. Documented below.
* `tags` - (Optional) Key-value mapping of resource tags. If configured with a provider [`defaultTags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block) present, tags with matching keys will overwrite those defined at the provider-level.

## `excludeMap` Configuration Block

* `account` - (Optional) A list of AWS Organization member Accounts that you want to exclude from this AWS FMS Policy.
* `orgunit` - (Optional) A list of IDs of the AWS Organizational Units that you want to exclude from this AWS FMS Policy. Specifying an OU is the equivalent of specifying all accounts in the OU and in any of its child OUs, including any child OUs and accounts that are added at a later time.

You can specify inclusions or exclusions, but not both. If you specify an `includeMap`, AWS Firewall Manager applies the policy to all accounts specified by the `includeMap`, and does not evaluate any `excludeMap` specifications. If you do not specify an `includeMap`, then Firewall Manager applies the policy to all accounts except for those specified by the `excludeMap`.

## `includeMap` Configuration Block

* `account` - (Optional) A list of AWS Organization member Accounts that you want to include for this AWS FMS Policy.
* `orgunit` - (Optional) A list of IDs of the AWS Organizational Units that you want to include for this AWS FMS Policy. Specifying an OU is the equivalent of specifying all accounts in the OU and in any of its child OUs, including any child OUs and accounts that are added at a later time.

You can specify inclusions or exclusions, but not both. If you specify an `includeMap`, AWS Firewall Manager applies the policy to all accounts specified by the `includeMap`, and does not evaluate any `excludeMap` specifications. If you do not specify an `includeMap`, then Firewall Manager applies the policy to all accounts except for those specified by the `excludeMap`.

## `securityServicePolicyData` Configuration Block

* `managedServiceData` - (Optional) Details about the service that are specific to the service type, in JSON format. For service type `SHIELD_ADVANCED`, this is an empty string. Examples depending on `type` can be found in the [AWS Firewall Manager SecurityServicePolicyData API Reference](https://docs.aws.amazon.com/fms/2018-01-01/APIReference/API_SecurityServicePolicyData.html).
* `policyOption` - (Optional) Contains the Network Firewall firewall policy options to configure a centralized deployment model. Documented below.
* `type` - (Required, Forces new resource) The service that the policy is using to protect the resources. For the current list of supported types, please refer to the [AWS Firewall Manager SecurityServicePolicyData API Type Reference](https://docs.aws.amazon.com/fms/2018-01-01/APIReference/API_SecurityServicePolicyData.html#fms-Type-SecurityServicePolicyData-Type).

## `policyOption` Configuration Block

* `networkFirewallPolicy` - (Optional) Defines the deployment model to use for the firewall policy. Documented below.
* `thirdparty_firewall_policy` - (Optional) Defines the policy options for a third-party firewall policy. Documented below.

## `networkFirewallPolicy` Configuration Block

* `firewallDeploymentModel` - (Optional) Defines the deployment model to use for the firewall policy. To use a distributed model, remove the `policyOption` section. Valid values are `CENTRALIZED` and `DISTRIBUTED`.

## `thirdparty_firewall_policy` Configuration Block

* `firewallDeploymentModel` - (Optional) Defines the deployment model to use for the third-party firewall policy. Valid values are `CENTRALIZED` and `DISTRIBUTED`.

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `id` - The AWS account ID of the AWS Firewall Manager administrator account.
* `policyUpdateToken` - A unique identifier for each update to the policy.
* `tagsAll` - A map of tags assigned to the resource, including those inherited from the provider [`defaultTags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block).

## Import

In Terraform v1.5.0 and later, use an [`import` block](https://developer.hashicorp.com/terraform/language/import) to import Firewall Manager policies using the policy ID. For example:

```typescript
// DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import { TerraformStack } from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { FmsPolicy } from "./.gen/providers/aws/fms-policy";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    FmsPolicy.generateConfigForImport(
      this,
      "example",
      "5be49585-a7e3-4c49-dde1-a179fe4a619a"
    );
  }
}

```

Using `terraform import`, import Firewall Manager policies using the policy ID. For example:

```console
% terraform import aws_fms_policy.example 5be49585-a7e3-4c49-dde1-a179fe4a619a
```

<!-- cache-key: cdktf-0.20.1 input-0ae1df390d47241169d44152adee2f18fc2ef166fb2e999418b584927f81b7b3 -->