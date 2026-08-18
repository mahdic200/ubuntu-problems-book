## Color Importer

```bash
#!/bin/bash

# Import an 18-entry RGB palette (foreground, background, 16 ANSI colors)

# into a new Konsole color scheme.

#

# Usage:

# ./konsole-import-palette.sh "MyScheme"

#

# Edit the `colors` array below to change the palette.

  

set -euo pipefail

  

scheme_name="${1:-MyImportedScheme}"

out_dir="$HOME/.local/share/konsole"

out_file="$out_dir/${scheme_name}.colorscheme"

  

mkdir -p "$out_dir"

  

# foreground, background, color0..color7 (normal), color0..color7 (intense)

colors=(

"rgb(0,172,161)" # 0 foreground

"rgb(0,1,26)" # 1 background

"rgb(23,20,33)" # 2 color0 (black)

"rgb(192,28,40)" # 3 color1 (red)

"rgb(38,162,105)" # 4 color2 (green)

"rgb(162,115,76)" # 5 color3 (yellow)

"rgb(0,85,190)" # 6 color4 (blue)

"rgb(163,71,186)" # 7 color5 (magenta)

"rgb(42,161,179)" # 8 color6 (cyan)

"rgb(208,207,204)" # 9 color7 (white)

"rgb(94,92,100)" # 10 color0 intense

"rgb(246,97,81)" # 11 color1 intense

"rgb(51,209,122)" # 12 color2 intense

"rgb(233,173,12)" # 13 color3 intense

"rgb(42,123,222)" # 14 color4 intense

"rgb(192,97,203)" # 15 color5 intense

"rgb(51,199,222)" # 16 color6 intense

"rgb(255,255,255)" # 17 color7 intense

)

  

if [ ${#colors[@]} -ne 18 ]; then

echo "Error: expected 18 colors, got ${#colors[@]}" >&2

exit 1

fi

  

# Parse "rgb(r,g,b)" -> "r,g,b"

rgb_to_triplet() {

echo "$1" | grep -o '[0-9]\+' | paste -sd, -

}

  

fg=$(rgb_to_triplet "${colors[0]}")

bg=$(rgb_to_triplet "${colors[1]}")

  

{

echo "[General]"

echo "Description=$scheme_name"

echo "Opacity=1"

echo "Wallpaper="

echo

  

echo "[Background]"

echo "Color=$bg"

echo

echo "[BackgroundIntense]"

echo "Color=$bg"

echo

  

echo "[Foreground]"

echo "Color=$fg"

echo

echo "[ForegroundIntense]"

echo "Color=$fg"

echo

  

# Normal colors: indices 2-9 -> Color0..Color7

for i in $(seq 0 7); do

idx=$((i + 2))

triplet=$(rgb_to_triplet "${colors[$idx]}")

echo "[Color${i}]"

echo "Color=$triplet"

echo

done

  

# Intense colors: indices 10-17 -> Color0Intense..Color7Intense

for i in $(seq 0 7); do

idx=$((i + 10))

triplet=$(rgb_to_triplet "${colors[$idx]}")

echo "[Color${i}Intense]"

echo "Color=$triplet"

echo

done

} > "$out_file"

  

echo "Wrote color scheme: $out_file"

echo "Select it in Konsole via: Settings > Edit Current Profile > Appearance > Color scheme > $scheme_name"
```