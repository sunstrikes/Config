# Garuda Mokka KDE Configuration

This documents the user-level Garuda Mokka desktop setup applied on CachyOS
with KDE Plasma 6.7 on Wayland. It intentionally preserves CachyOS package
repositories, GRUB, SDDM, and distribution-wide defaults.

## Scope

The visual and layout baseline is the official Garuda `garuda-mokka` package
version 1.4.8. System-level Garuda dependencies were not installed. Official
theme assets and Plasma configuration were installed in the user profile.

## Active Appearance

| Setting | Value |
| --- | --- |
| Global theme | `Mokka` |
| Plasma theme | `Mokka` |
| Color scheme | `Mokka` |
| Qt style | `kvantum-dark` with Kvantum `Mokka` |
| Icon theme | `Tela-circle-dracula-dark` |
| Cursor | `Catppuccin-Mocha-Mauve-Cursors` |
| Window decoration | `CatppuccinMocha-Classic` Aurorae |
| GTK theme | `Catppuccin-Purple-Dark-Catppuccin` |
| Interface font | Inter Bold 10 |
| KDE monospace font | Liberation Mono Bold 10 |
| Wallpaper | `Mokka-tree.jpg` through the `a2n.blur` plugin |
| Konsole profile | `sunstrike.profile` with the `Mokka` color scheme |

## User-Level Resources

| Resource | Location |
| --- | --- |
| Mokka Look-and-Feel and Plasma theme | `~/.local/share/plasma/` |
| Garuda panel and Dock templates | `~/.local/share/plasma/layout-templates/` |
| Kvantum Mokka theme | `~/.local/share/Kvantum/Mokka/` |
| Kvantum Qt6 style plugin | `~/.local/lib/qt6/plugins/styles/libkvantum.so` |
| Mokka color scheme | `~/.local/share/color-schemes/Mokka.colors` |
| Konsole Mokka scheme | `~/.local/share/konsole/Mokka.colorscheme` |
| Mokka wallpapers | `~/.local/share/wallpapers/garuda-mokka/` |
| Garuda launcher icons | `~/.local/share/icons/garuda/` |
| Tela Dracula Dark icons | `~/.local/share/icons/Tela-circle-dracula-dark/` |
| Catppuccin Mauve cursor | `~/.local/share/icons/catppuccin-mocha-mauve-cursors/` |
| Inter and JetBrains Mono Nerd Font | `~/.local/share/fonts/mokka/` |
| GTK theme | `~/.local/share/themes/Catppuccin-Purple-Dark-Catppuccin/` |

The GTK theme was generated with the same Colloid parameters used by the
official Garuda PKGBUILD:

```bash
./install.sh -n Catppuccin -t purple -c dark --tweaks catppuccin
```

## Font Alignment

KDE uses the official Mokka font settings: Inter Bold 10 for interface, menu,
toolbar, and window-title text; Inter Bold 8 for the smallest readable font;
and Liberation Mono Bold 10 for the KDE fixed-width font. GTK 3 and GTK 4 use
Inter Bold 10 with the same Tela icon and Catppuccin GTK theme.

The display uses 2x scale, so GTK keeps `gtk-xft-dpi=196608` (192 DPI) rather
than Garuda's default 120 DPI. `~/.config/fontconfig/fonts.conf` rejects the
Inter Variable family so KDE uses the installed static Inter faces and their
explicit Bold style correctly. Chinese glyphs fall back to the locally
installed PingFang SC font.

Kvantum is extracted from CachyOS's official package into the user profile.
`~/.config/environment.d/mokka-kvantum.conf` adds its Qt6 plugin path, and KDE
is configured with `widgetStyle=kvantum-dark` and Kvantum theme `Mokka`.

Kvantum's manager enumerates user themes from `~/.config/Kvantum/`. The
official Mokka resource remains at `~/.local/share/Kvantum/Mokka/`, with
`~/.config/Kvantum/Mokka` linked to it. The active configuration is written
through `kvantummanager --set Mokka` as `[General] theme=Mokka`; do not leave
`kvantum.kvconfig` empty. New Qt Widgets applications load
`~/.local/lib/qt6/plugins/styles/libkvantum.so` from the Plasma session
environment. Close and reopen existing Qt applications after changing this
setting. GTK applications continue to use Catppuccin by design.

## Plasma Layout

The current layout was reset from the official `Mokka` Look-and-Feel package.

- Top panel: Garuda launcher, Window Buttons, Window Title, global menu,
  centered digital clock, Panel Colorizer, system tray, and user switcher.
- Bottom Dock: centered, dodge-windows, icons-only task manager and Panel
  Colorizer. Its task manager keeps `fill=false` and overrides the official
  template's `iconSpacing=2` with `iconSpacing=0`, which removes the extra
  horizontal task-frame margin and keeps the icon cells compact and square.
- The wallpaper uses `a2n.blur` with `Mokka-tree.jpg`, active blur enabled,
  blur radius 40, and fill mode 2.

The four official Panel Colorizer presets are stored at:

```text
~/.local/share/garuda-mokka/panel-colorizer-presets/
```

The local copies of the official panel and Dock templates reference this path
instead of Garuda's `/usr/share` path. The preset data itself is unchanged.

The Panel Colorizer `Black` preset is stored at:

```text
~/.local/share/garuda-mokka/panel-colorizer-presets/Mokka Black/
```

Both Mokka Panel Colorizer applets use this preset. The user-level Mokka layout
templates persist the Black settings, and the Dock template keeps
`iconSpacing=0` so the task-manager cells remain compact and square:

```text
~/.local/share/plasma/layout-templates/org.garuda.desktop.defaultPanel/contents/layout.js
~/.local/share/plasma/layout-templates/org.garuda.desktop.defaultDock/contents/layout.js
```

The pre-change files are backed up under
`~/.config/kde-mokka-backups/20260802-154943-mokka-black/`.

## Installed Plasma And KWin Extensions

Installed through `paru` from AUR:

```text
kwin-effects-better-blur-dx 2.5.1-1
kwin-effect-rounded-corners-git 0.9.0.r14.g46b9436-1
plasma6-applets-window-title 0.9.0-2
plasma6-applets-panel-colorizer 7.3.0-1
plasma6-wallpapers-blurredwallpaper 3.5.2-1
```

KWin is configured with Garuda Mokka values for Better Blur DX and rounded
corners: 20px corner radius, blur decorations enabled, and the standard Mokka
Cover Switch, Desktop Change OSD, Fade, Magic Lamp, and Wobbly Windows flags.

### Rendering Stability Note

On 2026-08-02, standard Blur and Better Blur DX were restored for the Mokka
menu material, and Better Blur DX menu blur was enabled. The following are the
current KWin values:

| KWin effect | `~/.config/kwinrc` key | Applied value |
| --- | --- | --- |
| Translucency | `translucencyEnabled` | `false` |
| Blur | `blurEnabled` | `true` |
| Better Blur DX | `better_blur_dxEnabled` | `true` |
| Better Blur DX menus | `Effect-better-blur-dx/BlurMenus` | `true` |

The translucency effect remains disabled in response to KDE Bug 505775, which
reports NVIDIA/Wayland flickering with translucent windows. Blur and menu
material can reproduce that symptom on this host; disable the two blur keys
again if it returns. `a2n.blur` wallpaper configuration, Panel Colorizer,
rounded corners, packages, NVIDIA driver, and system-wide CachyOS settings are
unchanged.

Verify the persisted values and runtime active-effect state with:

```bash
kreadconfig6 --file kwinrc --group Plugins --key translucencyEnabled
kreadconfig6 --file kwinrc --group Plugins --key blurEnabled
kreadconfig6 --file kwinrc --group Plugins --key better_blur_dxEnabled
kreadconfig6 --file kwinrc --group Effect-better-blur-dx --key BlurMenus
qdbus6 org.kde.KWin /Effects org.freedesktop.DBus.Properties.Get \
  org.kde.kwin.Effects activeEffects
```

To disable blur again, restore
`~/.config/kde-mokka-backups/20260802-144445-restore-kwin-menu-blur/kwinrc`
and reload KWin, or set the two plugin keys and `BlurMenus` to `false` with
`kwriteconfig6`.

### Window Buttons Compatibility Build

Garuda's referenced AUR package `plasma6-applets-window-buttons` was no
longer compatible with Plasma 6.7 because it used the retired KDecoration2
interface. The current upstream `applet-window-buttons6` v0.14.0 was built
against KDecoration3 and installed to `~/.local`.

Only its `CMAKE_CXX_STANDARD` was updated from 14 to 20 because KDecoration3
headers require C++20. The plugin ID remains the official Mokka layout ID:

```text
org.kde.windowbuttons
```

`~/.config/environment.d/mokka-window-buttons.conf` adds the required user
QML import path. Plasma Shell was restarted after setting this environment;
the module loads successfully and no longer reports an import error.

## Applied Configuration Files

```text
~/.config/kdeglobals
~/.config/plasmarc
~/.config/kwinrc
~/.config/kcminputrc
~/.config/Kvantum/kvantum.kvconfig
~/.config/gtk-3.0/
~/.config/gtk-4.0/
~/.config/plasma-org.kde.plasma.desktop-appletsrc
~/.config/environment.d/mokka-window-buttons.conf
~/.config/environment.d/mokka-kvantum.conf
~/.config/konsolerc
~/.local/share/konsole/sunstrike.profile
```

Konsole now uses the installed official `Mokka.colorscheme` for both the
default `sunstrike` profile and its UI settings. Existing font, history, and
interaction preferences were preserved.

## Backups And Rollback

All pre-change KDE files are backed up under:

```text
~/.config/kde-mokka-backups/20260801-222312/
```

The Konsole-specific pre-change files are also saved at:

```text
~/.config/kde-mokka-backups/20260802-124014-konsole/
```

The Dock spacing rollback point is also saved at:

```text
~/.config/kde-mokka-backups/20260802-154530-dock-spacing/
```

The configuration immediately before disabling KWin translucency and blur is
saved at:

```text
~/.config/kde-mokka-backups/20260802-130208-disable-kwin-transparency-blur/
```

`before-mokka-layout/` contains the panel and KWin configuration immediately
before the official Mokka layout reset. `user-theme-preapply/` contains the
Kvantum, GTK, and input configuration before the global theme was applied.

To restore manually, copy the desired backed-up files back to their original
locations and restart Konsole. Restore `konsolerc` to `~/.config/` and
`sunstrike.profile` to `~/.local/share/konsole/`.
