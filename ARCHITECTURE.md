# AWS VPC Architecture — AnyCompany

This document describes the logical and security architecture of the implemented AWS Virtual Private Cloud (VPC) design.  
It illustrates the flow of network communication, segregation of resources, and foundational components for a secure, scalable cloud environment.

---

## 🌐 High-Level Overview

The **AnyCompany VPC** (`10.0.0.0/18`) serves as the secure network boundary within the AWS Cloud, logically divided across **two Availability Zones** to achieve high availability and fault tolerance.

The architecture supports the multi-language pet care application for AnyCompany Pet Store, designed to facilitate global pet adoption by removing language barriers.
Key Application Components and Services:
1. Application Logic (Python/Boto3): The core application logic is implemented using Python scripts. This code leverages the AWS SDK for Python (Boto3) to interact with necessary AWS services. This application manages user input, handles data validation, and retrieves pet information from a defined database structure (a Python dictionary in the lab scenario).
2. Translation Service (Amazon Translate): The application integrates Amazon Translate to perform neural machine translation. This service is critical for translating pet profiles, which are initially available only in English, into the user's preferred target language (e.g., Spanish, German, French, or Italian).
3. Core Data Structure: The application utilizes a PETS_DATABASE dictionary (which holds pet ID, name, breed, age, price, and background information) and a function to look up language codes (e.g., mapping "Spanish" to 'es').
4. Development Environment: The solution was built and tested within a Visual Studio Code (VS Code) IDE environment, enhanced with Amazon Q, an AI-powered assistant used for coding tasks and generating code snippets.
The logical flow involves the application code calling the Amazon Translate API via Boto3, passing the English text, and receiving the translated text back to display to the user in a formatted structure.

## 🧱 Infrastructure as Code (CloudFormation Template)

The following YAML snippet represents the logical structure of the AnyCompany VPC architecture, defined as a CloudFormation template.  
It automates the creation of the same components described above.

> Full template available under `/templates/vpc-template.yaml`.

```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: >
  AnyCompany VPC Infrastructure - 2-Tier Network (Public/Private Subnets)

Resources:
  AnyCompanyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/18
      EnableDnsHostnames: true
      EnableDnsSupport: true
      Tags:
        - Key: Name
          Value: AnyCompany VPC

  InternetGateway:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
        - Key: Name
          Value: AnyCompany IGW

  VPCGatewayAttachment:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref AnyCompanyVPC
      InternetGatewayId: !Ref InternetGateway

  PublicSubnetA:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref AnyCompanyVPC
      CidrBlock: 10.0.0.0/20
      AvailabilityZone: !Select [0, !GetAZs '']
      MapPublicIpOnLaunch: true
      Tags:
        - Key: Name
          Value: AnyCompany subnet public-1

  PublicSubnetB:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref AnyCompanyVPC
      CidrBlock: 10.0.16.0/20
      AvailabilityZone: !Select [1, !GetAZs '']
      MapPublicIpOnLaunch: true
      Tags:
        - Key: Name
          Value: AnyCompany subnet public-2

  PrivateSubnetA:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref AnyCompanyVPC
      CidrBlock: 10.0.32.0/20
      AvailabilityZone: !Select [0, !GetAZs '']
      Tags:
        - Key: Name
          Value: AnyCompany subnet private-1

  PrivateSubnetB:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref AnyCompanyVPC
      CidrBlock: 10.0.48.0/20
      AvailabilityZone: !Select [1, !GetAZs '']
      Tags:
        - Key: Name
          Value: AnyCompany subnet private-2
