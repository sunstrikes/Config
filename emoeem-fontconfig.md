# emoeem fontconfig deployment

Applied on 2026-08-02 for the KDE Plasma user session on CachyOS.

## Scope

- User fonts: `~/.local/share/fonts/emoeem-fontconfig/`
- Fontconfig: `~/.config/fontconfig/fonts.conf`
- KDE fonts: `~/.config/kdeglobals`

No `/etc/fonts`, pacman database, repository, bootloader, or display-manager
configuration was changed.

## Fonts

- MiSans 4.003 (official Xiaomi archive)
- LXGW ZhenKai GB 0.825 (upstream GitHub release)
- LXGW WenKai GB Screen 1.520 (upstream GitHub release; configured as the
  fallback for the upstream `LXGW WenKai Screen` name)
- ComicShannsMono Nerd Font 3.4.0 (Arch `otf-comicshanns-nerd` package contents)
- Maple Mono Normal NF CN 7.9 (available weights extracted from the upstream
  release archive)
- Sarasa Gothic 1.0.40, including Sarasa Mono SC (Arch
  `ttf-sarasa-gothic` package contents)
- Existing system `noto-fonts-cjk` and `noto-fonts-emoji`

The Fontconfig aliases follow
`https://github.com/emoeem/fontconfig/blob/main/font.config`: MiSans for
sans-serif, LXGW/Noto CJK for serif, and ComicShanns/Maple/Sarasa for
monospace. The configuration enables anti-aliasing, light hinting, RGB
subpixel rendering, the default LCD filter, disables embedded bitmaps except
for Noto Color Emoji.

KDE's general, menu, toolbar, small-text, and active-title fonts use MiSans;
its fixed-width font uses ComicShannsMono Nerd Font.

## Verification

```bash
fc-match sans-serif
fc-match monospace
fc-match 'LXGW ZhenKai GB'
fc-match 'Sarasa Mono SC'
fc-match -v sans-serif | rg 'antialias:|hintstyle:|rgba:|lcdfilter:'
systemctl --user is-active plasma-plasmashell.service
```

## Rollback

```bash
rm -rf ~/.local/share/fonts/emoeem-fontconfig
kwriteconfig6 --file kdeglobals --group General --key font 'Inter,10,-1,5,700,0,0,0,0,0,0,0,0,0,0,1,Bold,0,0'
kwriteconfig6 --file kdeglobals --group General --key menuFont 'Inter,10,-1,5,700,0,0,0,0,0,0,0,0,0,0,1,Bold,0,0'
kwriteconfig6 --file kdeglobals --group General --key toolBarFont 'Inter,10,-1,5,75,0,0,0,0,0,0,0,0,0,0,0,Bold'
kwriteconfig6 --file kdeglobals --group General --key smallestReadableFont 'Inter,8,-1,5,75,0,0,0,0,0,Bold'
kwriteconfig6 --file kdeglobals --group General --key fixed 'JetBrainsMono Nerd Font,10,-1,5,700,0,0,0,0,0,0,0,0,0,0,1,Bold,0,0'
kwriteconfig6 --file kdeglobals --group WM --key activeFont 'Inter,10,-1,5,75,0,0,0,0,0,Bold'
fc-cache -f
systemctl --user restart plasma-plasmashell.service
```

The rollback commands remove only the newly installed font directory and
restore the font values observed before this change. In `fonts.conf`, remove
the aliases and render matches added for this deployment while retaining the
pre-existing `Inter Variable` reject rule and `~/.local/share/fonts` directory.
