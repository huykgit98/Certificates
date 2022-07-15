## [fastlane match](https://docs.fastlane.tools/actions/match/)

This repository contains all your certificates and provisioning profiles needed to build and sign your applications. They are encrypted using OpenSSL via a passphrase.

**Important:** Make sure this repository is set to private and only your team members have access to this repo.

Do not modify this file, as it gets overwritten every time you run _match_.

### Installation

Make sure you have the latest version of the Xcode command line tools installed:

```
xcode-select --install
```

Install _fastlane_ using

```
[sudo] gem install fastlane -NV
```

or alternatively using `brew cask install fastlane`

### Usage

Navigate to your project folder and run

```
fastlane match appstore
```
```
fastlane match adhoc
```
```
fastlane match development
```
```
fastlane match enterprise
```

For more information open [fastlane match git repo](https://docs.fastlane.tools/actions/match/)

### New machine

To set up the certificates and provisioning profiles on a new machine, you just run the same command using:

```
fastlane match development
```

You can also run match in a readonly mode to be sure it won't create any new certificates or profiles.

```
fastlane match development --readonly
```

We recommend to always use readonly mode when running fastlane on CI systems. This can be done using

```
lane :beta do
  match(type: "appstore", readonly: is_ci)

  gym(scheme: "Release")
end
```

### Renew an expired certificate

Nuke command revoke your certificates and provisioning profiles, which leave you a clean slate for a new beginning.

```
fastlane match nuke development
fastlane match nuke distribution
fastlane match nuke enterprise
```

After clearing your account, you'll start from a clean slate, and you can run match to generate your certificates and profiles again.

```
fastlane match appstore
fastlane match distribution
fastlane match development
```

### Registering new devices

By using match, you'll save a lot of time every time you add new device to your Ad Hoc or Development profiles. Use match in combination with the register_devices action.

```
lane :beta do
  register_devices(devices_file: "./devices.txt")
  match(type: "adhoc", force_for_new_devices: true)
end
```

By using the force_for_new_devices parameter, match will check if the device count has changed since the last time you ran match, and automatically re-generate the provisioning profile if necessary. You can also use force: true to re-generate the provisioning profile on each run.

Important: The ```force_for_new_devices``` parameter is ignored for App Store provisioning profiles since they don't contain any device information.

If you're not using fastlane, you can also use the force_for_new_devices option from the command line:

```
fastlane match adhoc --force_for_new_devices
```

### Content

Passphrase: ```buvaty```

#### certs

This directory contains all your certificates with their private keys

#### profiles

This directory contains all provisioning profiles

------------------------------------

For more information open [fastlane match git repo](https://docs.fastlane.tools/actions/match/)
