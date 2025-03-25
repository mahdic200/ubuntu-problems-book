# wine font

For fixing wine font (small text issue) , you have to type :

```bash
wine regedit
```

Then you have to navigate to `HKEY_CURRENT_CONFIG` and `Software` and then `Fonts` and change the `LogPixels` setting .

double click on it and then select the `Deciaml` radio box .

For fixing the small text issue according to my current resolution (1920 * 1080) , the value `150` is excellent .