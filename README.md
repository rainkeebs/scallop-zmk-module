# Scallop ZMK Config

## Instructions

### Brand new to ZMK?

1. You will need to [sign up for a GitHub account](https://github.com/signup), and [fork this repository](https://docs.github.com/en/get-started/quickstart/fork-a-repo#forking-a-repository) so you can edit it.
2. Navigate to the **Actions** tab and click the "I understand my workflows, go ahead and run them" button to enable builds.
   ![Actions tab with "I understand my workflows" button](https://i.imgur.com/B7cTAE6.png)
3. Edit the [config/scallop.conf](config/scallop.conf) to enable/disable features. Edit [config/scallop.keymap](config/scallop.keymap) to change the keymap. Lastly, make sure the [build.yaml](build.yaml) file has your board in the "boards" list.
4. After committing your changes, your firmware will begin compiling. Assuming there are no typos or other problems, it will eventually be [downloadable from the Actions tab](https://zmk.dev/docs/user-setup#installing-the-firmware).
5. [Flash the non-settings-reset firmware](https://zmk.dev/docs/user-setup#flashing-uf2-files).

### Do you have a `zmk-config` repo already?

You can add this repository as a module by modifying your `config/west.yml` file.

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    # Add the base GitHub URL as a remote.
    - name: npwzrd # You can name this whatever you like; just make sure the "remote" below matches.
      url-base: https://github.com/npwzrd
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
    # Add the name of the repository as a project.
    - name: scallop-zmk-module
      remote: npwzrd
      revision: main # This is the name of the branch you want to use.
  self:
    path: config
```

After making this change, add a copy of [the configuration file](config/scallop.conf) and [the keymap](config/scallop.keymap) to the `config` folder that is already in your repo.

## Common Questions/Problems

### There was an error and the build didn't finish.

#### There is probably an error in your keymap.

[Clicking into the details of the action](https://docs.github.com/en/actions/quickstart#viewing-your-workflow-results) will show the error log. While very long, this log will often show the exact line number causing the problem in your keymap so it is worth reading closely.

Carefully double-check the last changes you made. ZMK syntax is very different from QMK syntax. Note that when editing keymaps, each ZMK code is generally preceded by a reference categorizing what that code does.

For example: the letter "Z" is categorized as a **k**ey **p**ress. To add the letter "Z" to a keymap, it must be written `&kp Z`.

A few more examples below:

| QMK | ZMK |
| --- | --- |
| `KC_A` | `&kp A` |
| `QK_REBOOT` | `&sys_reset` |
| `LT(1, KC_SPACE)` | `&lt 1 SPACE` |

There are many, many more differences. Don't forget to [check the ZMK docs](https://zmk.dev/docs/features/keymaps).

[nickcoutsos's in-browser keymap editor](https://nickcoutsos.github.io/keymap-editor) aims to provide a GUI for remapping, which may be helpful.

### I edited the keymap/configuration and flashed, but nothing was updated.

#### Make sure you are editing the files in the [config](config) folder.

Changes made in the `boards/arm` folder will be overridden or ignored; your customizations to the base belong in [config](config).

### After disconnecting the keyboard from Bluetooth, it won't reconnect.

#### Forget the Bluetooth connection on both the Scallop and the host, then try re-pairing.

If you don't remember which profile on the Scallop was used to connect to a specific device, this may mean cycling through each (`&bt BT_PRV`/`&bt BT_NXT`) and clearing them (`&bt BT_CLR`) one by one. [See ZMK's documentation for more info](https://zmk.dev/docs/behaviors/bluetooth#bluetooth-pairing-and-profiles).

As a last resort, try flashing the `settings_reset` firmware; this will force-clear everything.

## Further Troubleshooting Resources

- [ZMK's troubleshooting page](https://zmk.dev/docs/troubleshooting)
- Ask the ZMK experts in [ZMK's Discord](https://zmk.dev/community/discord/invite)
- Stop by #help in [the 40% Discord](https://discord.gg/40percent)
