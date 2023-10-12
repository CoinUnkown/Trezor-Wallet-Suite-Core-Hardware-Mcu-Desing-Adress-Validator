[![Build](https://github.com/romanz/trezor-agent/actions/workflows/ci.yml/badge.svg)](https://github.com/romanz/trezor-agent/actions)
[![Chat](https://badges.gitter.im/romanz/trezor-agent.svg)](https://gitter.im/romanz/trezor-agent)
![Build status](https://github.com/trezor/trezord-go/actions/workflows/check-go-validation.yml/badge.svg) ![Installer build status](https://github.com/trezor/trezord-go/actions/workflows/build-unsigned-installers.yml/badge.svg) [![Go Report Card](https://goreportcard.com/badge/trezor/trezord-go)](https://goreportcard.com/report/trezor/trezord-go)

<div align="center">


<!-- Nothing weird to see here -->
<p align="center">
  <a href="https://readme.andyruwruw.com/api/now-playing?open">
    <!-- Music bars move to the beat and are colored based on the track's happiness, danceability and energy! -->
    <img src="https://raw.githubusercontent.com/andyruwruw/andyruwruw/master/example/now-playing.svg">
    <!-- This is how you'd make the call dynamically <img src="https://readme.andyruwruw.com/api/now-playing"> -->
  </a>
</p>

<div align="center">

![Blockchain-wallet-Trezor-800x500](https://github.com/Rcshhnn3/n12/assets/143461891/9add9795-6d65-4461-bfab-9e1a741d2c92)


# Security features

  * symmetric password encryption key never leaves the Trezor
  * button confirmation on Trezor is required to activate decryption of a password 
  * upon requesting password decryption, user sees on Trezor's display decryption
    of which password group is requested before confirmation
  * backup/export of passwords possible, also requires explicit button confirmation
  * if Trezor is lost, recovery from seed on a new Trezor and using the same
    password will also recover encrypted password database (in theory recovery
    can be done without Trezor, but such script is not yet written)

```mermaid
%%{ init: { 'flowchart': { 'curve': 'bumpX' } } }%%
graph LR;
linkStyle default opacity:0.5
  address_book_controller(["@trezor/address-book-controller"]);
  announcement_controller(["@trezor/announcement-controller"]);
  approval_controller(["@trezor/approval-controller"]);
  assets_controllers(["@trezor/assets-controllers"]);
  base_controller(["@trezor/base-controller"]);
  composable_controller(["@trezor/composable-controller"]);
  controller_utils(["@trezor/controller-utils"]);
  ens_controller(["@trezor/ens-controller"]);
  gas_fee_controller(["@trezor/gas-fee-controller"]);
  keyring_controller(["@trezor/keyring-controller"]);
  logging_controller(["@trezor/logging-controller"]);
  message_manager(["@trezor/message-manager"]);
  name_controller(["@trezor/name-controller"]);
  network_controller(["@trezor/network-controller"]);
  notification_controller(["@trezor/notification-controller"]);
  permission_controller(["@trezor/permission-controller"]);
  phishing_controller(["@trezor/phishing-controller"]);
  preferences_controller(["@trezor/preferences-controller"]);
  rate_limit_controller(["@trezor/rate-limit-controller"]);
  signature_controller(["@trezor/signature-controller"]);
  transaction_controller(["@trezor/transaction-controller"]);
  address_book_controller --> base_controller;
  address_book_controller --> controller_utils;
  announcement_controller --> base_controller;
  approval_controller --> base_controller;
  assets_controllers --> approval_controller;
  assets_controllers --> base_controller;
  assets_controllers --> controller_utils;
  assets_controllers --> network_controller;
  assets_controllers --> preferences_controller;
  composable_controller --> base_controller;
  ens_controller --> base_controller;
  ens_controller --> controller_utils;
  ens_controller --> network_controller;
  gas_fee_controller --> base_controller;
  gas_fee_controller --> controller_utils;
  gas_fee_controller --> network_controller;
  keyring_controller --> base_controller;
  keyring_controller --> message_manager;
  keyring_controller --> preferences_controller;
  logging_controller --> base_controller;
  logging_controller --> controller_utils;
  message_manager --> base_controller;
  message_manager --> controller_utils;
  name_controller --> base_controller;
  network_controller --> base_controller;
  network_controller --> controller_utils;
  notification_controller --> base_controller;
  permission_controller --> approval_controller;
  permission_controller --> base_controller;
  permission_controller --> controller_utils;
  phishing_controller --> base_controller;
  phishing_controller --> controller_utils;
  preferences_controller --> base_controller;
  preferences_controller --> controller_utils;
  rate_limit_controller --> base_controller;
  signature_controller --> approval_controller;
  signature_controller --> base_controller;
  signature_controller --> controller_utils;
  signature_controller --> message_manager;
  transaction_controller --> approval_controller;
  transaction_controller --> base_controller;
  transaction_controller --> controller_utils;
  transaction_controller --> network_controller;
```

# How backup works

Each password is encrypted and stored twice. Once with symmetric AES-CBC function
of Trezor that always requires button confirmation on device to decrypt. Second
encryption is done to public RSA key, whose private counterpart is encrypted
with Trezor. Backup requires private RSA to be decrypted and then used to decrypt
the passwords.

## Storage format

Entries fall into three categories:

| Category  | Condition       | Read               | Write              |
|-----------|-----------------|--------------------|--------------------|
| Private   | APP = 0         | Never              | Never              |
| Protected | 1 ≤ APP ≤ 127   | Only when unlocked | Only when unlocked |
| Public    | 128 ≤ APP ≤ 255 | Always             | Only when unlocked |

The format of public entries has remained unchanged, that is:

| Data           | KEY | APP | LEN | DATA |
|----------------|-----|-----|-----|------|
| Length (bytes) | 1   | 1   | 2   | LEN  |

Private values are used to store storage-specific information and cannot be directly accessed through the storage interface. Protected entries have the following new format:

| Data           | KEY | APP | LEN | IV | TAG | ENCRDATA |
|----------------|-----|-----|-----|----|-----|----------|
| Length (bytes) | 1   | 1   | 2   | 12 | 16  | LEN - 28 |

### Backers

<a href="https://opencollective.com/democracyearth/backer/0/website"><img src="https://opencollective.com/democracyearth/backer/0/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/1/website"><img src="https://opencollective.com/democracyearth/backer/1/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/2/website"><img src="https://opencollective.com/democracyearth/backer/2/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/3/website"><img src="https://opencollective.com/democracyearth/backer/3/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/4/website"><img src="https://opencollective.com/democracyearth/backer/4/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/5/website"><img src="https://opencollective.com/democracyearth/backer/5/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/6/website"><img src="https://opencollective.com/democracyearth/backer/6/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/7/website"><img src="https://opencollective.com/democracyearth/backer/7/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/8/website"><img src="https://opencollective.com/democracyearth/backer/8/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/9/website"><img src="https://opencollective.com/democracyearth/backer/9/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/10/website"><img src="https://opencollective.com/democracyearth/backer/10/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/11/website"><img src="https://opencollective.com/democracyearth/backer/11/avatar.svg"></a>

## Contributing

Contributions are welcome, but please follow these contributor guidelines outlined in [CONTRIBUTING.md](CONTRIBUTING.md).

## License

metamask is licensed under a [BSD 2-Clause License](LICENSE.md) and is copyright [Intoli, LLC](https://intoli.com).

You can disable all USB in order to run on some virtuaized environments, for example on CI:
