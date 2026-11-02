# Legal notice and disclaimer

**English** · [Español](AVISO-LEGAL.md)

Read this notice before downloading, installing or using **G9 Control** or the information in this repository. By doing so, you accept its terms.

## 1. Unofficial project

G9 Control is an **independent, personal, non‑profit** project. It is **not affiliated with, sponsored, endorsed or approved** by Lenovo, GameSir / Guangzhou Xiaoji Technology, Microsoft, Sony Interactive Entertainment or any other company.

Lenovo, Legion, GameSir, Xbox, DualSense, PlayStation and all other trademarks mentioned belong to their respective owners. They are mentioned only to identify the hardware the app is compatible with.

## 2. Source of the information

The technical information in this repository was obtained by:

- observing the behavior of the controller connected to a device owned by the author (USB traffic, responses to commands and input events);
- analyzing the official app, **solely for the purpose of achieving interoperability** between the controller and an alternative app.

This repository **does not contain or distribute** third‑party code, binaries, resources, firmware, manuals or screenshots. The tables and frame examples are original. No protection measure has been circumvented.

## 3. No warranty

The app and the documentation are provided **“as is”**, **without warranty of any kind**, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, accuracy and absence of errors.

The information may be **incomplete or incorrect**. It was tested with a single controller and a single device: controller firmware 0.10, GCM protocol 2.8, Lenovo Legion Y700 Gen5 tablet. Other firmware versions, hardware revisions or Android versions may behave differently.

## 4. Use at your own risk

Using G9 Control or sending commands to the controller **is the sole responsibility of whoever does it**. Keep in mind that:

- The app **modifies the configuration stored in the controller**. Save a backup before changing anything.
- The controller firmware has known bugs that can **hang and reboot it**, or cause a write to **be lost** (see [Firmware issues and limitations](docs/05-firmware-issues-and-limitations.md)).
- The hex console lets you send **any command**. A wrong command can leave the controller in an unexpected state. Use it only if you know what you are doing.
- G9 Control **does not update the firmware** or enter update mode. Do not attempt it with unofficial tools.
- Using unofficial software **may affect the manufacturer’s warranty**. Check its terms.

## 5. Limitation of liability

To the maximum extent permitted by applicable law, **the authors shall not be liable for any damage**, whether direct, indirect, incidental, special or consequential, arising from the use of or inability to use G9 Control or the information in this repository. This includes, but is not limited to:

- damage to the controller, the tablet or other devices;
- loss of configuration or data;
- malfunction in games or applications;
- loss of warranty.

## 6. Privacy

G9 Control **does not request internet access**, does not collect personal data, contains no ads or analytics and sends no information to any server. It only talks to the controller over USB. Backups, profiles and logs are stored in the app’s storage on your device. Backups include the controller’s unique identifier (chip ID): do not share them. Profiles do not include it, so they can be shared.

## 7. Law

Laws on reverse engineering and interoperability vary by country. This notice **is not legal advice**. If you have questions about how it applies in your jurisdiction, consult a professional.

## 8. Takedown

If you hold any right and believe this repository infringes it, open an *issue* or contact the maintainer. The content will be reviewed and, if appropriate, removed.
