### CryptoTradingTools

Jekyll theme for [crypto trading tools](https://cryptotradingtools.com) -- site recovered after crash with no backup. 

Based on free jekyll theme **Dbyll**.

```
#!/bin/bash

echo "starting update..."
cd ct-tools-recovery
git pull
rsync -avP ./_site/ ../public_html
echo "done"
```

### License
- [MIT](http://opensource.org/licenses/MIT)

