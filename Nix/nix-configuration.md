#todo #linux #nix #nixos 

Relates: [[try hyprland]]
Relates: [[setup fan control]]

## Setup nvidia drivers:
To set up Nvidia drivers on NixOS, follow these steps:
Enable Unfree Packages:
Add the following line to your configuration.nix to allow unfree packages:
nix
```
nixpkgs.config.allowUnfree = true;
```

Configure Nvidia Drivers:
Add the necessary configurations to your configuration.nix:
nix
```
{ config, lib, pkgs, ... }:

{
  # Enable OpenGL
  hardware.opengl = {
    enable = true;
    driSupport = true;
    driSupport32Bit = true;
  };

  # Load Nvidia driver for Xorg and Wayland
  services.xserver.videoDrivers = ["nvidia"];

  hardware.nvidia = {
    modesetting.enable = true;
    nvidiaSettings = true;
    powerManagement.enable = true;  # Optional
    package = config.boot.kernelPackages.nvidiaPackages.stable;  # Optional: specify driver version
  };
}
```

