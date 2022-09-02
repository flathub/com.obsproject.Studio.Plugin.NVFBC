# Deprecation notice

As of OBS version 28.0.0 (released in 2022-09-01), this plugin no longer works. You can find more details as to why [here](https://gitlab.com/fzwoch/obs-nvfbc/-/issues/6#note_1071642813).

If you really want to use this plugin, you will have to downgrade OBS back to version 27.2.4, using this command:

```bash
# Remove 'sudo' if you installed OBS only for the current user.
sudo flatpak update --commit=0069ec300ce09337a585acfcefe0f2ecdfa0efcd08d920b2751c844fd026296a com.obsproject.Studio
```

And then you'll have to disable Flatpak from auto-updating OBS with:

```bash
flatpak mask com.obsproject.Studio
```

# NVFBC plugin for OBS Studio (Flatpak)

This plugin will be useless for you, unless you have a Tesla/Quadro GPU from NVIDIA, **or** patched NVIDIA drivers.

## HOWTO

**DISCLAIMER**: The following steps modify the behavior of your NVIDIA drivers. Although myself and many other people have been using patched drivers without issues, I will not be responsible for any damages that might happen. So, proceed at your own risk!

### 1 - Patching the drivers

**Note**: Skip this step if you have a professional-grade GPU model, such as Quadro or Tesla.

1. Clone the [nvidia-patch](https://github.com/keylase/nvidia-patch) project:

    ```bash
    git clone https://github.com/keylase/nvidia-patch.git
    ```

2. Patch the NVIDIA drivers (host drivers first):

    ```bash
    cd nvidia-patch/
    sudo ./patch.sh
    sudo ./patch-fbc.sh
    ```

3. Now patch the drivers for Flatpak:

    ```bash
    sudo ./patch.sh -f
    sudo ./patch-fbc.sh -f
    ```

4. Reboot your computer.

### 2 - Adding the NvFBC Source plugin on OBS

1. Open OBS Studio.
2. Right-click the `Sources` panel, go to `Add` and then select `NvFBC Source`.
3. (Optional) rename your source, hit `OK`.
4. (Optional) tweak the settings (source screen, FPS, etc.), hit `OK`.

    The whole process looks like this:
    ![GIF](../master/images/obs-add-source.gif?raw=true)

## Reverting

If, for whatever reason, you want to revert this whole process, you can restore the original driver files by running:

```bash
# Restores the original driver files for the host
sudo ./patch.sh -r
sudo ./patch-fbc.sh -r
# Restores the original driver files for Flatpak
sudo ./patch.sh -f -r
sudo ./patch-fbc.sh -f -r
```

Uninstall this plugin:

```bash
flatpak uninstall com.obsproject.Studio.Plugin.NVFBC
```

Then reboot your computer.