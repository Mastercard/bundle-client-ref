# Consumer Product Enrollment Reference Application

## Table of Contents
- [Overview](#overview)
  * [Compatibility](#compatibility)
  * [References](#references)
- [Usage](#usage)
  * [Prerequisites](#prerequisites)
  * [Access to sandbox](#access-to-sandbox)
  * [Onboarding and configuration](#onboarding-and-configuration)
  * [Integrating with Open API Generator](#integrating-with-openapi-generator)
  * [Build and Execute](#build-and-execute)
- [Use Cases](#use-cases)
- [API Reference](#api-reference)
  * [Authorization](#authorization)
  * [Request Examples](#request-examples)
- [Support](#support)

## Overview <a name="overview"></a>
The Consumer Product Enrollment(CPE) API was formerly the **Bundle Profile API**.

This is a product enrollment service that allows a user to enroll with Mastercard solutions. The Mastercard solutions are preconfigured bundles comprised of multiple products and services. This API user enables the simultaneous user enrollment into the products within the solutions using optional consents and payment card accounts, and also provides the ability to optionally store consumer information with Mastercard Consumer Lifecycle Services domain.

Consumer Product Enrollment enables:
 * New consumer enrollment into products and solutions
 * View and update consumer enrollment details
 * Update the product and solution enrollment details
 
 This Reference application is a guide for using CPE APIs for product and solution enrollments. 
 Please visit Mastercard Developer portal for more details about the API: [Mastercard Developers.](https://developer.mastercard.com/consumer-management/documentation/)

### Compatibility <a name="compatibility"></a>
* [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or later

### References <a name="references"></a>
* [Mastercard’s OAuth Signer library](https://github.com/Mastercard/oauth1-signer-java)
* [Using OAuth 1.0a to Access Mastercard APIs](https://developer.mastercard.com/platform/documentation/using-oauth-1a-to-access-mastercard-apis/)

## Usage <a name="usage"></a>

### Prerequisites <a name="prerequisites"></a>
* [Mastercard Developers Account](https://developer.mastercard.com/dashboard) with access to "MastercardON" API
* A text editor or IDE
* [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 
* [Apache Maven 3.3+](https://maven.apache.org/download.cgi)

### Access to sandbox <a name="access-to-sandbox"></a>
The Sandbox environment allows you to experiment with the API using a reference application prior to formal onboarding and adoption of the CPE API.


### Onboarding and Configuration <a name="onboarding-and-configuration"></a>

* Clone the project - git clone https://github.com/Mastercard/bundle-client-ref.git.
* Sign up to create a [Mastercard Developers account](https://developer.mastercard.com/account/sign-up). In case you already have an account, login and follow the below steps.
* Create a new project in your account and add BundleEnablement API to your project.  
* Check for the approved status in mastercard Developer portal post which approved client can access the API.
* Configure project and download your sandbox signing keys. It will download a zip file.  
* Unzip the downloaded key and select .p12 file(certificate) from zip and copy it to src/main/resources in the project folder.
* Open `${project.basedir}/src/main/resources/application.properties` and configure the below parameters.
      
     >**mastercard.bundle.client.api.base.path=https://sandbox.api.mastercard.com**, it is a static field, for making the API calls.
          
     >**mastercard.bundle.client.p12.path=BundleProfile.p12**, this refers to the p12 file name obtained from the aforementioned steps.
          
     >**mastercard.bundle.client.ref.app.consumer.key=Abcdfefgjhilklmnopqrstuvwxyz-dxcq_zD7IiPa0df175e!22a7fddba56e800000000000000000**, this refers to your consumer key you obtain when you create a project under API.
            
     >**mastercard.bundle.client.ref.app.keystore.password=pwd**, this refers to the password you obtain when you create the p12 key.
     
### Integrating with OpenAPI Generator <a name="integrating-with-openapi-generator"></a>
You may refer to [Open API Generator](https://github.com/OpenAPITools/openapi-generator) used by "bundle-client-ref" that generates the API client libraries from [Open API Specs](https://github.com/OAI/OpenAPI-Specification). It provides generators and library templates for supporting multiple languages and frameworks. (Note: This section is informational and developer who integrates need not perform these steps.)

Also see:
* [Open API Generator (maven Plugin)](https://mvnrepository.com/artifact/org.openapitools/openapi-generator-maven-plugin)
* [CONFIG OPTIONS for java](https://github.com/OpenAPITools/openapi-generator/blob/master/docs/generators/java.md)

#### OpenAPI Generator Plugin Configuration

* maven plugin to build modules from Open API - Generator from the swagger given.
```xml
<!-- https://mvnrepository.com/artifact/org.openapitools/openapi-generator-maven-plugin -->
<plugin>
    <groupId>org.openapitools</groupId>
    <artifactId>openapi-generator-maven-plugin</artifactId>
    <version>${openapi-generator.version}</version>
    <executions>
        <execution>
            <goals>
                <goal>generate</goal>
            </goals>
            <configuration>
                <inputSpec>${project.basedir}/src/main/resources/bundle-profile.yaml</inputSpec>
                <generatorName>java</generatorName>
                <library>okhttp-gson</library>
                <generateApiTests>false</generateApiTests>
                <generateModelTests>false</generateModelTests>
                <configOptions>
                    <sourceFolder>src/gen/main/java</sourceFolder>
                    <dateLibrary>java8</dateLibrary>
                </configOptions>
            </configuration>
        </execution>
    </executions>
</plugin>
```

### Build and Execute <a name="build-and-execute"></a>
Once you have added the correct properties, you are ready to build the application. You can do this by navigating to the project’s base directory from the terminal and then by running the following command.

`mvn clean install`

When the project builds successfully, you can run the following command to start the project.

`java -jar target/bundle_client-1.0.0.jar <argument as below>`
 Example: `java -jar target/bundle_client-1.0.0.jar createUser`

 Argument: An argument defines the feature an executor can use to run through the command line. If you don’t specify this argument, it will run all the features (create User, Read User, Update User (product / account)) one after the other. createUser, readUser, etc mentioned below are the arguments.

    * createUser - Provides initial enrollment of a user into products and solutions.
    * readUser   - Provides read-only view into the enrolled products of a given user.
    * addProduct - Adds product enrollment for the user.
	* replaceUser - Updates user's personal information across products.
    * addAccount - Provides addition of a payment account to a user.
    * removeAccount - Removes the payment account tagged with enrolled products. 
    * replaceAccount - Update user payment account on all products tied to the existing primary account number.
   
   
## Use Cases <a name="use-cases"></a>

> Case 1: [Enroll User](https://developer.mastercard.com/consumer-management/documentation/use-cases/products-enrollment/#enroll-user)

  - Provides initial enrollment of a user into products and solutions. This operation is used to complete the first user enrollment. 
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/users` | POST | [BundleUser](docs/BundleUser.md) | [BundleUserResponse](docs/BundleUserResponse.md) |
    
> Case 2: [View Enrolled Products](https://developer.mastercard.com/consumer-management/documentation/use-cases/products-enrollment/#view-enrolled-products)
  - Provides read-only view into the enrolled products of a given user. This operation is used to confirm enrollment status in concert with the POST enrollments and PATCH updates.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/users/{userid}` | GET | NA | [BundleUserResponse](docs/BundleUserResponse.md) |

> Case 3: [Add Product](https://developer.mastercard.com/consumer-management/documentation/use-cases/products-enrollment/#add-product)

  - Adds product enrollment for the user. This operation occurs only after the initial user enrollment (POST) and applies to solution journeys that gradually add product capabilities as the user incrementally interacts with the solution.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/users/{userid}/patches` | POST | [BundleUserPatch](docs/BundleUserPatch.md) | [BundleUserResponse](docs/BundleUserResponse.md) |
    
> Case 4: [Update User](https://developer.mastercard.com/consumer-management/documentation/use-cases/products-enrollment/#update-user) 

  - Updates user information across products. This operation is called when a user’s personal information is refreshed as part of a profile update.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/users/{userid}/patches` | POST | [BundleUserPatch](docs/BundleUserPatch.md) | [BundleUserResponse](docs/BundleUserResponse.md) |

	
> Case 5: [Add Payment Account](https://developer.mastercard.com/consumer-management/documentation/use-cases/products-enrollment/#add-payment-account)

  - Provides addition of a payment account to a user. This operation is called when a user’s payment information is refreshed as part of a profile update.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/users/{userid}/patches` | POST | [BundleUserPatch](docs/BundleUserPatch.md) | [BundleUserResponse](docs/BundleUserResponse.md) |
    
> Case 6: [Remove Payment Account](https://developer.mastercard.com/consumer-management/documentation/use-cases/products-enrollment/#remove-payment-account) 

  -  Removes the payment account tagged with enrolled products. This operation is called when a user’s payment information is refreshed as part of a profile update.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/users/{userid}/patches` | POST | [BundleUserPatch](docs/BundleUserPatch.md) | [BundleUserResponse](docs/BundleUserResponse.md) |

> Case 7: [Replace Payment Account](https://developer.mastercard.com/consumer-management/documentation/use-cases/products-enrollment/#replace-payment-account) 

  - Update user payment account on all products tied to the existing primary account number. This operation is called when a user’s payment information is refreshed as part of a profile update.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `/users/{userid}/patches` | POST | [BundleUserPatch](docs/BundleUserPatch.md) | [BundleUserResponse](docs/BundleUserResponse.md) |
    
> Case 8: [Enroll User](https://developer.mastercard.com/consumer-management/documentation/use-cases/user-enrollment/#enroll-user)

  - Provides initial enrollment of a user into Consumer Lifecycle Services. This operation is used to complete the first user enrollment or registration.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `enrollment/users` | POST | [EnrollmentUser](docs/EnrollmentUser.md) | [EnrollmentResponse](docs/EnrollmentResponse.md) |
    
> Case 9: [View User](https://developer.mastercard.com/consumer-management/documentation/use-cases/user-enrollment/#view-user)
  
  - Provides retrieval of enrollment details of a user enrolled to Consumer Lifecycle Services.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `enrollment/users/{userid}` | GET | NA | [EnrollmentUserResponse](docs/EnrollmentUserResponse.md) |
    
> Case 10: [Remove User](https://developer.mastercard.com/consumer-management/documentation/use-cases/user-enrollment/#remove-user) 
  
  - Provides deletion or removal of a user's enrollment from Consumer Lifecycle Services.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `enrollment/users/{userid}` | DELETE | NA | [EnrollmentUserResponse](docs/EnrollmentUserResponse.md) |
    
> Case 11: [Update User](https://developer.mastercard.com/consumer-management/documentation/use-cases/user-enrollment/#update-user)
  
  - Provides update capability for enrollment details of a user enrolled to Consumer Lifecycle Services.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `enrollment/users/{userid}` | PUT | [EnrollmentUser](docs/EnrollmentUser.md) | [EnrollmentUserResponse](docs/EnrollmentUserResponse.md) |
    
Case 12: [Partially Add User’s Details](https://developer.mastercard.com/consumer-management/documentation/use-cases/user-enrollment/#partially-add-user-s-details) 
  
  - Provides partial update capability of enrollment details by addition of fields for a user enrolled to Consumer Lifecycle Services.
  - Allows addition of user data without requiring other enrollment details to be passed in the request.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `enrollment/users/{userid}` | PATCH | [DiscretePatchesRequest](docs/DiscretePatchesRequest.md) | [EnrollmentUserResponse](docs/EnrollmentUserResponse.md) |
    
Case 13: [Partially Revise User’s Details](https://developer.mastercard.com/consumer-management/documentation/use-cases/user-enrollment/#partially-revise-user-s-details) 
  
  - Provides partial update capability of enrollment details by replacing field values for a user enrolled to Consumer Lifecycle Services.
  - Allows replacing of user data without requiring other enrollment details to be passed in the request.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `enrollment/users/{userid}` | PATCH | [DiscretePatchesRequest](docs/DiscretePatchesRequest.md) | [EnrollmentUserResponse](docs/EnrollmentUserResponse.md) |
    
Case 14: [Partally Remove User’s Details](https://developer.mastercard.com/consumer-management/documentation/use-cases/user-enrollment/#partally-remove-user-s-details)
  
  - Provides partial update capability of enrollment details by removing field values for a user enrolled to Consumer Lifecycle Services.
  - Allows removal of user data without requiring other enrollment details to be passed in the request.
  - For field level information, refer to model classes.
  
    | URL | Method | Request | Response |
    | :-- | :----- | :------ | :------- |
    | `enrollment/users/{userid}` | PATCH | [DiscretePatchesRequest](docs/DiscretePatchesRequest.md) | [EnrollmentUserResponse](docs/EnrollmentUserResponse.md) |
    
    
### Authorization <a name="authorization"></a>
For configuring your API client, the `com.mastercard.developer.interceptors` package provides you some request interceptor classes. These classes will take care of adding the correct [Authorization](https://github.com/Mastercard/oauth1-signer-java) header before sending the request.

### Request Examples <a name="request-examples"></a>
You can change the default input passed to APIs, modify values in the src/main/resources/templates for POST and UPDATE Use case. {userid} field is editable in the RequestHelper.java class. Below are the static User ID values configured for the Consumer Product Enrollment Reference Application. You may pass the below User IDs for GET operation to retrieve information on user’s enrolled products.

* For Products (airport, Wi-Fi), User ID - user234 is supported
* For Products (rewards, benefits, offers), User ID - user1235 is supported
* For Product: specialpay (Note: This is a test product), User ID - specialpayUser is supported


## Support <a name="support"></a>
For any clarifications, please reach out to Digital_Enablement_Team_3@mastercard.com.
