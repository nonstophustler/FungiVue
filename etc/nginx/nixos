# Minimal configuration for NixOS, optimized for open source kit dev (e.g., ultrasound mushroom imaging). Essentials only; commented parts for script mods.

{ config, pkgs, lib, ... }:

{
  imports = [
    ./hardware-configuration.nix  # Hardware scan results (keep minimal)
  ];

  # Bootloader (essential for boot)
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

  # Kernel (latest for hardware support, cost-free)
  boot.kernelPackages = pkgs.linuxPackages_latest;

  # Networking (basic hostname and manager)
  networking.hostName = "nixos-kit-dev";
  networking.networkmanager.enable = true;

  # Time zone (default to user preference)
  time.timeZone = "America/Los_Angeles";

  # Internationalisation (minimal UTF-8)
  i18n.defaultLocale = "en_US.UTF-8";

  # Enable SDDM login manager and Cinnamon desktop (essentials)
  services.xserver.enable = true;
  services.xserver.displayManager.sddm.enable = true;
  services.xserver.desktopManager.cinnamon.enable = true;

  # User account (basic for dev)
  users.users.devuser = {
    isNormalUser = true;
    description = "Kit Developer";
    extraGroups = [ "networkmanager" "wheel" ];
    shell = pkgs.bash;
  };

  # Allow unfree packages (for Brave, etc.)
  nixpkgs.config.allowUnfree = true;

  # System packages (essentials: Python, Brave)
  environment.systemPackages = with pkgs; [
    python3Full  # For kit scripts (e.g., imaging/ML in ultrasound kit)
    brave  # Browser for research/docs
    git  # For cloning open source repos
    vim  # Basic editor
  ];

  # Commented configurations for bash script modifications (uncomment/enable as needed for kit-specific features)

  # # Enable Docker (for containerized kit testing, e.g., aquaponics sim)
  # virtualisation.docker = {
  #   enable = true;
  #   enableOnBoot = true;
  # };

  # # Enable Hyprland (alternative WM for lightweight kit GUIs)
  # programs.hyprland = {
  #   enable = true;
  #   xwayland.enable = true;
  # };

  # # Sound with PipeWire (for multimedia kits like bio reactor audio logging)
  # services.pipewire = {
  #   enable = true;
  #   alsa.enable = true;
  #   pulse.enable = true;
  # };

  # # Graphics support (for visualization in educational kits)
  # hardware.graphics.enable = true;

  # # Garbage collection (weekly, keeps last 5 gens for cost-efficient storage)
  # systemd.timers.nix-gc-generations = {
  #   wantedBy = [ "timers.target" ];
  #   timerConfig = {
  #     OnCalendar = "weekly";
  #     Persistent = true;
  #   };
  # };
  # systemd.services.nix-gc-generations = {
  #   script = ''
  #     generations_to_delete=$(${pkgs.nix}/bin/nix-env -p /nix/var/nix/profiles/system --list-generations | ${pkgs.gawk}/bin/awk '{print $1}' | ${pkgs.coreutils}/bin/head -n -5 | ${pkgs.coreutils}/bin/tr '\n' ' ')
  #     if [ -n "$generations_to_delete" ]; then
  #       ${pkgs.nix}/bin/nix-env -p /nix/var/nix/profiles/system --delete-generations $generations_to_delete
  #     fi
  #     ${pkgs.nix}/bin/nix-collect-garbage
  #   '';
  #   serviceConfig.Type = "oneshot";
  # };

  # NixOS release version
  system.stateVersion = "25.05";
}
