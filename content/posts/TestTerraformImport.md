---
title: 测试 terraform import
date: 2022-07-27T00:27:00+08:00
comments: true
toc: true
tags:
 - Terraform
---

本篇将以阿里云、AWS、GCP等云商为例子，测试 terraform apply 和 import 命令，并观察不同的云商对于Terraform创建的资源是否会有相应的标签或者其他标识，以及对于同一个资源，将其导入至多个Terraform State中是否会有限制

# 阿里云

## apply

获取相关认证信息，通过以下模板创建EIP（敏感信息已脱敏）

```hcl
terraform {
  required_providers {
    alicloud = {
      source = "aliyun/alicloud"
      version = "1.177.0"
    }
  }
}

provider "alicloud" {
  ......
}

resource "alicloud_eip_address" "wzk_test" {
  address_name         = "wzk-test-eip"
  isp                  = "BGP"
  internet_charge_type = "PayByBandwidth"
  payment_type         = "PayAsYouGo"
}
```

创建成功

is-resource-from-terraform-being-tagged-1.png

回到阿里云控制台，查看eip相关属性，没有标签等信息能知道该资源是被Terraform创建且纳管的

is-resource-from-terraform-being-tagged-2.png

## import

### 一份资源导入一份Terraform State

在阿里云控制台创建eip，创建成功

is-resource-from-terraform-being-tagged-3.png

回到命令行准备使用 terraform import 命令导入

在模板中添加新的resource块，模板如下所示

```hcl
terraform {
  required_providers {
    alicloud = {
      source = "aliyun/alicloud"
      version = "1.177.0"
    }
  }
}

provider "alicloud" {
  ......
}

resource "alicloud_eip_address" "wzk_test" {
  address_name         = "wzk-test-eip"
  isp                  = "BGP"
  internet_charge_type = "PayByBandwidth"
  payment_type         = "PayAsYouGo"
}

# 需要导入的eip资源
resource "alicloud_eip_address" "imported_eip" {
  
}
```

运行 `terraform import alicloud_eip_address.imported_eip eip-bp1emck8mckdshu******`，成功导入

查看eip控制台页面，也没有相关的标识或标签

### 一份资源导入多份Terraform State

拿上文生成的 `eip-going-to-be-imported` 资源，导入另一个Terraform State，模板如下

```hcl
terraform {
  required_providers {
    alicloud = {
      source = "aliyun/alicloud"
      version = "1.177.0"
    }
  }
}

provider "alicloud" {
  ......
}

# 需要导入的eip资源
resource "alicloud_eip_address" "imported_eip" {
  
}
```

运行 `terraform import alicloud_eip_address.imported_eip eip-bp1emck8mckdshu******`，成功导入，无报错

### 题外话

导入资源后，在tf文件中删除相关的resouce语句块，运行terraform plan，查看预期结果

不出所料，提示对应资源将被 destroy

# AWS
## apply

获取相关认证信息，通过以下模板创建vpc与eip（敏感信息已脱敏）

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "4.23.0"
    }
  }
}

# Configure the AWS Provider
provider "aws" {
  region = "ap-southeast-1"
}

# Create a VPC
resource "aws_vpc" "test_wzk_vpc" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_eip" "test_wzk_eip" {
  vpc = true
  associate_with_private_ip = "10.0.0.12"
}
```

创建成功

到AWS控制台，查看创建出来的VPC和EIP，均未有相关标识信息

## import

### 一份资源导入一份Terraform State

在AWS的VPC界面创建新的