<h1 align="center">SALESMAN DBO</h1>

## Descriptions

A mobile app for your salesman to complete their daily activities, and is also connected to the Manager App so you can analyze their results anytime.

## FEATURE IMAGES/SCREENSHOT

<p align="center">

  <img src="./dashboard.jpeg" style="width:200px" align="center"/>

</p>

## Technologies Used

### Prerequisites

[![Node version][shield-node]](#)
[![Node.js version support][shield-watchman]](#)
[![Build status][shield-xcode]](#)
[![Code coverage][shield-cocoapods]](#)
[![Dependencies][shield-jdk]](#)
[![MIT licensed][shield-android-studio]](#)
[![MIT licensed][shield-android-sdk]](#)

[shield-node]: https://img.shields.io/badge/Node.js-%3E12-brightgreen
[shield-watchman]: https://img.shields.io/badge/Watchman-2022.09.05.00-brightgreen
[shield-xcode]: https://img.shields.io/badge/Xcode-12-brightgreen
[shield-cocoapods]: https://img.shields.io/badge/Cocoapods-1.10.1-brightgreen
[shield-jdk]: https://img.shields.io/badge/JDK-%3E%2011-brightgreen
[shield-android-studio]: https://img.shields.io/badge/android%20studio----brightgreen
[shield-android-sdk]: https://img.shields.io/badge/android%20SDK----brightgreen

### Framework

| Framework    | Description                                                                                                                                                           |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| React Native | an open-source JavaScript framework, designed for building apps on multiple platforms like iOS, Android, and also web applications, utilizing the very same code base |

## Cloning and Running the Application in local

Clone the project into local
`git clone git@bitbucket.org:admin_dbo/mobile-salesman-service.git` to clone the project to your local environment. You can also download the project manually by choosing the `mobile-salesman-service` branch and download the `zip` file.

### Setup environments

The template already has scripts to execute the project calling a specific environment defined into the package.json file. Keep in mind that if you are going to create new `envs` you have to define the script to build the project properly.

To define which env you want to use, just keep the structure `yarn [platform]: [environment]`

DEV: `yarn ios` or `yarn android`

STG: `yarn ios:staging` or `yarn android:staging`

PROD: `yarn ios:prod` o `yarn android:prod`

Also, you can use npm following the same rule as before: `npm run ios:staging`

Modify the environment variables files in root folder (`.env.development`, `.env.production` and `.env.staging`)

## Status

DBO Salesman app is still in progress.
