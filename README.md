# NixOS SDDM Wayland Rotate Display
Steps and Configuration to rotate the SDDM Display with Wayland through Weston on NixOS

1. To get the command SDDM uses to launch weston:
  ```
  grep weston /etc/sddm.conf
  ```
3. Our NixOS configuration /etc/nixos/configuration.nix:
  ```
  services.displayManager.sddm.enable = true;
  services.displayManager.sddm.wayland.enable = true;
  services.displayManager.sddm.wayland.compositor = "weston";
  services.displayManager.sddm.settings = {
  Wayland = {
    CompositorCommand = "/nix/store/xzx28k96shd3l886s676iq01ngjkwi8n-weston-13.0.1/bin/weston --shell=kiosk -c /tmp/weston.ini";
  };
};
  ```
3. Copy the existing weston.ini to tmp so we can edit it:
  ```
  cp /nix/store/xi5fnv7j6p0byqv3k7hczk4l2az5jf54-weston.ini /tmp/weston.ini
  ```
4. Append to /tmp/weston.ini your output and the rotation you prefer. Example:
  ```
  [output]
  name=DSI-1
  transform=rotate-270
  ```
Ideally a programs.weston.config or something similar to modify the existing weston.ini would remove the need for a rw copy of the weston.ini.
