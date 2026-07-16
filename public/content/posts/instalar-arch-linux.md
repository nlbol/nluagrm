## Requisitos

- Conexión a internet por cable
- Una USB de al menos 2GB
- Arquitectura x86_64

## Paso 1: Descargar la ISO

Descargar desde [archlinux.org/download](https://archlinux.org/download) y grabar en USB:

```bash
dd if=archlinux.iso of=/dev/sdX bs=4M status=progress
```

## Paso 2: Arrancar desde la USB

Reiniciar e ingresar al menú de arranque (F12, F2 o Del según el equipo).

## Paso 3: Particionado

```bash
fdisk -l
fdisk /dev/sda
# Crear particiones: EFI, swap, root
```

## Paso 4: Instalación base

```bash
pacstrap -K /mnt base linux linux-firmware
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

> Consultar la [Arch Wiki](https://wiki.archlinux.org/title/Installation_guide) para la guía oficial completa.
