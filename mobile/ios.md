---
coverY: 0
---

# 🍄 iOS

## Architecture

### Application main architecture (Clean Swift)

![](../iOS/assets/clean-full-picture-2.png)

The Clean Swift architecture is derived from the Clean Architecture proposed by Uncle Bob. They share many common concepts such as the components, boundaries, and models.

### The VIP Cycle

The view controller, interactor, and presenter are the three main components of Clean Swift. They act as input and output to one another as shown in the following diagram.

![](../iOS/assets/vip-2.png)

### Card component architecture (MVVM)

![](../iOS/assets/mvvm.png)

## Dependency management

* Cartage
* Cocoapods
* Swift Package Manager

![](../iOS/assets/dependency.png)

## Security

Jailbreak detection

* iOSSecuritySuite

SSL pinning

* public key pinning

#### **Data security**

* \[LV.1] General data (user default)
* \[LV.2] Credential (keychain)
* \[LV.3] Super credential (keychain + secure enclave)

![A12, S4, and later systems on chip (SoCs)](../iOS/assets/enclave.png)



## Web3

* trustwallet/**wallet-core**
* argentlabs/web3.swift

![](<../.gitbook/assets/Screen Shot 2565-07-08 at 15.24.57.png>)

## Code Quality

* Formatter > SwiftFormat
* Lint > SwiftLinter
* Unit Test > CodeCov

![](<../.gitbook/assets/Screen Shot 2565-07-08 at 14.21.22 (1).png>)

![](<../.gitbook/assets/Screen Shot 2565-07-08 at 14.21.55.png>)

* UI Test&#x20;
  * xCode UI Test&#x20;
  * Cucumber
  * Firebase Testlab

![UI Test process](<../.gitbook/assets/Screen Shot 2565-07-08 at 15.26.17.png>)

```swift
Feature: Display User Portfolio
  Background: Application is running
  Scenario: Check Portfolio Page with Existing Properties
    Given I am on "Portfolio" page
    Then I should see "Total Value"
    Then I should see "Receive" button is enabled
    Then I should see "Send" button is enabled
    Then I should see "Wallet" tab is focused
    Then I should see "Asset List" in descening order by amount
    Then I should see "Import Token" is enabled

  Scenario: Check Wallet Expanding
    Given I am on "Portfolio" page
    When I click "BUSD" asset
    Then I should see "Receive" button on asset item is enabled
    Then I should see "Send" button on asset item is enabled

  Scenario: Check If No Wallet Imported
    Given I don't have anny wallet import
    When I am on "Portfolio" page
    Then I should not see "Receive" button is visibled
    Then I should not see "Send" button is visibled

```

![](<../.gitbook/assets/Screen Shot 2565-07-08 at 14.25.57.png>)

## CI / CD

### CI: Github Actions + self host runner

> GitHub Actions is a **continuous integration and continuous delivery** (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that build and test every pull request to your repository, or deploy merged pull requests to production.

### **CD:** Fastlane&#x20;

> Automate your development and release process fastlane is an open source platform aimed at simplifying Android and iOS deployment. fastlane lets you automate every aspect of your development and release workflow.

### Build Pipeline

* Pull request
* Push develop
* Push main

![](../iOS/assets/pipeline-categories.png)

### Pipeline State

![](../iOS/assets/pipeline-state.png)

### Pipeline 1: On push request (PR)

![](../iOS/assets/pipeline-pr.png)

### Pipeline 2: On push request (Merged)

![](../iOS/assets/pipeline-push.png)

### Release & Tags

* Changed Log / Features Note
* Tags (Versioning symmetric https://semver.org)

![](../iOS/assets/release-tag.png)



## Tests monitor: codecov

![](../iOS/assets/codecov.png)

### Why? Github Actions + self-hosted runner

![Github action + cloud host runner](<../.gitbook/assets/Screen Shot 2565-07-10 at 20.51.46.png>)

![Mac Mini Cloud Hosting](<../.gitbook/assets/Screen Shot 2565-07-10 at 20.51.51.png>)

![Github runner on mac mini m1](<../.gitbook/assets/Screen Shot 2565-07-10 at 20.52.01 (1).png>)

![Fast delivery](<../.gitbook/assets/Screen Shot 2565-07-10 at 20.52.06.png>)

## Design System

* DEV x Designer (UI)
* StyleKit
* LanguageKit

![](<../.gitbook/assets/Screen Shot 2565-07-08 at 13.56.01.png>)

## Monitoring

* Crash monitoring: Firebase Crashlytics
* App performance: Firebase app performance
* Notification (Discord)

![](<../.gitbook/assets/Screen Shot 2565-07-08 at 13.54.34.png>)

## Analytics

* Mixpanel

![](../iOS/assets/mixpanel.png)



## Xcode Template

> XCode Templates is a tool for creating code snippets to give you a better starting point to achieve your goal. In this tutorial I will walk you through preparing a custom template for MVVM project architecture.
>
> #### Scene
>
> * NameViewController.swift
> * NamePresenter.swift
> * NameInteractor.swift
> * NameRouter.swift
> * NameFacade.swift
> * NameSceneModel.swift
> * NameSceneWorker.swift
> * Name.storybroad
>
> #### View Component
>
> * NameView.swift
> * NameViewModel.swift
>
>

![](../iOS/assets/xcode-2.png) ![](../iOS/assets/xcode-1.png)



