# Pacman Package Manager Guide

`pacman` is the default package manager for Arch Linux. It combines a simple binary package format with an easy-to-use build system.

## 1. System Maintenance
- **Update System**: Synchronize with repositories and update all packages.
  `sudo pacman -Syu`

## 2. Installing Packages
- **Install a Package**: Downloads and installs the specified package(s).
  `sudo pacman -S package_name`
- **Install as Dependency**: Install a package without marking it as explicitly installed.
  `sudo pacman -S --asdeps package_name`

## 3. Removing Packages
- **Remove Package**: Removes the package but leaves its dependencies.
  `sudo pacman -R package_name`
- **Remove Package and Dependencies**: Removes the package and any of its dependencies not required by other packages.
  `sudo pacman -Rs package_name`
- **Total Removal**: Removes a package, its dependencies, and all configuration files.
  `sudo pacman -Rns package_name`

## 4. Querying the Database
- **Search for Package**: Search for a package in the remote repositories.
  `pacman -Ss keyword`
- **List Installed Packages**: View everything installed on your system.
  `pacman -Q`
- **Search Installed**: Search for a keyword specifically in your installed packages.
  `pacman -Qs keyword`
- **Package Info**: Display detailed information about a specific package.
  `pacman -Qi package_name` (Local) / `pacman -Si package_name` (Remote)
- **Identify Owner**: Find out which package a specific file belongs to.
  `pacman -Qo /path/to/file`

## 5. Cleaning Up
- **Clean Cache**: Remove old, uninstalled package tarballs from `/var/cache/pacman/pkg/`.
  `sudo pacman -Sc`
- **Remove Orphans**: Remove packages that were installed as dependencies but are no longer required by any software.
  `sudo pacman -Rns $(pacman -Qdtq)`